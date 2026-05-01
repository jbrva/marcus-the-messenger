# Deploying Marcus to Cloudflare Pages

A reference guide for deploying this project via GitHub → Cloudflare Pages auto-deploy. Follow it once, then forget about it — after setup, every `git push` triggers a redeploy automatically.

**Status:** ✅ Deployed and live. Pending: merge the wrangler config PR (Step 2½) and optional polish items.

**Live URL:**

```
https://marcus-the-messenger.ctso.workers.dev/
```

## The big picture

```
Local edits  →  git push  →  GitHub repo  →  Cloudflare Pages  →  live URL
                                              (auto-detects push)
```

You do `git push`. Cloudflare watches the repo, sees the new commit, rebuilds (which for a static site is just "copy files to edge"), and your changes are live in about 30 seconds.

Bonus: every pull request gets its own preview URL automatically (`pr-N.your-project.pages.dev`), so you can share work-in-progress without touching production.

## One-time setup

### Step 1 — GitHub repo

- [x] `git init` in the project folder
- [x] `git add .`
- [x] `git commit -m "Initial commit: Marcus and the Lost Scroll"`
- [x] `gh repo create marcus-the-messenger --public --source=. --push`

Result: `github.com/jbrva/marcus-the-messenger` is live with all four project files on `main`.

### Step 2 — Cloudflare Pages

Open the Cloudflare dashboard:

```
https://dash.cloudflare.com
```

Sign in, then:

- [x] Left sidebar → **Workers & Pages**
- [x] Click the blue **Create** button
- [x] In the dialog, pick the **Pages** tab → **Connect to Git**
- [x] Pick **GitHub** as the provider
- [x] In the GitHub OAuth window, authorize the Cloudflare Pages app
- [x] When GitHub asks where to install: pick **Only select repositories**, choose `jbrva/marcus-the-messenger`, click **Install**  *(installed with "All repositories" — narrowing tracked under Optional polish)*
- [x] Back in Cloudflare with the repo selected → click **Begin setup**
- [x] Fill in build settings exactly as below, then click **Save and Deploy**:

