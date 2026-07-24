# My Public Actions

Central repository to automate open source contribution workflows: keep forks synced, discover issues to fix, track your pull requests, watch upstream releases, and measure your contribution activity.

## Available Workflows

| Workflow | File | Schedule | Description |
| --- | --- | --- | --- |
| Sync All Forks | `upgrade_forks.yml` | Daily at 00:00 UTC | Syncs all your forks with their upstream parents |
| Report Fork Issues | `report_fork_issues.yml` | Daily at 12:00 UTC | Generates a report of open issues from upstream repos |
| Track Pull Requests | `track_pull_requests.yml` | Daily at 09:00 UTC | Tracks the status of your open PRs across all repos |
| Watch Upstream Releases | `watch_releases.yml` | Daily at 06:00 UTC | Detects new releases in upstream repos of your forks |
| Hunt Good First Issues | `hunt_good_first_issues.yml` | Every Monday at 10:00 UTC | Finds beginner-friendly issues across popular repos |
| Contribution Stats | `contribution_stats.yml` | 1st of each month at 00:00 UTC | Monthly stats of your open source activity |

All workflows can also be triggered **manually** from the *Actions* tab.

## Setup

### 1. Create a Personal Access Token (PAT)

1. Go to **GitHub → Settings → Developer settings → Personal access tokens → Tokens (classic)**.
2. Click **Generate new token (classic)**.
3. Check the **`repo`** scope (full repository access).
4. Save the generated token (it won't be shown again).

### 2. Add the token as a Secret

1. In this repository, go to **Settings → Secrets and variables → Actions**.
2. Click **New repository secret**.
3. Name: `FORKS_SYNC_TOKEN`
4. Value: paste the token from the previous step.

### 3. Run any workflow

- **Manual:** *Actions* tab → select a workflow → *Run workflow*.
- **Automatic:** each workflow runs on its own schedule (see table above).

---

## Workflow Details

### Sync All Forks (`upgrade_forks.yml`)

Iterates over every fork in your account and runs `gh repo sync` on each one. Runs daily.

### Report Fork Issues (`report_fork_issues.yml`)

Scans the upstream (parent) repositories of all your forks and generates a report of open issues at `reports/issues_report.md`.

Includes: issue number, title, labels, dates, direct links, and counts of `good first issue` / `help wanted` labels.

### Track Pull Requests (`track_pull_requests.yml`)

Monitors all your open PRs across every repository and generates `reports/pr_tracker.md`.

Includes: PR title, repo, draft status, dates, and a **stale PR alert** for PRs with no activity in 14+ days.

### Watch Upstream Releases (`watch_releases.yml`)

Detects new releases in the upstream repos of your forks and generates `reports/release_watcher.md`.

Keeps state between runs (`reports/.release_state.json`) to flag only **new** releases with a 🆕 badge. Tells you when to sync your forks.

### Hunt Good First Issues (`hunt_good_first_issues.yml`)

Searches for `good first issue` and `help wanted` labeled issues across popular languages (TypeScript, JavaScript, Python, Go, Rust, Java, C#, C++) and generates `reports/good_first_issues.md`.

Runs weekly to find fresh opportunities beyond just your forks.

### Contribution Stats (`contribution_stats.yml`)

Generates a monthly activity report at `reports/contribution_stats.md`.

Includes: PRs opened/merged, issues opened/closed, commits authored, repository counts (public + private), and an **activity score**. Tracks trend over time in `reports/.contribution_trend.csv`.

> Private repos are included as **aggregate counts only** — no titles, URLs, or sensitive details are exposed.

---

## Live Reports

The sections below are **auto-updated** by the workflows. Each report is also saved as a standalone file in the `reports/` directory.

### Fork Issues Report

> Full report: [`reports/issues_report.md`](reports/issues_report.md)

<!-- BEGIN: fork-issues -->
_No data yet. Run the **Report Fork Issues** workflow to generate this report._
<!-- END: fork-issues -->

### Pull Request Tracker

> Full report: [`reports/pr_tracker.md`](reports/pr_tracker.md)

<!-- BEGIN: pr-tracker -->
_No data yet. Run the **Track Pull Requests** workflow to generate this report._
<!-- END: pr-tracker -->

### Upstream Releases

> Full report: [`reports/release_watcher.md`](reports/release_watcher.md)

<!-- BEGIN: release-watcher -->
_No data yet. Run the **Watch Upstream Releases** workflow to generate this report._
<!-- END: release-watcher -->

### Good First Issues

> Full report: [`reports/good_first_issues.md`](reports/good_first_issues.md)

<!-- BEGIN: good-first-issues -->
_No data yet. Run the **Hunt Good First Issues** workflow to generate this report._
<!-- END: good-first-issues -->

### Contribution Stats

> Full report: [`reports/contribution_stats.md`](reports/contribution_stats.md)

<!-- BEGIN: contribution-stats -->
_No data yet. Run the **Contribution Stats** workflow to generate this report._
<!-- END: contribution-stats -->

---

## How to use the reports for open source contributions

1. Check the **Good First Issues** or **Fork Issues Report** sections above to find issues to work on.
2. Check the **Upstream Releases** section to ensure your fork is up to date (run *Sync All Forks* if needed).
3. Create a `development` branch in your fork.
4. Work on the fix referencing the issue number in your commits (`Closes #NNN`).
5. Open a Pull Request from your fork to the upstream repository.
6. Monitor your PRs in the **Pull Request Tracker** section and follow up on stale ones.
7. Review your monthly stats in the **Contribution Stats** section to track your progress.

## Notes

- All workflows use the same `FORKS_SYNC_TOKEN` secret — no additional tokens needed.
- If a fork has local changes that conflict with upstream, that repo's sync will fail and be reported in the summary, but the rest will continue.
- Repository limits: 200 forks per run, 30 issues per upstream repo, 100 PRs, 500 commits (adjust `--limit` in each workflow if you need more).
- Reports are committed to the `reports/` directory **and embedded in this README** automatically.
- The GitHub Search API has rate limits; very active accounts may see capped results in Contribution Stats.
