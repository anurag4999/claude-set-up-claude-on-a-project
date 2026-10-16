# Notes

## CLAUDE.md: what's in it and what I left out

I kept only what Claude can't easily infer from a single file, or would likely get wrong:
- the three commands I run (`dev`, `test`, `lint`), with the reminder that CI runs both lint and tests
- four conventions written as "use X, not Y":
  - CommonJS, not ES modules
  - routes go through `db/store.js` functions, never the `users` array
  - the `{ error }` response shape with 400/404
  - tests use `node:test` and `supertest` against the exported `app`, not Jest and not a running server
- a three-line architecture note covering `server.js`, `routes/` and `db/store.js`

I deliberately left out:
- the endpoint list, the dependencies, and the quote and semicolon style: they're obvious from the code or enforced by ESLint, so they would only cost context
- a "never touch `.env`" instruction: `CLAUDE.md` is only guidance, so that rule lives in a `deny` permission, which is enforced
- any secrets or one-off task notes

## Permission rules

- **allow:** `npm test` and `npm run lint`. Both are safe and run constantly, so prompting for them every time is just noise.
- **ask:** `git push`. It's outward-facing, so I want to confirm each one.
- **deny:** reading or editing `.env`, and `git push --force` / `git push -f`.

Without the deny rules:
- Claude could read `.env` while exploring and pull real secrets into its context. It could also change them.
- A force-push could overwrite history on the remote and destroy other people's work.

Deny wins over ask, so these are blocked outright, not just confirmed.

These rules are guardrails, not a full security boundary:
- Bash rules match how a command starts, so `git push origin main --force` slips past the deny rule. It still hits the `git push` ask rule.
- `Read(./.env)` blocks the Read tool, but not `cat .env` in a shell.

## Other change: `.gitignore`

Every line after the first started with spaces, and git treats leading spaces as part of the pattern. So `.env` and `.claude/settings.local.json` were **not** being ignored, which `git check-ignore` confirmed. I removed the leading spaces so both are ignored as intended.

## Verification

- `/memory`: `CLAUDE.md` shows as the loaded project memory.
- `/permissions`: the allow, ask and deny rules above are all listed.
