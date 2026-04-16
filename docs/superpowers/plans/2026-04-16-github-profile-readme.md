# GitHub Profile README Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Decorate the `Kairyx-dev/Kairyx-dev` profile README with animated header, tech badges, GitHub stats widgets, contribution snake, and a footer link — all in Tokyo Night theme.

**Architecture:** Single `README.md` rewrite using embedded images from external widget services (github-readme-stats, streak-stats, trophies, activity-graph, readme-typing-svg) plus a GitHub Action workflow that generates a contribution snake SVG daily and pushes it to an `output` branch.

**Tech Stack:** Markdown, HTML (for layout), Shields.io badges, GitHub Actions (Platane/snk, crazy-max/ghaction-github-pages)

---

## File Map

| Action | Path | Responsibility |
|--------|------|---------------|
| Modify | `README.md` | Complete profile page — 8 sections of widgets |
| Create | `.github/workflows/snake.yml` | Daily cron job generating contribution snake SVGs |

---

### Task 1: Create Snake GitHub Action Workflow

**Files:**
- Create: `.github/workflows/snake.yml`

- [ ] **Step 1: Create the workflow file**

```yaml
name: Generate Snake

on:
  schedule:
    - cron: "0 0 * * *"
  workflow_dispatch:

permissions:
  contents: write

jobs:
  generate:
    runs-on: ubuntu-latest
    timeout-minutes: 10

    steps:
      - name: Generate snake SVGs
        uses: Platane/snk/svg-only@v3
        with:
          github_user_name: Kairyx-dev
          outputs: |
            dist/github-snake.svg
            dist/github-snake-dark.svg?palette=github-dark

      - name: Push to output branch
        uses: crazy-max/ghaction-github-pages@v4
        with:
          target_branch: output
          build_dir: dist
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

Write this to `.github/workflows/snake.yml`.

- [ ] **Step 2: Validate YAML syntax**

Run: `python3 -c "import yaml; yaml.safe_load(open('.github/workflows/snake.yml')); print('YAML OK')"`

Expected: `YAML OK`

- [ ] **Step 3: Commit**

```bash
git add .github/workflows/snake.yml
git commit -m "ci: add snake contribution graph workflow"
```

---

### Task 2: Write README — Typing SVG Header and Tech Badges

**Files:**
- Modify: `README.md` (complete rewrite — replaces existing default template)

- [ ] **Step 1: Write README with header and badges**

Replace the entire contents of `README.md` with:

```markdown
<div align="center">

<!-- Typing SVG Header -->
<a href="https://git.io/typing-svg">
  <img src="https://readme-typing-svg.demolab.com?font=Fira+Code&size=28&duration=3000&pause=1000&color=7AA2F7&center=true&vCenter=true&width=500&lines=Backend+Engineer;Android+Engineer+(4y);Multi-platform+Developer;Spring+Cloud+%C2%B7+CI%2FCD+enthusiast;Curious+about+agentic+AI" alt="Typing SVG" />
</a>

<br/><br/>

