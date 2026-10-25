# Cypress Heroes Demo Application

This is the repo for the Testing APIs with Cypress webinar. For a more complete version of Cypress Heroes app (including more in-depth API tests), visit the main repo [here](https://github.com/cypress-io/cypress-heroes).

## Tech stack

- **Client** (`client-react`): React, Vite, Tailwind CSS
- **Server** (`server`): NestJS, Prisma, SQLite
- **Tests**: Cypress (API, E2E and component tests)

## Prerequisites

- [Node.js](https://nodejs.org/) 16 or newer (18 LTS recommended) and npm
- Git

## Installation

```bash
git clone <repo-url>
cd cypress-heroes-api-webinar
npm install
```

The root `postinstall` script automatically runs `npm install` in both `client-react` and `server`.

## Configuration

The client reads the API address from the `VITE_API_URL` environment variable. Create the file `client-react/.env.local`:

```
VITE_API_URL=http://localhost:3001
```

The server needs no configuration: it listens on port `3001` by default (override with `PORT`) and uses a SQLite database stored in `server/prisma/data/`.

## Running the application

### 1. Prepare the database

From the `server` folder, create the database and fill it with seed data (this also resets it to a clean state at any time):

```bash
cd server
npm run prisma:resetdb
```

### 2. Start the server

```bash
cd server
npm run start:dev
```

The API is available at <http://localhost:3001>.

### 3. Start the client

In a second terminal:

```bash
cd client-react
npm run dev
```

The app is available at <http://localhost:3000>.

### Test accounts

| Role  | Email            | Password  |
| ----- | ---------------- | --------- |
| User  | `test@test.com`  | `test123` |
| Admin | `admin@test.com` | `test123` |

## Running the tests

### API tests (the focus of the webinar)

Start the server first (see above), then in a new terminal:

```bash
cd server
npx cypress open   # interactive mode
npm run test:api   # headless mode
```

Tests live in `server/cypress/e2e/`. The base URL is `http://localhost:3001`.

### E2E and component tests (client)

The client and server must both be running for E2E tests.

```bash
cd client-react
npx cypress open                 # interactive mode
npx cypress run                  # E2E, headless
npx cypress run --component      # component tests, headless
```

> The `test:e2e:ci` / `test:ct:ci` npm scripts use `--record` and require a Cypress Cloud project and record key, so use `npx cypress run` locally.

## Useful scripts

Run from the repo root:

| Command                   | Description                                                   |
| ------------------------- | ------------------------------------------------------------- |
| `npm run build`           | Build the client and the server                               |
| `npm run start:ci`        | Reset the DB and start the built server and client preview     |
| `npm run test:api:ci`     | Run API tests headless                                        |

## Troubleshooting

- **Client can't reach the API / requests go to `undefined`**: `client-react/.env.local` is missing or `VITE_API_URL` is wrong. Restart `npm run dev` after creating it.
- **Port 3000 or 3001 is already in use**: stop the process using it. The client port is fixed in `client-react/vite.config.ts`; change the server port with the `PORT` variable.
- **Tests fail with unexpected data**: reset the database with `npm run prisma:resetdb` in `server`.
- **`bcrypt` install errors**: make sure you use a supported Node.js version, then delete `node_modules` and run `npm install` again.
