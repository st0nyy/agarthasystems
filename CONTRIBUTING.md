# Contributing to agarthasystems

Thanks for helping out! This document explains **how work gets from an idea into `main`**, the rules everyone follows, and what "done" means. If you see a term you don't recognise, check the [Glossary](#glossary) at the bottom.

---

## Table of contents

1. [Workflow at a glance](#workflow-at-a-glance)
2. [Step-by-step workflow](#step-by-step-workflow)
   1. [Start with an issue](#1-start-with-an-issue)
   2. [Create a branch](#2-create-a-branch)
   3. [Make commits](#3-make-commits)
   4. [Open a pull request](#4-open-a-pull-request)
   5. [Review](#5-review)
   6. [Merge](#6-merge)
3. [Rules](#rules)
4. [Definition of Done](#definition-of-done)
5. [Worked example](#worked-example)
6. [Additional info: Git tips](#additional-info-git-tips)
   1. [Keeping your branch up to date](#keeping-your-branch-up-to-date)
   2. [Merge conflicts: don't panic](#merge-conflicts-dont-panic)
   3. [Emergency commands](#emergency-commands)
   4. [Golden rules](#golden-rules)
7. [Glossary](#glossary)

---

## Workflow at a glance

```
Issue  →  Branch  →  Commits  →  Pull Request  →  Review (1 approval)  →  Squash merge into main
 #12      feature/12-log-set   feat: ...         "Closes #12"                  (by the PR author)
```

1. Every change starts with an **issue**.
2. Create a **branch** from `main` named `type/issue-number-short-name`.
3. Write **commits** using [Conventional Commits](https://www.conventionalcommits.org/).
4. Open a **pull request (PR)** using the template and link the issue with `Closes #N`.
5. Get **one approval** from another contributor.
6. The **author squash-merges** the PR once it's approved.

---

## Step-by-step workflow

### 1. Start with an issue

No code is written without an issue. The issue is where we agree **what** should be done and **why** before anyone spends time on **how**.

- Search existing issues first so you don't create a duplicate.
- Create a new issue on GitHub (**Issues → New issue**) and include:
  - **Title**: short and specific, e.g. "Allow logging a single set during a workout".
  - **Description**: the problem or the goal. For bugs, include steps to reproduce, what you expected and what actually happened.
  - **Acceptance criteria**: a checklist of concrete, testable conditions that must be true for the issue to count as solved (see [Glossary](#glossary)).
- Assign the issue to yourself before you start, so two people don't work on the same thing.

> Tiny fixes (a typo in docs, for example) still get an issue. It takes 30 seconds and keeps history traceable.

### 2. Create a branch

Always branch from an up-to-date `main`:

```bash
git checkout main
git pull origin main
git checkout -b feature/12-log-set
```

**In VS Code (no typing):**

1. Click the **branch name** in the bottom-left corner of the status bar and pick **`main`**.
2. Open **Source Control** (`Ctrl+Shift+G`), click the **`...`** menu at the top and choose **Pull**.
3. Click the branch name in the status bar again and choose **+ Create new branch...**. Type `feature/12-log-set` and press `Enter`. VS Code creates the branch and switches to it.

<!-- TODO: add GIF: ![Creating a branch in VS Code](docs/images/vscode-create-branch.gif) -->

**Branch name format:** `type/issue-number-short-name`

| Part | Meaning | Example |
|---|---|---|
| `type` | The kind of change (see table below) | `feature` |
| `issue-number` | The number of the issue this branch solves | `12` |
| `short-name` | 2–4 lowercase words joined with hyphens | `log-set` |

**Allowed branch types:**

| Branch type | Use it for | Matching commit type |
|---|---|---|
| `feature/` | New functionality | `feat:` |
| `fix/` | Bug fixes | `fix:` |
| `docs/` | Documentation only | `docs:` |
| `refactor/` | Restructuring code without changing behaviour | `refactor:` |
| `test/` | Adding or fixing tests only | `test:` |
| `chore/` | Tooling, dependencies, config, CI | `chore:` / `ci:` / `build:` |

Examples: `feature/12-log-set`, `fix/31-negative-weight-crash`, `docs/7-setup-guide`.

Rules for names: lowercase only, hyphens instead of spaces, no special characters.

### 3. Make commits

We use **Conventional Commits**: every commit message starts with a *type*, a colon and a short summary.

**Format:**

```
<type>(<optional scope>): <short summary>

<optional body: what changed and why>

<optional footer: e.g. "Closes #12" or "BREAKING CHANGE: ...">
```

**Commit types:**

| Type | Meaning | Example |
|---|---|---|
| `feat` | A new feature | `feat: add endpoint to log a set` |
| `fix` | A bug fix | `fix: reject negative weight values` |
| `docs` | Documentation only | `docs: explain how to run locally` |
| `style` | Formatting only (spaces, semicolons). No logic changes | `style: run formatter on workout module` |
| `refactor` | Code change that neither fixes a bug nor adds a feature | `refactor: extract set validation into helper` |
| `perf` | Performance improvement | `perf: cache exercise list` |
| `test` | Adding or correcting tests | `test: cover empty workout case` |
| `build` | Build system or dependencies | `build: upgrade express to 5.x` |
| `ci` | CI configuration (e.g. GitHub Actions) | `ci: run lint on pull requests` |
| `chore` | Other maintenance that doesn't touch app code | `chore: update .gitignore` |
| `revert` | Reverts a previous commit | `revert: feat: add endpoint to log a set` |

**Scope** (optional) is the area of the codebase in parentheses, e.g. `feat(workouts): ...` or `fix(auth): ...`.

**Breaking changes** (anything that forces other code or users to change, such as a renamed API field) must be marked with `!` after the type **and** a footer:

```
feat(api)!: rename "reps" field to "repetitions"

BREAKING CHANGE: clients must send "repetitions" instead of "reps".
```

**How to commit:**

```bash
git add <file>                 # stage the files you want in the commit
git commit -m "feat(workouts): add endpoint to log a set"
```

**In VS Code (no typing):**

1. Open **Source Control** (`Ctrl+Shift+G`). Your edited files are listed under **Changes**.
2. Hover over a file and click **+** (*Stage Changes*). To stage everything, hover over the **Changes** heading and click its **+**. Staged files move to **Staged Changes**.
3. Click a file to see a side-by-side diff of what you changed. This is a good moment to check your work.
4. Type your Conventional Commit message in the **Message** box at the top.
5. Click **✓ Commit**.

<!-- TODO: add GIF: ![Committing in VS Code](docs/images/vscode-commit.gif) -->

**Good commit messages:**

- Use the imperative mood: "add", "fix", "remove", not "added" or "fixes".
- Start the summary in lowercase, with no full stop at the end.
- Keep the summary under ~72 characters.
- Make each commit one logical change.

### 4. Open a pull request

When your branch is ready (or early, as a **draft PR**, if you want feedback):

```bash
git push -u origin feature/12-log-set
```

**In VS Code (no typing):**

1. After your first commit on a new branch, Source Control shows a **Publish Branch** button. Click it.
2. For later pushes, click **Sync Changes** in Source Control, or the **sync icon** (↻ with arrows) next to the branch name in the status bar.
3. VS Code may show a pop-up offering to **Create Pull Request**. You can use it, or open the PR on GitHub as described below.

<!-- TODO: add GIF: ![Publishing a branch in VS Code](docs/images/vscode-publish-branch.gif) -->

> **Tip:** the official **GitHub Pull Requests** extension lets you create, review and merge PRs without leaving VS Code.

Then open a PR on GitHub **from your branch into `main`**.

- **Use the PR template.** It fills in automatically when you open a PR. Complete every section and don't delete headings.
- **Title**: must follow Conventional Commits, e.g. `feat: add endpoint to log a set`. Because we squash-merge, **the PR title becomes the commit message on `main`.**
- **Link the issue**: put `Closes #N` in the description, where **N is the issue number** (not the PR number). For example, `Closes #12` for issue #12. GitHub then closes the issue automatically when the PR is merged. `Fixes #N` and `Resolves #N` work too.
- **Description**: explain what changed, why and how to test it. Add screenshots for UI changes.
- **Self-review**: read through your own diff before requesting review.
- Request a review from at least one other contributor.

### 5. Review

- **One approval** from someone other than the author is required before merging.
- Reviewers should respond within **24–48 hours** (see [Rules](#rules)).
- Reviewers check: correctness, readability, tests, and whether the [Definition of Done](#definition-of-done) is met.
- Comment types reviewers can use to make intent clear:
  - **blocking**: must be fixed before approval.
  - **nit:** a small, optional suggestion (short for "nitpick").
  - **question:** asking for clarification, not necessarily requesting a change.
- The author replies to or resolves every comment. If you disagree, discuss it in the thread. Don't silently ignore it.
- If new commits are pushed after approval that significantly change the PR, ask for a re-review.

### 6. Merge

- **The author merges**, not the reviewer, once the PR is approved and all checks pass.
- Use **"Squash and merge"** on GitHub. All commits on the branch are combined into one commit on `main`.
- Make sure the final squash commit message is a clean Conventional Commit (edit it in the merge dialogue if needed).
- **Delete the branch** after merging (GitHub shows a button for this).
- If `main` has moved on and your branch has conflicts, update your branch first. See [Keeping your branch up to date](#keeping-your-branch-up-to-date) and [Merge conflicts](#merge-conflicts-dont-panic).

---

## Rules

1. **Never push directly to `main`.** All changes go through a pull request, including small ones and changes by maintainers.
2. **Keep PRs small: aim for fewer than 400 changed lines** (additions + deletions, excluding generated files such as lock files). Large PRs are hard to review well. If a change is big, split it into several PRs, each with its own issue or as sub-tasks of one issue.
3. **Review within 24–48 hours.** If you're requested as a reviewer, give at least a first response in that window. If you can't, say so and suggest someone else.
4. **Significant technical decisions need an ADR in `docs/adr/`.** See [ADR](#glossary) in the glossary. A decision is *significant* if it is hard to reverse or affects the whole project, for example choosing a database, framework or auth method, changing the data model, or adding a major dependency. File naming: `docs/adr/NNNN-short-title.md` (e.g. `docs/adr/0001-use-postgresql.md`). Each ADR covers: **Context** (the problem), **Decision** (what we chose), **Alternatives considered**, and **Consequences** (trade-offs).
5. **No secrets in the repo; use `.env`.** Secrets include passwords, API keys, tokens, database connection strings and private keys. Put them in a local `.env` file, which is ignored by git via `.gitignore`. When you add a new variable, add it **with a placeholder value** to `.env.example` so others know it exists. If a secret is ever committed, tell a maintainer immediately and **rotate (replace) the secret**. Deleting it in a later commit is not enough, because it stays in git history.

---

## Definition of Done

A PR is only ready to merge when **all** of these are true:

- [ ] **Acceptance criteria in the issue are met.** Every checklist item in the linked issue is satisfied.
- [ ] **Tests added/updated and passing.** New behaviour has tests, changed behaviour has updated tests, and the whole test suite passes locally and in CI.
- [ ] **Lint passes.** The linter reports no errors.
- [ ] **Docs updated if behaviour changed.** README, API docs, `.env.example` or other docs reflect the change.

Each item is explained in more detail below.

> The commands below assume the standard npm scripts (`npm test`, `npm run lint`). If the project's `package.json` uses different script names, use those instead.

### Tests added/updated and passing

**What it means:** a test is a small piece of code that runs your code and checks that it behaves as expected, for example "logging a set with a negative weight returns an error". Tests prove your change works today and stop someone from breaking it by accident later.

**When you need to write or change tests:**

| Your change | What to do with tests |
|---|---|
| New feature | Add tests for the normal case **and** the edge cases (empty input, invalid values, limits). |
| Bug fix | First write a test that **fails** because of the bug, then fix the bug so the test passes. This proves the fix works and keeps the bug from coming back (a *regression test*). |
| Changed behaviour | Update the existing tests to match the new behaviour. Don't just delete a failing test. Understand why it fails first. |
| Refactor (no behaviour change) | Existing tests should pass **without** changes. If they need changing, the behaviour probably changed. |
| Docs, config or style only | Usually no tests needed. Say so in the PR description. |

**What a good test looks like:**

- It tests **one thing**, and its name says what: `rejects a set with zero reps`, not `test1`.
- It covers the acceptance criteria from the issue. Each criterion should be checked by at least one test.
- It is **independent**: it doesn't rely on other tests running first or on leftover data.
- It is **deterministic**: it gives the same result every time. No dependence on the current date, random numbers or real network calls unless those are controlled (mocked).
- It lives next to similar tests and follows the existing naming pattern (e.g. `*.test.js`).

**Types of tests:**

- **Unit test:** checks one function or module on its own. Fast, and the bulk of our tests.
- **Integration test:** checks several parts working together, e.g. an API endpoint and the database.
- **End-to-end (E2E) test:** drives the whole app like a real user would. Slow, so keep these for the most important flows.

**How to run them:**

```bash
npm test                 # run the whole test suite
npm test -- --watch      # re-run tests automatically as you edit (if supported)
npm test -- --coverage   # show which lines are covered by tests (if supported)
```

**"Passing" means:**

- The **whole** suite passes, not just the tests you wrote. Your change may have broken something elsewhere.
- It passes **locally** before you push, **and** in **CI** (the green check on the PR).
- No tests are skipped or disabled to make the suite pass (`.skip`, `xit`, commented-out tests). If a test has to be skipped temporarily, explain why in the PR and open an issue to fix it.
- **Flaky tests** (ones that sometimes pass and sometimes fail with no code change) count as failing. Report them in an issue instead of re-running until they go green.

### Lint passes

**What it means:** a **linter** is a tool that reads code without running it and flags problems, such as unused variables, likely bugs, inconsistent style or banned patterns. A **formatter** (e.g. Prettier) automatically fixes layout such as indentation, quotes and line length. Together they keep the codebase consistent, so reviews can focus on logic instead of style.

**How to run it:**

```bash
npm run lint             # check for problems
npm run lint -- --fix    # automatically fix what can be fixed
npm run format           # run the formatter (if the project has one)
```

**"Passes" means:**

- `npm run lint` finishes with **zero errors**. Fix warnings in code you touched where reasonable, and don't add new ones.
- The lint check in CI is green.
- **Don't silence the linter to get it to pass.** Avoid `// eslint-disable` comments. If a rule genuinely doesn't fit a specific line, disable it for **that line only**, with a comment explaining why:

  ```js
  // eslint-disable-next-line no-console -- CLI output is intended here
  console.log(result);
  ```

- Don't change the linter or formatter configuration in a feature PR. If you think a rule should change, open a separate issue or PR for it so it can be discussed.

**Tip:** install the ESLint and Prettier extensions in your editor so problems show up while you type, and turn on "format on save".

### Docs updated if behaviour changed

**What it means:** if your change alters what the app does, how to set it up or how to use it, the written documentation must change in the same PR. Docs that describe old behaviour are worse than no docs, because people trust them.

**Ask yourself:** "If someone read the docs before my change and then used the app after it, would anything surprise them?" If yes, update the docs.

**What to update, and when:**

| If your change... | Update... |
|---|---|
| Changes how to install, run or build the project | `README.md` (setup or usage section) |
| Adds or renames an environment variable | `.env.example` (with a placeholder value) and the setup section of `README.md` |
| Adds, changes or removes an API endpoint, request field or response | The API documentation (endpoint, method, fields, example request and response, error cases) |
| Changes something a user sees or does | User-facing docs or help text, plus screenshots in the PR |
| Makes a significant technical decision | A new ADR in `docs/adr/` (see [Rules](#rules)) |
| Changes how contributors work (branching, tooling, scripts) | This `CONTRIBUTING.md` |
| Involves tricky or non-obvious logic | A short code comment explaining **why** the code does what it does, not **what** it does |

**Good docs are:**

- **In the same PR** as the code change, not "in a follow-up later".
- **Concrete:** they include example commands, requests or values someone can copy.
- **Short:** they explain what the reader needs and link elsewhere for detail.

If no docs need changing, say so in the PR description ("Docs: no behaviour change"), so the reviewer knows you checked.

---

## Worked example

You want to add the ability to log a set during a workout.

1. Open issue **#12** "Allow logging a single set during a workout" with these acceptance criteria:
   - [ ] User can submit exercise, weight and reps
   - [ ] Invalid values (negative weight, 0 reps) are rejected with an error message
2. Branch: `git checkout -b feature/12-log-set`
3. Commit as you go:
   - `feat(workouts): add endpoint to log a set`
   - `test(workouts): cover invalid set input`
   - `docs: document set logging endpoint`
4. Push and open a PR titled `feat(workouts): add set logging`, fill in the template, and write `Closes #12`.
5. A teammate reviews and approves within 24–48 hours.
6. You click **Squash and merge**. `main` gets one commit, `feat(workouts): add set logging`, issue #12 closes automatically, and you delete the branch.

---

## Additional info: Git tips

### Keeping your branch up to date

If `main` moved on while you were working (a teammate merged something), bring those changes into your branch:

```bash
git switch main && git pull
git switch feature/12-log-set
git merge main
```

**In VS Code (no typing):**

1. Click the **branch name** in the status bar and pick **`main`**.
2. In **Source Control**, open the **`...`** menu and choose **Pull**.
3. Click the branch name again and pick your branch (`feature/12-log-set`).
4. Open the **`...`** menu and choose **Branch → Merge...**, then pick **`main`**. You can also press `Ctrl+Shift+P` and run **Git: Merge Branch...**.

<!-- TODO: add GIF: ![Merging main into your branch in VS Code](docs/images/vscode-merge-main.gif) -->

Do this often. Small, frequent syncs mean small conflicts. After merging, run the tests again and push.

### Merge conflicts: don't panic

A **conflict** happens when two branches changed the same lines. Git can't decide which version to keep, so it marks the file like this:

```text
<<<<<<< HEAD
const MAX_SETS = 10;
=======
const MAX_SETS = 8;
>>>>>>> main
```

The top section (between `<<<<<<< HEAD` and `=======`) is **your** version. The bottom section (between `=======` and `>>>>>>> main`) is the **incoming** one.

To resolve it:

1. Edit the file to what it *should* be. That might be your version, theirs, or a combination.
2. Delete the marker lines (`<<<<<<<`, `=======`, `>>>>>>>`).
3. Mark the file as resolved and finish the merge:

   ```bash
   git add <file>
   git commit
   ```

**In VS Code (no typing):**

1. Conflicted files appear in **Source Control** under **Merge Changes**, marked with a **!**. Click one to open it.
2. Above each conflict, VS Code shows clickable options:
   - **Accept Current Change**: keep your version.
   - **Accept Incoming Change**: keep the version from `main`.
   - **Accept Both Changes**: keep both, one after the other. You'll usually need to tidy up afterwards.
   - **Compare Changes**: view the two versions side by side.
3. For bigger conflicts, click **Resolve in Merge Editor** (bottom right of the file). It shows your version, the incoming version and the result in separate panes, with checkboxes to choose what goes in.
4. When no conflict markers are left, save the file and click **+** next to it to stage it.
5. Once every conflicted file is staged, click **✓ Commit**. The message is pre-filled.

<!-- TODO: add GIF: ![Resolving a merge conflict in VS Code](docs/images/vscode-merge-conflict.gif) -->

If you're unsure which version is right, ask the teammate who wrote the other one. To give up and go back to how things were before the merge, run `git merge --abort`. In VS Code, press `Ctrl+Shift+P` and run **Git: Abort Merge**.

### Emergency commands

| Situation | Command | In VS Code (no typing) |
|---|---|---|
| Undo changes to a file you haven't committed | `git restore <file>` | Source Control → hover the file under **Changes** → click **↶ Discard Changes** |
| Unstage a file you `git add`-ed by mistake | `git restore --staged <file>` | Source Control → hover the file under **Staged Changes** → click **−** (*Unstage Changes*) |
| Fix the message of your last commit (before pushing) | `git commit --amend` | Source Control → **`...`** → **Commit → Commit (Amend)** |
| Set work aside temporarily to switch branches | `git stash`, later `git stash pop` | Source Control → **`...`** → **Stash → Stash**, later **Stash → Pop Latest Stash** |
| See the history | `git log --oneline --graph` | Source Control → expand the **Graph** section at the bottom. For one file's history: right-click the file → **Open Timeline** |
| Undo a commit that's already on `main` | `git revert <commit-id>` (makes a new commit that reverses it) | On GitHub, open the merged PR and click **Revert**. This creates a new PR that undoes it, so the change still goes through review |

<!-- TODO: add GIF: ![Discarding and unstaging changes in VS Code](docs/images/vscode-discard-unstage.gif) -->
<!-- TODO: add GIF: ![Stashing in VS Code](docs/images/vscode-stash.gif) -->

> Because `main` is protected, a revert also has to go through a branch and a PR. GitHub's **Revert** button does this for you.

> ⚠️ `git restore <file>` **permanently discards** your uncommitted changes to that file. There is no undo.

### Golden rules

- **Never commit directly to `main`.**
- **Never use `git push --force` on a shared branch.** It overwrites other people's work.
- **Pull before you start work each day.**

---

## Glossary

| Term | Meaning |
|---|---|
| **Issue** | A GitHub ticket describing a bug, feature or task. Every issue has a number (e.g. `#12`). |
| **Acceptance criteria** | A checklist in the issue listing specific, testable conditions that must be true for the work to be considered complete. |
| **`main`** | The primary branch. It should always contain working, reviewed code. |
| **Branch** | A separate line of development where you make changes without affecting `main`. |
| **Commit** | A saved snapshot of changes in git, with a message describing them. |
| **Conventional Commits** | A standard format for commit messages: `type(scope): summary`. See [conventionalcommits.org](https://www.conventionalcommits.org/). |
| **Scope** | Optional part of a commit message naming the affected area, e.g. `(auth)` in `fix(auth): ...`. |
| **Breaking change** | A change that forces other code or users to adapt (e.g. removing or renaming an API field). Marked with `!` and a `BREAKING CHANGE:` footer. |
| **PR (Pull Request)** | A request on GitHub to merge your branch into `main`. It is where code review happens. |
| **Draft PR** | A PR marked as not ready for review yet. Useful for sharing work in progress early. |
| **PR template** | A pre-filled description form that appears automatically when you open a PR (stored in `.github/pull_request_template.md`). |
| **`Closes #N`** | A keyword in the PR description that links the PR to issue number **N** and closes that issue automatically on merge. N is the **issue** number, not the PR number. |
| **Approval** | A reviewer marking the PR as "Approve" on GitHub, meaning they're happy for it to be merged. |
| **Review / code review** | Another contributor reading your changes to catch bugs, suggest improvements and share knowledge. |
| **Nit** | Short for "nitpick": a minor, optional review suggestion. |
| **CI (Continuous Integration)** | Automated checks (tests, lint) that run on GitHub for every PR. |
| **Lint / linter** | A tool that automatically checks code for style problems and likely bugs. |
| **Squash merge** | A merge method that combines all commits from a branch into a single commit on `main`, keeping history clean. |
| **Merge conflict** | When git can't automatically combine your changes with changes on `main` because both edited the same lines. You must resolve it by hand. |
| **Rebase** | An alternative to merging `main` into your branch: it replays your commits on top of the latest `main`. |
| **ADR (Architecture Decision Record)** | A short document recording an important technical decision, why it was made and which alternatives were considered. Stored in `docs/adr/`. |
| **Secret** | Any sensitive credential: passwords, API keys, tokens, private keys, connection strings. |
| **`.env`** | A local, git-ignored file holding environment variables such as secrets and config. Never committed. |
| **`.env.example`** | A committed template of `.env` listing every variable name with placeholder values, so others know what to set. |
| **Rotate a secret** | Revoke a leaked credential and replace it with a new one. |
| **Definition of Done (DoD)** | The shared checklist that every PR must satisfy before it can be merged. |
| **Changed lines** | Lines added plus lines removed in a PR, as shown by GitHub's diff counter. |
| **Test suite** | All of the project's tests together, run with `npm test`. |
| **Unit / integration / E2E test** | Tests of one piece on its own, of several pieces working together, and of the whole app driven like a real user, respectively. |
| **Regression test** | A test written for a bug fix that fails while the bug exists and passes once it's fixed, so the bug can't silently return. |
| **Edge case** | An unusual or extreme input, such as empty, zero, negative or very large values. |
| **Mock** | A fake stand-in for something a test shouldn't really call (a network service, the clock, the database) so the test stays fast and predictable. |
| **Flaky test** | A test that sometimes passes and sometimes fails without any code change. Treated as failing. |
| **Coverage** | How much of the code is run by the tests, usually shown as a percentage. |
| **Formatter** | A tool (e.g. Prettier) that automatically rewrites code layout to a consistent style. |
