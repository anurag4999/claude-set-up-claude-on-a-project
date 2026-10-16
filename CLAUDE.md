# CLAUDE.md

An Express REST API for managing users, backed by an in-memory store.

## Commands

- `npm run dev`: start the API on :3000 in watch mode
- `npm test`: run the tests
- `npm run lint`: run ESLint. CI runs lint and tests, so run both before committing.

## Conventions

- Use CommonJS `require`, not ES `import`.
- Routes access data through `db/store.js` functions, not the `users` array directly. Add a store function when a route needs a new operation.
- Return errors as `res.status(code).json({ error: "..." })`: 400 for invalid input, 404 for a missing resource.
- Write tests with `node:test` and `supertest` against `require("../server")`, not Jest, and don't call `listen()`.

## Architecture

- `server.js` is the entry point. It mounts the routers and exports `app`.
- `routes/` has one file per resource. A new resource means a new `routes/<name>.js` file, mounted in `server.js`.
- `db/store.js` is the only data layer. It is in-memory and resets on restart.
