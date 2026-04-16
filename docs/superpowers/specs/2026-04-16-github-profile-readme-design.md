# GitHub Profile README — Design Spec

- **Date:** 2026-04-16
- **Repository:** `Kairyx-dev/Kairyx-dev` (special profile repo)
- **Status:** Approved for implementation
- **Owner:** Kairyx

---

## 1. Goal

Decorate the personal GitHub profile README with widgets and graphs that
introduce the developer through tools and activity, **without** any
single-line self-introduction or "looking for a job" signaling.

The README is a quiet professional self-introduction — visitors should
understand the developer's stack, focus areas, and activity at a glance.

## 2. Constraints & Decisions

| Decision | Choice | Rationale |
|---|---|---|
| Tone | Professional self-introduction, **no** active job-seeking signal | User explicitly stated they are not looking for a new job |
| Identity framing | None — let widgets and badges speak | User opted out of one-line bio in favor of stack/graph display |
| Visual density | **Rich** (badges + stats + langs + streak + trophies + activity graph + snake) | Picked from density options |
| Color theme | **Tokyo Night** (dark, cool gradient) | Picked from theme options; consistent across all widgets |
| Header style | **Typing SVG animation** (cycling lines) | Provides motion as a focal anchor without static bio text |
| Language | **English** throughout | International standard for GitHub profiles |
| WakaTime | **Excluded** — replaced by commit-based Top Languages card | User declined external service signup |
| Personal info exposure | Hide email, hide company; expose `kairyx-dev.github.io` only | User privacy preference |

## 3. Page Layout

Top-to-bottom order:

```
[1] Typing SVG header (animated, cycles 5 lines)
[2] Tech badges (3 grouped rows)
[3] Two-column row: GitHub Stats │ Top Languages
[4] Streak Stats (full width)
[5] Trophies (full width, single row)
[6] Activity Graph (full width)
[7] Contribution Snake (full width)
[8] Footer: link to kairyx-dev.github.io
```

## 4. Component Specs

### 4.1 Typing SVG header

- Source: `DenverCoder1/readme-typing-svg`
- Five lines, cycling infinitely:
  1. `Backend Engineer`
  2. `Android Engineer (4y)`
  3. `Multi-platform Developer`
  4. `Spring Cloud · CI/CD enthusiast`
  5. `Curious about agentic AI`
- Color: `7AA2F7` (Tokyo Night cyan)
- Font size: `28`
- Width: `500`
- Cursor: enabled, blinking
- Center-aligned in markdown via `<div align="center">`

### 4.2 Tech badges (Shields.io, "for-the-badge" style)

Three grouped rows, center-aligned:

- **Mobile** — Android · Kotlin · Jetpack Compose · Flutter · Swift · Kotlin Multiplatform
- **Backend** — Java · Spring Boot · Spring Cloud
- **Web/Other** — TypeScript · React

Each badge: official brand color background + white text + brand logo.
Use `https://img.shields.io/badge/<label>-<color>?style=for-the-badge&logo=<logo>&logoColor=white`.

### 4.3 GitHub Stats card (3a)

- Source: `anuraghazra/github-readme-stats` (`api/`)
- Username: `Kairyx-dev`
- Theme: `tokyonight`
- Flags: `show_icons=true`, `hide_border=true`
- Note: `count_private=true` intentionally omitted — the public demo instance cannot read private contributions without OAuth setup
- Layout: 50% width column (left)

### 4.4 Top Languages card (3b)

- Source: `anuraghazra/github-readme-stats` (`api/top-langs/`)
- Username: `Kairyx-dev`
- Theme: `tokyonight`
- Layout: `compact`, top 8 langs, `hide_border=true`
- Layout: 50% width column (right, paired with 4.3)

### 4.5 Streak Stats card

- Source: `DenverCoder1/github-readme-streak-stats`
- Username: `Kairyx-dev`
- Theme: `tokyonight`
- `hide_border=true`
- Full width, center-aligned

### 4.6 Trophies

- Source: `ryo-ma/github-profile-trophy`
- Username: `Kairyx-dev`
- Theme: `tokyonight`
- `column=7`, `margin-w=15`, `no-frame=true`
- Layout wraps automatically — typically renders as 1–2 rows depending on trophy count

### 4.7 Activity Graph

- Source: `Ashutosh00710/github-readme-activity-graph`
- Username: `Kairyx-dev`
- Theme: `tokyo-night`
- `area=true`, `hide_border=true`
- Full width

### 4.8 Contribution Snake

- Source: `Platane/snk` (GitHub Action)
- Output: SVG pushed to `output` branch by automation
- README references the SVG using `<picture>` for theme switching:
  - Dark variant: `github-snake-dark.svg`
  - Light variant: `github-snake.svg`
- Full width

### 4.9 Footer

Single line, center-aligned:

```
🌐 kairyx-dev.github.io
```

Linked to `https://kairyx-dev.github.io`.

## 5. Automation — Snake Workflow

A new GitHub Action is required to keep the snake SVG fresh.

- **Path:** `.github/workflows/snake.yml`
- **Triggers:**
  - `schedule`: cron `0 0 * * *` (daily at 00:00 UTC)
  - `workflow_dispatch`: manual trigger button
- **Steps:**
  1. Checkout
  2. Run `Platane/snk@v3` action with:
     - `github_user_name`: `Kairyx-dev`
     - `outputs`:
       - `dist/github-snake.svg`
       - `dist/github-snake-dark.svg?palette=github-dark`
  3. Push `dist/` to `output` branch via `crazy-max/ghaction-github-pages@v3`
- **Permissions:** workflow needs `contents: write` to push the `output` branch

## 6. Error Handling & Maintenance

| Concern | Behavior | Mitigation |
|---|---|---|
| External widget service downtime | Image fails to load, broken-image icon shown | Markdown alt text describes intent (e.g., `alt="GitHub Stats"`) |
| Stats card cache | ~1 hour cache before stats refresh | Acceptable — stats are not time-critical |
| Snake action failure | Outdated snake animation | Manual `workflow_dispatch` trigger as recovery; daily cron retries |
| Theme inconsistency | One widget renders in wrong theme | Single `theme=tokyonight` parameter shared across all widget URLs — review during implementation |
| Username typo | All widgets show "user not found" | Treat `Kairyx-dev` as a single source of truth; define once if templating, otherwise verify all URLs |

## 7. Out of Scope

Explicitly **not** included in this design (per user direction):

- One-line self-introduction or bio text
- Email contact line
- Company/employer mention
- Social media links beyond `kairyx-dev.github.io`
- WakaTime card / external service signups
- Visitor counter, profile views badge
- Pinned repos rearrangement (handled in GitHub UI, not README)
- Internationalized (Korean) variants

## 8. Acceptance Criteria

The implementation is complete when:

1. `README.md` renders the eight sections in the specified order
2. All widgets visibly use Tokyo Night color theme
3. Typing SVG cycles all five lines
4. Tech badges in three grouped rows with brand logos
5. GitHub Stats and Top Languages display side-by-side on a desktop viewport (≥768px wide)
6. Snake workflow file exists at `.github/workflows/snake.yml` and runs successfully on manual dispatch
7. After workflow run, snake SVG is reachable from `output` branch and renders in README
8. Footer links to `https://kairyx-dev.github.io`
9. No company name, no email, no Korean text appears
