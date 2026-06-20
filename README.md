# FSAE EV · Team Progress Tracker

Two-tab (Weekly / Monthly) team tracker with GitHub storage and Cloudflare Worker auth proxy.
Division leads only need a **password** — the GitHub token lives in Cloudflare.

---

## Files

| File | Purpose |
|---|---|
| `index.html` | The entire dashboard (upload to GitHub repo root) |
| `data.json` | Starts empty `{}` — grows as data is saved |
| `worker.js` | Cloudflare Worker — deploy once, handles all saves |

---

## Step-by-step deploy

### 1 — Create the GitHub repo
1. [github.com](https://github.com) → **+ New repository**
2. Name: `fsae-tracker` · Visibility: **Public**
3. Create repository

### 2 — Upload files to the repo
1. In the repo, click **uploading an existing file**
2. Upload `index.html` and `data.json`
3. Commit changes

### 3 — Enable GitHub Pages
1. Repo → **Settings → Pages**
2. Source: `main` branch, `/ (root)` folder → **Save**
3. After ~60 seconds your dashboard is live at:
   `https://<username>.github.io/fsae-tracker/`

### 4 — Create a GitHub Personal Access Token
1. Go to [github.com/settings/tokens/new](https://github.com/settings/tokens/new)
2. Note: `FSAE Tracker Worker`
3. Expiration: set past your competition date (or No expiration)
4. Scopes: tick **repo** (top-level checkbox)
5. **Generate token** — copy it immediately (shown only once)

### 5 — Deploy the Cloudflare Worker
1. Sign up free at [cloudflare.com](https://cloudflare.com)
2. Dashboard → **Workers & Pages** → **Create application** → **Create Worker**
3. Name it `fsae-tracker` → **Deploy**
4. Click **Edit code** → paste the entire contents of `worker.js` → **Save and deploy**
5. Go to **Settings → Variables** → add these environment variables:

| Variable | Value |
|---|---|
| `GITHUB_TOKEN` | The PAT from Step 4 |
| `GITHUB_OWNER` | Your GitHub username |
| `GITHUB_REPO` | `fsae-tracker` |
| `LEAD_PASSWORD` | A password of your choice (e.g. `fsae2025`) |

6. Click **Encrypt** on GITHUB_TOKEN and LEAD_PASSWORD for security
7. Copy your Worker URL — it looks like `https://fsae-tracker.yourname.workers.dev`

### 6 — Connect the dashboard
1. Open the dashboard (GitHub Pages URL)
2. Click the **⚙** button (top right)
3. Enter:
   - **Worker URL:** the URL from Step 5
   - **Lead Password:** the LEAD_PASSWORD you set
4. Click **Save & Test Connection** — you'll see `✓ Synced` if it works

---

## Daily use

### For division leads
- Open the dashboard URL → click **→ Lead Edit** → enter password
- **Weekly tab** → pick your division → add tasks, check them off, log meetings
- **Monthly tab** → same workflow for monthly goals
- All saves auto-commit to `data.json` in the repo

### For viewers (whole team)
- Just open the GitHub Pages URL — live data, no setup needed
- Hard-refresh (`Ctrl+Shift+R`) to get latest data

---

## Tabs and sections

| Tab | What it tracks |
|---|---|
| **Weekly** | Short-term sub-goals per week. Each division has Tasks + Meeting + History. |
| **Monthly** | Overall monthly goals. Same structure — Tasks, Meeting, History. |

Weekly and monthly tasks are **completely independent** — monthly goals are not auto-generated from weekly tasks.

Each division's **Meeting** section logs: date, attendees, discussion points, decisions made.

---

## Changing the lead password
Update `LEAD_PASSWORD` in Cloudflare Worker Settings → Variables, then tell leads to re-enter it in the ⚙ Setup panel.
