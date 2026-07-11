---
name: digital-garden
description: Turn raw voice-dump transcripts (from Claude Code voice mode or the Claude web/app voice feature) into personal digital-garden essays — dated, first-person, cross-linked — and publish them into a lightweight React+Vite personal site. Bootstraps a new site from a bundled template if none exists yet, or slots into an existing digital-garden-style repo. Use when the user has voice memos, rambling transcripts, or stream-of-consciousness dumps they want turned into a personal site or blog. Triggers: digital garden, voice dump to site, personal site from transcript, turn my voice notes into a blog, build my digital garden.
---

# Digital Garden Skill

A voice dump is raw material, not a draft. This skill's job is judgement: figure out which parts of a rambling transcript are actually an essay, write that essay in the person's own voice without inventing anything they didn't say, and slot it into a real site that looks like a personal digital garden — not a corporate blog.

Reference implementation this skill is modeled on: [Sai Yashwanth's digital garden](https://saiyashwanth.com) (source in `~/Dev/DigitalGarden`). Read a few of its `public/content/*.md` files if you want to recalibrate voice before writing — short, personal, dated, unpolished-but-clear, heavy on cross-linking to the author's own past pieces.

## Inputs

Ask for (or locate in the working directory):

1. **The voice-dump transcript(s)** — raw text from Claude Code voice mode, the Claude web/app voice feature, or any other dictation. Expect filler words, tangents, repetition, and multiple unrelated ideas mixed into one dump. Do not clean this up before reading it fully.
2. **The target site** — where the essays should live. If unspecified, use the current working directory.
3. **Identity basics**, only if bootstrapping a brand-new site (name, one-line bio, social links). Don't ask for these if the target site already exists — read them from its `public/static/about.md` instead.

## Process

### Step 1 — Detect: existing garden or bootstrap new

Check the target directory for `public/content/` and `public/static/article.md`.

- **Exists** → this is an existing digital garden. Work inside it, matching its established conventions (read 2-3 existing `public/content/*.md` files and the `article.md` index to learn its section names, dating style, and voice before writing anything new).
- **Doesn't exist** → copy this skill's `template/` directory into the target, then fill in the placeholders (`__SITE_NAME__`, `__TAGLINE__`, `__GITHUB_URL__`, `__TWITTER_URL__`, etc. — grep for `__` to find them all) using the identity basics gathered above. Tell the user to run `npm install && npm run dev` to preview once content is in.

### Step 2 — Segment the dump into essay-worthy ideas

Read the whole transcript before touching it. A single dump often contains zero, one, or several distinct essays. For each candidate:

- It needs a real point — an opinion, a lesson learned, an event, a reflection. "Testing testing" or a dump that trails into someone's grocery list is not an essay; discard it.
- Don't force one transcript into exactly one essay. Split cleanly if the person jumped topics; merge if they circled back to the same point twice.
- If nothing in the dump clears the bar, say so — don't manufacture an essay out of thin material.

### Step 3 — Write each essay in the site's voice, not a generic one

Rules, non-negotiable:

- **First person, no invention.** Every claim, number, event, or opinion must trace back to something actually said in the transcript. Rambling can be restructured into coherent paragraphs; content cannot be added. If the person trails off or contradicts themselves, resolve it the way they clearly meant, or leave a note rather than guessing.
- **Short and direct.** Look at the reference site's essays — most are 300-800 words, plain paragraphs, no bullet-heavy listicle structure unless the person actually spoke in a list.
- **Format** (matches the reference site exactly):
  ```
  # <Title>

  *Written on <Month DD, YYYY>*

  <optional one-line summary/hook, plain or italicized>

  <body paragraphs>
  ```
  Use today's date unless the transcript names a different date.
- **Cross-link.** Scan the existing `article.md` index (or the essays already written this session) for topically related pieces and link to them inline, e.g. `[the last time I wrote about this](./slug)`. This is what makes it a *garden* and not a pile of blog posts.
- **No fabricated media.** Only reference an image if the user actually provides one. Don't invent placeholder screenshots.
- **Title and slug.** Title is short and specific, never clickbait. Slug is kebab-case ASCII derived from the title, matching the flat/foldered convention already used in `public/content/`.

### Step 4 — Save and index

1. Write each essay to `public/content/<slug>.md` (or into a subfolder if it's clearly part of a series, mirroring how the reference site nests `twitter_essays/`, `ai_playbooks/`, etc.).
2. Update `public/static/article.md`: insert a line `*Mon DD, YYYY* __[Title](./slug)__` in reverse-chronological order within the right section. Default to whatever the site's general/miscellaneous section is called (e.g. "Mini Essays and Articles"); only use a "Series" section if this essay is explicitly part of a multi-post arc, and only touch "Pinned" if the user asks.

### Step 5 — Identity pages are curated, not a dumping ground

If the transcript reveals genuine bio or life-event material (a new job, a milestone, a project shipped), it *may* belong on `about.md`, `home.md`, or `timeline.md` — but these pages are the person's curated self-presentation. Propose the addition and show the exact diff; never overwrite these files without the user confirming first.

## Output

- One `.md` file per essay under `public/content/`
- `public/static/article.md` updated with the new entries
- If bootstrapped: a working Vite+React site the user can run with `npm install && npm run dev`
- A short summary to the user: which essays were written, which raw material was discarded and why, and any identity-page changes proposed but not yet applied

---

*From the Turi Labs AI Agent Learn series — [turilabs.in](https://turilabs.in)*