<!-- Tech Badges: Mobile -->
![Android](https://img.shields.io/badge/Android-3DDC84?style=for-the-badge&logo=android&logoColor=white)
![Kotlin](https://img.shields.io/badge/Kotlin-7F52FF?style=for-the-badge&logo=kotlin&logoColor=white)
![Jetpack Compose](https://img.shields.io/badge/Jetpack_Compose-4285F4?style=for-the-badge&logo=jetpackcompose&logoColor=white)
![Flutter](https://img.shields.io/badge/Flutter-02569B?style=for-the-badge&logo=flutter&logoColor=white)
![Swift](https://img.shields.io/badge/Swift-F05138?style=for-the-badge&logo=swift&logoColor=white)
![KMP](https://img.shields.io/badge/KMP-7F52FF?style=for-the-badge&logo=kotlin&logoColor=white)

<!-- Tech Badges: Backend -->
![Java](https://img.shields.io/badge/Java-ED8B00?style=for-the-badge&logo=openjdk&logoColor=white)
![Spring Boot](https://img.shields.io/badge/Spring_Boot-6DB33F?style=for-the-badge&logo=springboot&logoColor=white)
![Spring Cloud](https://img.shields.io/badge/Spring_Cloud-6DB33F?style=for-the-badge&logo=spring&logoColor=white)

<!-- Tech Badges: Web/Other -->
![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=for-the-badge&logo=typescript&logoColor=white)
![React](https://img.shields.io/badge/React-61DAFB?style=for-the-badge&logo=react&logoColor=black)

</div>
```

Note: Badge rows will naturally wrap based on viewport width. The `<div align="center">` centers all content. No explicit line breaks needed between badge groups — they flow as inline elements. The blank lines between comment groups provide readability in the source.

- [ ] **Step 2: Verify badge URL format**

Run: `grep -c 'img.shields.io/badge' README.md`

Expected: `11` (11 badge URLs total: 6 Mobile + 3 Backend + 2 Web)

- [ ] **Step 3: Commit**

```bash
git add README.md
git commit -m "feat: add typing SVG header and tech badges"
```

---

### Task 3: Add GitHub Stats and Top Languages (Side-by-Side)

**Files:**
- Modify: `README.md` (append before closing `</div>`)

- [ ] **Step 1: Add stats section**

Insert the following block **before** the closing `</div>` tag at the end of `README.md`:

```markdown

<br/>

<!-- GitHub Stats + Top Languages -->
<img src="https://github-readme-stats.vercel.app/api?username=Kairyx-dev&show_icons=true&theme=tokyonight&hide_border=true" height="195" alt="GitHub Stats" />
&nbsp;
<img src="https://github-readme-stats.vercel.app/api/top-langs/?username=Kairyx-dev&layout=compact&theme=tokyonight&hide_border=true&langs_count=8" height="195" alt="Top Languages" />
```

The two `<img>` tags at the same height sit side-by-side inside the centered `<div>`. The `&nbsp;` adds a small gap between them. Using `height="195"` ensures both cards have identical vertical size, preventing misalignment.

- [ ] **Step 2: Verify stat URLs contain correct username**

Run: `grep -c 'username=Kairyx-dev' README.md`

Expected: `2` (stats card + top-langs card)

- [ ] **Step 3: Commit**

```bash
git add README.md
git commit -m "feat: add GitHub stats and top languages cards"
```

---

### Task 4: Add Streak Stats, Trophies, and Activity Graph

**Files:**
- Modify: `README.md` (append before closing `</div>`)

- [ ] **Step 1: Add streak, trophies, and activity graph**

Insert the following block **before** the closing `</div>` tag:

```markdown

<br/>

<!-- Streak Stats -->
<img src="https://streak-stats.demolab.com?user=Kairyx-dev&theme=tokyonight&hide_border=true" alt="Streak Stats" />

<br/><br/>

<!-- Trophies -->
<img src="https://github-profile-trophy.vercel.app/?username=Kairyx-dev&theme=tokyonight&column=7&margin-w=15&no-frame=true" alt="Trophies" />

<br/><br/>

<!-- Activity Graph -->
<img src="https://github-readme-activity-graph.vercel.app/graph?username=Kairyx-dev&theme=tokyo-night&area=true&hide_border=true" width="100%" alt="Activity Graph" />
```

Notes:
- Streak and Trophies use default sizing (server-rendered width).
- Activity Graph gets `width="100%"` since the server returns a wide SVG.
- All three use `theme=tokyonight` (activity graph uses `tokyo-night` — that's the correct theme name for that specific service, note the hyphen).

- [ ] **Step 2: Verify all Tokyo Night theme params**

Run: `grep -c 'tokyonight\|tokyo-night' README.md`

Expected: `5` (stats, top-langs, streak, trophies use `tokyonight`; activity-graph uses `tokyo-night`)

- [ ] **Step 3: Commit**

```bash
git add README.md
git commit -m "feat: add streak stats, trophies, and activity graph"
```

---

### Task 5: Add Contribution Snake and Footer

**Files:**
- Modify: `README.md` (append before closing `</div>`)

- [ ] **Step 1: Add snake and footer**

Insert the following block **before** the closing `</div>` tag:

```markdown

<br/>

<!-- Contribution Snake -->
<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/Kairyx-dev/Kairyx-dev/output/github-snake-dark.svg" />
  <source media="(prefers-color-scheme: light)" srcset="https://raw.githubusercontent.com/Kairyx-dev/Kairyx-dev/output/github-snake.svg" />
  <img alt="Contribution Snake" src="https://raw.githubusercontent.com/Kairyx-dev/Kairyx-dev/output/github-snake.svg" width="100%" />
</picture>

<br/><br/>

<!-- Footer -->
<a href="https://kairyx-dev.github.io">
  🌐 kairyx-dev.github.io
</a>
```

The `<picture>` element automatically selects the dark or light snake SVG based on the visitor's GitHub theme preference. The `<img>` fallback uses the light variant.

**Important:** The snake SVGs at these URLs will return 404 until the snake workflow runs for the first time. This is expected — after the workflow runs, they will resolve. The README will show a broken image until then.

- [ ] **Step 2: Verify snake URLs point to output branch**

Run: `grep -c 'Kairyx-dev/Kairyx-dev/output/' README.md`

Expected: `3` (dark source, light source, fallback img)

- [ ] **Step 3: Verify footer link**

Run: `grep 'kairyx-dev.github.io' README.md`

Expected: two matches — the `<a href>` URL and the display text.

- [ ] **Step 4: Commit**

```bash
git add README.md
git commit -m "feat: add contribution snake and footer"
```

---

### Task 6: Push and Verify

**Files:** (none modified — verification only)

- [ ] **Step 1: Push all commits to origin**

```bash
git push origin main
```

- [ ] **Step 2: Trigger snake workflow manually**

```bash
gh workflow run snake.yml
```

Wait ~2 minutes for the workflow to complete, then verify:

```bash
gh run list --workflow=snake.yml --limit=1
```

Expected: status `completed`, conclusion `success`.

- [ ] **Step 3: Verify snake SVG exists on output branch**

```bash
gh api repos/Kairyx-dev/Kairyx-dev/contents/github-snake.svg?ref=output --jq '.name'
```

Expected: `github-snake.svg`

- [ ] **Step 4: Visual verification**

Open `https://github.com/Kairyx-dev` in a browser and verify:

1. Typing SVG animates through all 5 lines
2. 11 tech badges render with logos and correct colors
3. GitHub Stats and Top Languages cards sit side-by-side
4. Streak Stats card shows current streak
5. Trophy row renders with Tokyo Night theme
6. Activity Graph shows contribution heatmap
7. Snake animation is visible (not a broken image)
8. Footer link to kairyx-dev.github.io is clickable
9. No Korean text, no email, no company name anywhere

---

## Acceptance Criteria Mapping

| Criterion (from spec) | Task |
|---|---|
| README renders eight sections in order | Tasks 2–5 |
| All widgets use Tokyo Night theme | Tasks 2–5 (verified in steps) |
| Typing SVG cycles five lines | Task 2 |
| Tech badges in three grouped rows | Task 2 |
| Stats + Langs side-by-side on desktop | Task 3 |
| Snake workflow exists and runs | Tasks 1, 6 |
| Snake SVG reachable from output branch | Task 6 |
| Footer links to kairyx-dev.github.io | Task 5 |
| No company/email/Korean text | Task 6 (visual check) |
