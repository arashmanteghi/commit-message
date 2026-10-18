---
name: suggest-message
description: Suggest a commit message for staged or unstaged git changes. Use when the user asks for a commit message.
argument-hint: "[staged|unstaged]"
allowed-tools: Bash(git diff:*), Bash(git status:*), Bash(git branch:*)
model: sonnet
effort: medium
---

Suggest a commit message. Do not commit and do not change any files.

## Context
- Branch: !`git branch --show-current`
- Status: !`git status --short`

## Format

`<TICKET> <type>: <subject>`

- `<TICKET>`: the ticket key from the branch name, for example `COREX-12345`.
  If the branch has no ticket key, leave it out and start with `<type>`.
- `<type>`: exactly one type from the list below.
- `<subject>`: follow the casing rules below.

### Types

| Type | Use for |
|---|---|
| `feat` | A new feature or new behaviour for the user |
| `fix` | A bug fix |
| `refactor` | A code change that does not change behaviour |
| `perf` | A change that makes the code faster or smaller |
| `test` | Adding or updating tests only |
| `docs` | Documentation only |
| `style` | Formatting only (spaces, commas), no logic change |
| `build` | Build system or dependencies (`package.json`, pnpm, turbo) |
| `ci` | CI config and scripts (GitHub Actions) |
| `chore` | Other maintenance (config, tooling) that fits no type above |
| `revert` | Reverting an earlier commit |

If the change mixes types, pick the type of the main change.

### Casing rules for the subject

- Start the subject with a lowercase verb in the imperative mood ("add", not "added").
- Write every other word in lowercase, except:
  - Important words that name the target of the change (the feature, page or domain),
    for example `Deposit`, `Login`, `Cashier`. Start these with an uppercase letter.
  - Acronyms, like `KYC`, `RG`, `SGA`, `MFA`. Keep them all uppercase.
  - Code names (components, hooks, functions). Keep their exact casing, like `useDepositLimit`.

### Examples

- `COREX-12345 fix: align Deposit button on Cashier page`
- `COREX-12345 feat: add Login retry for MFA errors`
- `COREX-12345 refactor: move useDepositLimit hook to RG domain`
- `chore: update Biome config` (no ticket in the branch name)

## Steps
1. If the argument is "unstaged", read `git diff`. If it is "staged" or empty, read `git diff --cached`.
   If that diff is empty, say so and offer the other one.
2. Pick the type that fits the main change.
3. Write the first line in the format above, 72 characters or less.
4. Add a short body only if the "why" is not clear from the subject.
5. Show 1 main suggestion and 1 shorter alternative.
