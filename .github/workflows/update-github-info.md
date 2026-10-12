---
name: update-github-info
on:
  schedule:
    - cron: daily
  workflow_dispatch:

permissions:
  contents: read

tools:
  edit:
  web-fetch:

network:
  allowed:
    - defaults
    - github.blog
    - github.com
    - awesome-copilot.github.com

pre-agent-steps:
  - name: Fetch update sources
    run: |
      set -euo pipefail
      mkdir -p "$GITHUB_WORKSPACE/.github/aw-source-cache"
      curl --fail --show-error --silent --location --max-time 40 https://github.blog/latest/ --output "$GITHUB_WORKSPACE/.github/aw-source-cache/github-blog-latest.html"
      curl --fail --show-error --silent --location --max-time 40 https://github.blog/changelog/ --output "$GITHUB_WORKSPACE/.github/aw-source-cache/github-blog-changelog.html"
      curl --fail --show-error --silent --location --max-time 40 https://awesome-copilot.github.com/workflows/ --output "$GITHUB_WORKSPACE/.github/aw-source-cache/awesome-copilot-workflows.html"

safe-outputs:
  create-pull-request:
    allowed-files:
      - site/content/github-info.md
---

Update the repository's GitHub information for Mona:

1. Read `notes/mona-notes.md` for Mona's guidance and priorities.
2. Fetch `https://github.blog/latest/`, `https://github.blog/changelog/`, and `https://awesome-copilot.github.com/workflows/` with the web-fetch tool when it is available.
3. The workflow has also downloaded those sources to `.github/aw-source-cache/github-blog-latest.html`, `.github/aw-source-cache/github-blog-changelog.html`, and `.github/aw-source-cache/awesome-copilot-workflows.html`. Read these local files instead if web-fetch is unavailable. These cache files are input only; do not add, edit, or commit them.
4. Update `site/content/github-info.md` with relevant, current information from those sources while preserving the file's existing style and structure.
5. Open a pull request with the changes for Mona to review. Do not write directly to `main`.
