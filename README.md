# Laravel Export Kit

Laravel patterns for large exports (queued, chunked, streamed to S3, N+1-free), plus an interactive simulator that shows why they matter.

> **Status: in development.** This repo is the plan and the write-up; the interactive demo is being built next.
> Star or watch the repo to see it land. The live demo link will appear here.

Part of [Julynard Tago-on's portfolio](https://julynardtago-on.netlify.app/#projects). Based on fixing timeouts and N+1 queries across dozens of BI export processors at CoreProc.
Client code is private, so this is a clean-room demo with fictional data. No client names or real data.

## What the demo will show

- [ ] Pick a dataset size (10k / 100k / 1M rows) and a strategy: **naive** (Eloquent `get()`, N+1) vs **optimised** (eager loading, chunked queued jobs, streamed output)
- [ ] Watch a live query counter (the N+1 detector flags the repeated query), memory gauge, elapsed time and chunk progress
- [ ] See the naive strategy hit the timeout at large sizes while the optimised one finishes
- [ ] Before/after Laravel code for each step
- [ ] A `/laravel` folder with real, readable PHP: an example chunked export processor and an N+1 detector

Every key action will have an **"In production"** note explaining how the real system does it.

## How the production system works

The real system runs on Laravel. The demo simulates these behaviours in the browser so it can stay fast and always on.

- Nightly exports of many tables to an S3 bucket read by Power BI
- Export processors built on a queued, chunked CSV pipeline (Horizon job batches, collate-and-upload step)
- N+1 removed with eager loading, plus a static audit of the other export processors
- Query builder + `lazy()` cursors instead of hydrated models for big fact tables; chunk size and worker count tuned per export
- One nightly run report that classifies every export job's outcome instead of a flood of per-job emails

## Planned stack

- Vue 3 + TypeScript + Vite
- Tailwind CSS v4
- Pinia (state saved in the browser, with a **Reset demo** button)
- Vitest for the domain logic, ESLint + Prettier + `vue-tsc` in CI
- PHP 8.3 / Laravel 12 snippets in `/laravel`
- GitHub Actions → GitHub Pages (free, always on)

## Roadmap

1. Project scaffold, CI (lint, type-check, test, build) and GitHub Pages deploy
2. Domain logic with Vitest tests
3. UI with seeded data, guided steps and "In production" notes
4. Screenshot/GIF in this README and the live demo link

## Author

**Julynard Tago-on**, Laravel / PHP full-stack engineer · [Portfolio](https://julynardtago-on.netlify.app) · [GitHub](https://github.com/Julynard) · [LinkedIn](https://www.linkedin.com/in/julynard-tago-on/)

## License

[MIT](LICENSE)
