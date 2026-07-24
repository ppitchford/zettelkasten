---
title: "Company as Code"
source: "https://www.instapaper.com/read/1997503180"
author:
  - "[[Daniel Rothmann]]"
published: 8 min
created: 2026-07-24
---
[blog.42futures.com](https://blog.42futures.com/p/company-as-code) ·

Last week, as I sat across from our [ISO 27001](https://www.iso.org/standard/27001) information security auditor, watching them strenuously work through our documentation, a thought struck me:

> Here we are, a software company, with nearly all of our operations running in interconnected digital systems, yet the core of our business—our policies, procedures, and organisational structure—is a *basic* collection of documents.

It just felt ironic. We use advanced tools to automate compliance checks, store our code in version-controlled repositories, and manage our infrastructure as code. However, when describing and managing our company, we resort to digital paper and tidbits of info distributed across people in the building.

The disconnect became increasingly apparent as I reflected on our day-to-day process: 90% of our products, documents, communication, and decision-making live in digital channels. That’s data. It lives in the cloud, spread over SaaS solutions that specialise in handling individual work processes—all systems with robust APIs and programmatic access.

At the centre of it all sits a lonely island of documents: our ambitions, goals, policies and formal structures. And I think those are pretty important.

Our security posture was solid before we even considered ISO 27001 because we’d already worked hard to comply with our customer’s requirements. Between collecting evidence for controls, arguing about and updating policy wording, document review, and the actual audit, ***we spent hundreds of additional person-hours*** that could’ve otherwise been spent creating great products for our users.

![](https://substackcdn.com/image/fetch/$s_!P-Is!%2Cw_848%2Cc_limit%2Cf_auto%2Cq_auto:good%2Cfl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F9cddc69a-a86d-4a18-82c9-4ffa6a4eeb98_1232x928.jpeg)

## A missing link

If we desire operational data to be so rich, why do we accept organisational data to be so sparse? We’ve revolutionised how we handle infrastructure with Infrastructure as Code (IaC), how we manage deployments with GitOps, and how we handle security with Policy as Code.

We see the benefit.

But when representing our organisation (the beating heart of our operations), we apply old-school methods.

Imagine if we could represent our entire organisational structure programmatically instead—not a static picture, but a living, breathing digital representation of our company that can be versioned, queried, tested, and automatically verified. A system where policy changes could be tracked as code changes, where compliance could be continuously monitored, and where the relationships between people, processes and technology could be explicitly mapped and understood.

![](https://substackcdn.com/image/fetch/$s_!vFMg!%2Cw_848%2Cc_limit%2Cf_auto%2Cq_auto:good%2Cfl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa47e3728-e301-4e34-936e-42155b113034_1232x928.jpeg)

## Thinking bigger

Existing solutions like HRIS systems manage people data but struggle with policy relationships. GRC tools track compliance but rarely connect meaningfully to organisational structure. I’m proposing we think bigger. It’s about creating a holistic, programmatic representation of the *entire* organisation: a “company manifest” that serves as a single source of truth for organisational structure, policy, and operations.

Consider how helpful this might be for:

- **Compliance audits:** Instead of manually piecing together evidence from various systems, auditors could query the company manifest directly, with clear traceability between policies and their implementations.
- **Policy changes:** Updates could be version-controlled, and automated impact analysis could show which teams and processes would be affected.
- **Organisational design:** Leaders could model structural changes in a “staging environment” before implementing them, gaining a better understanding of how changes cause ripple effects throughout the company.

## Why isn’t this a thing?

As this idea swirls around in my mind, I find more questions than answers.

Has anyone tried this yet? If not, why not?

**Is it because organisations are inherently too complex and dynamic to be represented as code?** If so, that seems at odds with regulation and standards, where we expect corporate activities to be so uniform and procedural that we can reliably stamp them as compliant, non-compliant, legal, or illegal.

**Is it because we haven’t yet found the right abstraction level—the equivalent of what Infrastructure as Code did for system administration?**

The tools and concepts exist: Graph databases for representing organisational relationships, domain-specific languages for describing business rules, and API-first architectures for integration.

**Maybe what’s missing is a framework to bring these ideas together in a way that’s powerful enough to be useful and simple enough to be used.**

![](https://substackcdn.com/image/fetch/$s_!nrqf!%2Cw_848%2Cc_limit%2Cf_auto%2Cq_auto:good%2Cfl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff5b87009-60f1-4725-8645-04d63920976b_1232x928.jpeg)

## Putting it into practice

Let’s think about how this could work.

In this section, I’ll fantasise about what I would want from “Company as Code.” Then, I’ll map that to some system components that could address those needs.

As an engineer and business stakeholder, I would want a company model to be:

#### Queryable

The system must trace relationships between people, policies, and systems—similar to code dependency tracking. Users should easily see the organisation from different angles, such as which people are affected by a policy and who owns it.

#### Versionable

Explicit tracking of organisational changes, including who made them, what changed, and why. This is crucial for audits and understanding organisational evolution.

#### Integrated

Seamless data exchange with existing tools (Azure, Slack, etc.) to maintain an up-to-date organisational picture and enforce tool configurations based on policy.

#### Testable

A “staging environment” where organisational changes can be modeled before implementation, supporting automated tests for individual rules and controls.

#### Accessible

Though powered by code, the interface should be intuitive enough for non-technical leaders to use effectively.

Bringing this vision to life requires several puzzle pieces. Each component must address specific requirements to approximate something that can be powerful while being relatively simple to use.

![](https://substackcdn.com/image/fetch/$s_!pkbW!%2Cw_848%2Cc_limit%2Cf_auto%2Cq_auto:good%2Cfl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F57dbeeb3-a5af-4113-ad9c-e5ec70c80519_1232x928.jpeg)

### A declarative language for organisations

Drawing inspiration from Infrastructure as Code tools like Terraform, let’s imagine a declarative Domain Specific Language (DSL) that reads naturally while expressing a formal structure.

The basic syntax follows a clear pattern:

```js
EntityType "Identifier" {
    References = AnotherEntity.Identifier
    Attribute = Value
    ListAttribute = [
        "Item One",
        "Item Two"
    ]
}
```

A type, unique identifier, and set of attributes define each entity. Entities can reference each other using dot notation, creating a web of relationships that forms our organisational graph.

#### Specifying an organisation

Let's walk through how you could define a small engineering team in this language:

1. First, let’s define the roles that exist in the organisation:
```js
Role "SoftwareEngineer" {
    Responsibilities = [
        "Write clean, maintainable code",
        "Participate in code reviews",
        "Document technical decisions"
    ]
}

Role "EngineeringManager" {
    Responsibilities = [
        "Provide technical leadership",
        "Conduct performance reviews",
        "Manage team resources"
    ]
}
```
1. Next, we’ll create an organisational unit for our team:
```js
OrganisationalUnit "EngineeringTeam" {
    Department = "Engineering"
    CostCenter = "ENG-001"
}
```
1. With these structures in place, we can define the actual people and their relationships:
```js
Person "AliceSmith" {
    FullName = "Alice Smith"
    Role = Role.EngineeringManager
    Unit = OrganisationalUnit.EngineeringTeam
    Email = "alice.smith@company.com"
}

Person "BobJohnson" {
    FullName = "Bob Johnson"
    Role = Role.SoftwareEngineer
    Unit = OrganisationalUnit.EngineeringTeam
    Manager = Person.AliceSmith
    Email = "bob.johnson@company.com"
}
```
1. Policy definitions create a framework for compliance:
```js
PolicyGroup "SecurityPolicies" {
    Owner = Person.AliceSmith
}

PolicyRule "MFARequired" {
    Group = PolicyGroup.SecurityPolicies
    Enforcement = "Mandatory"
    ComplianceLevel = "Critical"
}
```
1. Finally, we can map these policies to external requirements:
```js
ExternalRequirement "ISO27001_A9_4_1" {
    Standard = "ISO 27001:2013"
    Control = "A.9.4.1"
    ComplianceLevel = "Mandatory"
}

ComplianceMapping "MFACompliance" {
    Requirement = ExternalRequirement.ISO27001_A9_4_1
    ImplementingPolicies = [PolicyRule.MFARequired]
}
```

This declarative approach allows us to build a complete picture of our organisation, from high-level structures to individual policies and their regulatory implications. Each definition is clear and self-documenting, while the references between entities create a graph of relationships that can be analysed, validated, and used to automate organisational processes.

![](https://substackcdn.com/image/fetch/$s_!FnF0!%2Cw_848%2Cc_limit%2Cf_auto%2Cq_auto:good%2Cfl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F0bfb9e3a-a857-4388-a2c5-11c5a7c5d1f3_2126x927.png)

The entities declared in the DSL form a graph.

With a representation like this, we benefit from organising definitions into logical files and directories, treating organisational changes like code changes: versioned, reviewed, and validated before application.

This enables practices like reviewing changes through pull requests, testing policy modifications before rollout, and automatically generating compliance documentation while tracking the evolution of organisational structures over time.

![](https://substackcdn.com/image/fetch/$s_!Hdjr!%2Cw_848%2Cc_limit%2Cf_auto%2Cq_auto:good%2Cfl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F9f69136a-14c5-4aa0-bbaa-1c0a90819230_1232x928.jpeg)

### Building a model

While Infrastructure as Code tools like Terraform work with directed acyclic graphs (DAGs) to determine deployment order, an organisation's structure is inherently more interconnected. People manage other people, policies reference activities that refer back to policies, and teams have complex interdependencies. This calls for an undirected cyclic graph model to represent these rich relationships.

```js
public record Node(
    string Id,          // e.g. "Person.AliceSmith"
    string Type,        // e.g. "Person", "PolicyRule"
    List<Edge> Relations = null
);

public record Edge(
    string FromId, 
    string ToId,
    string RelationType, // e.g. "ManagedBy"
);

public class CompanyGraph
{
    private Dictionary<string, Node> _nodes = new();
    private List<Edge> _edges = new();

    public void AddNode(string id, string type) =>
        _nodes[id] = new Node(id, type);

    public void AddRelation(string fromId, string toId, string type)
    {
        var edge = new Edge(fromId, toId, type);
        _edges.Add(edge);
        (_nodes[fromId].Relations ??= new()).Add(edge);
        (_nodes[toId].Relations ??= new()).Add(edge);
    }

    // Example: Find all requirements impacted by changing a policy
    public IEnumerable<Node> GetImpactedRequirements(string policyId) =>
        _nodes[policyId].Relations
            .Where(e => e.RelationType == "ImplementsRequirement")
            .Select(e => _nodes[e.FromId == policyId ? e.ToId : e.FromId])
            .Where(req => req.Type == "ExternalRequirement");
}
```

The example above shows a naive implementation of a queryable graph structure based on a declarative DSL. This representation makes it easier to answer questions requiring multiple references in the structure, such as "Which external requirements would be affected if we changed our MFA policy?"

![](https://substackcdn.com/image/fetch/$s_!XLJb!%2Cw_848%2Cc_limit%2Cf_auto%2Cq_auto:good%2Cfl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4811476e-62d6-4158-9343-b72f542fcc87_1232x928.jpeg)

### Bridging models and reality

A DSL can define an organisation's structure and rules, but the model must be connected to real-world data to be useful. This requires a storage strategy that can:

- Persist the organisational graph itself
- Store data associated with graph entities (like evidence)
- Enable validation of considered changes

A way to deliver this at scale would be to combine several data stores: A graph database for the organisational model, a relational database for associated data, and an event store for audit trail and change tracking. If you wanted to start small, you could serialise all this information to your local filesystem instead.

To activate external data, we would need an integration framework with a plug-in architecture to bridge the organisational model and production systems. This would handle three key aspects: (1) data collection from integrated systems like GitHub or Azure, mapping evidence to nodes in our graph; (2) policy validation that programmatically checks collected evidence against rules; and (3) policy enforcement where organisational changes automatically trigger updates across connected systems—from employee provisioning to access permission changes.

The DSL would supply this functionality using built-in modules or user-supplied plug-ins to perform the work needed to interact with other systems. Let’s imagine a Control entity in the DSL which uses a custom script to perform routine checks on MFA usage and links results to the compliance mapping:

```js
Control "MFAMonitoring" {
    Implements = ComplianceMapping.MFACompliance
    
    Verify {
        Script = "Security/mfa-checks.js"
        Methods = [
            "allUsersHaveMfaEnabled"
        ]
        Frequency = "Daily"
    }
}
```

Then in `security/mfa-checks.js`:

```js
export async function allUsersHaveMfaEnabled() {
    const users = await myAuthClient.listUsers()
    return users.every(user => user.mfaEnabled)
}
```

The solution should be open and extendable to maximise its usefulness, allowing for custom integrations, validations, and automation. Terraform does a good job of this with its “ [Providers](https://developer.hashicorp.com/terraform/language/providers) ” plug-in architecture, although integrations to inspect and modify data in corporate systems might need to support more custom scripting.

For organisations wanting to test this approach, you could start small:

1. Model just your organisational structure and reporting lines.
2. Add key policies and compliance mappings.
3. Begin connecting to your most critical systems.

![](https://substackcdn.com/image/fetch/$s_!Vw8K!%2Cw_848%2Cc_limit%2Cf_auto%2Cq_auto:good%2Cfl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F90ad1389-c5e3-45e2-96ec-3938cf5e1cdd_1232x928.jpeg)

### Making it shine

A code-based declaration of an organisation offers powerful capabilities for technologists, but we also need to acknowledge that many business stakeholders don’t “think in code”.

The elegance of a DSL doesn’t have to be limited to those most comfortable with text editors and version control software. A genuinely accessible “Company as Code” solution must connect a programmatic representation and the business users who make daily decisions about organisation structure, policy and compliance.

This could be solved with a low-code / no-code interface where:

- **Business leaders** drag and drop organisational entities and reporting structures while the system generates code declarations behind the scenes.
- **Compliance officers** use simple forms to define policies and instantly visualise which parts of the business they affect. When regulations change, they can quickly identify gaps or conflicts through visual highlighting.
- **Technologists** keep the codebase organised and provide data integrations and tools to implement policy in external systems.

With an approach like this, we can maintain the rigour of the code model while making it accessible to everyone. Changes through the interface update the underlying code, preserving that single source of truth that can be versioned and validated.

![](https://substackcdn.com/image/fetch/$s_!Hb3s!%2Cw_848%2Cc_limit%2Cf_auto%2Cq_auto:good%2Cfl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff1848a14-156f-41ce-91e6-2561a0c9f985_1232x928.jpeg)

### Final notes

Beyond the technical feasibility, the real promise lies in the organisational benefits: faster audits, clearer decision-making, and better understanding of changes' impact before implementing them. **Hundreds of hours spent on compliance documentation could instead be invested in creating value.**

In this post, I hoped to establish that a codification of organisational structure is ***missing*** and that it’s ***buildable***. Practical? Don’t know. Viable? I don’t have the answer. ***Buildable, though?*** Yes. I believe so.

Let me know what you think about it.

Daniel Rothmann runs [42futures](https://42futures.com/), where he helps technical leaders validate high-stakes technical decisions through structured software pilots.