# Repository Guidelines

## Project Structure & Module Organization
- `bin/` holds the CLI entrypoint (`cue-command.js`) that wires CLI args to the runtime.
- `src/` contains core modules (`cli.js`, `handler.js`, `proto.js`, DB/helpers).
- `docs/` and `protocol.md` document the Human Agent Protocol and CLI behavior.
- `assets/` is only for documentation images; no runtime dependencies.

## Build, Test, and Development Commands
- `npm ci`: install dependencies with the lockfile (matches CI).
- `npm run prepare`: CI syntax check for `src/cli.js` and `src/handler.js`.
- `node -c src/cli.js`: ad-hoc syntax check for new/edited files.
- `node bin/cue-command.js --help`: run the CLI locally without global install.

## Coding Style & Naming Conventions
- Node.js >= 20, CommonJS (`require`, `module.exports`).
- Indentation: 2 spaces; use semicolons.
- Naming: `camelCase` for functions/vars, `lowercase` filenames, and short, action-based verbs (e.g., `parseArgs`, `protoApply`).
- Keep CLI output plain text and deterministic (no ANSI styling or interactive prompts).

## Testing Guidelines
- There are no unit tests; CI is syntax-only.
- Run `npm run prepare` and ensure `node -c` passes for any new `.js` files in `src/` or `bin/`.
- If you add tests in the future, keep them near the module (e.g., `src/__tests__/`) and document the runner here.

## Commit & Pull Request Guidelines
- Commit messages follow a conventional prefix style (e.g., `feat:`, `docs:`, `chore:`).
- Use DCO sign-off: `git commit -s` adds the required `Signed-off-by` line.
- Keep PRs small, include context and repro steps for bug fixes, and ensure CI passes.

## Security & Configuration Notes
- The CLI interacts with a local SQLite mailbox at `~/.cue/cue.db`; do not commit user data.
- Update `protocol.md` and `docs/proto.md` together when changing protocol behavior.
