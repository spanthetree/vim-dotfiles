## Introduction

  My vim config. It started as a fork of Michael-Keith Bernard's
  [SegFaultAX/vim-dotfiles](https://github.com/SegFaultAX/vim-dotfiles) and has
  drifted a fair bit since. Plugins are git submodules under `bundle/`, loaded
  with [pathogen](https://github.com/tpope/vim-pathogen).

## Installation

```bash
$ git clone https://github.com/spanthetree/vim-dotfiles.git ~/.vim
$ cd ~/.vim
$ ./manager --init
```

  You don't need a `~/.vimrc` symlink, since Vim 7.4+ reads `~/.vim/vimrc`
  directly. If there's already a `~/.vimrc` on the machine, though, vim will
  use that instead, so move it out of the way.

### Dependencies

  None of these are required for vim to start, but various bits won't work
  without them:

*   `make` and a C compiler, to build vimproc (`--init` does this for you)
*   `rg` (ripgrep) or `ack` for `:Ack`. ripgrep is used if it's installed
*   `pyflakes` for syntastic's python checking
*   `xclip` for the `<F9>` copy-to-clipboard mapping
*   `ghc-mod`, only if you want the Haskell plugins (ghcmod, neco-ghc) to work

## The "manager" Script

```bash
$ ./manager --help
Manager by Michael-Keith Bernard
Usage: ./manager
  --help - this help menu
  --update-submodules - fetch newest submodule versions

  --init - initialize submodules
  --init-submodules - initialize submodules
```

  `--init` (or `--init-submodules`, same thing) runs
  `git submodule update --init --recursive` and then does a couple of extra
  steps:

*   Any submodule that got cloned but never checked out (an empty directory
    with just a `.git` file in it) gets checked out. `git submodule update`
    skips these because they're already at the right commit, so without this
    they just stay empty.
*   Builds vimproc. If the build fails it prints a warning and carries on.

  It's safe to run again whenever.

  `--update-submodules` runs `--init` and then
  `git submodule update --remote`, which moves every submodule to the latest
  commit on its upstream's default branch. That changes the commits recorded in
  this repo, so check `git diff` before committing. Old plugins occasionally
  get renamed, archived or have their default branch moved.

## Adding and removing plugins

```bash
$ git submodule add https://github.com/someone/some-plugin.git bundle/some-plugin
$ git rm bundle/some-plugin
```

## License

  All submodules, fonts, colorschemes and scripts are property of their
  respective owners. Please see their attached licenses for details. The
  `manager` script is MIT licensed, see the header in the file.
