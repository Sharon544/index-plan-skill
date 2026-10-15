---
name: index-plan
description: Save the current plan as a new artifact page and add it to the project's plan index artifact (creating the index if the project has none). Use when the user invokes /index-plan, or asks to save/publish the plan as an artifact and add it to the index.
---

# index-plan

Keep every plan as its own artifact page, and one index page per project that
lists them all, newest first. Never overwrite an earlier plan's artifact: each plan is
a new page.

## Where things live

Everything is kept outside the session scratchpad, which is deleted when a session ends:

```
~/.claude/plan-artifacts/<project>/
    registry.json   index url + every plan's version, title, url, source file, date
    _style.html     the shared <head> (title placeholder, fonts, CSS tokens, dark mode)
    index.html      source of the index artifact
    plan-v<N>.html  source of each plan artifact
```

`<project>` is the basename of the session's working directory, e.g. `my-app`.

## Steps

1. **Find the current plan.** Use the plan file named in the session's plan-mode
   message (`~/.claude/plans/*.md`). Publish only the **current** plan:
   the top section, not older versions that are still further down the file.
   If there is no plan file, use the plan most recently written in chat.
   If it is unclear which plan is meant, ask.

2. **Read the registry.** If `registry.json` doesn't exist, this project has no index
   yet: go to step 5's "no index" branch after publishing the plan.
   The new version is the highest `version` in the registry plus 1 (or 1 if there is
   no registry).

3. **Write the plan page.** `plan-v<N>.html` = `_style.html` with the `<title>` changed
   to `<Project> Plan v<N>`, then a `<main>` body following the earlier plan pages
   (read the latest `plan-v*.html` for the structure, if there is one):
   - header: eyebrow `<project> · plan v<N> of <date>`, an `h1`, and a one-sentence lede;
   - sections from the plan: context, decisions, answers to the user's questions if any,
     changes or steps, verification;
   - footer: date, the version it follows, and the plan file path.

   Keep it short and scannable: lists over paragraphs, numbered where order matters.
   Load the `artifact-design` skill first if `_style.html` is missing (a new project).

4. **Publish the plan** with the Artifact tool, as a **new** artifact: no `url`, `icon: "plan"`,
   and a one-sentence `description`. Add it to `registry.json` (version, title, url,
   source, date).

5. **Update the index.**
   - **Index exists:** edit `index.html`.
     - Add the new plan as the first `<li class="entry current">`, with meta
       `<date> · current plan`, a link to its URL and a one-line summary.
     - Change the previous current entry to `class="entry"`, with meta
       `<date> · superseded by v<N>`.
     - Then publish with `url` set to the registry's index URL. If this session has
       neither read nor published that index, read it first with `action: "read"`. If
       the live version differs from `index.html` (edited elsewhere), merge into the
       live version.
   - **No index:**
     - Create `index.html` from `_style.html` (create `_style.html` first if needed),
       titled `<Project> plans index`, with a lede and an `.entries` list: one
       `<li class="entry">` per plan holding a small meta line (date and status),
       the plan title as a link, and a one-line summary. Style the `current` entry
       so it stands out (e.g. an accent border).
     - Publish it as a new artifact (`icon: "index"`) and write `registry.json`.
     - Offer once to pin it to the sidebar; pin only if the user says yes.

6. **Trim the plan file.** Leave only the plan just published in the plan file; earlier
   versions are already saved as artifacts. The next plan **replaces** the file and does
   not include this one (v4 must not contain v3).

7. **Reply briefly:** the new plan link and the index link. No recap of the plan itself.

## Notes

- Artifacts are private until the user shares them. Say so only if they mention sending
  a page to someone.
- Don't republish older plan pages, and don't change their content.
