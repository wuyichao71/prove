# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

`prove` is a collection of **physics/math proof write-ups**, transcribed from the user's
handwritten notes (usually shared as photos) into clean Markdown + LaTeX. It is a documentation
repository — there is no build system, no application code, and no test/lint/run commands. Files
are plain Markdown rendered by any KaTeX-compatible viewer (VS Code preview, Obsidian, GitHub).

Current content: `liouville-theorem.{zh,en}.md` — Liouville's theorem proved two ways
(continuity-equation method; volume invariance under canonical transformations).

## Authoring conventions

- **Bilingual = two separate complete files, not interleaved.** When a document is requested in
  中英双语 / bilingual, produce one full Chinese file and one full English file
  (`<name>.zh.md` / `<name>.en.md`) with **parallel structure, section numbering, and equations** —
  each independently readable. Do not mix Chinese prose with English terms in a single file.
- **Math** uses KaTeX-compatible LaTeX: inline `$...$`, display `$$...$$`. Verify it renders
  (no unsupported macros) before considering a document done.
- **Faithful to the notes.** Reconstruct each algebra step from the source notes; fill in
  skipped or crossed-out steps into a clean, complete chain. Annotate which handwritten figure
  each proof/section corresponds to so the user can cross-check.
- Cross-reference the sibling-language file at the bottom of each document.

## memory/ directory

`memory/` holds the harness's structured cross-session memory (`MEMORY.md` index + one file per
fact). It is **not** part of the proof deliverables and is separate from this CLAUDE.md — do not
treat its contents as project documentation to publish.

## Git

Work happens on `claude/*` branches and is fast-forward merged into `main`. `main` is checked out
in the primary worktree; feature work may run in a `.claude/worktrees/` worktree of the same repo.
