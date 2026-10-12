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

safe-outputs:
  create-pull-request:
    allowed-files:
      - site/content/github-info.md
---

Update the repository's GitHub information for Mona:

1. Read `notes/mona-notes.md` for Mona's guidance and priorities.
2. Fetch `https://github.blog/latest/` and `https://github.blog/changelog/` with the web-fetch tool.
3. Update `site/content/github-info.md` with relevant, current information from those sources while preserving the file's existing style and structure.
4. Open a pull request with the changes for Mona to review. Do not write directly to `main`.
