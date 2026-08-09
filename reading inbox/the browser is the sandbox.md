---
title: "the browser is the sandbox"
source: "https://simonwillison.net/2026/Jan/25/the-browser-is-the-sandbox/"
author:
  - "Simon Willison"
published:
created: 2026-08-07
---
**[the browser is the sandbox](https://aifoc.us/the-browser-is-the-sandbox/)**. Paul Kinlan is a web platform developer advocate at Google and recently turned his attention to coding agents. He quickly identified the importance of a robust sandbox for agents to operate in and put together these detailed notes on how the web browser can help:

> This got me thinking about the browser. Over the last 30 years, we have built a sandbox specifically designed to run incredibly hostile, untrusted code from anywhere on the web, the instant a user taps a URL. \[...\]
> 
> Could you build something like Cowork in the browser? Maybe. To find out, I built a demo called [Co-do](http://co-do.xyz/) that tests this hypothesis. In this post I want to discuss the research I've done to see how far we can get, and determine if the browser's ability to run untrusted code is useful (and good enough) for enabling software to do more for us directly on our computer.

Paul then describes how the three key aspects of a sandbox - filesystem, network access and safe code execution - can be handled by browser technologies: the [File System Access API](https://developer.chrome.com/docs/capabilities/web-apis/file-system-access) (still Chrome-only as far as I can tell), CSP headers with `<iframe sandbox>` and WebAssembly in Web Workers.

Co-do is a very interesting demo that illustrates all of these ideas in a single application:

You select a folder full of files and configure an LLM provider and set an API key, Co-do then uses CSP-approved API calls to interact with that provider and provides a chat interface with tools for interacting with those files. It does indeed feel similar to [Claude Cowork](https://simonwillison.net/2026/Jan/12/claude-cowork/) but without running a multi-GB local container to provide the sandbox.

My biggest complaint about `<iframe sandbox>` remains how thinly documented it is, especially across different browsers. Paul's post has all sorts of useful details on that which I've not encountered elsewhere, including a complex [double-iframe technique](https://aifoc.us/the-browser-is-the-sandbox/#the-double-iframe-technique) to help apply network rules to the inner of the two frames.

Thanks to this post I also learned about the `<input type="file" webkitdirectory>` tag which turns out to work on Firefox, Safari *and* Chrome and allows a browser read-only access to a full directory of files at once. I had Claude knock up a [webkitdirectory demo](https://tools.simonwillison.net/webkitdirectory) to try it out and I'll certainly be using it for projects in the future.

![Screenshot of a dark-themed file explorer application. Top stats bar shows: 12179 TOTAL FILES, 2079 FOLDERS, 244 MB TOTAL SIZE, 97 FILE TYPES in cyan text. Search bar with placeholder "Search files..." and "All types" dropdown. Left panel labeled "File tree" (showing 12179) displays folder hierarchy: datasette > .claude > skills > building-datasette-plugins containing HOOKS.md (10.7 KB, selected/highlighted), INTERNALS.md (10.1 KB), SKILL.md (3.7 KB), TESTING.md (8.4 KB), settings.local.json (280 B); also shows .eggs folder with pytest_runner-6.0.1-py3.9.egg. Right panel "File preview" shows selected file details: Name: HOOKS.md, Path: datasette/.claude/skills/building-datasette-plugins/HOOKS.md, Size: 10.7 KB, Type: text/markdown, Last modified: 12/20/2025, 9:28:59 AM. Preview content shows: "# Plugin Hooks Reference" followed by "All hooks use the @hookimpl decorator. Accept only the parameters you need." then "## Database Connection Hooks" and "### prepare_connection(conn, database, datasette)" with description "Called when a new SQLite connection is created. Use to register custom SQL functions." Bottom section "File type distribution" shows horizontal bar chart: .py (4439), .no ext (3358), .dat (1068), .pyc (925), .txt (332), .mo (321), .po (321), .html (249).](https://static.simonwillison.net/static/2026/webkit-file-tree.jpg)