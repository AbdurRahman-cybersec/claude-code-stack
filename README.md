# Nova's Claude Code Enhancement Stack

> **SOC Analyst · AskCMMC.ai · Aurora/NEXUS · Zorin OS Linux**
> Full setup guide for everything installed in this session.

---

## What This Repo Contains

| File | Description |
|------|-------------|
| `claude-code-repos.html` | Full ecosystem guide — all 6 repos, CLI connectors, setup instructions, DeepSeek swap |
| `nova-tool-instructions.html` | Detailed usage instructions for every tool with exact commands and prompts |
| `README.md` | This file |

---

## What Was Installed

### Claude Code Plugins (via `ds` session)

| Plugin | Marketplace | What It Does |
|--------|-------------|--------------|
| `andrej-karpathy-skills` | `forrestchang/andrej-karpathy-skills` | Discipline rules — think before code, surgical changes only |
| `superpowers` | `obra/superpowers-marketplace` | Spec before code, parallel agents, branch discipline |
| `ecc` | `affaan-m/ECC` | CVE scanning, prompt injection blocking, token optimization |
| `ruflo-core` | `ruvnet/ruflo` | 100+ parallel agents, swarm orchestration, shared memory |

### Install Commands (run inside `ds`)
```bash
/plugin marketplace add forrestchang/andrej-karpathy-skills
/plugin install andrej-karpathy-skills@karpathy-skills

/plugin marketplace add obra/superpowers-marketplace
/plugin install superpowers@superpowers-marketplace

/plugin marketplace add affaan-m/ECC
/plugin install ecc@ecc

/plugin marketplace add ruvnet/ruflo
/plugin install ruflo-core@ruflo

/reload-plugins
```

After reload you should see:
```
Reloaded: 4 plugins · 77 skills · 70 agents · 36 hooks · 6 plugin MCP servers
```

---

### Git Clones (installed to `~/tools/`)

| Tool | Repo | Path | Purpose |
|------|------|------|---------|
| Open Design | `github.com/nexu-io/open-design` | `~/tools/open-design` | 72 brand-grade design systems |
| Printing Press | `github.com/mvanhorn/cli-printing-press` | via `go install` | CLI factory |

```bash
# Open Design
git clone https://github.com/nexu-io/open-design ~/tools/open-design

# Printing Press binary
go install github.com/mvanhorn/cli-printing-press/v4/cmd/cli-printing-press@latest

# Add Go to PATH (permanent)
echo 'export PATH=$PATH:$(go env GOPATH)/bin' >> ~/.bashrc
source ~/.bashrc

# Starter pack CLIs
npx -y @mvanhorn/printing-press-library install starter-pack

# Factory skill (lets ds build new CLIs on demand)
npx skills add mvanhorn/cli-printing-press/skills -g -a claude-code -y
```

---

### Obsidian Skills
Already configured before this session. Vault paths wired into `CLAUDE.md`:
```
/home/amk13/Documents/Obsidian Vault
```

---

### DeepSeek API (`ds` alias)
Already configured before this session. Claude Code routed to DeepSeek API via `ds` command.

---

## Global CLAUDE.md

Location: `~/.claude/CLAUDE.md`

The full file was already well configured. Two lines added during this session:

```markdown
## Design System
DESIGN_SYSTEM_PATH: ~/tools/open-design
DESIGN_REFERENCE: Linear
```

Full CLAUDE.md includes:
- Identity and role context (Nova, SOC Analyst, USCA, AskCMMC.ai)
- Obsidian vault structure and paths
- Session START rules (auto-load project CLAUDE.md and daily log)
- Session END rules (update CLAUDE.md, append to daily log)
- Directory → project mapping
- Active projects (SmoothPlay Android, AskCMMC.ai, Job Search)
- General rules (no git push, no system packages, token efficiency)
- Design system path (added this session)

---

## Printing Press CLIs Installed

### Starter Pack (auto-installed)
| CLI | Binary | Skill | Use |
|-----|--------|-------|-----|
| ESPN | `espn-pp-cli` | `pp-espn` | Live scores, standings, 17 sports |
| Flight Goat | `flight-goat-pp-cli` | `pp-flight-goat` | Google Flights + Kayak search |
| Movie Goat | `movie-goat-pp-cli` | `pp-movie-goat` | TMDb + OMDb movie discovery |
| Recipe Goat | `recipe-goat-pp-cli` | `pp-recipe-goat` | Recipe search + nutrition data |

