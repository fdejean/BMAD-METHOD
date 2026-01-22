![BMad Method](banner-bmad-method.png)

[![Version](https://img.shields.io/npm/v/bmad-method?color=blue&label=version)](https://www.npmjs.com/package/bmad-method)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Node.js Version](https://img.shields.io/badge/node-%3E%3D20.0.0-brightgreen)](https://nodejs.org)
[![Discord](https://img.shields.io/badge/Discord-Join%20Community-7289da?logo=discord&logoColor=white)](https://discord.gg/gk8jAdXWmj)

**Build More, Architect Dreams** — An AI-driven agile development framework with 21 specialized agents, 50+ guided workflows, and scale-adaptive intelligence that adjusts from bug fixes to enterprise systems.

**100% free and open source.** No paywalls. No gated content. No gated Discord. We believe in empowering everyone, not just those who can pay.

## Why BMad?

Traditional AI tools do the thinking for you, producing average results. BMad agents act as expert collaborators who guide you through structured workflows to bring out your best thinking.

- **Scale-Adaptive**: Automatically adjusts planning depth based on project complexity (Level 0-4)
- **Structured Workflows**: Grounded in agile best practices across analysis, planning, architecture, and implementation
- **Specialized Agents**: 12+ domain experts (PM, Architect, Developer, UX, Scrum Master, and more)
- **Complete Lifecycle**: From brainstorming to deployment, with just-in-time documentation

## Quick Start

**Prerequisites**: [Node.js](https://nodejs.org) v20+

```bash
npx bmad-method@alpha install
```

Follow the installer prompts to configure your project. Then run:

```bash
*workflow-init
```

This analyzes your project and recommends a track:

| Track           | Best For                  | Time to First Story |
| --------------- | ------------------------- | ------------------- |
| **Quick Flow**  | Bug fixes, small features | ~5 minutes          |
| **BMad Method** | Products and platforms    | ~15 minutes         |
| **Enterprise**  | Compliance-heavy systems  | ~30 minutes         |

## Modules

| Module                                | Purpose                                                  |
| ------------------------------------- | -------------------------------------------------------- |
| **BMad Method (BMM)**                 | Core agile development with 34 workflows across 4 phases |
| **BMad Builder (BMB)**                | Create custom agents and domain-specific modules         |
| **Creative Intelligence Suite (CIS)** | Innovation, brainstorming, and problem-solving           |

## Using the BMCP Extension (This Fork)

This fork adds a custom module at `custom-modules/bmcp` that:
- Auto-chains `create-story` → implementation plan → GitHub issue creation → offload
- Requires confirmation before creating issues/offloading
- Keeps plans and stories consistent via a Story Snapshot check

### Install in a New Repo

1. Install base BMad:

```bash
npx bmad-method@alpha install
```

2. Copy the custom module into your new repo:

```
<new-repo>/custom-modules/bmcp
```

3. Run the installer and choose **Install Custom Module**, pointing to:

```
<new-repo>/custom-modules/bmcp
```

4. Rebuild agents (required after custom modules/customizations):

```bash
npx bmad-method@alpha install
```

### What Changes in the Workflow

When you run `create-story` (SM agent), it now:
1. Creates the story file
2. Generates a detailed implementation plan
3. Prompts for confirmation
4. Creates GitHub issues and offloads tasks (if approved)

When you run `dev-story`, it now:
1. Implements the story as usual
2. Prompts to sync GitHub issues (story + task issues) after completion

### GitHub Issue Structure (Parent/Child)

The orchestrator now creates hierarchical issues:
- Epic issue → sub-issues for each Story
- Story issue → sub-issues for each Task

It uses GitHub MCP `sub_issue_write` for parent/child linking and also embeds task lists in issue bodies for visibility.

### Known Sync Loopholes (How Issues Can Drift)

- If MCP tools are misconfigured or unavailable, issue creation/sync halts and leaves partial state.
- If `github-issue-map.md` is deleted or edited, `dev-story` cannot sync issues reliably.
- Re-running `mcp-issue-orchestrator` without cleanup can create duplicate issues.
- Manual edits to issue titles or story keys break the story/task mapping.
- If the story changes after the plan is generated, tasks can become stale (the workflow halts on mismatch).
- If `sub_issue_write` fails, parent/child links may be missing even if issues exist.
- If users close or modify issues manually, the sync step may not reflect intent.

Docs:
- [Install Custom Modules](https://docs.bmad-method.org/how-to/installation/install-custom-modules/)
- [Agent Customization Guide](https://docs.bmad-method.org/how-to/customization/customize-agents/)

## Documentation

**[Full Documentation](http://docs.bmad-method.org)** — Tutorials, how-to guides, concepts, and reference

- [Getting Started Tutorial](http://docs.bmad-method.org/tutorials/getting-started/getting-started-bmadv6/)
- [Upgrading from Previous Versions](http://docs.bmad-method.org/how-to/installation/upgrade-to-v6/)

### For v4 Users

- **[v4 Documentation](https://github.com/bmad-code-org/BMAD-METHOD/tree/V4/docs)**

## Community

- [Discord](https://discord.gg/gk8jAdXWmj) — Get help, share ideas, collaborate
- [YouTube](https://www.youtube.com/@BMadCode) — Tutorials, master class, and podcast (launching Feb 2025)
- [GitHub Issues](https://github.com/bmad-code-org/BMAD-METHOD/issues) — Bug reports and feature requests
- [Discussions](https://github.com/bmad-code-org/BMAD-METHOD/discussions) — Community conversations

## Support BMad

BMad is free for everyone — and always will be. If you'd like to support development:

- ⭐ [Star us on GitHub](https://github.com/bmad-code-org/BMAD-METHOD/) — Helps others discover BMad
- 📺 [Subscribe on YouTube](https://www.youtube.com/@BMadCode) — Master class launching Feb 2026
- ☕ [Buy Me a Coffee](https://buymeacoffee.com/bmad) — Fuel the development
- 🏢 Corporate sponsorship — DM on Discord
- 🎤 Speaking & Media — Available for conferences, podcasts, interviews (Discord)

## Contributing

We welcome contributions! See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

## License

MIT License — see [LICENSE](LICENSE) for details.

---

**BMad** and **BMAD-METHOD** are trademarks of BMad Code, LLC. See [TRADEMARK.md](TRADEMARK.md) for details.

[![Contributors](https://contrib.rocks/image?repo=bmad-code-org/BMAD-METHOD)](https://github.com/bmad-code-org/BMAD-METHOD/graphs/contributors)

See [CONTRIBUTORS.md](CONTRIBUTORS.md) for contributor information.
