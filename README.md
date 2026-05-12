# Data and Software Belgium Blog - Summer 2026

This repository contains a template for your Team's Blog. It is a Hugo-based blog uses a customized Blowfish theme. Blowfish supports multiple authors.

## Local Development Setup

**Note:** Everyone needs a GitHub account!

<!-- ### Prerequisites

- Git
- Hugo (Extended version) -->

### Installing Hugo

You need the **extended** version of Hugo. The Blowfish theme uses SCSS, which the standard build cannot compile. To avoid "works on my machine" surprises, install the same version that our deployment workflow uses: **v0.124.0** (pinned in `.github/workflows/hugo.yaml`). A close minor version is usually fine, but if you see a build failure locally that doesn't reproduce in CI (or vice versa), version drift is the first thing to check.

After installing, verify with:

```bash
hugo version
```

You should see `extended` in the output (e.g. `hugo v0.124.0+extended ...`).

#### macOS

Using Homebrew (installs the extended build by default):

```bash
brew install hugo
```

#### Windows

Pick one of these — in order of ease:

1. **winget** (built into Windows 10/11):
   ```powershell
   winget install Hugo.Hugo.Extended
   ```
2. **Chocolatey**:
   ```powershell
   choco install hugo-extended
   ```
3. **Manual install**: download `hugo_extended_<version>_windows-amd64.zip` from [Hugo Releases](https://github.com/gohugoio/hugo/releases), extract it to a folder like `C:\Hugo\bin`, then add that folder to your PATH (Start → "Edit environment variables for your account" → Path → New). Open a **new** terminal before running `hugo version`.
4. **WSL** (recommended if you already use it): install inside Ubuntu/WSL with `sudo apt install hugo` — but check the version, as Ubuntu's apt repo can lag. Snap (`sudo snap install hugo`) is usually closer to current.

#### Linux

Snap gives you a current extended build:

```bash
sudo snap install hugo
```

### Getting Started

1. One team member needs to fork the repo. We'll refer to this team member as the _blog owner_. **Change the repo name to your team name if you have one. If not, no better time to come up with one.**
2. The blog owner should add remaining team members as collaborators in GitHub. You can do this from the repo's Settings tab → Collaborators and Teams. Each team member will receive an invitation via email which they will need to accept.
3. **Enable GitHub Pages** (see the next section — do this once, right after forking, before anyone pushes code).
4. Clone the team's repository:

   ```bash
   git clone <repository-url>
   cd <repository-name>
   ```

5. Start the development server:

   ```bash
   hugo server -D
   ```

   The site will be available at `http://localhost:1313`. The `-D` flag includes drafts; leave it off to preview exactly what will deploy.

## Deploying to GitHub Pages

This repo ships with a GitHub Actions workflow (`.github/workflows/hugo.yaml`) that builds and deploys the site on every push to `main`. You do **not** need to commit a `public/` folder or run any build commands yourself — the workflow handles it.

### One-time setup (blog owner does this once, right after forking)

1. Go to your forked repo on GitHub.
2. Click **Settings → Pages**.
3. Under **Build and deployment → Source**, select **GitHub Actions**. *(This is the step almost everyone misses. If Pages is left on "Deploy from a branch", the workflow will appear to succeed but nothing will publish.)*
4. If your fork lives under a GitHub **organization** rather than a personal account, the org owner may need to enable GitHub Pages for the org first under the org's Settings → Pages.
5. Push any commit (or re-run the workflow from the Actions tab) to trigger the first deployment.

### Where your site will live

Your published site URL will be:

```
https://<github-username-or-org>.github.io/<repo-name>/
```

The workflow sets Hugo's `baseURL` automatically — **do not hardcode a URL** in `hugo.toml` or it will break the deployed site.

### Watching a deploy

1. After you push to `main`, open the **Actions** tab in GitHub.
2. Click the most recent "Deploy Hugo site to Pages" run.
3. A green check means the site is live at the URL above. A red X means the build failed — click into the failed step to see the error.

### Common deploy problems

- **Site loads but is blank / CSS missing** → Source isn't set to "GitHub Actions" (see step 3 above), or someone hardcoded `baseURL` in the Hugo config.
- **Build is green but my post isn't showing** → Check `draft: false` and that the `date:` is not in the future.
- **Build fails on a file path** → GitHub's runner is Linux and case-sensitive. `MyImage.JPG` referenced as `myimage.jpg` works on macOS/Windows locally but breaks the deploy. Match filename case exactly.
- **Build fails with a SCSS or theme error** → Likely a Hugo version mismatch. Check `hugo version` locally against the version pinned in `.github/workflows/hugo.yaml`.

## Working as a Team

Multiple students pushing to the same repo is where most groups get stuck. The rules below are simple but worth following from day one — recovering from a tangled history mid-quarter is much more painful than preventing it.

### Pick a branching convention — and stick to it

Pick **one** of these as a team. Don't mix them.

- **Option A — Branch per post (recommended).** Each author creates a short-lived branch for each post (`git checkout -b post/eric-week1-recap`), pushes it, and opens a Pull Request into `main`. Another teammate reviews and merges. This keeps `main` deployable and gives you a natural review checkpoint. The tradeoff: it's a little more git overhead.
- **Option B — Everyone commits to `main`.** Simpler, but you **must** pull before you start and push often, and one person's bad commit can break the deployed site for everyone. Only choose this if your team is small and comfortable with git.

Whichever you pick, write it in your team's pinned Slack/Teams message so new contributors don't guess.

### Daily git rhythm

Regardless of which option you picked:

1. **Pull before you start working.** `git pull` (or `git pull --rebase` if your team prefers a linear history). Skipping this is the #1 cause of merge conflicts.
2. **Commit small, commit often.** One commit per logical change ("Add week 1 recap post", "Fix image path in week 1 post"). Don't batch a week of work into one commit.
3. **Push often.** Every time you reach a stopping point. Unpushed commits are invisible to your teammates and not backed up.
4. **Read the commit message before merging a PR.** If you don't understand what changed, ask before clicking merge.

### When two people edit the same file

You'll get a merge conflict. Don't panic, and don't `git reset --hard` to "make it go away" — that throws away work.

- Open the conflicted file. You'll see `<<<<<<<`, `=======`, and `>>>>>>>` markers showing both versions.
- Decide what the file *should* look like (often: keep both changes), delete the markers, save.
- `git add <file>` and finish the merge/rebase.
- If you're not sure, **ask a teammate before committing** — undoing a bad conflict resolution is harder than getting it right the first time.

### "My post shows up locally but not on the deployed site"

This is the most common confusion. Check, in order:

1. Did you push? `git status` should say "Your branch is up to date with 'origin/main'."
2. Is your PR merged into `main`? (Option A only.)
3. Did the Actions run go green? Open the **Actions** tab on GitHub.
4. Is `draft: false` in your frontmatter? `hugo server -D` shows drafts locally, but the deployed site does not.
5. Is your `date:` in the past? Future-dated posts are hidden by default on the deployed site.
6. Hard-refresh your browser (Cmd/Ctrl+Shift+R). GitHub Pages caches aggressively.

### Things to avoid

- **Don't force push to `main`** (`git push --force`). It will erase your teammates' work without warning.
- **Don't commit the `public/` folder.** It's the build output — the Actions workflow generates it. It's already gitignored; don't add it back.
- **Don't edit someone else's author JSON or content folder** without telling them. Each person owns their own `content/<AuthorName>/` directory and `data/authors/<name>.json` file.

## Adding New Authors

1. For each Team Member, update the `data/authors` folder to include a new `.json` file based on the examples included.
2. For each Team Member, create a new folder in `content`. Inside each, add an `_index.md` file

## Creating New Content

### Basic Post Structure

1. Create a new markdown file in your author directory or in `content/team_posts/`
2. Use the following frontmatter structure:

```markdown
---
title: "Your Post Title"
description: "Brief description of your post"
date: YYYY-MM-DD
draft: false
tags: ["tag1", "tag2"]
categories: ["category1"]
authors:
  - "eric_gerber"
  - "mark_fontenot"
---
```

### Markdown Basics

- Use `#` for headings (e.g., `# Heading 1`, `## Heading 2`)
- Use `*` or `_` for emphasis (`*italic*`, `**bold**`)
- Create lists with `-` or `1.`
- Create links with `[text](url)`
- Create code blocks with triple backticks (```)

## Working with Images and Assets

### Adding Images

1. Place your images in the `assets` directory
2. Reference images in your markdown using the following syntax:

```markdown
![Alt text](/your-image.jpg)
```

### Best Practices for Images

- Use descriptive filenames
- Optimize images for web use
- Recommended formats: JPG for photos, PNG for graphics with transparency
- Keep image sizes reasonable (recommended max width: 1200px)

## Additional Resources

- [Hugo Documentation](https://gohugo.io/documentation/)
- [Markdown Guide](https://www.markdownguide.org/)
- [Blowfish Theme Documentation](https://blowfish.page/)
