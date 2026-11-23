# MealKit documentation

Source of the documentation for the [MealKit](https://github.com/MealKit-Engine/MealKit) engine.
Sphinx + MyST (Markdown). English is the source language; Korean is a gettext translation in `locale/ko/LC_MESSAGES/`.

> **Beta (v0.x).** MealKit is under active development: APIs and file formats may change between versions, and projects are moved forward with the upgrade tool. Feedback and bug reports are welcome.

## Build locally

```bat
python -m venv .venv
.venv\Scripts\python -m pip install -r requirements.txt
.venv\Scripts\python tools\build.py html      :: _build\html\en\index.html, _build\html\ko\index.html
.venv\Scripts\python tools\build.py check --engine ..\MealKit
```

- After editing a page: `tools\build.py update`, then translate the new (empty) and changed (fuzzy) messages in the `.po` files.
- `check` builds both languages with warnings as errors, fails on untranslated, fuzzy or leftover messages,
  and compares `ENGINE_VERSION` with the engine's `VERSION` (MAJOR.MINOR).
- `tools\build.py linkcheck` checks external links.

## Branches

There is only `master`, which describes the current engine (`ENGINE_VERSION` = the engine's current MINOR).
Older versions are not kept; the 0.4 documentation is the tag `docs-0.4`. MealKit stays a 0.x beta, so no per-version branches are planned. Hosting on Read the Docs starts when the repositories are published.

## License

Documentation: [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). Code examples in the documentation: MIT. See `LICENSE.md`.
