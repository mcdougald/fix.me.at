<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset=".github/assets/hero-dark.png">
    <img src=".github/assets/hero.png" alt="fix.me.at landing page" width="880">
  </picture>
</p>

<h1 align="center">fix.me.at</h1>

<p align="center"><strong>Error tracking without a new SDK.</strong></p>

<p align="center">
  <img src="https://img.shields.io/badge/status-in%20development-171717?style=flat-square" alt="Status: in development">
  <img src="https://img.shields.io/badge/license-source--available-404040?style=flat-square" alt="License: source-available">
  <img src="https://img.shields.io/badge/self--host-supported-525252?style=flat-square" alt="Self-host: supported">
  <img src="https://img.shields.io/badge/part%20of-me.at-737373?style=flat-square" alt="Part of me.at">
</p>

<p align="center">
  <a href="#features">Features</a> ·
  <a href="#how-it-works">How it works</a> ·
  <a href="#built-with">Built with</a>
</p>

Switching error trackers usually means ripping out one SDK and wiring in another. I built fix.me.at with a Sentry-SDK-compatible ingest endpoint, so you point the `@sentry/*` SDK you already ship at a different DSN, and the SDK, its version and every hook you have written stay exactly as they are. Errors are grouped into issues, ingest is storm-capped, and secrets and PII are scrubbed before anything is stored. It is for teams who want error tracking and nothing else.

<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset=".github/assets/overview-dark.svg">
    <img src=".github/assets/overview.svg" alt="fix.me.at at a glance: keep your Sentry SDK, issues not just events, storm-capped ingest, scrubbed before stored" width="880">
  </picture>
</p>

## Why fix.me.at

| The usual error tracker | fix.me.at |
| --- | --- |
| A new SDK to install | The Sentry SDK you already ship, with a new DSN |
| A spike turns into an overage bill | A project over its limit gets 429s, with no overage price |
| Source maps can change how errors group | Source maps are a display overlay, and grouping stays on the original frames |
| Traces and spans kept whether you want them or not | Exceptions lifted out of OTLP data, spans never retained |

## Features

- **Speaks the Sentry SDK protocol**: the ingest endpoint accepts the Sentry envelope protocol at a pinned version, so moving over is a one-line DSN change per service. A new upstream SDK release cannot quietly change what ingest accepts.
- **Issues, not just events**: events are grouped into issues by fingerprint, with occurrence counts, release and environment tags, and a resolve, ignore and assign workflow.
- **Alert rules**: first occurrence, regression and threshold alerts, delivered to email and webhook.
- **One crashing client cannot flood the queue**: per-project rate limits, a monthly event allowance, a shared ceiling across tenants, and a per-project kill switch. Size and shape limits are checked before the request body is read, and a project over its limit gets 429s, with no overage price.
- **Scrub-then-store**: request headers, cookies and stack-frame locals are scrubbed on the server before anything is persisted. Events are kept for 30 days by default.
- **Source maps without changing grouping**: maps uploaded with `sentry-cli` resolve frames as a display overlay only. Fingerprints stay on the original frames, and a stack that cannot be fully resolved is shown as minified, labelled as such.
- **OTLP exception ingest**: OTLP logs and traces endpoints lift exceptions out of log records and span events; spans themselves are never retained.
- **Plain HTTP APIs and self-hosting**: issues, events, alert rules and usage meters are JSON over HTTP. Run it against Postgres, an S3-compatible bucket and Redis, or entirely in memory, with an offline license check that never calls home.

## How it works

<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset=".github/assets/flow-dark.svg">
    <img src=".github/assets/flow.svg" alt="How fix.me.at works: send, limit, scrub, group, alert" width="880">
  </picture>
</p>

Your existing Sentry SDK sends events to a new DSN. Size, shape and rate limits are checked before the request body is read, then headers, cookies and stack-frame locals are scrubbed on the server before anything is stored. Events are grouped into issues by fingerprint, and alert rules fire to email and webhook.

## Built with

<p>
  <img src="https://img.shields.io/badge/Next.js-16-000000?style=flat-square&logo=nextdotjs&logoColor=white" alt="Next.js 16">
  <img src="https://img.shields.io/badge/React-19-171717?style=flat-square&logo=react&logoColor=white" alt="React 19">
  <img src="https://img.shields.io/badge/TypeScript-5-262626?style=flat-square&logo=typescript&logoColor=white" alt="TypeScript">
  <img src="https://img.shields.io/badge/Tailwind%20CSS-4-404040?style=flat-square&logo=tailwindcss&logoColor=white" alt="Tailwind CSS 4">
  <img src="https://img.shields.io/badge/Drizzle-Postgres-525252?style=flat-square&logo=drizzle&logoColor=white" alt="Drizzle ORM and Postgres">
  <img src="https://img.shields.io/badge/S3-object%20storage-737373?style=flat-square&logo=amazons3&logoColor=white" alt="S3-compatible object storage">
</p>

- Next.js 16 (App Router), React 19, TypeScript
- Tailwind CSS 4
- Drizzle ORM with Postgres
- S3-compatible object storage for event payloads
- Better Auth
- next-intl

## Part of the me.at family

fix.me.at is one of my [`*.me.at`](https://me.at) apps. They share one design system, one account, and one app shell. Development happens in a private monorepo; this repository is the project's public-facing home.

## License

Source-available. See [LICENSE](LICENSE).
