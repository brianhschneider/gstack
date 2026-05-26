---
name: art-direction
preamble-tier: 2
version: 0.1.0
description: |
  Art direction wizard for consumer apps. Guides a creator through brief →
  style → asset spec → Replicate prompt engineering → selection → test plan.
  Reads BRAND-DNA.md and the project's STYLE-GUIDE.md as context.
  Saves a completed brief + prompt configs to the project.
  Forward-compatible with /game-design output (takes game_spec JSON as Stage 1 input).
allowed-tools:
  - Bash
  - Read
  - Write
  - Edit
  - WebSearch
  - AskUserQuestion
triggers:
  - art direction
  - make assets
  - brand assets
  - design assets
  - replicate prompts
  - style guide
  - visual identity
---

## Preamble (run first)

```bash
_UPD=$(~/.claude/skills/gstack/bin/gstack-update-check 2>/dev/null || true)
[ -n "$_UPD" ] && echo "$_UPD" || true
mkdir -p ~/.gstack/sessions
touch ~/.gstack/sessions/"$PPID"
_BRANCH=$(git branch --show-current 2>/dev/null || echo "unknown")
echo "BRANCH: $_BRANCH"
_TEL=$(~/.claude/skills/gstack/bin/gstack-config get telemetry 2>/dev/null || true)
_TEL_START=$(date +%s)
_SESSION_ID="$$-$(date +%s)"
echo "TELEMETRY: ${_TEL:-off}"
mkdir -p ~/.gstack/analytics
if [ "$_TEL" != "off" ]; then
  echo '{"skill":"art-direction","ts":"'$(date -u +%Y-%m-%dT%H:%M:%SZ)'","repo":"'$(basename "$(git rev-parse --show-toplevel 2>/dev/null)" 2>/dev/null || echo "unknown")'"}'  >> ~/.gstack/analytics/skill-usage.jsonl 2>/dev/null || true
fi
eval "$(~/.claude/skills/gstack/bin/gstack-slug 2>/dev/null)" 2>/dev/null || true
GSTACK_SLUG="${SLUG:-unknown}"
echo "GSTACK_SLUG: $GSTACK_SLUG"
~/.claude/skills/gstack/bin/gstack-timeline-log '{"skill":"art-direction","event":"started","branch":"'"$_BRANCH"'","session":"'"$_SESSION_ID"'"}' 2>/dev/null &
```

---

## Stage 0 — Context Load

Before asking anything, read the available brand and style context.

```bash
# Brand DNA (cross-app)
BRAND_DNA="$HOME/.gstack-dev/plans/brand/BRAND-DNA.md"
[ -f "$BRAND_DNA" ] && echo "BRAND_DNA: found" || echo "BRAND_DNA: missing"

# Experience Grammar (shared interaction patterns)
EXP_GRAMMAR="$HOME/.gstack-dev/plans/brand/EXPERIENCE-GRAMMAR.md"
[ -f "$EXP_GRAMMAR" ] && echo "EXPERIENCE_GRAMMAR: found" || echo "EXPERIENCE_GRAMMAR: missing"

# Art Style Library
ART_STYLES_DIR="$HOME/.gstack-dev/plans/brand/art-styles"
if [ -d "$ART_STYLES_DIR" ]; then
  STYLE_COUNT=$(ls "$ART_STYLES_DIR"/*.md 2>/dev/null | grep -v README | wc -l | tr -d ' ')
  echo "ART_STYLES: $STYLE_COUNT styles in library"
  ls "$ART_STYLES_DIR"/*.md 2>/dev/null | grep -v README | while read f; do
    NAME=$(grep '^name:' "$f" | head -1 | sed 's/name: //')
    STATUS=$(grep '^status:' "$f" | head -1 | sed 's/status: //')
    echo "  STYLE: $NAME ($STATUS)"
  done
else
  echo "ART_STYLES: library not found"
fi

# Project style guide
REPO_ROOT=$(git rev-parse --show-toplevel 2>/dev/null || echo "")
STYLE_GUIDE=""
[ -n "$REPO_ROOT" ] && [ -f "$REPO_ROOT/docs/STYLE-GUIDE.md" ] && STYLE_GUIDE="$REPO_ROOT/docs/STYLE-GUIDE.md"
[ -n "$STYLE_GUIDE" ] && echo "STYLE_GUIDE: found at $STYLE_GUIDE" || echo "STYLE_GUIDE: missing"

# Existing briefs in this project
BRIEF_DIR=""
[ -n "$REPO_ROOT" ] && BRIEF_DIR="$REPO_ROOT/docs/briefs"
[ -d "$BRIEF_DIR" ] && echo "BRIEFS: $(ls -t $BRIEF_DIR/*.md 2>/dev/null | wc -l | tr -d ' ') existing" || echo "BRIEFS: 0"

# Draft session files
PROJ_DIR="$HOME/.gstack/projects/${GSTACK_SLUG:-unknown}"
mkdir -p "$PROJ_DIR"
DRAFTS=$(ls -t "$PROJ_DIR"/art-direction-draft-*.md 2>/dev/null)
DRAFT_COUNT=$(echo "$DRAFTS" | grep -c '.md' 2>/dev/null || echo 0)
echo "DRAFT_COUNT: $DRAFT_COUNT"
[ "$DRAFT_COUNT" -gt 0 ] && echo "$DRAFTS" | head -3 | while read f; do
  TITLE=$(grep '"campaign"' "$f" 2>/dev/null | head -1 | sed 's/.*"campaign": *"\([^"]*\)".*/\1/' || echo "untitled")
  MTIME=$(stat -f "%Sm" -t "%Y-%m-%d %H:%M" "$f" 2>/dev/null || date -r "$f" "+%Y-%m-%d %H:%M" 2>/dev/null || echo "unknown")
  echo "DRAFT: $f | campaign=$TITLE | date=$MTIME"
done
```

