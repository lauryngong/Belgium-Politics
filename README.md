# Data and Software Belgium Blog - Summer 2025

This repository contains a template for your Team's Blog. It is a Hugo-based blog uses a customized Blowfish theme. Blowfish supports multiple authors.

## Local Development Setup

**Note:** Everyone needs a GitHub account!

<!-- ### Prerequisites

- Git
- Hugo (Extended version) -->

### Installing Hugo

#### Installing Hugo on macOS

Using Homebrew:

```bash
brew install hugo
```

#### Installing Hugo on Windows

1. Download the latest release from [Hugo Releases](https://github.com/gohugoio/hugo/releases)
2. Extract the downloaded zip file
3. Add the Hugo executable to your system PATH

### Getting Started

1. One team member needs to fork the repo. We'll refer to this team member as the _blog owner_. **Change the name to your team name if you have one. If not, no better time to come up with one.**
1. The blog owner should add remaining team members as collaborators in GitHub. you can do this from the repo's Settings Tab > Collaborators and Teams. Each team member will receive an invitation to join the repo via email which they will need to accept.
1. Clone the team's repository:

```bash
git clone [repository-url]
cd
```

2. Start the development server:

```bash
hugo server -D
```

The site will be available at `http://localhost:1313`

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
