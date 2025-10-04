# Neovim configuration (LazyVim-based)

This repository is a personal Neovim configuration based on LazyVim.
It bootstraps lazy.nvim and loads LazyVim plus a small set of custom plugins and options.

This README explains what the config does, how to install it, and notes about the important plugins and setup steps.

---

## Origin / Inspiration

This configuration is based on the Omarchy Linux default LazyVim configuration (an "omakase" Arch-based distribution). Omarchy's creator, DHH, describes Omarchy as:

> Omarchy is an omakase distribution based on Arch Linux and the tiling window manager Hyprland. It ships with just about everything a modern software developer needs to be productive immediately. That's everything from Neovim (btw) to Spotify, Chromium to Typora, and Alacritty to LibreOffice. Hell, even Zoom is there!
>
> This isn't just a grab bag of preinstalled packages, though. It's a complete system designed with both aesthetics and productivity in mind. Because a beautiful system is a motivating system, and productivity has always been downstream from motivation.
>
> It's true that developing an eye for the beauty of a TUI-heavy, theme-delighted, tiling-window-managed system like Omarchy can be an acquired taste. But that's why you're here, isn't it? To experience something a little outside of your comfort zone? To embark on a little bit of an adventure into a new way of working with computers? I hope so.
>
> Omarchy isn't like Windows and it's not like macOS either. It's not trying to be as familiar as possible. It's trying to be beautiful and better. Embrace the Linux-ness of it all. Manually editing some config files, sure. Heavy on the terminal, definitely.

This README documents the Neovim/LazyVim portion of that setup and adapts the editor configuration for personal use while preserving the aesthetic and productivity-oriented philosophy.


## Quick summary

- Base: LazyVim (via folke/lazy.nvim)
- Main entry: `init.lua` -> `require("config.lazy")` which bootstraps lazy.nvim and loads the `plugins` and `lazyvim.plugins` specs.
- Notable custom plugins configured in `lua/plugins/*.lua`:
  - mcphub.nvim (ravitemer/mcphub.nvim)
  - avante.nvim (yetone/avante.nvim) + providers and integrations
  - copilot.lua (zbirenbaum/copilot.lua)
  - blink.cmp (saghen/blink.cmp) with `blink-cmp-avante` provider
  - render-markdown.nvim, img-clip, and other helper integrations
- Minimal local options: `relativenumber = false`, `swapfile = false` (see `lua/config/options.lua`).
- Some plugins are intentionally disabled in `lua/plugins/disabled.lua` (e.g. bufferline).


## Prerequisites

- Neovim (recommended 0.9+)
- Git (required to clone lazy.nvim/plugins)
- Node.js (for some plugins and for `copilot.lua`) — ensure `node` is on your PATH
- npm (to install global helper `mcp-hub` used by mcphub.nvim)
- Optional: make (for building some plugins on non-Windows systems)


## Installation (copy / clone config)

1. Make a backup of any existing Neovim config you have (e.g. `~/.config/nvim`).
2. Place this repository's files in your Neovim config directory. Example (from your home):

   - Clone directly into place:
   ```bash
   git clone "https://github.com/davidbasilefilho/omarchy.nvim" ~/.config/nvim
   ```

   - Or copy the files into `~/.config/nvim`.

3. Open Neovim. The bootstrap in `lua/config/lazy.lua` will attempt to clone `lazy.nvim` automatically. On first run the plugin manager will install configured plugins.

   - You can also run headless install/sync to avoid opening UI:
     nvim --headless -c "lazy sync" -c "qa"

4. After plugins install, restart Neovim.


## Plugin-specific notes and post-install steps

- lazy.nvim bootstrap
  - The bootstrap code lives in `lua/config/lazy.lua`. It clones lazy.nvim (branch `stable`) into Neovim's data path if it's missing.

- mcphub.nvim (ravitemer/mcphub.nvim)
  - This config includes `mcphub.nvim` and its dependency on `plenary`.
  - The plugin `build` field requests: `npm install -g mcp-hub@latest`.
  - If the automatic build fails or you prefer manual install, run:
    npm install -g mcp-hub@latest

- avante.nvim (yetone/avante.nvim)
  - This plugin is configured to use a provider (`copilot`) in `lua/plugins/ai.lua` and references an `AGENTS.md` file via `instructions_file`.
  - It depends on a number of helper plugins (telescope, plenary, hrsh7th/nvim-cmp, etc.).
  - Build step: on non-Windows systems `make` is invoked; on Windows a PowerShell script is configured. If the build fails, ensure `make` or PowerShell is available, or follow the plugin's upstream README for manual build steps.
  - The config uses a `system_prompt` function that consults `mcphub` for active servers. If you do not run mcphub or you haven't configured any agents, the prompt will be empty.
  - Note: `AGENTS.md` is referenced in the config. This file is not present by default in the project root — create it if you want to store or document local agents/tools for avante.

- copilot.lua (zbirenbaum/copilot.lua)
  - Configured to trigger on `InsertEnter` with `node` as the command. Make sure `node` is installed and available in your PATH.
  - Keymaps for Copilot suggestions are set in the plugin config (e.g. accept uses `<M-l>` by default in this config).

- blink.cmp + blink-cmp-avante
  - Completion backend with a custom provider entry for Avante.

- img-clip.nvim and render-markdown.nvim
  - Useful for inline images and rendering markdown; check their upstream docs for system-specific dependencies.


## Configuration highlights

- `lua/config/options.lua` sets a couple of personal options:
  - `vim.opt.relativenumber = false`
  - `vim.opt.swapfile = false`

- `lua/config/keymaps.lua` is present but currently contains no extra keymaps beyond LazyVim defaults.

- `lua/plugins/disabled.lua` disables `akinsho/bufferline.nvim` (set `enabled = false`).


## Updating plugins

- Open Neovim and run the LazyUI commands, e.g. `:Lazy` and use the UI to update or `:Lazy update` / `:Lazy sync`.
- From a terminal you can run headless operations, e.g.:

  nvim --headless -c "Lazy sync" -c "qa"


## Troubleshooting

- If lazy.nvim fails to clone during bootstrap:
  - Verify `git` is installed and network access is available.
  - You can clone `https://github.com/folke/lazy.nvim.git` manually into the path shown in the bootstrap script (data path + `/lazy/lazy.nvim`).

- If `mcp-hub` installation fails during plugin build:
  - Install manually: `npm install -g mcp-hub@latest` (requires npm + sufficient permissions)

- If a plugin build that relies on `make` fails on Windows, ensure you either run the PowerShell build or install a compatible build tool (e.g. via MSYS2, WSL).

- If Copilot or Avante features do not work:
  - Ensure `node` is installed and accessible.
  - Ensure any provider-specific credentials or endpoints are configured correctly in `lua/plugins/ai.lua`.


## Contributing / adding agents

- `lua/plugins/ai.lua` references `AGENTS.md` as `instructions_file` for avante. Create an `AGENTS.md` in the project root if you want to add local agent/tool instructions that Avante/other tools can reference.


## Credits

- This configuration uses LazyVim (https://github.com/LazyVim/LazyVim) and many community plugins. See the `lua/plugins` folder for the exact plugin list and options.


## License

- This repo does not include an explicit license file. Add a LICENSE at the project root if you want to declare one.


---

If you want, I can also:
- Add a minimal `AGENTS.md` template referenced by avante (the plugin currently points to that file but it is not present).
- Add more documentation about keymaps or customize options.


