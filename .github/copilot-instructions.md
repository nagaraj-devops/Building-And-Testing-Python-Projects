<!-- .github/copilot-instructions.md - guidance for AI coding agents -->
# Repo-specific Copilot Instructions

Purpose: Quickly orient AI coding agents to be productive in this repository.

- **Big picture**: This is a small demo repo for "Building And Testing Python Projects". There is no complex service architecture: the repository contains a simple Python project layout with a CI workflow that runs linting and tests. See `README.md` and the CI workflow at `.github/workflows/build-test-python-test-ci.yml` for the canonical build/test commands.

- **Key files**:
  - `README.md` — high-level repo title (minimal).
  - `.github/workflows/build-test-python-test-ci.yml` — main CI definition; shows exact lint/test commands and the Python matrix (3.8, 3.10).
  - `tests.py` — CI runs `pytest tests.py`; look for it at the repo root. If you change test layout, update the workflow.
  - `requirements.txt` (optional) — CI will install it if present.

- **Build / Test / Lint commands (exact, copyable)**:
  - Install deps: `python -m pip install --upgrade pip && pip install flake8 pytest`
  - If `requirements.txt` exists: `pip install -r requirements.txt`
  - Lint (strict errors):
    - `flake8 . --count --select=E9,F63,F7,F82 --show-source --statistics`
  - Lint (full, non-blocking):
    - `flake8 . --count --exit-zero --max-complexity=10 --max-line-length=127 --statistics`
  - Tests with coverage (as run in CI):
    - `pytest tests.py --doctest-modules --junitxml=junit/test-results.xml --cov=com --cov-report=xml --cov-report=html`

- **Important CI notes / gotchas**:
  - The CI's pytest invocation targets coverage for package `com` (`--cov=com`). Confirm the repository actually contains a package named `com`; otherwise update the workflow to the correct package/module (for example `--cov=your_package` or `--cov=.`).
  - CI expects `tests.py` at repo root. If you rename or reorganize tests to a `tests/` package, update `.github/workflows/...yml` accordingly.
  - CI creates `junit/test-results.xml` and coverage reports; if you change test output locations, keep CI consistent.

- **Project-specific patterns & conventions**:
  - Lightweight demo project — no complex packaging is enforced. Many changes will require adjusting the single CI workflow rather than multiple pipelines.
  - Linting is intentionally split into a strict early-fail run (catch syntax/runtime lint blockers) followed by a relaxed run (complexity/line-length only).
  - Tests are invoked directly on `tests.py` (not `pytest` discovery of `tests/` directory). Use that exact invocation unless you update CI.

- **When editing or adding tests**:
  - Run the exact CI commands locally to replicate CI behavior (install flake8/pytest, then run the flake8 commands and the pytest invocation above).
  - If you introduce a package (e.g., `src/my_pkg`), update the `--cov=` option and add an `__init__.py` as needed.

- **Examples of concise prompts for the agent**:
  - "Fix flake8 E9 errors from `flake8 . --select=E9,F63,F7,F82`; show a minimal patch and explain why each fix was made." 
  - "Update the CI workflow to run pytest on `tests/` directory and set `--cov=src`; provide the updated YAML and a short rationale." 
  - "The CI uses `--cov=com` but there is no `com` package — detect the intended package name and propose an updated workflow line." 

- **If you find or add new top-level scripts/packages**:
  - Update `.github/workflows/build-test-python-test-ci.yml` to install any new runtime/test dependencies and to point `pytest` and `--cov` to the correct locations.

- **Merging guidance (if this file already exists)**:
  - Preserve any local guidance that mentions custom prompts or internal conventions. Add or replace only repo-specific actionable items (commands, file locations, CI details).

If anything here is unclear or you want more detail (package structure, intended publish targets, or local dev flow), tell me what to expand and I'll iterate.
