---
title: "Modern email can be built from borrowed parts"
source: "https://en.andros.dev/blog/d7ed8b07/modern-email-can-be-built-from-borrowed-parts/?utm_source=hackernewsletter&utm_medium=email&utm_term=fav"
author:
published: 2026-07-27
created: 2026-08-02
---
Let's design the successor to email on top of HTTP, fixing the design flaws SMTP has been dragging along for 40 years. Piece by piece. The goal is not to replace the current mail system, heaven forbid!, but to learn, have fun and discover current technologies to swap out every element: sending, receiving, gateway, keys, and so on. The system will never talk to Gmail or any classic email provider: it only talks to itself. The only thing we are going to keep is the shape of the addresses, `user@domain`. Everything else gets reinvented.

Since it is a mail system over HTTP, a good name for the protocol would be HMTP: Hypertext Mail Transfer Protocol (the S for *Simple* in SMTP gives up its seat to the H for HTTP).

Now it's time to prepare the tech stack: HTTP, TLS, WebFinger, ActivityPub, Webmention, Ed25519, HPKE, sigchains, etc.

## The bill of materials

This design doesn't invent a single technology: everything already exists.

| Problem | Existing technology | Who uses it today |
| --- | --- | --- |
| Transport and status codes | HTTP | The whole web |
| Transport encryption | TLS + Let's Encrypt | The whole web |
| User discovery | WebFinger (RFC 7033) | Mastodon and the Fediverse |
| Message delivery | POST to an inbox | ActivityPub |
| Sender verification at the source | The Webmention and DKIM pattern | IndieWeb, all of email |
| Signatures | Ed25519 | SSH, Signal |
| Content encryption | HPKE (RFC 9180) | MLS, TLS ECH |
| Identity that survives key rotation | Sigchains | ATProto (Bluesky), Keybase |
| Reading and synchronization | JMAP (RFC 8620) | Fastmail |
| Push notifications | SSE / WebPush | Every browser |
| First-contact consent | Message requests | Signal, Instagram |
| Attachments by reference with hashes | Content addressing | Git, IPFS, Matrix |

Every piece we are going to use is standardized, deployed and battle-tested at scale by millions of people; the only new thing is the assembly.

Choosing HTTP is not just pragmatism. It solves, right out of the gate, several things any new protocol would have to build at some point:

- **TLS, virtual hosting and SNI**: for free.
- **Status codes**: the catalog a mail protocol needs already exists in HTTP. `202 Accepted` (queued for delivery), `429 Too Many Requests` + `Retry-After` (rate control), `404` / `410` (mailbox unknown/gone), `3xx` (mailbox moved), `413` (too large). Even paid anti-spam has had a code reserved since 1997: `402 Payment Required`.
- **Existing infrastructure**: proxies, load balancers, Nginx, libraries in every language. The protocol stops being a new server and becomes a *convention over HTTP*, like Webmention or Micropub.

And there is a historical irony that closes the circle: HTTP headers descend directly from email headers (RFC 822). Moving mail to HTTP is not a kidnapping, it's a homecoming.

Let's move to the next level: how do we discover a user, verify their identity, deliver the message, sign it and encrypt it, all over HTTP?

## 1\. Discovery and delegation (a better MX)

Delegation is solved with a static document:

```
GET https://example.com/.well-known/hmtp/ana
```

```json
{
  "inbox": "https://mail.migadu.example/hmtp/inbox/ana",
  "keys": { ... },
  "devices": [ ... ]
}
```

The `inbox` can live on another host: that **is** the MX record, but without touching DNS. A static blog on GitHub Pages can delegate its mail to a provider by serving a JSON file. And it allows something MX never did: *per-user* delegation (each mailbox of a domain on a different provider). We wouldn't even need to invent the route: WebFinger (RFC 7033) does exactly this, and Mastodon has already proven it scales.

Does taking discovery out of DNS sound like heresy? Today's email already took that step: MTA-STS (RFC 8461) publishes its policy at `https://mta-sts.domain/.well-known/mta-sts.txt`, precisely because deploying TLS with Let's Encrypt turned out to be easier than deploying DNSSEC. The real price of this decision is a different one: the domain has to serve HTTPS. It's a coupling MX doesn't have, and we accept it in exchange for per-user delegation.

## 2\. Identity that survives key rotation

Identity cannot *be* a key (they get lost, they expire); it has to be something that keys *back*. A proposal with two anchors:

