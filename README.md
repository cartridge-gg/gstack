# gstack

> "I don't think I've typed like a line of code probably since December, basically, which is an extremely large change." — [Andrej Karpathy](https://fortune.com/2026/03/21/andrej-karpathy-openai-cofounder-ai-agents-coding-state-of-psychosis-openclaw/), No Priors podcast, March 2026

Game development is finally in the same moment software hit first: one person with AI can move like a studio. Not because art direction, design judgment, or playtesting got easy. Because the mechanical parts got radically cheaper.

That changes what matters. The bottleneck is no longer "can we build it?" The bottleneck is: did we choose the right fantasy, the right loop, the right slice, the right launch surface?

**gstack is an AI-native indie studio workflow.** It turns Claude Code into a virtual team: a game director who rethinks the concept, a technical director who locks architecture and save strategy, a feel reviewer who catches vague UX, a reviewer who finds production bugs, a QA lead who opens a real browser, and a release engineer who packages builds and launch notes.

This is not a generic prompt pack. It is an opinionated production system for getting from idea to prototype to vertical slice to launch.

Fork it. Improve it. Make it yours.

**Who this is for:**
- **Solo indie developers** — one person wearing design, code, and production hats
- **Small game teams** — teams trying to get to a convincing slice faster
- **Technical artists and designer-programmers** — people who care about feel, flow, and implementation equally

## Quick start

1. Install gstack (30 seconds — see below)
2. Run `/office-hours` — describe the game you want to make
3. Run `/plan-ceo-review` on any concept, feature, or vertical-slice idea
4. Run `/review` on any branch with changes
5. Run `/qa` on your web surface, docs, or launch page
6. Stop there. You'll know if this is for you.

## Install — 30 seconds

**Requirements:** [Claude Code](https://docs.anthropic.com/en/docs/claude-code), [Git](https://git-scm.com/), [Bun](https://bun.sh/) v1.0+, [Node.js](https://nodejs.org/) (Windows only)

### Step 1: Install on your machine

Open Claude Code and paste this. Claude does the rest.

> Install gstack: run **`git clone https://github.com/garrytan/gstack.git ~/.claude/skills/gstack && cd ~/.claude/skills/gstack && ./setup`** then add a "gstack" section to CLAUDE.md that says to use the /browse skill from gstack for all web browsing, never use mcp\_\_claude-in-chrome\_\_\* tools, and lists the available skills: /office-hours, /plan-ceo-review, /plan-eng-review, /plan-design-review, /design-consultation, /review, /ship, /land-and-deploy, /canary, /benchmark, /browse, /qa, /qa-only, /design-review, /setup-browser-cookies, /setup-deploy, /retro, /investigate, /document-release, /codex, /cso, /autoplan, /careful, /freeze, /guard, /unfreeze, /gstack-upgrade, /dojo-review, /dojo-world, /dojo-client, /dojo-model, /dojo-system, /dojo-test. Then ask the user if they also want to add gstack to the current project so teammates get it.

### Step 2: Add to your repo so teammates get it (optional)

> Add gstack to this project: run **`cp -Rf ~/.claude/skills/gstack .claude/skills/gstack && rm -rf .claude/skills/gstack/.git && cd .claude/skills/gstack && ./setup`** then add a "gstack" section to this project's CLAUDE.md that says to use the /browse skill from gstack for all web browsing, never use mcp\_\_claude-in-chrome\_\_\* tools, lists the available skills: /office-hours, /plan-ceo-review, /plan-eng-review, /plan-design-review, /design-consultation, /review, /ship, /land-and-deploy, /canary, /benchmark, /browse, /qa, /qa-only, /design-review, /setup-browser-cookies, /setup-deploy, /retro, /investigate, /document-release, /codex, /cso, /careful, /freeze, /guard, /unfreeze, /gstack-upgrade, /dojo-review, /dojo-world, /dojo-client, /dojo-model, /dojo-system, /dojo-test, and tells Claude that if gstack skills aren't working, run `cd .claude/skills/gstack && ./setup` to build the binary and register skills.

Real files get committed to your repo (not a submodule), so `git clone` just works. Everything lives inside `.claude/`. Nothing touches your PATH or runs in the background.

### Codex, Gemini CLI, or Cursor

gstack works on any agent that supports the [SKILL.md standard](https://github.com/anthropics/claude-code). Skills live in `.agents/skills/` and are discovered automatically.

Install to one repo:

```bash
git clone https://github.com/garrytan/gstack.git .agents/skills/gstack
cd .agents/skills/gstack && ./setup --host codex
```

When setup runs from `.agents/skills/gstack`, it installs the generated Codex skills next to it in the same repo and does not write to `~/.codex/skills`.

Install once for your user account:

```bash
git clone https://github.com/garrytan/gstack.git ~/gstack
cd ~/gstack && ./setup --host codex
```

`setup --host codex` creates the runtime root at `~/.codex/skills/gstack` and
links the generated Codex skills at the top level. This avoids duplicate skill
discovery from the source repo checkout.

Or let setup auto-detect which agents you have installed:

```bash
git clone https://github.com/garrytan/gstack.git ~/gstack
cd ~/gstack && ./setup --host auto
```

For Codex-compatible hosts, setup now supports both repo-local installs from `.agents/skills/gstack` and user-global installs from `~/.codex/skills/gstack`. All 28 skills work across all supported agents. Hook-based safety skills (careful, freeze, guard) use inline safety advisory prose on non-Claude hosts.

## See it work

```
You:    I want to build a co-op dungeon crawler where two players improvise
        spells by combining runes in real time.
You:    /office-hours
Claude: [asks about the fantasy, the loop, the first 60 seconds, and the
        smallest slice that proves the game is worth continuing]

You:    I want players to feel clever under pressure. The first minute needs
        to get them combining runes immediately. I don't know if online co-op
        belongs in the first slice.

Claude: I'm going to push back on the framing. You said "co-op dungeon
        crawler." But what you actually described is a high-pressure
        spell improvisation game.
        [extracts the actual player fantasy]
        [challenges 4 premises — you agree, disagree, or adjust]
        [generates 3 implementation approaches with effort estimates]
        RECOMMENDATION: Ship the smallest vertical slice that proves the
        spell-combo loop is fun. Delay the world-scale ambitions until a
        stranger wants "one more run."
        [writes design doc → feeds into downstream skills automatically]

You:    /plan-ceo-review
        [reads the design doc, challenges scope, runs 10-section review]

You:    /plan-eng-review
        [ASCII diagrams for data flow, state machines, error paths]
        [test matrix, failure modes, security concerns]

You:    Approve plan. Exit plan mode.
        [writes 2,400 lines across 11 files. ~8 minutes.]

You:    /review
        [AUTO-FIXED] 2 issues. [ASK] Race condition → you approve fix.

You:    /qa https://staging.myapp.com
        [opens real browser, clicks through flows, finds and fixes a bug]

You:    /ship
        Tests: 42 → 51 (+9 new). PR: github.com/you/app/pull/42
```

You said "co-op dungeon crawler." The agent said "you're building a spell improvisation game" — because it listened to the player fantasy, not just the genre label. Eight commands, end to end. That is not a copilot. That is a team.

## The sprint

gstack is a process, not a collection of tools. The skills run in the order a sprint runs:

**Concept → Plan → Build → Review → Test → Ship → Reflect**

Each skill feeds into the next. `/office-hours` writes a design doc that `/plan-ceo-review` reads. `/plan-eng-review` writes a test plan that `/qa` picks up. `/review` catches bugs that `/ship` verifies are fixed. Nothing falls through the cracks because every step knows what came before it.

| Skill | Your specialist | What they do |
|-------|----------------|--------------|
| `/office-hours` | **Studio Greenlight** | Start here. Six forcing questions that reframe the game before you write code: player fantasy, reference point, core loop, first 60 seconds, proof of fun, and vertical slice. |
| `/plan-ceo-review` | **Game Director** | Rethink the concept. Find the 10-star game hiding inside the request. Four modes: Expansion, Selective Expansion, Hold Scope, Reduction. |
| `/plan-eng-review` | **Technical Director** | Lock in architecture, data flow, diagrams, edge cases, save strategy, build concerns, and tests. Forces hidden assumptions into the open. |
| `/plan-design-review` | **Feel Reviewer** | Rates each design dimension 0-10, explains what a 10 looks like, then edits the plan to get there. Catches generic UX and missing interaction states before implementation. |
| `/design-consultation` | **Design Partner** | Build a complete design system from scratch. Researches the landscape, proposes creative risks, generates realistic product mockups. |
| `/review` | **Staff Engineer** | Find the bugs that pass CI but blow up in production. Auto-fixes the obvious ones. Flags completeness gaps. |
| `/investigate` | **Debugger** | Systematic root-cause debugging. Iron Law: no fixes without investigation. Traces data flow, tests hypotheses, stops after 3 failed fixes. |
| `/design-review` | **Designer Who Codes** | Same audit as /plan-design-review, then fixes what it finds. Atomic commits, before/after screenshots. |
| `/qa` | **QA Lead** | Test your web surfaces, launch pages, and adjacent flows, find bugs, fix them with atomic commits, re-verify. Auto-generates regression tests for every fix. |
| `/qa-only` | **QA Reporter** | Same methodology as /qa but report only. Pure bug report without code changes. |
| `/cso` | **Chief Security Officer** | OWASP Top 10 + STRIDE threat model. Zero-noise: 17 false positive exclusions, 8/10+ confidence gate, independent finding verification. Each finding includes a concrete exploit scenario. |
| `/ship` | **Build Engineer** | Sync main, run tests, audit coverage, push, open PR. Bootstraps test frameworks if you don't have one. |
| `/land-and-deploy` | **Release Engineer** | Merge the PR, wait for CI and deploy, verify production health. One command from "approved" to "verified in production." |
| `/canary` | **SRE** | Post-deploy monitoring loop. Watches for console errors, performance regressions, and page failures. |
| `/benchmark` | **Performance Engineer** | Baseline page load times, Core Web Vitals, and resource sizes. Compare before/after on every PR. |
| `/document-release` | **Technical Writer** | Update all project docs to match what you just shipped. Catches stale READMEs automatically. |
| `/retro` | **Eng Manager** | Team-aware weekly retro. Per-person breakdowns, shipping streaks, test health trends, growth opportunities. `/retro global` runs across all your projects and AI tools (Claude Code, Codex, Gemini). |
| `/browse` | **QA Engineer** | Real Chromium browser, real clicks, real screenshots. ~100ms per command. |
| `/setup-browser-cookies` | **Session Manager** | Import cookies from your real browser (Chrome, Arc, Brave, Edge) into the headless session. Test authenticated pages. |
| `/autoplan` | **Review Pipeline** | One command, fully reviewed plan. Runs game-direction → feel → technical review automatically with encoded decision principles. Surfaces only taste decisions for your approval. |
| `/dojo-review` | **Dojo Architect** | Decide whether the game should be onchain, map Dojo/Starknet constraints, and define what belongs onchain versus offchain. |
| `/dojo-world` | **World Designer** | Write a Dojo PRD/TDD for a `client/` + `contract/` monorepo with models, systems, invariants, and test coverage. |
| `/dojo-client` | **Client Engineer** | Once systems exist, run Sozo build/codegen, wire generated types into the client, and build a workable integration layer. |
| `/dojo-model` | **Model Architect** | Design and implement small, testable Dojo models with explicit keys, storage boundaries, and invariants. |
| `/dojo-system` | **System Engineer** | Build focused, stateless Dojo systems that read models, apply rules, write world state, and emit events. |
| `/dojo-test` | **Testing Engineer** | Build system-level unit coverage, world integration tests, and end-to-end onchain gameplay flows. |

### Power tools

| Skill | What it does |
|-------|-------------|
| `/codex` | **Second Opinion** — independent code review from OpenAI Codex CLI. Three modes: review (pass/fail gate), adversarial challenge, and open consultation. Cross-model analysis when both `/review` and `/codex` have run. |
| `/careful` | **Safety Guardrails** — warns before destructive commands (rm -rf, DROP TABLE, force-push). Say "be careful" to activate. Override any warning. |
| `/freeze` | **Edit Lock** — restrict file edits to one directory. Prevents accidental changes outside scope while debugging. |
| `/guard` | **Full Safety** — `/careful` + `/freeze` in one command. Maximum safety for prod work. |
| `/unfreeze` | **Unlock** — remove the `/freeze` boundary. |
| `/setup-deploy` | **Deploy Configurator** — one-time setup for `/land-and-deploy`. Detects your platform, production URL, and deploy commands. |
| `/gstack-upgrade` | **Self-Updater** — upgrade gstack to latest. Detects global vs vendored install, syncs both, shows what changed. |

**[Deep dives with examples and philosophy for every skill →](docs/skills.md)**

## Parallel sprints

gstack works well with one sprint. It gets interesting with ten running at once.

[Conductor](https://conductor.build) runs multiple Claude Code sessions in parallel — each in its own isolated workspace. One session on `/office-hours`, another on `/review`, a third implementing a feature, a fourth running `/qa`. All at the same time. The sprint structure is what makes parallelism work — without a process, ten agents is ten sources of chaos. With a process, each agent knows exactly what to do and when to stop.

---

Free, MIT licensed, open source. No premium tier, no waitlist.

I open sourced how I build games and interactive tools. You can fork it and make it your own.

## Docs

| Doc | What it covers |
|-----|---------------|
| [Skill Deep Dives](docs/skills.md) | Philosophy, examples, and workflow for every skill (includes Greptile integration) |
| [Builder Ethos](ETHOS.md) | Builder philosophy: Boil the Lake, Search Before Building, three layers of knowledge |
| [Architecture](ARCHITECTURE.md) | Design decisions and system internals |
| [Browser Reference](BROWSER.md) | Full command reference for `/browse` |
| [Contributing](CONTRIBUTING.md) | Dev setup, testing, contributor mode, and dev mode |
| [Changelog](CHANGELOG.md) | What's new in every version |

## Privacy & Telemetry

gstack includes **opt-in** usage telemetry to help improve the project. Here's exactly what happens:

- **Default is off.** Nothing is sent anywhere unless you explicitly say yes.
- **On first run,** gstack asks if you want to share anonymous usage data. You can say no.
- **What's sent (if you opt in):** skill name, duration, success/fail, gstack version, OS. That's it.
- **What's never sent:** code, file paths, repo names, branch names, prompts, or any user-generated content.
- **Change anytime:** `gstack-config set telemetry off` disables everything instantly.

Data is stored in [Supabase](https://supabase.com) (open source Firebase alternative). The schema is in [`supabase/migrations/001_telemetry.sql`](supabase/migrations/001_telemetry.sql) — you can verify exactly what's collected. The Supabase publishable key in the repo is a public key (like a Firebase API key) — row-level security policies restrict it to insert-only access.

**Local analytics are always available.** Run `gstack-analytics` to see your personal usage dashboard from the local JSONL file — no remote data needed.

## Troubleshooting

**Skill not showing up?** `cd ~/.claude/skills/gstack && ./setup`

**`/browse` fails?** `cd ~/.claude/skills/gstack && bun install && bun run build`

**Stale install?** Run `/gstack-upgrade` — or set `auto_upgrade: true` in `~/.gstack/config.yaml`

**Codex says "Skipped loading skill(s) due to invalid SKILL.md"?** Your Codex skill descriptions are stale. Fix: `cd ~/.codex/skills/gstack && git pull && ./setup --host codex` — or for repo-local installs: `cd "$(readlink -f .agents/skills/gstack)" && git pull && ./setup --host codex`

**Windows users:** gstack works on Windows 11 via Git Bash or WSL. Node.js is required in addition to Bun — Bun has a known bug with Playwright's pipe transport on Windows ([bun#4253](https://github.com/oven-sh/bun/issues/4253)). The browse server automatically falls back to Node.js. Make sure both `bun` and `node` are on your PATH.

**Claude says it can't see the skills?** Make sure your project's `CLAUDE.md` has a gstack section. Add this:

```
## gstack
Use /browse from gstack for all web browsing. Never use mcp__claude-in-chrome__* tools.
Available skills: /office-hours, /plan-ceo-review, /plan-eng-review, /plan-design-review,
/design-consultation, /review, /ship, /land-and-deploy, /canary, /benchmark, /browse,
/qa, /qa-only, /design-review, /setup-browser-cookies, /setup-deploy, /retro,
/investigate, /document-release, /codex, /cso, /autoplan, /careful, /freeze, /guard,
/unfreeze, /gstack-upgrade, /dojo-review, /dojo-world, /dojo-client, /dojo-model,
/dojo-system, /dojo-test.
```

## License

MIT. Free forever. Go build something.
