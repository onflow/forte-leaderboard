# AGENTS.md

Guidance for AI coding agents (Claude Code, Codex, Cursor, Copilot, Gemini CLI, and others)
working in this repository. Loaded into agent context automatically — keep it concise.

## Overview

Team registry for **Forte Hacks** (Flow community hackathon, Oct 1–31). Participants submit
a PR adding their team entry to `registry.yaml`; a GitHub Action validates the entry using a
Bun/TypeScript script. Package name is `rewtf-registry` (see `package.json`). The README
"Bonus Rewards" and "Leaderboard" sections are placeholders ("Coming soon") — no leaderboard
UI, scoring code, or reward distribution logic exists in this repo.

## Build and Test Commands

Package manager is **Bun**. No `build`, `test`, or `lint` scripts are defined in
`package.json`. Available scripts:

- `bun install` — install dependencies (lockfile is `bun.lockb`).
- `bun run get-token` — runs `scripts/bin/get-access-token.ts`; requires env vars
  `APP_ID`, `APP_PRIVATE_KEY`, `INSTALLATION_ID` (GitHub App credentials).
- `bun run validate-registry` — runs `scripts/bin/validate-registry.ts` against
  `registry.yaml`; requires env var `GITHUB_TOKEN`. Writes result to
  `tmp/validation-result.json`.

TypeScript is configured with `noEmit: true` (`tsconfig.json`); scripts execute directly
via Bun. Biome is a dev dependency (`@biomejs/biome ^2.1.4`) but no npm script invokes it —
run `bunx biome check .` or `bunx biome format .` manually if needed.

## Architecture

```
.
├── registry.yaml            # Source of truth — teams append YAML entries here
├── package.json             # Bun project, scripts entry point
├── bunfig.toml              # Bun config; loads .cdc files as text
├── tsconfig.json            # strict, ESNext, moduleResolution: bundler, noEmit
├── scripts/
│   ├── bin/
│   │   ├── get-access-token.ts     # GitHub App JWT → installation access token
│   │   ├── validate-registry.ts    # Validates the LAST entry in registry.yaml
│   │   └── README.md               # Per-script usage notes
│   ├── src/
│   │   └── types.ts                # ValidationResult interface
│   └── tsconfig.json               # extends root tsconfig
└── .github/workflows/
    └── validate-registry.yml  # Runs on PRs that touch registry.yaml
```

The validation script uses `@octokit/rest` to verify each entry's GitHub usernames, repo URLs,
and Flow-ecosystem eligibility. The access-token script uses `@octokit/auth-app` to mint an
installation token for the CI workflow.

## Conventions and Gotchas

- **Validator only checks the last entry.** `validate-registry.ts` reads
  `registry.yaml`, filters to objects, and picks `entries[entries.length - 1]`
  (see `validateRegistry` in `scripts/bin/validate-registry.ts`). Edits earlier in
  the file are not re-validated by CI.
- **Flow-ecosystem eligibility rules** (from `validateFlowEcosystemRequirements`):
  repo qualifies if its README contains "on flow" or "flow blockchain" (case-insensitive),
  OR `package.json` has `@onflow/fcl` or `@onflow/kit`, OR `go.mod` references
  `github.com/onflow/flow-go-sdk` or `github.com/onflow/flow-go`, OR a Hardhat
  config contains `evm.nodes.onflow.org`. README.md in this repo advertises
  `@onflow/react-sdk` as acceptable, but the validator code only checks
  `@onflow/fcl` / `@onflow/kit` — README and code disagree.
- **Wallet address formats** enforced by `validateWalletAddresses`:
  EVM must match `^0x[0-9a-fA-F]{40}$` (42 chars total); Flow must match
  `^0x[0-9a-fA-F]{16}$` (18 chars total). Wallets are optional per the template
  in `registry.yaml` but the README labels both required — trust the code.
- **CI trigger is `pull_request_target`** scoped to `paths: registry.yaml` on `main`
  (`.github/workflows/validate-registry.yml`). Changes outside `registry.yaml` do
  not trigger validation.
- **`tmp/` is gitignored** (`.gitignore` line: `tmp/*`); the validation script writes
  `tmp/validation-result.json` and the workflow reads it with `jq`.
- **Runtime floor:** `engines` in `package.json` requires Node `>=20.0.0` and
  Bun `>=1.0.0`.
- **License:** MIT (`LICENSE`, Copyright 2025 Flow).

## Files Not to Modify

- `bun.lockb` — binary Bun lockfile; regenerate via `bun install`, do not hand-edit.
- `registry.yaml` entries owned by other teams — only append new entries at the end
  of the file, matching the template block at the top.