- **Cryptographic continuity**: the discovery document publishes the current key and a chain of rotations, where each new key is signed by the previous one. Anyone who knew you with key N can verify key N+1 without trusting anybody. It's a sigchain, what ATProto does with DIDs or what Keybase used to do.
- **Domain control as a fallback**: if you lose the key without a signed rotation (your laptop gets stolen), the domain declares a new key without a chain, with a mandatory announcement period (say 30 days) during which the servers that knew you show the warning "identity re-anchored by domain, not by signature".

However, domain-anchored identity has its own Achilles heel: a domain is not owned, it is rented. If you stop paying, it expires and someone registers it, the new owner publishes their keys in your `.well-known` and from that moment on they receive your mail and sign as you. Nobody can tell the legitimate heir from the squatter. It's the problem ATProto tries to solve by separating identity from the domain with DIDs, at the price of another piece of infrastructure. We know it's a real problem with a known solution. Now let's move on.

## 3\. Delivery with a queue (store-and-forward)

Delivery is a POST to the recipient's inbox:

```
POST /hmtp/inbox/ana HTTP/1.1
Host: mail.migadu.example
Content-Type: application/hmtp+json
```

The key is not the request, it's *who* makes it. Your client doesn't deliver directly to the recipient, but to **your own server** (an authenticated POST to your outbox), and it is your server that queues, retries with exponential backoff and honors `Retry-After`. It's admitting that SMTP's MUA/MSA/MTA separation was right. One of email's quiet strokes of genius is that if the destination server is down, your server retries for days and you forget about it.

But let's add an improvement SMTP never had. Every message carries an ID that is the hash of its content, so retries are idempotent. The receiving server deduplicates by ID and the classic "duplicate email because the ACK failed" disappears by construction.

The full cycle of a delivery, with the destination node down on the first attempt:

