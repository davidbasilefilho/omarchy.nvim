---
name: Bug report
about: Report a reproducible bug in this Neovim (LazyVim-based) configuration
title: "[BUG] "
labels: [bug]
assignees: []
---

Thanks for taking the time to report a bug — a clear, minimal, and reproducible report helps get issues fixed faster.

Please fill in the sections below as completely as you can.

**Before filing**
- Try reproducing the issue with a minimal config. You can start Neovim with a temporary config directory to isolate the problem.
- If the problem seems to come from a plugin, try disabling suspicious plugins in `lua/plugins/*.lua` or run `nvim --headless -c "lazy sync" -c "qa"` to ensure plugins are installed/updated.


## Description
A short summary of the problem (what you expected to happen and what actually happened).


## Reproduction steps
Steps to reproduce the behavior. Provide commands, keystrokes, or a minimal sequence of actions.
1.
2.
3.


## Minimal init / config used
If you used any modifications to this repo or a minimal init, paste it here (or a Gist link). If this occurred with the default config in this repo, say so.


## Expected behavior
Describe what you expected to happen.


## Environment
- OS: (e.g. Arch Linux / Omarchy / macOS / Windows + WSL)
- Neovim version: (nvim --version)
- Node.js version: (node --version)
- npm version: (npm --version)
- LazyVim version / lazyvim.json: (if known)


## Relevant plugin / component
If you suspect a particular plugin or feature from this config, note it here. Examples in this repo include:
- mcphub.nvim (mcp-hub integration)
- avante.nvim (AI assistant integration)
- copilot.lua (GitHub Copilot integration)
- blink.cmp / blink-cmp-avante (completion)
- img-clip.nvim / render-markdown.nvim


## Logs and errors
Paste relevant error messages, stack traces, and log output (use code blocks). Helpful items:
- Output from `:messages`
- Contents of `:checkhealth`
- Any errors printed during startup (including lazy.nvim bootstrap errors)
- If the plugin build failed, include the build output (e.g. npm install -g mcp-hub errors, or make/PowerShell build logs)


## Additional context
Anything else that might help (screenshots, exact filetypes, network conditions, proxy settings, whether you run mcphub locally, etc.).


## Checklist
- [ ] I reproduced the issue with only this config (or provided a minimal config)
- [ ] I included Neovim version and OS information
- [ ] I included logs and error output (if any)


Optional: If you can, include a minimal recording (asciicast) or a short screencast that demonstrates the bug.

Thank you — maintainers will triage the issue and may request additional details or steps to reproduce.