**After running:**

1. If `BRAND_DNA: found`, read it with the Read tool. Pay particular attention to the **Invariants** table and **Market Research Grounding** section — these inform every stage.
2. If `EXPERIENCE_GRAMMAR: found`, read it. Know the six patterns (Reveal, Assembly, Collect, Present, Confirm, Anticipate) — you'll reference them in Stage 1 and Stage 6.
3. If `ART_STYLES` shows styles in library, note the names and statuses — you'll offer them as named choices in Stage 4 instead of generating prompts from scratch.
4. If `STYLE_GUIDE: found`, read it with the Read tool. This is the primary per-app style reference.
5. If BRAND_DNA and STYLE_GUIDE are both missing, note this — you'll help the creator establish both during the session.
6. If `DRAFT_COUNT > 0`, offer to resume (same pattern as /game-design resume detection).

If a `/game-design` output file exists in the project (look for `game-design-*.md` in the project slug dir):

```bash
ls -t "$HOME/.gstack/projects/${GSTACK_SLUG:-unknown}"/game-design-*.md 2>/dev/null | head -3
```

If found, offer: "I see a game design doc for [title]. Want to use it as the brief for Stage 1? That connects your game design directly to art direction."

---

## Navigation and Skip Rules

Same as /game-design. **SKIP** = set field to `"TBD"` and continue. **GO BACK** = re-run the stage, preserve later fields. Draft file is the authoritative state.

---

## Stage 1 — The Brief

**Framing (say this):** "Before generating anything, we need a brief — who this is for, what they should feel, and what they should do. Good prompts come from good briefs. Weak briefs produce generic assets. Let's be specific."

**Read the draft file if it exists. If `project` and `audience` are set (not TBD), skip to Stage 2.**

**If a game-design doc was offered in Stage 0 and accepted:** Read it, extract `title`, `target_audience`, `golden_feeling`, and `tagline`. Pre-fill the brief fields below and confirm with the user before continuing.

Ask:

> **Stage 1 of 6: The Brief**
>
> What is this asset set for?

Options:
- A) App — screens, UI components, product moments
- B) Marketing — social, email, ads, landing page
- C) Physical product — box art, card print, merch
- D) Full set — app + marketing + physical together
- Other (free text)

Ask:

> **The audience:** Who is the specific person this is for? Not "everyone" — the one person most likely to respond to this. Age range, situation, what they already believe about this type of product.
>
> (Free text. If you have a psychographic segment from market research, name it.)

Ask:

> **The feeling:** When they see this for the first time, what should they feel? Give me 3 words.
>
> (Free text — e.g., "Exclusive. Theatrical. Surprised." or "Warm. Nostalgic. Safe.")

