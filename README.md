# Asynchronous Quiz Evaluation & Leaderboard System

Local Turborepo monorepo for an SDET take-home assignment. It uses Bun workspaces, Next.js App Router, an Elysia API, BullMQ workers, Redis, SQLite, Drizzle ORM, Playwright, k6, and Bun tests.

## Prerequisites

- Bun 1.2+
- Redis running locally
- Docker if you want to use the included Testcontainers helper
- k6 for performance tests

Start Redis locally with Docker:

```bash
docker run --rm -p 6379:6379 redis:7-alpine
```

Before starting, fork this repository and clone your fork locally.

## Setup

```bash
bun install
bunx playwright install
bun run db:migrate
```

The SQLite database defaults to `packages/db/data/quiz.sqlite`. Override it with `DATABASE_URL=file:/absolute/path/to/quiz.sqlite` or `DATABASE_URL=/absolute/path/to/quiz.sqlite`.

## Development

Run all package dev scripts through Turborepo:

```bash
bun run dev
```

Useful package-level commands:

```bash
bun --cwd apps/api run dev
bun --cwd packages/workers run dev
bun --cwd apps/web run dev
```

Default local URLs:

- Web: `http://localhost:3000`
- API: `http://localhost:3001`
- Redis: `redis://127.0.0.1:6379`

## Tests

```bash
bun run typecheck
bun run test
bun run test:e2e
bun run test:perf
```

The k6 script writes its summary to `packages/testing/dist/perf-results.json`.

## API

Submit a quiz:

```bash
curl -X POST http://localhost:3001/api/quiz/submit \
  -H 'content-type: application/json' \
  -d '{"userId":"alice","quizId":"quiz-1","answers":["A","C","B","D","A"]}'
```

Get leaderboard:

```bash
curl http://localhost:3001/api/leaderboard
```

Health check:

```bash
curl http://localhost:3001/health
```