### Recommended to Install Next
```bash
# CVE feed — SOC use
npx -y @mvanhorn/printing-press-library install nvd

# LinkedIn + 27 platforms — job hunting + OSINT
npx -y @mvanhorn/printing-press-library install scrape-creators

# Daily security intel
npx -y @mvanhorn/printing-press-library install hackernews

# Your Supabase backend — direct agent access
npx -y @mvanhorn/printing-press-library install supabase
```

---

## Superpowers — 3-Step Workflow

Always use this order. Never skip to execute.

```
/superpowers:brainstorm [what you want to build]
/superpowers:write-plan
# review the spec
/superpowers:execute-plan
```

---

## Ruflo — Swarm Configs

Create `.claude/swarm.json` in your project:

### Detection Engineering
```json
{
  "agents": [
    { "role": "researcher", "task": "map MITRE technique to detection opportunities" },
    { "role": "writer",     "task": "write SPL/KQL detection query" },
    { "role": "reviewer",   "task": "validate query against log samples" }
  ],
  "shared_memory": true
}
```

### Security Audit
```json
{
  "agents": [
    { "role": "scanner",  "task": "scan all files for hardcoded secrets and CVEs" },
    { "role": "reviewer", "task": "review auth logic for injection vulnerabilities" },
    { "role": "reporter", "task": "compile findings into a structured report" }
  ],
  "shared_memory": true
}
```

### Full App Feature
```json
{
  "agents": [
    { "role": "backend",  "task": "build the API endpoint and DB schema" },
    { "role": "frontend", "task": "build the UI component for this feature" },
    { "role": "tests",    "task": "write unit and integration tests for both" }
  ],
  "shared_memory": true
}
```

Run with:
```bash
ds swarm run .claude/swarm.json
```

---

## Open Design — Prompts That Work

Reference any design system by name in your `ds` prompt:

```
"Refine this dashboard to Linear-level polish. Reference the Linear design system."
"Build a login form using Stripe's design patterns — clean, minimal, high trust."
"Style this SOC dashboard using Linear's dark theme tokens. High information density."
"Review this UI against the Airbnb design system. Score on all 5 dimensions and fix the lowest."
```

Best systems for your work: **Linear** (dashboards), **Stripe** (forms), **Vercel** (dark dev tools), **GitHub** (code-adjacent UI)

---

## ECC — 3 Features That Matter

```bash
# Enable the 3 useful features
ds config set security.cve_scan true
ds config set security.injection_guard true
ds config set tokens.optimize true

# Manual CVE scan on demand (inside ds):
# "Scan all dependencies in this project for CVEs"

# Test injection blocking (inside ds):
# "Check this user input for prompt injection: [paste string]"
```

---

## DeepSeek Swap

`ds` alias already configured. To verify which API you're hitting:
```bash
echo $ANTHROPIC_BASE_URL
```
- Blank or `https://api.anthropic.com` = Claude (Anthropic billing)
- `https://api.deepseek.com` = DeepSeek

Use `ds` for batch runs and experiments. Use `claude` for production builds where precision matters.

---

## Verify Everything Is Running

```bash
# In terminal
go version
cli-printing-press --version
echo $PATH | grep go

# Inside ds
/plugin list
# Should show: andrej-karpathy-skills, superpowers, ecc, ruflo-core
```

---

## References

| Resource | URL |
|----------|-----|
| Printing Press homepage | https://printingpress.dev/ |
| CLI Factory repo | https://github.com/mvanhorn/cli-printing-press |
| CLI Library repo | https://github.com/mvanhorn/printing-press-library |
| Open Design repo | https://github.com/nexu-io/open-design |
| Ruflo (formerly Claude Flow) | https://github.com/ruvnet/ruflo |
| DeepSeek awesome-agent repo | https://github.com/deepseek-ai/awesome-deepseek-agent |
| Go installer | https://go.dev/dl/ |
| Node.js | https://nodejs.org/ |

---

*Session completed — Zorin OS Linux · Claude Code v2.1.148 · ds alias (DeepSeek API)*