Ask:

> **The moment:** Describe the scene we're designing toward — the specific thing this person does or says when the asset lands perfectly. Not a metric. A moment.
>
> (Free text — e.g., "They show the character card to their partner and say 'oh my god that's actually Margaret'")

Ask:

> **The action:** What single thing should they do after encountering this asset?
>
> (Free text — e.g., "Click 'Order kit'", "Share to Instagram Stories", "Sign up for launch notification")

Ask:

> **Campaign name:** Short slug for this asset set — used in filenames and Discord. Lowercase, hyphens.
>
> (Free text — e.g., "kit-launch-v1", "character-card-refresh", "kickstarter-hero")

**Synthesis:** Generate a `brief_summary` sentence: "[Campaign] is for [audience] who should feel [3 words] when they [action]."

**Write to draft file** at `~/.gstack/projects/{GSTACK_SLUG}/art-direction-draft-{campaign}-{datetime}.md`.

**Also write the brief to the project** at `{REPO_ROOT}/docs/briefs/{campaign}-brief.md` using the BRIEF-TEMPLATE.md format. Fill Sections 1–3 from the answers above.

**Transition:** "Brief locked. Now let's define the style."

---

## Stage 2 — Style Direction

**Framing (say this):** "Style direction is the aesthetic contract — what does this look like, what does it borrow from, what does it explicitly reject? We're looking for a named archetype and 2-3 reference points. These anchor every prompt we write."

**Re-read the draft file. If `aesthetic_archetype` is set (not TBD), skip to Stage 3.**

If a STYLE-GUIDE.md was found in Stage 0, read the "Aesthetic archetype," "Competitive References," and "Replicate Style Anchor" sections. Pre-fill suggestions and confirm with the user.

Ask:

> **Stage 2 of 6: Style Direction**
>
> Choose an aesthetic archetype for this campaign. (If the STYLE-GUIDE.md already defines one, confirm it or override for this specific campaign.)

Options:
- A) Dark theatrical luxury — near-black, warm gold, collectible objects, premium staging
- B) Warm collectible craft — earth tones, handmade feel, artisanal texture, warmth
- C) Playful maximalist — bold color, pattern, energy, joyful density
- D) Clean minimal premium — white space, single object, Hermès-level restraint
- Other (free text — name your archetype)

Ask:

> **References:** Name 2 brands or campaigns that represent the aesthetic direction for this asset set. What specifically should we borrow from each?
>
> (Free text — e.g., "Theory11 playing cards: the single-card hero photography and collector's box. Criterion Collection: the serialization and curatorial authority.")

Ask:

> **Hard avoids:** What 2 things should this explicitly NOT look like? What aesthetic would betray the brief?
>
> (Free text — e.g., "Stock photography. SaaS dashboard blues.")

Ask:

> **Existing design system:** Are there specific colors, fonts, or CSS patterns from this project's style guide that must carry through? Or are we exploring something new?
>
> Options: A) Carry existing system through (read STYLE-GUIDE.md) / B) Explore new direction / C) Partial — specify what carries and what's open

**Synthesis:** Generate a `style_anchor` — a single Replicate-ready style description (70–120 words) that captures the aesthetic. This goes into every prompt for this campaign. If a STYLE-GUIDE.md Replicate Style Anchor exists, extend it; don't replace it.

Example format:
```
cinematic candlelit noir photography, warm amber and deep shadow tones, rich blacks,
luxurious textures, elegant theatrical staging, sharp detail on subject, moody
atmospheric light, no text overlays, film-quality depth of field, [archetype-specific additions]
```

**Write to draft file:** `aesthetic_archetype`, `style_anchor`, `references`, `avoids`.
**Update brief file Sections 4:** Fill style direction.

**Transition:** "Style anchor locked: [style_anchor]. Now let's spec the assets."

---

## Stage 3 — Asset Specification

**Framing (say this):** "We're not trying to generate everything today. Three assets done well beat ten assets done poorly. Let's pick the highest-leverage three for this brief."

**Re-read the draft file. If `asset_spec` is set (not TBD), skip to Stage 4.**

If a STYLE-GUIDE.md was found, read its "Asset Types (priority order)" section. Offer it as the starting checklist.

Present the full asset menu (organize by category the user chose in Stage 1):

