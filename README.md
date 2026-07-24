# Forks Upgrader

Central repository to keep all your forks automatically synced with their upstream (original) repositories.

## How it works

A GitHub Action iterates over every fork in your account and runs `gh repo sync` on each one. It triggers:

- **Automatically** every Monday at midnight (`cron: '0 0 * * 1'`).
- **Manually** from the *Actions* tab → *Sync All Forks* → *Run workflow*.

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

### 3. Run it

- **Manual:** *Actions* tab → select *Sync All Forks* → *Run workflow*.
- **Automatic:** runs on its own every Monday.

## Notes

- If a fork has local changes that conflict with upstream, that repo's sync will fail and be reported in the summary, but the rest will continue.
- The repository limit per run is 200 (adjust `--limit` in the workflow if you need more).
