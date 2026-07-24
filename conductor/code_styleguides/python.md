# Python Style Guide — team-experiments

Applies to Python tooling/scripts added to this repo. Standardized on **ruff** (lint + import sort) and **black** (format), with type hints throughout.

## Tooling

| Concern | Tool | Notes |
|---------|------|-------|
| Formatting | `black` | Line length 88 (default). Do not hand-format. |
| Linting | `ruff` | Includes `E`, `F`, `I` (isort), `UP`, `B` rule sets. |
| Import sorting | `ruff` (`I`) | No separate isort. |
| Type checking | `mypy` or `pyright` | Aim for `strict` on new modules. |
| Package/deps | `uv` (preferred) or `pip` + `requirements.txt` | Pin versions. |

### Suggested `pyproject.toml` starting point

```toml
[tool.black]
line-length = 88

[tool.ruff]
line-length = 88
target-version = "py311"

[tool.ruff.lint]
select = ["E", "F", "I", "UP", "B"]

[tool.mypy]
strict = true
```

## Conventions

- **Type hints required** on all public functions, methods, and module-level constants.
- **Docstrings**: one-line summary for every public function/class; expand with Args/Returns/Raises when non-obvious. No comments that merely restate the code.
- **Naming**: `snake_case` for functions/variables, `PascalCase` for classes, `UPPER_SNAKE` for constants.
- **Imports**: standard lib → third-party → local, separated by blank lines (ruff enforces).
- **Errors**: raise specific exceptions; validate inputs at boundaries; never swallow exceptions silently.
- **Secrets**: read from environment (e.g. `os.environ` / `pydantic-settings`); never hard-code tokens or write them to blackboard files.
- **Structure**: keep I/O and integration code (HubSpot, n8n, filesystem) separate from pure logic so it stays testable.

## Testing

- Use `pytest`. Add tests for non-trivial logic (state parsing, integration payloads).
- Name tests `test_<unit>.py`; keep fixtures small and explicit.
- Tests are recommended, not blocking, per the project workflow policy.