**App assets:** Character portrait card, loading/reveal state, cast gallery, review hero, empty state, onboarding screen, icon set, app icon

**Marketing assets:** Social card 1:1, social card 16:9, OG image 1200×630, email header, landing page hero, press kit hero, product mockup, campaign banner

**Physical product assets:** Box cover art, card print-ready file (character card), flat-lay product photography, Kickstarter header, sell sheet, rulebook diagram

Ask:

> **Stage 3 of 6: Asset Spec**
>
> From the list above, which 3 assets are highest priority for this brief? (Pick the ones where a great asset most directly drives the outcome from Stage 1.)
>
> (Free text — name 3, we'll spec the rest as backlog)

For each of the 3 selected assets, ask:

> **[Asset name]:** What are the format requirements?
> - Dimensions?
> - File format (PNG / SVG / PDF / print-ready)?
> - Screen (RGB) or print (CMYK)?
> - Any required text / logo / legal?
> - Any specific constraints (mobile-first, must work at 50% size, etc.)?

**Write to draft file:** `asset_spec` array with name, dimensions, format, surface, constraints.
**Update brief file Section 5:** Fill asset specification table.

**Transition:** "Three assets scoped. Let's engineer the prompts."

---

## Stage 4 — Prompt Engineering + Generation

**Framing (say this):** "I'll write a Replicate prompt for each asset using the brief and style anchor. These are designed to be run as a batch — 8-12 variants per asset type. Consistency across the set matters more than any single perfect image."

**Re-read the draft file. If `prompt_configs` is set (not TBD), skip to Stage 5.**

For each asset in `asset_spec`, generate:
1. A **primary prompt** — specific to this asset, incorporating the style anchor
2. A **negative prompt** — what to exclude (based on "hard avoids" from Stage 2)
3. **Recommended model** — from the list below
4. **Batch size** — 8 variants unless the user specifies otherwise

### Portrait / Likeness Assets — Use the Art Style Library

**For any asset involving a person's face, always choose from the named styles in the art-styles catalog.** Do not generate portrait prompts from scratch — tested styles with known identity fidelity exist.

Read the matching style file from `~/.gstack-dev/plans/brand/art-styles/` and use its exact prompt, model, and parameters.

| Style | File | Identity | Best for | Watch out for |
|-------|------|----------|---------|---------------|
| Noir Detective | `noir-detective-v1.md` | 4/5 | Dossier inserts, header art, B&W character sheets | Defaults to B&W photorealistic (not illustrated). Heavy shadow + B&W reads **sad and old**, not theatrical and fun. For warm, playful murder mystery vibes, use Art Nouveau or Art Deco instead. Only use Noir when the B&W press-photo look is intentional. |
| Victorian Engraving | `victorian-engraving-v1.md` | 4/5 | murder-at-yours dossier docs, archival apps | |
| Art Nouveau | `art-nouveau-v1.md` | 4/5 | murder-at-yours portrait cards (confirmed winner), elegant apps | Cream backgrounds are the wrong execution — prompt must explicitly specify dark background and jewel-tone palette. |
| Classical Oil Portrait | `oil-portrait-v1.md` | 5/5 | Universal premium — works for all apps | |
| Graphic Novel | `graphic-novel-v1.md` | 4/5 | TCG/games, younger audiences, modern apps | |

**On Noir for party/social apps:** Standard noir direction (heavy shadow, B&W, brooding) is the wrong emotional register for apps where the goal is fun, flirty, or celebratory. If the brief calls for upbeat energy, steer toward Art Nouveau (warm gold, painterly, collectible), Art Deco (bold geometric, gold on black, confident), or Pulp (energetic, color, kinetic). Reserve Noir for dossier/document surfaces where a moodier tone is appropriate.

**Styles to never use for likeness:** Anime/manga (~1/5 identity), impressionist (~2/5), cubist (~1/5), heavy watercolor (~2/5).

Ask:

> **Which portrait style for this brief?**
> [List the approved styles with their identity score and app fit from the catalog]
>
> Or: use a style not in the catalog yet — I'll generate a new prompt, but it won't have tested identity scores. Add it to the catalog after testing.

### Non-Portrait Asset Model Selection

| Asset type | Recommended Replicate model | Notes |
|-----------|----------------------------|-------|
| Marketing photography / scene | `black-forest-labs/flux-pro` | Best photorealistic quality |
| Fast iteration / concept | `black-forest-labs/flux-schnell` | 4-step, cheap, good for batching |
| Product mockups / lifestyle shots | `black-forest-labs/flux-pro` | |
| Illustrated / artistic (no face) | `stability-ai/sdxl` | |
| Logo / icon | Human designer or vector — do NOT use diffusion | Diffusion models don't produce clean vectors |

### Global Negative Prompts — Always Applied

**Every prompt config, regardless of asset type or style, must include this base negative string.** Append it to all negative prompts before saving configs — never omit it:

```
no text, no words, no letters, no writing, no labels, no typography, no brand names,
no watermarks, no signatures, no captions, no titles, no subtitles, no inscriptions
```

**Why this is non-negotiable:** Diffusion models hallucinate plausible-looking text whenever a prompt touches objects that typically carry text (envelopes, boxes, dossiers, wax seals, emblems, bottles, books). The hallucinated text is always wrong — either gibberish or an existing brand name — and fixing it after generation costs a full re-run. The negative is cheap and prevents the problem entirely.

For circular emblem / seal designs specifically, also add:
```
no circular text, no ring of letters, no text around border, no inscribed motto
```
Circular text around emblems is the hardest hallucination to suppress — even with strong negatives, faint ring text sometimes appears. If it persists after two re-prompts, composite a clean vector logo over the seal in production rather than fighting the model further.

Ask:

> **Stage 4 of 6: Prompt Review**
>
> I've written a prompt set for your [N] assets. Review and approve before we run:
>
> [Show each asset with its prompt, negative prompt, model, and batch size]
>
> Anything to change, or run as-is?

Options: A) Run as-is / B) Adjust — let user specify changes / C) Let me write my own prompts