```
Bob's serverAna's serverAna's clientBob's serverAna's serverAna's client#mermaid-1785729302436{font-family:"trebuchet ms",verdana,arial,sans-serif;font-size:16px;fill:#333;}@keyframes edge-animation-frame{from{stroke-dashoffset:0;}}@keyframes dash{to{stroke-dashoffset:0;}}#mermaid-1785729302436 .edge-animation-slow{stroke-dasharray:9,5!important;stroke-dashoffset:900;animation:dash 50s linear infinite;stroke-linecap:round;}#mermaid-1785729302436 .edge-animation-fast{stroke-dasharray:9,5!important;stroke-dashoffset:900;animation:dash 20s linear infinite;stroke-linecap:round;}#mermaid-1785729302436 .error-icon{fill:#552222;}#mermaid-1785729302436 .error-text{fill:#552222;stroke:#552222;}#mermaid-1785729302436 .edge-thickness-normal{stroke-width:1px;}#mermaid-1785729302436 .edge-thickness-thick{stroke-width:3.5px;}#mermaid-1785729302436 .edge-pattern-solid{stroke-dasharray:0;}#mermaid-1785729302436 .edge-thickness-invisible{stroke-width:0;fill:none;}#mermaid-1785729302436 .edge-pattern-dashed{stroke-dasharray:3;}#mermaid-1785729302436 .edge-pattern-dotted{stroke-dasharray:2;}#mermaid-1785729302436 .marker{fill:#333333;stroke:#333333;}#mermaid-1785729302436 .marker.cross{stroke:#333333;}#mermaid-1785729302436 svg{font-family:"trebuchet ms",verdana,arial,sans-serif;font-size:16px;}#mermaid-1785729302436 p{margin:0;}#mermaid-1785729302436 .actor{stroke:#9370DB;fill:#ECECFF;stroke-width:1;}#mermaid-1785729302436 rect.actor.outer-path[data-look="neo"]{filter:drop-shadow(1px 2px 2px rgba(185, 185, 185, 1));}#mermaid-1785729302436 rect.note[data-look="neo"]{stroke:#aaaa33;fill:#fff5ad;filter:drop-shadow(1px 2px 2px rgba(185, 185, 185, 1));}#mermaid-1785729302436 text.actor>tspan{fill:black;stroke:none;}#mermaid-1785729302436 .actor-line{stroke:#9370DB;}#mermaid-1785729302436 .innerArc{stroke-width:1.5;stroke-dasharray:none;}#mermaid-1785729302436 .messageLine0{stroke-width:1.5;stroke-dasharray:none;stroke:#333;}#mermaid-1785729302436 .messageLine1{stroke-width:1.5;stroke-dasharray:2,2;stroke:#333;}#mermaid-1785729302436 [id$="-arrowhead"] path{fill:#333;stroke:#333;}#mermaid-1785729302436 .sequenceNumber{fill:white;}#mermaid-1785729302436 [id$="-sequencenumber"]{fill:#333;}#mermaid-1785729302436 [id$="-crosshead"] path{fill:#333;stroke:#333;}#mermaid-1785729302436 .messageText{fill:#333;stroke:none;}#mermaid-1785729302436 .labelBox{stroke:#9370DB;fill:#ECECFF;filter:none;}#mermaid-1785729302436 .labelText,#mermaid-1785729302436 .labelText>tspan{fill:black;stroke:none;}#mermaid-1785729302436 .loopText,#mermaid-1785729302436 .loopText>tspan{fill:black;stroke:none;}#mermaid-1785729302436 .sectionTitle,#mermaid-1785729302436 .sectionTitle>tspan{fill:black;stroke:none;}#mermaid-1785729302436 .loopLine{stroke-width:2px;stroke-dasharray:2,2;stroke:#9370DB;fill:#9370DB;}#mermaid-1785729302436 .note{stroke:#aaaa33;fill:#fff5ad;}#mermaid-1785729302436 .noteText,#mermaid-1785729302436 .noteText>tspan{fill:black;stroke:none;font-weight:normal;}#mermaid-1785729302436 .activation0{fill:#f4f4f4;stroke:#666;}#mermaid-1785729302436 .activation1{fill:#f4f4f4;stroke:#666;}#mermaid-1785729302436 .activation2{fill:#f4f4f4;stroke:#666;}#mermaid-1785729302436 .actorPopupMenu{position:absolute;}#mermaid-1785729302436 .actorPopupMenuPanel{position:absolute;fill:#ECECFF;box-shadow:0px 8px 16px 0px rgba(0,0,0,0.2);filter:drop-shadow(3px 5px 2px rgb(0 0 0 / 0.4));}#mermaid-1785729302436 .actor-man circle,#mermaid-1785729302436 line{fill:#ECECFF;stroke-width:2px;}#mermaid-1785729302436 g rect.rect{filter:drop-shadow(1px 2px 2px rgba(185, 185, 185, 1));stroke:#9370DB;}#mermaid-1785729302436 .node .neo-node{stroke:#9370DB;}#mermaid-1785729302436 [data-look="neo"].node rect,#mermaid-1785729302436 [data-look="neo"].cluster rect,#mermaid-1785729302436 [data-look="neo"].node polygon{stroke:#9370DB;filter:drop-shadow(1px 2px 2px rgba(185, 185, 185, 1));}#mermaid-1785729302436 [data-look="neo"].swimlane.cluster rect{filter:none;}#mermaid-1785729302436 [data-look="neo"].node path{stroke:#9370DB;stroke-width:1px;}#mermaid-1785729302436 [data-look="neo"].node .outer-path{filter:drop-shadow(1px 2px 2px rgba(185, 185, 185, 1));}#mermaid-1785729302436 [data-look="neo"].node .neo-line path{stroke:#9370DB;filter:none;}#mermaid-1785729302436 [data-look="neo"].node circle{stroke:#9370DB;filter:drop-shadow(1px 2px 2px rgba(185, 185, 185, 1));}#mermaid-1785729302436 [data-look="neo"].node circle .state-start{fill:#000000;}#mermaid-1785729302436 [data-look="neo"].icon-shape .icon{fill:#9370DB;filter:drop-shadow(1px 2px 2px rgba(185, 185, 185, 1));}#mermaid-1785729302436 [data-look="neo"].icon-shape .icon-neo path{stroke:#9370DB;filter:drop-shadow(1px 2px 2px rgba(185, 185, 185, 1));}#mermaid-1785729302436 :root{--mermaid-font-family:"trebuchet ms",verdana,arial,sans-serif;}down: no responsequeue: retries with exponential backoffsignature verified, deduplicated by idPOST /outbox (signed message)1202 queued2GET /.well-known/hmtp/bob3Bob's inbox and keys4POST /hmtp/inbox/bob (envelope + sealed body)5POST /hmtp/inbox/bob (retry, same id)6GET /.well-known/hmtp/ana7Ana's signing key8201 delivered9
```

Notice that the diagram contains the entire protocol: the two GETs to `.well-known` are discovery and verification, the POST is the delivery, and the queue lives where it should, on the sender's server.

