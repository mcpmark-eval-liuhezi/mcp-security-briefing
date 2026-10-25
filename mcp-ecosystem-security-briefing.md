# MCP Ecosystem Security Briefing

Prepared for the team's MCP tooling standardization decision meeting. All facts below were pulled directly from GitHub (API) on **2026-10-25 (UTC)** during the research run for this briefing.

## 1. The MCP organization and its flagship repository

- **GitHub organization login (exact):** `modelcontextprotocol`
- **Most-starred repository in that organization:** `modelcontextprotocol/servers` ("Model Context Protocol Servers") — 91,549 stars, 11,865 forks, TypeScript, default branch `main`.
- Runner-up for context: `modelcontextprotocol/python-sdk` (24,664 stars), then `modelcontextprotocol/typescript-sdk` (13,651 stars).

### README summary (modelcontextprotocol/servers)

The repository is a collection of *reference implementations* for the Model Context Protocol, plus references to community-built servers and additional resources. The README points out that anyone looking for a list of MCP servers should browse the MCP Registry, and that this repository now houses only the small number of reference servers maintained by the MCP steering group. It carries an explicit warning that the servers are intended as educational examples demonstrating MCP features and SDK usage — **not production-ready solutions** — and that developers must evaluate their own security requirements and implement safeguards based on their specific threat model. The current reference servers are: Everything (reference/test server), Fetch, Filesystem, Git, Memory, Sequential Thinking, and Time; a larger set of older servers (GitHub, GitLab, Google Drive, PostgreSQL, Puppeteer, Redis, Sentry, Slack, SQLite, etc.) has been archived to a separate `servers-archived` repository. TypeScript-based servers can be run directly with `npx` (e.g. `@modelcontextprotocol/server-memory`), and Python-based servers with `uvx` or `pip`, typically configured into an MCP client such as Claude Desktop.

## 2. The three open pull requests updated most recently in modelcontextprotocol/servers

As returned by GitHub during this run (sorted by most recent update, state = open):

| # | PR number | Title (exactly as returned) | Last updated |
|---|-----------|------------------------------|--------------|
| 1 (newest) | **#4551** | feat(everything): serve both protocol eras on SDK v2 (2026-07-28 + legacy) | most recently updated of the three |
| 2 | **#3180** | feat(fetch): add SSRF protection and comprehensive security test suite | 2026-09-25T15:01:54Z |
| 3 | **#4817** | fix(ci): restrict Claude workflow mentions | 2026-09-21T08:29:45Z |

### Newest of the three — PR #4551, in detail

**What it does:** Migrates the `everything` reference server to the MCP TypeScript SDK v2 scoped packages (`@modelcontextprotocol/server`, `/node`, `/core`, `/server-legacy` at 2.0.0) and adds support for protocol revision 2026-07-28 alongside the legacy revisions (2024-10-07 … 2025-11-25) from a single codebase.

**Files it changes (54 files):**

1. `package-lock.json`
2. `src/everything/README.md`
3. `src/everything/__tests__/prompts.test.ts`
4. `src/everything/__tests__/registrations.test.ts`
5. `src/everything/__tests__/resources.test.ts`
6. `src/everything/__tests__/server.test.ts`
7. `src/everything/__tests__/tools.test.ts`
8. `src/everything/docs/architecture.md`
9. `src/everything/docs/extension.md`
10. `src/everything/docs/features.md`
11. `src/everything/docs/how-it-works.md`
12. `src/everything/docs/instructions.md`
13. `src/everything/docs/startup.md`
14. `src/everything/docs/structure.md`
15. `src/everything/package.json`
16. `src/everything/prompts/args.ts`
17. `src/everything/prompts/completions.ts`
18. `src/everything/prompts/index.ts`
19. `src/everything/prompts/resource.ts`
20. `src/everything/prompts/simple.ts`
21. `src/everything/resources/files.ts`
22. `src/everything/resources/index.ts`
23. `src/everything/resources/session.ts`
24. `src/everything/resources/subscriptions.ts`
25. `src/everything/resources/templates.ts`
26. `src/everything/server/index.ts`
27. `src/everything/server/logging.ts`
28. `src/everything/server/notifier.ts` (added)
29. `src/everything/server/request-state.ts` (added)
30. `src/everything/server/roots.ts`
31. `src/everything/tools/echo.ts`
32. `src/everything/tools/get-annotated-message.ts`
33. `src/everything/tools/get-env.ts`
34. `src/everything/tools/get-resource-links.ts`
35. `src/everything/tools/get-resource-reference.ts`
36. `src/everything/tools/get-roots-list.ts`
37. `src/everything/tools/get-structured-content-list.ts` (added)
38. `src/everything/tools/get-structured-content.ts`
39. `src/everything/tools/get-sum.ts`
40. `src/everything/tools/get-tiny-image.ts`
41. `src/everything/tools/gzip-file-as-resource.ts`
42. `src/everything/tools/index.ts`
43. `src/everything/tools/simulate-research-query.ts`
44. `src/everything/tools/toggle-simulated-logging.ts`
45. `src/everything/tools/toggle-subscriber-updates.ts`
46. `src/everything/tools/trigger-elicitation-request-async.ts` (removed)
47. `src/everything/tools/trigger-elicitation-request.ts`
48. `src/everything/tools/trigger-long-running-operation.ts`
49. `src/everything/tools/trigger-sampling-request-async.ts` (removed)
50. `src/everything/tools/trigger-sampling-request.ts`
51. `src/everything/tools/trigger-url-elicitation.ts`
52. `src/everything/transports/sse.ts`
53. `src/everything/transports/stdio.ts`
54. `src/everything/transports/streamableHttp.ts`