After approval, output the prompt configs as a JSON block AND as ready-to-run Replicate CLI commands:

```bash
# Asset 1: [asset name]
# Model: [model ID]
# Batch: run this 8 times (vary seed each time)
replicate run [model-id] \
  prompt="[prompt]" \
  negative_prompt="[negative_prompt]" \
  width=[width] height=[height] \
  seed=$RANDOM
```

**Save configs to two places:**
1. Draft file: `prompt_configs` field
2. Project: `{REPO_ROOT}/docs/briefs/{campaign}-prompt-configs.json`

Tell the user: "Run these in your terminal, or paste into Replicate's web UI. Save all outputs to `docs/assets/{campaign}/raw/` — we'll select from them in Stage 5."

**Important:** Do not automatically call Replicate or any external API. The user runs the generation; this skill produces the prompts and configs they need.

**Transition:** "Configs saved. Run the generation and come back when you have variants to review."

If the user has already run generation and has variants to show, skip straight to Stage 5.

---

## Stage 5 — Selection + Critique

**Framing:** "Good selection requires honesty. We're measuring each variant against the brief — does it produce the feeling, does it serve the audience, does it drive the action? Not 'is it pretty.'"

**Re-read the draft file. If `selected_assets` is set (not TBD), skip to Stage 6.**

Ask the user to share the variants (filenames or describe what they generated).

For each asset type, provide a critique framework based on the brief:

> **Critique for [asset name]:**
> Brief criteria: [feeling] / [audience signal] / [action it should drive]
>
> For each variant, assess:
> 1. **Feeling** (0-3): Does it produce [the 3 words from Stage 1]?
> 2. **Audience signal** (0-3): Would [the specific person from Stage 1] recognize this as made for them?
> 3. **Action pull** (0-3): Does it make someone want to [action from Stage 1]?
> 4. **Style fit** (0-3): Does it match the style anchor and STYLE-GUIDE.md?
>
> Total /12. Anything above 9 is worth selecting. Below 6 means re-prompt.

After critique, ask:

> Which variant(s) are you selecting? (Can select one per asset, or multiple if running A/B)

**Write to draft file:** `selected_assets` with filename, scores, and brief rationale.
**Update brief file "Selection Notes" section.**

**If no variant scores above 6, OR the user wants to improve a specific asset:**

### Stage 5b — Reprompt Loop

The gallery at `{REPO_ROOT}/docs/assets/{campaign}/gallery.html` is the visual review surface. Tell the user to open it, look at the cards, and point at what's wrong. Then diagnose and fix.