## 4\. Signing and encrypting with the keys we already have

The message is a signed object, not loose text:

```json
{
  "id": "sha256:9f2c...",
  "from": "ana@example.com",
  "to": ["bruno@example.org"],
  "date": "2026-07-26T10:00:00Z",
  "in_reply_to": "sha256:11ab...",
  "sealed": "<subject and body encrypted with HPKE>",
  "signature": "..."
}
```

This buys us a lot of things:

- **Authenticity at rest**: stored mail carries its cryptographic proof. A forward keeps the original signature. Forging the sender becomes impossible even through chains of forwards.
- **Sender verification without DKIM**: the receiving server GETs the `.well-known` of the `from` domain and checks that the key signs. It's the same move as Webmention verification. The proof is fetched at the source.
- **E2E**: the discovery document publishes encryption keys (X25519); the subject and the body are sealed together with HPKE. The envelope (from, to, id, date) stays visible for routing and filtering; everything else, only for the recipient. PGP dragged the mistake of leaving the subject in the clear for decades, and that metadata gave more than one person away. We don't repeat it here.
- **Threads**: `in_reply_to` and `references` by content hash. Conversations reconstructed without heuristics.
- **Attachments**: outside the message. The file is encrypted with a single-use key and the sender's server stores an opaque blob; the `{hash, url, size, key}` reference travels inside the sealed body, so E2E covers attachments too (it's the Matrix pattern). The hash is of the encrypted blob: any server can verify integrity without being able to read anything. The recipient's server mirrors it at delivery, not at read time: nobody learns when you open an attachment, and the sender can delete the original once the mirrors confirm. No more base64 bloating mailboxes, and the costs flip along the way: whoever sends, hosts. With a deliberate side effect: with no attachments inside, a message's JSON is always small, parsing it whole in memory stops being a risk, and the server can enforce an aggressive cap with a `413`.

A detail that matters: the `id` is the hash of the plaintext message, computed before encrypting, and the signature covers that same content. That way every participant in a thread shares the same identifiers even though each copy travels sealed with a different key. The server deduplicates by comparing ids without reading anything; the recipient verifies hash and signature after decrypting.

Signing the whole message has a price DKIM knows well: nobody can touch it in transit. And mailing lists live off exactly that, adding headers (`List-Id`, the unsubscribe link) to the message they distribute. It's no coincidence that DKIM lets you choose which headers to sign. In HMTP a list wouldn't modify your message: it would publish a new one, signed by the list, referencing the original by its hash. The reader would see who wrote and who distributed, each with their own signature. Another real problem with a known solution. Let's move on.

## 5\. Layered anti-spam

Like in any system, this is a complex problem that needs several layers of defense. With HMTP we could start with three:

1. **Identity cost**: signing as `ana@example.com` requires serving the key document at `example.com`. Identity is anchored to a domain, and domains cost money. It's the sybil cost that self-signed identities don't have. However, a domain gives you infinite subdomains and mailboxes, so the cost slows down the mass creation of *independent* identities, not of addresses. We work at the root level.
2. **First-contact consent**: an unknown sender doesn't get into the mailbox; they land in a "requests" box with their first message visible (like Signal's message requests). You accept, and the thread opens forever. A stranger can *knock on your door*, which is the essential property of mail, but they can't fill up your living room.
3. **Optional postage for strangers**: the server can answer a first contact with a `402`. Configurable per mailbox; the cost of cold spamming stops being zero.

Let's be honest about layer 2, because it carries thirty years of history against it. Challenge-response systems were tried in the nineties and failed in two ways. One: the requests box can end up being the new spam folder, the problem just moves to another room. And two, the lethal one: mail written by no human (your purchase confirmation, the invoice, the bank, everything coming from a `no-reply@`) never passes a challenge, and that mail is most of the real volume.

That's why consent here is passive: nobody solves captchas or follows links, the first message arrives whole, waits in plain sight, and you decide with one tap. If you initiated the contact, the thread was born open. And for the machine mail you asked for yourself (signing up on a site that will send you invoices), acceptance is once and forever. Does it eliminate spam? No. No consent system eliminates it, it reorders it. The bet is that a requests box with the message in plain sight is a better deal than a statistical filter deciding blindly what reaches your mailbox.

## 6\. Reading gets specified too

