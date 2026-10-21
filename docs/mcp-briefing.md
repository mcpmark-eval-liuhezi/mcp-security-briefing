# MCP Security Briefing

Team briefing covering the Model Context Protocol (MCP) and a supply-chain cross-check of recent GitHub security advisories affecting Python (pip) packages. This document is the starting point for our security review as we begin adopting MCP.

## What MCP Is

The Model Context Protocol (MCP) is an open-source standard for connecting AI applications to external systems. Introduced by Anthropic in November 2024, it gives AI applications such as Claude, ChatGPT, and developer tools a consistent, protocol-based way to reach tools, data sources, and services. Developers expose capabilities through **MCP servers**, and AI applications act as **MCP clients** that discover and invoke them.

### Why it matters

- **It solves the "N x M" integration problem.** Without a shared protocol, every AI application had to build a custom connector for every tool or data source. MCP collapses N x M integrations into N + M, so each client and server implements the protocol just once.
- **It is gaining broad industry adoption.** OpenAI, Google DeepMind, and Microsoft have adopted or support the protocol, and MCP was donated to the Linux Foundation's Agentic AI Foundation, signalling that it is being treated as neutral industry infrastructure rather than a proprietary advantage.

## Supply-Chain Angle: Recent Python (pip) Advisories

As we begin adopting MCP, we will be pulling in MCP servers, SDKs, and their transitive dependencies. The following recent GitHub security advisories affecting Python (pip) packages are relevant reminders of the supply-chain risks involved.

| GHSA ID | Package | Severity | Summary |
|---|---|---|---|
| GHSA-58r4-6rv3-gfh8 | Pillow | high | Out-of-bounds access in `_imagingmorph` on a zero-width image |
| GHSA-65qm-jvmh-6pqg | Pillow | high | GIF seek keeps rejected size after DecompressionBombError, allowing DoS |
| GHSA-7hjg-qchp-46pj | trijigo-kalip | critical | Malicious code in trijigo-kalip (PyPI) — exfiltrates basic host information on install/import |
| GHSA-v2ww-gvf4-g796 | jiklpo-tsunari-kay | critical | Malicious code in jiklpo-tsunari-kay (PyPI) — fake http-downloaded dependencies / dependency confusion |
| GHSA-2x8w-jgwq-x69p | hijok-lampam | critical | Malicious code in hijok-lampam (PyPI) — fake http-downloaded dependencies / dependency confusion |
| GHSA-hf2v-wrj5-w7gc | ssemver | critical | Malicious code in ssemver (PyPI) — downloads and executes malware during installation (typosquatting) |

### Why this matters for MCP adoption

- **Typosquatting and dependency confusion are active threats.** Packages like `ssemver` (a typosquat of `semver`) and the `2026-10-vacsdk` campaign packages demonstrate that attackers are actively publishing look-alike Python packages. MCP servers and SDKs installed from PyPI are exposed to the same attack patterns.
- **Dependency hygiene is a first-class control.** Pin dependencies, verify package names and sources, and use a lockfile/dependency review tooling when installing MCP-related packages.
- **Vulnerabilities in widely-used libraries propagate quickly.** The two recent Pillow advisories are a reminder that even mature, heavily-used libraries ship vulnerabilities (out-of-bounds write and a decompression-bomb DoS), and MCP tooling that processes untrusted files inherits those risks.

## Recommended Next Steps

1. Inventory every MCP server and dependency we plan to adopt, with provenance.
2. Enable Dependabot alerts on this repository and pin all MCP-related dependencies.
3. Review the MCP servers' exposed tools and resources before granting access to internal systems.
4. Re-run the advisory sweep before each major dependency change.

---

*Sources: MCP overview from [modelcontextprotocol.io](https://modelcontextprotocol.io/docs/getting-started/intro) and Anthropic's [MCP announcement](https://www.anthropic.com/news/model-context-protocol). Advisory data from the GitHub Advisory Database (pip ecosystem), retrieved 2026-10-21.*