**Failure → Fix mapping:**

| Criterion failing | What it usually means | Prompt fix |
|---|---|---|
| **Feeling** | Wrong emotional register, wrong palette temperature | Adjust style descriptors — warmer/cooler tones, energy words, mood references |
| **Audience signal** | Too generic, could be for anyone | Add specificity to staging, props, or era that speaks to *this* audience |
| **Action pull** | No focal point, nothing desirable in frame | Add a hero object, strengthen the composition anchor, increase drama of the reveal |
| **Style fit** | Drifted from brief archetype | Re-read the style_anchor and check which terms are missing from the prompt |

**Process:**
1. User identifies the failing card(s) in the gallery
2. Pull the original prompt for that asset from `{campaign}-prompt-configs.json`
3. Identify the failing criterion and apply the matching fix above
4. Generate a revised prompt — show the before/after diff so the user sees what changed
5. Save revised config to `{campaign}-prompt-configs-v{N+1}.json` (don't overwrite — preserve history)
6. Re-run only the failing asset — not the full batch

**Re-run until the asset scores ≥ 9/12 or the user decides to move on.** Three re-prompt rounds is the limit before escalating: if something still isn't landing after 3 rounds, the issue is usually the brief (wrong emotional target or wrong style archetype), not the prompt.

**Transition:** "Assets selected. Let's build the test plan."

---

## Stage 6 — Test Plan

**Framing:** "Assets without a test plan are opinions. A test plan makes them evidence. This doesn't need to be complex — just specific enough that someone can report results without ambiguity."

**Re-read the draft file. If `test_plan` is set (not TBD), complete the session.**

Ask:

> **Stage 6 of 6: Test Plan**
>
> Where are these assets going first? (Multi-select OK)

Options:
- A) In the app — an A/B test or direct ship
- B) Marketing channel — social, email, ads
- C) Physical — sending to printer for a proof
- D) Internal review only — not shipping yet

For each selected surface, ask the minimum viable questions:

**If A (in-app):**
> Which screens? What's variant A vs. B? What metric tells you it worked? How long do you run it?

**If B (marketing):**
> Which channel? What's the success metric (CTR, click rate, saves, purchases)? When do you decide?

**If C (physical):**
> Who reviews the proof? What's the go/no-go criteria? By when?

**Write to draft file:** `test_plan` with surfaces, metrics, timeline.
**Update brief file "Test Plan" section.**

### Generate Survey + Gallery (always — not optional)

Regardless of which surfaces the user selected, always generate two HTML files as part of Stage 6 completion:

**1. Gallery** at `{REPO_ROOT}/docs/assets/{campaign}/gallery.html`

The gallery is the visual review surface used throughout Stage 5b. If it doesn't exist yet, generate it now from the selected_assets in the draft file. Structure:
- Dark background matching the project's STYLE-GUIDE.md palette (default: `#0f0e0c` bg, `#C9A84C` gold, `#F5EDD8` text)
- All portrait style variants in a 6-up comparison strip at the top (even single-variant styles)
- Individual sections per style below, with the confirmed winner styled with a gold border glow
- Scene assets in a 2-column grid — production-ready only (no "replaced" originals)
- CSS foil shimmer on card hover (::before gradient translateX)
- Status badges: "Winner" (gold), "Select" (muted gold), "Re-prompt" (amber)

**2. Survey** at `{REPO_ROOT}/docs/assets/{campaign}/survey.html`

The survey is how you collect team votes before committing to a portrait style. Generate it as a self-contained HTML file (no server required — submits by copying results to clipboard). Structure:

- **Q1: Best portrait** — image card grid, one per portrait style variant. Cards are selectable via `<input type="radio">` + CSS. Show all portrait variants from `selected_assets`.
- **Q2: Rate all styles** — one row per portrait style, 5-star rating using the CSS row-reverse hover trick (no JS required).
- **Q3: Strongest scene asset** — image card grid, one per scene asset.
- **Q4: What's missing** — `<textarea>` for open feedback.
- **Q5: Name** — text input (optional).
- **Submit** — formats results as a Discord-paste-ready block, copies to clipboard via `navigator.clipboard.writeText()`, shows a "Copied!" confirmation.

