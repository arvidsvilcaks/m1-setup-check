# What make test does

`make test` runs `pytest -q` from the repo root ([Makefile](../Makefile)). The `-q` flag keeps the output short. The repo has no `pytest.ini`, `pyproject.toml` or `setup.cfg`, so pytest uses its defaults and picks up every `test_*.py` file. Right now that is just [tests/test_smoke.py](../tests/test_smoke.py), which has two checks:

1. **`test_openapi_document_can_be_loaded`** opens [docs/openapi.yaml](../docs/openapi.yaml) and checks that it is valid YAML, that the `openapi` version starts with `3.`, and that it defines at least one entry under `paths`. It doesn't check the content of the contract. `make lint-contract` does that, using [tools/lint_contract.py](../tools/lint_contract.py).
2. **`test_participant_files_are_present`** checks that these files exist: `.claude/settings.json`, `.devcontainer/devcontainer.json`, `CLAUDE.md`, `Makefile`, `tracker/CR-2.md` and `tracker/README.md`. If any are missing, it fails and lists them, with the message in Latvian ("Trūkst faili: …", meaning "Missing files").

When run in this Codespace, both tests passed in 0.09s.

## Compared with the other targets

- **`make verify-setup`** checks the environment: the Python version, the required packages (Pydantic must be v2), `claude --version`, and that `setup/claude-answer.md` exists and isn't empty. It then runs only the smoke test file.
- **`make lint-contract`** checks the OpenAPI contract. Per CLAUDE.md, run it whenever you change `docs/openapi.yaml`, in addition to `make test`.