| Field | Value |
|-------|-------|
| Project name | `marcus-the-messenger` (becomes the subdomain — pick something you'll be happy seeing forever; project names are immutable) |
| Production branch | `main` |
| Framework preset | None |
| Build command | *(leave blank)* |
| Build output directory | `/` |
| Root directory | *(leave blank)* |
| Environment variables | *(none)* |

- [x] Wait ~30 seconds for the first build to finish — success message will show a clickable URL
- [x] Open the live URL and confirm the game loads:

```
https://marcus-the-messenger.ctso.workers.dev/
```

### Step 2½ — Accept the wrangler config PR

After the first deploy, Cloudflare opens a PR titled **"Add Cloudflare Workers configuration"** on your repo. Merging it adds a `wrangler.jsonc` file that codifies your build settings, which prevents future build failures and makes the warnings about `workers_dev` / `preview_urls` go away.

Open the PR list:

```
https://github.com/jbrva/marcus-the-messenger/pulls
```

- [ ] Click the **"Add Cloudflare Workers configuration"** PR (#1)
- [ ] Click **Files changed** and skim — should be a small `wrangler.jsonc` plus possibly one other config file
- [ ] Back on **Conversation** tab, scroll to the bottom and click **Squash and merge** → **Confirm squash and merge**
- [ ] Pull the change down locally so your working copy matches:

```bash
cd "/Users/jbirthisel/Documents/Claude/Projects/Roman Numerals Game"
git pull
```

After merging, future deploys read settings from the committed `wrangler.jsonc` and the warnings about default-enabled `workers_dev` / `preview_urls` disappear.

### Project dashboard

Bookmark this for direct access to the project's deploys, settings, and custom domains:

```
https://dash.cloudflare.com/?to=/:account/workers-and-pages/view/marcus-the-messenger
```

(Cloudflare auto-fills your account ID after sign-in.)

## Day-to-day workflow (after setup)

Whenever you want to update the live game, from the project folder:

```bash
git add .
git commit -m "Describe what changed"
git push
```

Cloudflare automatically deploys within ~30 seconds. You can watch the build progress in the dashboard under **Workers & Pages → marcus-the-messenger → Deployments**.

## What gets served

After deploy, these URLs work (each in a copy block):

The game:

```
https://marcus-the-messenger.ctso.workers.dev/
```

The printable cheat sheet:

```
https://marcus-the-messenger.ctso.workers.dev/cheat-sheet.html
```

The handoff doc (serves as plain text):

```
https://marcus-the-messenger.ctso.workers.dev/README.md
```

The parent/teacher guide (serves as plain text):

```
https://marcus-the-messenger.ctso.workers.dev/for-parents-and-teachers.md
```

This deploy guide (serves as plain text):

```
https://marcus-the-messenger.ctso.workers.dev/DEPLOY.md
```

The markdown files serve as plain text. That's normal — browsers don't render markdown. If you want them hidden from the public site, see "Optional polish" below.

## Optional polish

These are nice-to-haves, not blockers. Tackle whichever you care about, in any order.

### Narrow Cloudflare's GitHub access to just this repo

If you accidentally granted Cloudflare access to **All repositories** during setup, narrow it down. This won't break the deploy as long as `marcus-the-messenger` stays in the selected list.

Open GitHub's installed apps page:

```
https://github.com/settings/installations
```

Then:

- [ ] Find **Cloudflare Pages** → click **Configure**
- [ ] Scroll to **Repository access** → switch to **Only select repositories**
- [ ] Click **Select repositories** → choose `marcus-the-messenger`
- [ ] Click **Save**

The webhook to `marcus-the-messenger` keeps working; Cloudflare just loses the ability to see your other repos.

### Add a `.gitignore`

- [ ] Run from the project folder:

```bash
cat > .gitignore <<'EOF'
.DS_Store
Thumbs.db
*.swp
*~
.vscode/
.idea/
EOF
git add .gitignore
git commit -m "Add .gitignore"
git push
```

Keeps OS junk (especially macOS's `.DS_Store`) out of future commits.

### Hide the markdown docs from the public site

If you'd rather visitors not stumble across `README.md` or `DEPLOY.md`, move all markdown into a `docs/` folder:

- [ ] Run from the project folder:

```bash
mkdir docs
git mv README.md docs/
git mv for-parents-and-teachers.md docs/
git mv DEPLOY.md docs/
git commit -m "Move docs out of webroot"
git push
```

Cloudflare still serves `index.html` and `cheat-sheet.html` from the root; the markdown files live on GitHub but stop being served at the public URL.

### Map a custom domain

Once a deploy is live, in the Cloudflare Pages dashboard:

- [ ] Click your project → **Custom domains** tab → **Set up a custom domain**
- [ ] Enter the domain you want (e.g., `marcus.yourdomain.com`)
- [ ] If the domain's DNS is already managed in your Cloudflare account, the CNAME and TLS cert get configured automatically. If it's elsewhere, follow Cloudflare's instructions on screen.
- [ ] Wait a few minutes for DNS propagation, then test the new URL

### Eliminate font flash on first load

The game pulls Cinzel and Lora from Google Fonts. First-time visitors see a brief flash of system fonts before the Roman lettering loads. To eliminate:

- [ ] Download the Cinzel and Lora `.woff2` files
- [ ] Drop them in a `fonts/` folder in the project
- [ ] Replace the Google Fonts `<link>` tags in `index.html` and `cheat-sheet.html` with `@font-face` rules pointing at the local files
- [ ] Commit and push

Adds ~50KB to first page load but no flash and no Google dependency.

## Troubleshooting

**Build fails with "no output directory found":** Confirm the build output directory is set to `/` (a single forward slash) in the Cloudflare project settings. Pages → your project → Settings → Builds & deployments.

**Custom domain isn't activating:** Check that the DNS record is `Proxied` (orange cloud) in your Cloudflare DNS panel. Pages requires proxied for the auto-issued TLS cert to attach.

**Pushed but Cloudflare didn't deploy:** Check the Cloudflare dashboard → your project → Deployments. If nothing's listed for the latest commit, the GitHub webhook may have stalled — go to the project's **Settings → Builds & deployments** and click **Retry deployment**.

**Want to roll back to a previous version:** Each deploy is preserved indefinitely. In **Deployments**, click the three-dot menu on any past deploy and select **Rollback to this deployment**. Live within seconds.

## Reference

Cloudflare Pages docs:

```
https://developers.cloudflare.com/pages
```

Repo:

```
https://github.com/jbrva/marcus-the-messenger
```

Live URL (after Step 2 completes):

```
https://marcus-the-messenger.ctso.workers.dev
```

GitHub App scope settings (for narrowing repo access):

```
https://github.com/settings/installations
```
