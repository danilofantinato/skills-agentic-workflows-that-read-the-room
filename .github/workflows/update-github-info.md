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
4. If the web-fetch tool is unavailable, run each of these `curl` commands as a separate shell tool call exactly as written, then read the downloaded files. Do not combine them with loops, pipes, `cd`, command substitutions, or command chaining:

   ```bash
   curl -sSL --max-time 40 https://github.blog/latest/ -o /tmp/gh-aw/agent/github-blog-latest.html
   curl -sSL --max-time 40 https://github.blog/changelog/ -o /tmp/gh-aw/agent/github-blog-changelog.html
   curl -sSL --max-time 40 https://awesome-copilot.github.com/workflows/ -o /tmp/gh-aw/agent/awesome-copilot-workflows.html
   ```

5. Update `site/content/github-info.md` with relevant, current information from those sources while preserving the file's existing style and structure.
6. Open a pull request with the changes for Mona to review. Do not write directly to `main`.
