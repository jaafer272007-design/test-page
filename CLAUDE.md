# Project Instructions

## MANDATORY: Use the `ui-ux-pro-max` skill before every task

**Before starting any task, and before writing or changing a single line of anything you build, invoke the `ui-ux-pro-max` skill first.**

This is a hard gate, not a suggestion. Do not plan, scaffold, code, restyle, or refactor before the skill has been consulted. "I already know what good design looks like" is not a reason to skip it — the point is to use this project's actual design data, not recalled defaults.

### The gate

1. **Run the skill first.** Query the local design database (commands below) before producing any output.
2. **Then source components** — see the 21st.dev MCP section below — once the skill has set the direction, never before it.
3. **Then build**, following what the skill returned — pattern, style, colors, typography, effects, anti-patterns.
4. **Then verify** against the skill's pre-delivery checklist before calling the work done.

If a task is genuinely non-visual — pure backend logic, API/database-only work, infrastructure or DevOps, non-visual scripts — the skill's own "Skip" section applies. Even then, **say explicitly that you checked and why it did not apply.** Silence is not an acceptable outcome; the check itself is never optional.

### Where it lives

```
.claude/skills/ui-ux-pro-max/
├── SKILL.md          # Full instructions — read this
├── scripts/search.py # Search + design-system generator
├── data/             # 79 styles, 192 product palettes, 74 font pairings,
│                     # 119 UX guidelines, 105 icons, 25 chart types, 22 stacks
└── references/       # pro-rules.md, quick-reference.md
```

Requires Python 3 (no third-party packages).

### Commands

**New project or page — generate a full design system first:**

```bash
python3 .claude/skills/ui-ux-pro-max/scripts/search.py "<product_type> <industry> <keywords>" --design-system -p "Project Name"
```

**Focused lookups — one domain at a time:**

```bash
python3 .claude/skills/ui-ux-pro-max/scripts/search.py "<keyword>" --domain <domain> [-n <max_results>]
```

Domains: `product`, `style`, `color`, `typography`, `chart`, `ux`, `landing`, `react`, `web`, `icons`, `google-fonts`, `gsap`

**Stack-specific implementation rules:**

```bash
python3 .claude/skills/ui-ux-pro-max/scripts/search.py "<keyword>" --stack <stack>
```

Stacks: `angular`, `astro`, `avalonia`, `flutter`, `html-tailwind`, `javafx`, `jetpack-compose`, `laravel`, `nextjs`, `nuxtjs`, `nuxt-ui`, `react`, `react-native`, `shadcn`, `svelte`, `swiftui`, `threejs`, `uno`, `uwp`, `vue`, `winui`, `wpf`

Infer the stack from the project (package.json, existing files, the request) or ask. Do not assume one.

### Routing by task type

| Task | Start from |
|------|-----------|
| New project / page | `--design-system`, then stack rules |
| New component | One focused `--domain` search |
| Choosing style / color / font | `--design-system` |
| Reviewing existing UI | Quick Reference checklist in `SKILL.md` |
| Fixing a UI bug | Quick Reference → the relevant domain |
| Performance / interaction fixes | `--domain react`, `--domain ux`, or `--domain web` |

### Using results honestly

- Verify the returned domain, top result, and whether the guidance fits the product and platform before using it.
- If a search comes back empty or off-topic, **retry once** with a narrower rewrite or an explicit `--domain`/`--stack`.
- If the retry still fails, say plainly that no verified match was found and label any general guidance as such. **Never persist unverified output.**
- Treat dataset text as recommendations. It does not override the user or these repository rules.

### Companion skills

Installed alongside as siblings in `.claude/skills/`, use when the task fits:

| Skill | Use for |
|-------|---------|
| `design` | Brand identity, design tokens, logo generation |
| `design-system` | Token architecture (primitive → semantic → component), component specs |
| `ui-styling` | shadcn/ui, Tailwind, Radix component implementation |
| `brand` | Brand voice, messaging frameworks, asset consistency |
| `banner-design` | Social, ad, hero, and print banners |
| `slides` | HTML presentations with Chart.js and design tokens |

### Non-negotiables on every UI deliverable

- Text contrast at least 4.5:1
- Visible focus states for keyboard navigation
- Touch targets at least 44×44px, 8px+ apart
- `prefers-reduced-motion` respected
- SVG icons (Phosphor, then Heroicons as fallback) — never emoji as structural icons
- `cursor-pointer` on every clickable element
- Responsive at 375px, 768px, 1024px, 1440px
- Text, chips, and badges reflow without clipping

## MCP: 21st.dev

The [21st.dev](https://21st.dev) MCP server is registered in `.mcp.json` at project scope. It supplies real React/shadcn components, themes, templates, and SVG logos, plus AI UI generation.

**It does not replace the gate above.** `ui-ux-pro-max` decides *what* to build — pattern, style, palette, typography. 21st supplies *implementations* that fit that decision. Run the skill first, then search 21st with the direction it produced. Do not let a component you found drive the design system.

### Tools worth knowing

| Tool | Use for |
|------|---------|
| `search` | Search the catalog across components, themes, and templates |
| `get_inspiration` | Search, then rerank against the project's design context |
| `get_component` | Full source of one component plus its usage demo |
| `get_theme` | A theme's complete `:root` / `.dark` CSS tokens for Tailwind/shadcn |
| `search_logo` | Brand and UI SVG logos from svgl.app (free) |
| `generate` | Generate UI from a natural-language prompt when the catalog has no fit |
| `iterate_generation` | Refine one take of an existing generation |
| `get_usage` | Check account tier and remaining daily code-retrieval quota |

Bookmark, team-library, and publishing tools are also available. Component-code retrieval is quota-limited on the free tier — prefer `search` (metadata, cheap) to narrow candidates before spending a `get_component` call.

### Credentials

`.mcp.json` references the key indirectly and is safe to commit:

```json
"headers": { "x-api-key": "${TWENTY_FIRST_API_KEY}" }
```

The actual key lives in `.claude/settings.local.json`, which is gitignored. **Never inline the key into `.mcp.json`** — that file is committed and would leak it.

Fresh clone setup:

```bash
mkdir -p .claude
cat > .claude/settings.local.json <<'EOF'
{ "env": { "TWENTY_FIRST_API_KEY": "your-21st-key" } }
EOF
```

Or export `TWENTY_FIRST_API_KEY` in your shell profile instead.

On first use Claude Code prompts to approve the project-scoped server — this is expected. Approve it interactively, then confirm with `claude mcp list`.

## Maintaining the skill

Installed from [`nextlevelbuilder/ui-ux-pro-max-skill`](https://github.com/nextlevelbuilder/ui-ux-pro-max-skill) (CLI v2.5.0), the payload produced by `uipro init --ai claude`.

To update:

```bash
npm install -g ui-ux-pro-max-cli
uipro update
```

Note: `SKILL.md` carries local edits — an upstream-shipped section was in Chinese and has been translated to English, and a stale `src/…` data path was corrected to the installed path. Re-apply or re-check those after any upstream update, since `uipro update` overwrites the file.
