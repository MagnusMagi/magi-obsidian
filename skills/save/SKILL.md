---
name: save
description: Saves the work done in this session to the user's Obsidian vault as notes, or updates an existing note — project decisions and plans, session summaries, research findings — following the vault's own folders, templates and note style. Use when the user says "save to Obsidian", "write this to my vault", "add this to my notes", "take a note of this", or types /save, in any language.
---

# Save to Obsidian

Write durable work into the user's vault as plain markdown, the way the vault's owner would have written it. No tooling, no transaction log, no ledgers: create and update notes directly with Write and Edit.

## Vault

The vault is the folder in `$OBSIDIAN_VAULT` (read it with `echo "$OBSIDIAN_VAULT"`). If it is unset and the current directory is a vault, use that. Otherwise ask the user for the vault's absolute path and suggest setting `OBSIDIAN_VAULT` in the `env` block of `~/.claude/settings.json`. A vault has an `.obsidian/` directory; if the folder has none, write nothing and tell the user in one line that the vault was not found.

## Learn the vault first

Every vault is organized differently. Before writing, find out how this one works, and follow it:

1. **Folders:** list the top-level folders and see where notes like the one you are about to write already live (projects, research, meeting notes…).
2. **Templates:** if the vault has a templates folder, use the matching template. Find it in `.obsidian/templates.json` (`folder`), in Templater's `.obsidian/plugins/templater-obsidian/data.json` (`templates_folder`), or as a folder named `Templates`. Fill in Templater or core-template placeholders yourself (`{{date}}`, `{{title}}`, `<% tp.date.now() %>`…); never leave them unexpanded.
3. **Existing notes:** read one or two notes of the same kind and copy their frontmatter properties, heading names, heading language and date format.
4. **Links:** if `.obsidian/app.json` has `"useMarkdownLinks": true`, write `[Note](Note.md)` links; otherwise `[[Note]]` wikilinks.
5. **Index notes:** if the vault has a home, index or map-of-content note that lists notes like yours, add a line to it.

Fall back to the defaults below only for what the vault does not already settle.

## What to write, what not to

Write:
- **Project decisions and plans:** the chosen path, the options ruled out and why, an approved plan, a spec summary.
- **Session summary:** what was done, what is left, the next step.
- **Research findings:** with their sources, when they are worth coming back to.

Do not write: one-line fixes, approaches tried and dropped, intermediate steps, anything already in the code or git history. Never write passwords, API keys, tokens or personal data.

Write only verified information. Check any detail you are unsure of (screen content, a number, a date) against its source, or leave it out.

## Where to write

1. If the work belongs to a project that already has a note, update that note: find it by its name, folder and `aliases`. Append a dated entry where the note keeps its history, bring any status or task list in it up to date, and update an `updated`-style date property if it has one.
2. If the project has no note yet, create one where the vault keeps project notes.
3. If a decision or topic runs longer than a few paragraphs, give it its own note next to the project note and link it from there. If a note on that topic exists, update it instead of opening a second one.
4. If the destination is unclear, pick the closest match and tell the user where it went.

Always read a note before changing it. Never delete or rewrite lines the user wrote by hand; only append, or update the parts you wrote yourself.

## Journal

After every save, also add the work to today's journal note, so the vault has one chronological record across projects.

1. **Where:** the vault's daily note. Read its folder, date format and template from `.obsidian/daily-notes.json` (`folder`, `format` in Moment.js tokens, `template`) or, if the Periodic Notes plugin is on, from `.obsidian/plugins/periodic-notes/data.json` (`daily`). If neither sets a folder, use `Journal/` with `YYYY-MM-DD.md`.
2. **Create** today's note if it does not exist, from the daily template when there is one, otherwise with a `# YYYY-MM-DD` heading.
3. **Append** under a `## Claude` heading (add the heading once, at the end of the note): one bullet per project or topic you saved, `- HH:MM [[Project]] — <what was done, one line>`. Link the note you wrote to; the details stay there, not in the journal.
4. Skip the journal when the user asks for a single note only, or says not to.

## Defaults for an empty vault

Use these only when the vault has no folders, templates or notes to follow:

- Project notes go in `Projects/<Project>/<Project>.md`, everything else in `Resources/<Title>.md`. Create folders only when writing their first note.
- A project note has a two-or-three-sentence summary, then `## Status`, `## Open tasks` (checkboxes) and `## Log` with `### YYYY-MM-DD <short title>` entries (done, decision and why, next).
- Frontmatter: `created`, `updated` (`YYYY-MM-DD`) and `tags`.

## Writing

- Write in the language the user is speaking, unless the vault's existing notes or headings use another; then match the vault.
- Dates in the vault's format, else `YYYY-MM-DD`. Turn relative phrases like "yesterday" or "last week" into absolute dates.
- Note names are unique within the vault; never create two notes with the same name.

## When done

List every note you created or updated, by its path in the vault, in one line, for example:
"Obsidian → Projects/Atlas/Atlas.md updated, Projects/Atlas/Atlas design decisions.md created, Journal/2026-11-11.md updated."