Email standardized sending (SMTP) and reading (IMAP/POP) as separate worlds. We don't need to invent anything: reading, synchronizing and searching mailboxes over JSON/HTTP is already solved and standardized by the IETF. It's called JMAP. HMTP would define delivery; reading is JMAP with one new object type. Push to the client with SSE or WebPush. The full cycle (send, deliver, read, sync) stays on HTTP.

## The conclusion

Every problem of email has a solution deployed and working: discovery in Mastodon, delivery in ActivityPub, verification in the IndieWeb, rotatable identity in Bluesky, reading in Fastmail, consent in Signal. An upgrade to email already exists, but nobody has assembled it.

We solve many problems with elegant, modern solutions:

- **Reputation and social permission** (SPF, DKIM, DMARC, reverse PTR, blocklists, months of warming up an IP): replaced by cryptographic verification on every message, one signature and one GET to the sender's `.well-known`. A property that can be verified doesn't need reputation.
- **Sender spoofing**: impossible by construction. The receiver checks the signature against the key published on the `from` domain, and the signature travels with the message even when forwarded.
- **Mail delegation** (the MX record): a static document in `.well-known`, with per-user delegation and no DNS changes.
- **Content privacy** (the PGP nobody ever got around to configuring): end-to-end encryption by default; the receiving server stores a body, and a subject, it cannot read.
- **Spam** (statistical filters after the fact): first-contact consent (strangers knock on the door, they don't walk into the mailbox), costly identity anchored to a domain, and optional postage with `402`.
- **Duplicates on retry**: the message id is the hash of its content, so retries are idempotent and the receiver deduplicates by construction.
- **Threads rebuilt with heuristics**: `in_reply_to` points to the hash of the parent message; the conversation is a verifiable graph.
- **Light mailboxes**: encrypted attachments by reference, mirrored at delivery. Whoever sends, hosts: spam with heavy attachments costs the spammer disk space, not you.
- **Simple, familiar infrastructure**: everything travels over standard HTTPS, behind the same Nginx and the same certificate that already serve your website.
- **Key rotation** (DKIM's operational nightmare): one command. Since nobody pins your key, the new one is trusted instantly and the stolen one becomes useless just as fast.

Talk is cheap, so I implemented a working prototype in Python: [github.com/tanrax/hmtp](https://github.com/tanrax/hmtp). All in a single file. The prototype covers the full transport: signed delivery, verification at the source, end-to-end encryption, first-contact consent, deduplication, threads, a queue with exponential backoff and key rotation in one command. It deliberately leaves out the pieces at the periphery: the chain of signed rotations (receivers fetch your key live on every delivery instead of pinning it, so the chain only starts paying off once nodes cache keys), the `402` postage, attachments by reference and JMAP reading. The README has the quickstart (your first message in two minutes, mailing yourself), a demo of two nodes exchanging encrypted mail on your machine, and the full production guide, from DNS to systemd. Remember it's a design experiment with unaudited cryptography; don't use it for secrets anyone depends on. Although it could be a lightweight communication system for whatever ecosystem comes to your mind.

I hope you enjoyed the ride. And if you ever send your first HMTP message, I'd love to hear about it.

*Update (July 29, 2026): I revised the article after the [Hacker News discussion](https://news.ycombinator.com/item?id=49066639). The subject now travels encrypted along with the body, attachments are covered by E2E and mirrored at delivery, the article clarifies what the message hash covers and how mailing lists would fit, and it acknowledges the limits of first-contact consent.*

- [The bill of materials](#blog-single-content__title--the-bill-of-materials)
- [1\. Discovery and delegation (a better MX)](#blog-single-content__title--1-discovery-and-delegation-a-better-mx)
- [2\. Identity that survives key rotation](#blog-single-content__title--2-identity-that-survives-key-rotation)
- [3\. Delivery with a queue (store-and-forward)](#blog-single-content__title--3-delivery-with-a-queue-store-and-forward)
- [4\. Signing and encrypting with the keys we already have](#blog-single-content__title--4-signing-and-encrypting-with-the-keys-we-already-have)
- [5\. Layered anti-spam](#blog-single-content__title--5-layered-anti-spam)
- [6\. Reading gets specified too](#blog-single-content__title--6-reading-gets-specified-too)
- [The conclusion](#blog-single-content__title--the-conclusion)

This work is under a [Attribution-NonCommercial-NoDerivatives 4.0 International license.](https://creativecommons.org/licenses/by-nc-nd/4.0/)

2 mentions from other sites