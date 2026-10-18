# commit-message skill

A Claude Code skill. It suggests a commit message for your git changes.
It only suggests. It never commits and never changes files.

It works in all projects, because it lives in `~/.claude/skills/`.

## Commands

| Command | What it reads |
|---|---|
| `/commit-message` | Staged changes (the default) |
| `/commit-message staged` | Staged changes |
| `/commit-message unstaged` | Unstaged changes |

You can also just ask "suggest a commit message". Claude may use the skill without the slash command.

## Message format

`<TICKET> <type>: <subject>`

- `<TICKET>` comes from the branch name, for example `COREX-12345`. If the branch has none, it is left out.
- `<type>` is one of the types below.
- `<subject>` follows the casing rules below.

Example: `COREX-12345 fix: align Deposit button on Cashier page`

## Types

The type tells the reader what kind of change this is, before they read the code.
Pick one type. If the change mixes types, pick the type of the main change.

| Type | Use it when | Why it helps | Example |
|---|---|---|---|
| `feat` | You add a new feature or new behaviour for the user. | Shows what is new for the user. Good input for release notes. | `COREX-12345 feat: add Login retry for MFA errors` |
| `fix` | You fix a bug. | Shows a broken thing now works. Helps when you look for the change that fixed (or caused) a bug. | `COREX-12345 fix: align Deposit button on Cashier page` |
| `refactor` | You change how the code is written, but not what it does. | Tells the reviewer to expect no change in behaviour. | `COREX-12345 refactor: move useDepositLimit hook to RG domain` |
| `perf` | You make the code faster or the bundle smaller. | Shows the change is about speed or size, not a feature or a fix. | `COREX-12345 perf: lazy load Game list images` |
| `test` | You add or change tests only. | Shows production code did not change, so the risk is low. | `COREX-12345 test: add tests for Withdrawal form validation` |
| `docs` | You change documentation only (README, `docs/*.md`, comments). | Shows no code changed. Easy to review. | `COREX-12345 docs: explain Signup flow in auth README` |
| `style` | You change formatting only (spaces, commas, import order). No logic change. | Tells the reviewer they can skim it. | `COREX-12345 style: format Cashier files with Biome` |
| `build` | You change the build system or dependencies (`package.json`, pnpm, turbo). | Shows the change can affect how the project builds or installs. | `COREX-12345 build: upgrade Next.js to 15.2` |
| `ci` | You change CI config or scripts (GitHub Actions). | Shows only the pipeline changed, not the app. | `COREX-12345 ci: cache pnpm store in Checks workflow` |
| `chore` | Other maintenance that fits no type above (config, tooling, cleanup). | Keeps small housekeeping changes apart from real features and fixes. | `chore: update Biome config` |
| `revert` | You undo an earlier commit. | Shows clearly that an old change was taken back. | `COREX-12345 revert: add Login retry for MFA errors` |

## Casing rules for the subject

- Start with a lowercase verb in the imperative mood ("add", not "added").
- Write every other word in lowercase, except:
  - Important words that name the target of the change (feature, page or domain), like `Deposit`, `Login`, `Cashier`. These start with uppercase.
  - Acronyms, like `KYC`, `RG`, `SGA`, `MFA`. These stay all uppercase.
  - Code names, like `useDepositLimit`. These keep their exact casing.

## Files

| File | Purpose |
|---|---|
| `SKILL.md` | The instructions Claude follows. Edit this to change the rules. |
| `README.md` | This doc, for people. Claude does not load it. |