Use the project palette throughout. Images load from `raw/portraits/` and `raw/scenes/` via relative paths — the survey must be served alongside the assets folder to work (it won't open as a standalone file).

**How to share the survey with your team:**

The survey references images via relative paths, so it needs to be served with the assets folder alongside it. Two options:

- **Netlify Drop** (fastest, no account needed): Go to `netlify.com/drop` and drag the entire `docs/assets/{campaign}/` folder onto the page. You get a live shareable URL in ~10 seconds.
- **GitHub Pages** (permanent URL): Commit and push `docs/assets/` to the branch. Enable Pages in repo Settings → Pages → Source: your branch, folder: `/docs`. URL becomes `{username}.github.io/{repo}/assets/{campaign}/survey.html`.

Tell the user both options and suggest Netlify Drop for immediate sharing.

**Post brief to Discord reminder:** "Share the survey to #briefs and post results to #results after [date]."

---

## Session Complete — Outputs

Summarize what was produced:

```
✓ Brief saved:      {REPO_ROOT}/docs/briefs/{campaign}-brief.md
✓ Prompt configs:   {REPO_ROOT}/docs/briefs/{campaign}-prompt-configs.json
✓ Draft file:       ~/.gstack/projects/{GSTACK_SLUG}/art-direction-draft-{campaign}-{datetime}.md
✓ Style guide:      {REPO_ROOT}/docs/STYLE-GUIDE.md (updated if changed)
✓ Gallery:          {REPO_ROOT}/docs/assets/{campaign}/gallery.html
✓ Survey:           {REPO_ROOT}/docs/assets/{campaign}/survey.html
```

If no STYLE-GUIDE.md existed before this session, offer to create one now:
> "You don't have a STYLE-GUIDE.md yet. Want me to generate one from this session's style direction? It'll serve as the starting reference for all future /art-direction runs on this project."

If yes: generate STYLE-GUIDE.md using the BRAND-DNA.md template structure, populated from this session's Stage 2 answers.

---

## Draft File Format

```markdown
---
skill: art-direction
project: [project name]
campaign: [campaign slug]
created: [datetime]
---

## Brief
- project: "[value or TBD]"
- campaign: "[value or TBD]"
- asset_type: "[app|marketing|physical|full or TBD]"
- audience: "[value or TBD]"
- feeling: "[3 words or TBD]"
- the_moment: "[value or TBD]"
- action: "[value or TBD]"
- brief_summary: "[synthesized sentence or TBD]"

## Style
- aesthetic_archetype: "[value or TBD]"
- style_anchor: "[Replicate-ready description or TBD]"
- references: "[value or TBD]"
- avoids: "[value or TBD]"

## Assets
asset_spec:
  - name: "[value or TBD]"
    dimensions: "[value or TBD]"
    format: "[value or TBD]"
    surface: "[value or TBD]"
    constraints: "[value or TBD]"

## Prompts
prompt_configs:
  - asset: "[value or TBD]"
    model: "[value or TBD]"
    prompt: "[value or TBD]"
    negative_prompt: "[value or TBD]"

## Selection
selected_assets:
  - asset: "[value or TBD]"
    file: "[value or TBD]"
    scores: {feeling: 0, audience: 0, action: 0, style: 0}
    rationale: "[value or TBD]"

## Test Plan
test_plan:
  - surface: "[value or TBD]"
    metric: "[value or TBD]"
    timeline: "[value or TBD]"
    reviewer: "[value or TBD]"
```

---

## Telemetry

```bash
_TEL_END=$(date +%s)
_TEL_DUR=$(( _TEL_END - _TEL_START ))
~/.claude/skills/gstack/bin/gstack-timeline-log '{"skill":"art-direction","event":"completed","branch":"'$(git branch --show-current 2>/dev/null || echo unknown)'","outcome":"success","duration_s":"'"$_TEL_DUR"'","session":"'"$_SESSION_ID"'"}' 2>/dev/null || true
if [ "$_TEL" != "off" ]; then
  echo '{"skill":"art-direction","duration_s":"'"$_TEL_DUR"'","outcome":"success","session":"'"$_SESSION_ID"'","ts":"'$(date -u +%Y-%m-%dT%H:%M:%SZ)'"}' >> ~/.gstack/analytics/skill-usage.jsonl 2>/dev/null || true
fi
```
