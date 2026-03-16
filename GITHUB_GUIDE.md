# 📖 VANA — GitHub Setup & Deployment Guide

Complete step-by-step instructions to get VANA live on GitHub Pages.
Estimated time: **10–15 minutes**.

---

## What you will have at the end

- A **private/public GitHub repository** storing all your VANA code
- A **live public URL** — `https://YOUR_USERNAME.github.io/vana/` — accessible from any browser, phone, or tablet
- **Auto-deployment** — every time you save a change and push to GitHub, the live site updates automatically within 90 seconds

---

## Before you start — checklist

Make sure you have:
- [ ] A GitHub account at [github.com](https://github.com) (free)
- [ ] Git installed on your computer — check by opening Terminal and typing `git --version`
- [ ] The `vana-v2.zip` file downloaded and unzipped on your computer
- [ ] A code editor (VS Code recommended — free at [code.visualstudio.com](https://code.visualstudio.com))

---

## Part 1 — Create the repository on GitHub

### Step 1 — Go to github.com and sign in

### Step 2 — Create a new repository
Click the **＋** icon in the top-right corner → **New repository**

Fill in the form exactly like this:

| Field | What to enter |
|-------|--------------|
| Repository name | `vana` |
| Description | Vegetation & Avian Native Atlas — biodiversity intelligence for landscape design |
| Visibility | **Public** ← important: GitHub Pages requires Public on the free plan |
| Initialize with README | **Leave unchecked** ← you already have a README |
| Add .gitignore | None |
| Choose a licence | None |

Click **Create repository**

### Step 3 — You will see a blank repository page
GitHub shows a page with setup instructions. Leave this page open — you will need the repository URL shown there.

The URL will look like:
```
https://github.com/YOUR_USERNAME/vana.git
```

---

## Part 2 — Push your files from your computer

### Step 4 — Open Terminal (Mac) or Command Prompt (Windows)

**Mac:** Press `Cmd + Space` → type `Terminal` → press Enter

**Windows:** Press `Windows key` → type `cmd` → press Enter

### Step 5 — Navigate to your VANA folder
Type this command, replacing the path with where you unzipped the file:

```bash
cd /path/to/your/vana
```

**Examples:**
```bash
# Mac — if unzipped to Downloads
cd ~/Downloads/vana

# Windows — if unzipped to Downloads
cd C:\Users\YourName\Downloads\vana
```

To confirm you are in the right place, type:
```bash
ls
```
You should see these files listed:
```
README.md
GITHUB_GUIDE.md
index.html
data/
.github/
```

### Step 6 — Initialise git in this folder

```bash
git init
```

You will see: `Initialized empty Git repository in .../vana/.git/`

### Step 7 — Add all files to git

```bash
git add .
```

The `.` means "add everything in this folder". No output means it worked.

### Step 8 — Create your first commit

```bash
git commit -m "VANA v1.0 — initial release"
```

You will see a list of files being committed. This is normal.

### Step 9 — Set the branch name to main

```bash
git branch -M main
```

No output means it worked.

### Step 10 — Connect your local folder to GitHub

Copy the repository URL from the GitHub page you left open in Step 3. Replace `YOUR_USERNAME` below with your actual GitHub username:

```bash
git remote add origin https://github.com/YOUR_USERNAME/vana.git
```

**Example:**
```bash
git remote add origin https://github.com/greenscapedesignz/vana.git
```

### Step 11 — Push your files to GitHub

```bash
git push -u origin main
```

GitHub will ask for your username and password.

> **Important for password:** GitHub no longer accepts your account password here. You need a **Personal Access Token** instead.
>
> To create one:
> 1. Go to github.com → click your profile photo → **Settings**
> 2. Scroll down to **Developer settings** (bottom of left sidebar)
> 3. Click **Personal access tokens** → **Tokens (classic)**
> 4. Click **Generate new token (classic)**
> 5. Set Note: `vana deploy`
> 6. Set Expiration: 90 days
> 7. Check the **repo** checkbox (top option)
> 8. Click **Generate token**
> 9. **Copy the token immediately** — GitHub only shows it once
> 10. Paste this token when Terminal asks for your password

You will see output like:
```
Enumerating objects: 8, done.
Counting objects: 100% (8/8), done.
Writing objects: 100% (8/8), 72.45 KiB | 2.1 MiB/s, done.
Branch 'main' set up to track remote branch 'main' from 'origin'.
```

Your files are now on GitHub. Refresh the GitHub repository page to see them.

---

## Part 3 — Enable GitHub Pages

### Step 12 — Go to repository Settings
On your repository page on GitHub, click the **Settings** tab (near the top, with a gear icon).

### Step 13 — Open Pages settings
In the left sidebar, scroll down and click **Pages**.

### Step 14 — Set the source
Under **Build and deployment**:
- Source: select **GitHub Actions** from the dropdown

Click **Save** if a save button appears.

### Step 15 — Wait for the first deployment
The `.github/workflows/deploy.yml` file you pushed will now run automatically.

To watch it:
1. Click the **Actions** tab on your repository page
2. You will see a workflow called **Deploy VANA to GitHub Pages** running
3. A yellow circle = in progress. Green tick = successful. Red cross = error.
4. First deployment takes approximately 60–90 seconds.

### Step 16 — Get your live URL
Once the workflow shows a green tick:
1. Go back to **Settings** → **Pages**
2. At the top you will see:

```
Your site is live at https://YOUR_USERNAME.github.io/vana/
```

3. Click **Visit site** to open your live VANA app

---

## Part 4 — Making changes and updating the live site

Every time you make a change to any file, you run these 3 commands to update the live site:

```bash
# 1. Stage your changes
git add .

# 2. Commit with a note about what you changed
git commit -m "Add new species to insectivore guild"

# 3. Push to GitHub — live site updates in ~90 seconds
git push
```

That is it. No other steps needed.

---

## Useful commands reference

```bash
# Check which files have changed since last commit
git status

# See the history of commits
git log --oneline

# See what changed in a specific file
git diff index.html

# Undo changes to a file before committing (revert to last committed version)
git checkout -- index.html

# See which remote repository you are connected to
git remote -v
```

---

## Troubleshooting

### "Permission denied" when pushing
Your Personal Access Token may have expired or been entered incorrectly. Generate a new one (Step 11 instructions) and try again.

### "Repository not found" error
Check that the URL in your `git remote add origin` command exactly matches your GitHub repository URL. Verify with:
```bash
git remote -v
```

### GitHub Actions workflow fails (red cross)
Click on the failed workflow in the Actions tab to see the error message. The most common cause is a file permission issue — run:
```bash
git add .
git commit -m "Fix file permissions"
git push
```

### Site shows a blank page or 404
- Check that GitHub Pages source is set to **GitHub Actions** (not "Deploy from a branch")
- Check that the Actions workflow completed successfully (green tick)
- The URL must include `/vana/` at the end: `https://YOUR_USERNAME.github.io/vana/`
- Wait 2–3 minutes after a green tick — DNS propagation can take a moment

### Changes are not showing on the live site
- Confirm your push went through: `git push` should show "Everything up-to-date" or a transfer summary
- Check the Actions tab — a new workflow should have triggered within 30 seconds of your push
- Try a hard refresh in your browser: `Cmd + Shift + R` (Mac) or `Ctrl + Shift + R` (Windows)

---

## What the repository looks like on GitHub

After successful setup, your repository at `github.com/YOUR_USERNAME/vana` will show:

```
📁 .github/workflows/
   📄 deploy.yml          ← auto-deployment workflow
📁 data/
   📄 plants.json         ← plant database
   📄 species.json        ← species database
📄 GITHUB_GUIDE.md        ← this guide
📄 README.md              ← project documentation
📄 index.html             ← main application
```

The README.md content displays automatically below the file list on GitHub, giving anyone who visits your repository a clear overview of the project.

---

## Sharing the app

Once live, you can share the URL with:

- **Clients** — send the URL directly, works on any phone or tablet
- **Colleagues** — no login required to view
- **Developers** — send the repository URL for them to clone and continue building

To make the repository **private** (so only you can see the code, but the site stays public):
- Settings → General → Danger Zone → Change repository visibility → Private
- Note: GitHub Pages sites remain publicly accessible even when the repository is private — on the free plan this requires a paid account. On the free plan, keep the repository Public.

---

*Questions or issues? Open an issue on the repository or contact greenscapedesignz.com*
