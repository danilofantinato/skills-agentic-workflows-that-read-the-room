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
  bash: [curl]

network:
  allowed:
    - defaults
    - github.blog
    - github.com
    - awesome-copilot.github.com

safe-outputs:
  create-pull-request:
    allowed-files:
      - site/content/github-info.md
---

Update the repository's GitHub information for Mona:

1. Read `notes/mona-notes.md` for Mona's guidance and priorities.
2. Fetch `https://github.blog/latest/` and `https://github.blog/changelog/` with the web-fetch tool.
3. Fetch `https://awesome-copilot.github.com/workflows/` with the web-fetch tool as an Awesome Copilot workflows source.
4. If the web-fetch tool is unavailable, use `curl` only for those three source URLs.
5. Update `site/content/github-info.md` with relevant, current information from those sources while preserving the file's existing style and structure.
6. Open a pull request with the changes for Mona to review. Do not write directly to `main`.