**Review status:** The PR has one review, from `copilot-pull-request-reviewer[bot]` (GitHub's Copilot automated PR reviewer), which reviewed 52 of the 54 changed files and generated 3 comments. No human review has been submitted.

**Checks status:** The combined status for head commit `b6fe776e5084f992f2bceadf0dfc7d416cac73b9` is **pending**, with 0 check runs reported (no CI results posted at the time of this run).

## 3. Discussion activity

**GitHub Discussions are disabled (and empty) on `modelcontextprotocol/servers`.** The repository's `has_discussions` flag is `false` and a discussions query returns a total count of 0, so there is no recent discussion or comment activity to summarize.

## 4. Published high/critical npm security advisories (our product runs on Node)

Both advisories below were pulled from GitHub's reviewed global security advisories on **2026-10-25 (UTC)**; advisory data changes over time, so re-verify before the meeting. Version ranges are taken from the full advisory records, not the list entries.

### Advisory 1 — Critical

- **GHSA ID:** GHSA-pqh8-4pj7-32xx
- **Severity:** critical (CVSS 9.6, CVSS:3.1/AV:N/AC:L/PR:N/UI:R/S:C/C:H/I:H/A:H; CVE-2026-77363)
- **Affected package (npm):** `obsidian-tc`
- **Vulnerable version range:** `< 1.3.1` (first patched version: 1.3.1)
- **Summary:** The Streamable HTTP transport was constructed without DNS-rebinding protection, allowed hosts/origins, or Origin-header validation. With the documented `auth.mode: "none"` loopback default, a drive-by web page could POST to `http://127.0.0.1/mcp` and receive full vault read/write scopes via DNS rebinding or a forged Origin. Fixed in 1.3.1; workarounds include requiring JWT auth or placing the transport behind a proxy that validates `Origin`.

### Advisory 2 — High

- **GHSA ID:** GHSA-xjj6-f33q-929c
- **Severity:** high (CVSS 7.5, CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:N/I:N/A:H; CVE-2026-109869)
- **Affected package (npm):** `undici`
- **Vulnerable version ranges (three, per the full advisory record):**
  - `>= 6.23.0, < 6.29.1` (first patched: 6.29.1)
  - `>= 7.18.0, < 7.30.1` (first patched: 7.30.1)
  - `>= 8.0.0, < 8.12.2` (first patched: 8.12.2)
- **Summary:** `undici`'s `fetch()` crashes the Node.js process when a malicious server returns a response with more than five content codings and closes the connection before sending the body. A remote, unauthenticated server can trigger this with a single response even when the application correctly catches the `fetch()` rejection. No workaround; upgrade to a patched version.

## 5. Takeaways for the decision meeting

1. The reference servers in `modelcontextprotocol/servers` are explicitly **not production-ready** — treat them as patterns to copy with our own security review, never as drop-in components.
2. Active upstream security work is visible in the most recently updated PRs: SSRF hardening for the fetch server (#3180) and CI workflow trigger hardening against untrusted actors (#4817). These match the kinds of controls we should require in our own MCP servers.
3. Node runtime dependencies need attention now: `undici` (DoS) has patched releases across the 6.x/7.x/8.x lines, and any MCP-adjacent tooling should be checked against both advisories above.
