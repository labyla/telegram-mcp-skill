# Repository Guidelines

## Project Structure & Module Organization

This repository is a Python MCP server for Telegram. Core package code lives in
`telegram_mcp/`; `telegram_mcp/tools/` contains MCP tool groups such as
messages, media, contacts, chats, accounts, folders, groups, and profile.
`main.py` is the server entry point, `session_string_generator.py` creates
Telegram session strings, and `sanitize.py` holds shared output sanitization
logic. Tests live in `tests/` and generally mirror the behavior under test
(`tests/test_runtime.py`, `tests/test_sanitize.py`, etc.). Static README images
are in `screenshots/`. Docker support is defined by `Dockerfile` and
`docker-compose.yml`.

## Build, Test, and Development Commands

- `uv sync`: install runtime and development dependencies from `pyproject.toml`
  and `uv.lock`.
- `uv run main.py`: run the MCP server locally.
- `uv run session_string_generator.py`: generate a Telegram session string.
- `uv run pytest`: run the test suite configured in `pyproject.toml`.
- `uv run pytest --cov --cov-report=term-missing`: run coverage; the configured
  minimum is 80%.
- `uv run black --check .` and `uv run flake8 .`: verify formatting and linting.
- `pre-commit run --all-files`: run the same local hooks used for formatting,
  linting, Docker checks, and whitespace cleanup.
- `docker compose --env-file .env.example config`: validate Compose
  configuration without using real credentials.

## Coding Style & Naming Conventions

Use Python 3.10+ and follow existing module style. Black is configured with a
99-character line length and Python 3.11 target. Prefer explicit async code for
Telethon/MCP interactions. Use `snake_case` for functions, variables, and module
names; use clear tool names that match their Telegram action. Keep changes
surgical: avoid unrelated refactors, new abstractions, or broad formatting churn.

## Testing Guidelines

Tests use `pytest`, `pytest-asyncio`, and `pytest-cov`. Add or update tests in
`tests/` for behavior changes, especially validation, sanitization, runtime
setup, file path security, and media handling. Name files `test_*.py` and test
functions `test_*`. Prefer deterministic unit tests over live Telegram API calls.

## Commit & Pull Request Guidelines

Recent history uses conventional-style commits such as `feat(messages): ...`,
`feat: ...`, `fix: ...`, and `style: ...`. Keep commits focused and describe the
observable behavior change. Pull requests should include a short summary, linked
issue when applicable, test results, and screenshots only for README or visible
client documentation changes.

## Security & Configuration Tips

Never commit `.env`, Telegram API credentials, session strings, or downloaded
private media. Use `.env.example` for examples and dummy values in tests. Be
careful with file path handling and sanitization: Telegram content is
user-controlled, so preserve existing validation and output-cleaning patterns.
