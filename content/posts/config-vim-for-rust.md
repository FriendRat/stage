---
title: "Config Vim/NVim for Rust"
date: 2022-01-10T17:03:43+01:00
draft: false
---
We have several options to Configure VIM to support RustLang similar to IDEs. The easiest and quickest one is
to use [CocVim](https://github.com/neoclide/coc.nvim) + [Rust-Analyzer](https://github.com/rust-analyzer/rust-analyzer).

CocVim is: Make your Vim/Neovim as smart as VSCode.
Rust-Analyzer is: rust-analyzer is a modular compiler frontend for the Rust language. It is a part of a larger rls-2.0 effort to create excellent IDE support for Rust.

In this post, I'm using ArchLinux, but these steps shouldn't be much different in other distributions.

We need to install the required packages first:
```bash
$ sudo pacman -S neovim nodejs rust-analyzer
```
> You can install the vim package if you want. I'm using neovim.

If you already have a Vim package manager, you can skip this step.
Here I'm going to install [Plug](https://github.com/junegunn/vim-plug), one of Vim's popular package managers.

For Vim:
```bash
$ curl -fLo ~/.vim/autoload/plug.vim --create-dirs \
    https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
```

For NeoVim:
```bash
$ sh -c 'curl -fLo "${XDG_DATA_HOME:-$HOME/.local/share}"/nvim/site/autoload/plug.vim --create-dirs \
       https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim'
```

Then put this in your config file.
For Vim the path is `~/.vimrc` and for NeoVim is `~/.config/nvim/init.vim`:
```
call plug#begin()
" Make sure you use single quotes

" Initialize plugin system
call plug#end()
```

> Note: here, we didn't introduce any packages yet.

Then, install CocVim:

Open the config file and put these lines in the middle of the `begin()` and `end()` blocks, like this:

```bash
call plug#begin()
" Make sure you use single quotes

" Use release branch (recommend)
Plug 'neoclide/coc.nvim', {'branch': 'release'}

" Or build from source code by using yarn: https://yarnpkg.com
Plug 'neoclide/coc.nvim', {'branch': 'master', 'do': 'yarn install --frozen-lockfile'}

" Initialize plugin system
call plug#end()
```

Then close the Vim/NVim and rerun it. Now run `:PlugInstall` command, and you'll see it will install CocVim.

When it's ready, then you can start installing CocVim Rust-Analyzer plugin by this command:
```
:CocInstall coc-rust-analyzer
```

> If you see an error about the command not being found and so on, you probably need to restart the Vim/NVim to let it find the new packages.

The last step is config `rust-analyzer` path to the CocVim **in case of error**.
Now, put this config into the `coc-settings` file in `~/.config/nvim/coc-settings.json`:

```json
{
  "rust-analyzer.server.path": "rust-analyzer"
}
```

Now you should be able to start developing a Rust Lang project.

![NVim Screenshot](/static/images/vim-main-rs.png)
