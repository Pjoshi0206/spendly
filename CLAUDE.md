# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

Spendly — a personal expense tracker being built as a step-by-step learning project (Flask backend, server-rendered Jinja templates, vanilla JS/CSS, SQLite). Many pieces are intentionally unfinished placeholders (see `app.py` and `database/db.py`) that get filled in as later "steps" of the tutorial — don't be surprised to find routes that just return a string like `"Logout — coming in Step 3"`, or a `database/db.py` with only a docstring-style comment describing functions (`get_db()`, `init_db()`, `seed_db()`) that haven't been written yet.

## Commands

```bash
source venv/bin/activate                 # activate the existing venv (don't recreate it)
pip install -r requirements.txt          # install/sync deps (Flask, Werkzeug, pytest, pytest-flask)
flask --app app run --port 5001          # run the dev server (or: python app.py, which also uses debug=True, port 5001)
pytest                                   # run tests (pytest-flask is installed but no test files exist yet)
```

There is no build step, linter, or frontend bundler — templates and CSS/JS are served directly by Flask via `static/` and `templates/`.

## Architecture

- **`app.py`** — single-file Flask app; all routes live here (no blueprints). Real routes (`/`, `/register`, `/login`, `/terms`, `/privacy`) render templates; placeholder routes (`/logout`, `/profile`, `/expenses/add`, `/expenses/<id>/edit`, `/expenses/<id>/delete`) are stubs awaiting later implementation.
- **`database/db.py`** — intended home for all SQLite access (`get_db()`, `init_db()`, `seed_db()`), not yet implemented. The DB file (`expense_tracker.db`) is gitignored and created locally.
- **`templates/base.html`** — the shared layout (nav, footer, Google Fonts, `static/css/style.css`, `static/js/main.js`). Every page template extends it via `{% extends "base.html" %}` and fills `{% block title %}`, `{% block content %}`, and optionally `{% block head %}` / `{% block scripts %}` for page-specific CSS/JS.
- **`static/css/style.css`** — the *only* stylesheet in the project (there is no `landing.css` or per-page CSS despite what a casual request might assume — check this file first before creating a new one). It's organized in commented sections (Variables, Reset, Navbar, Hero, Buttons, Modal, Features, Auth, Responsive, …) using CSS custom properties defined in `:root` (colors, fonts, radii) — reuse those variables rather than hardcoding values.
- **`static/js/main.js`** — shared site-wide JS, currently just a placeholder comment. Page-specific JS (e.g. a modal that only exists on the landing page) is written inline in that template's `{% block scripts %}` rather than added to `main.js`, so it isn't loaded on every page.
- Page-specific interactive elements (e.g. the landing-page "see how it works" modal) are scoped with their own CSS classes/JS rather than reusing global component classes like `.btn-primary`, to avoid unintentionally restyling other pages that share `style.css`.
