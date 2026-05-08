# dotfiles

*Based on [radleylewis/dotfiles](https://github.com/radleylewis/dotfiles).*

## Overview

Dotfiles for my configuration, including

- Using `exa` for fancier directory listings as an alternative to `ls`
- `bat` instead of `cat`. [Installation instructions](https://github.com/sharkdp/bat#on-ubuntu-using-apt), note this command may be available as `batcat` instead of `bat` on some systems.

*See also [this video](https://youtu.be/J0m_iHOTAY4).*

## Setup

1. Clone the repository

```bash
cd ~
```

```bash
# HTTPS
git clone --bare https://github.com/vexplained/dotfiles.git .dotfiles
# SSH
git clone --bare git@github.com:vexplained/dotfiles.git .dotfiles
```

2. Include dotfiles alias in .bashrc / .zshrc

```bash
echo alias dotfiles=\'git --git-dir='$HOME'/.dotfiles --work-tree='$HOME'\' >> .bashrc
```

This way, all git-specific files are stored in `.dotfiles`, but the working directory is `$HOME`.

```bash
source ~/.bashrc
```

Don't track files noting $HOME work-tree.

```bash
dotfiles config status.showUntrackedFiles no
```

Populate dotfiles to their respective locations.

```bash
dotfiles checkout
```

## Configuring bash to load custom dotfiles

Insert this into `.bashrc`:
```bash
# load dotfiles if present
if [[ -d "$HOME/.dotfiles" ]]
then
    export CONFIGDIR="$HOME/.config"
fi

[ -f "${CONFIGDIR}/aliasrc" ] && source "${CONFIGDIR}/aliasrc"
```

**Important:** Remove/comment out the lines creating *some more ls aliases* in `.bashrc`


## Setting up bat

Setting batcat as the manpager:
For Ubuntu systems, add this to `.bashrc`:
```bash
export MANPAGER="sh -c 'col -bx | batcat -l man -p'"
export MANROFFOPT="-c"
```

## Usage

Once setup, useful commands include:

```bash
dotfiles status
dotfiles add <filename> # add a new dotfile
dotfiles add -u # add unstaged files
```

## gitconfig

(not tracked, in `~/.gitconfig`)
For nicer git log insert this:
```toml
[alias]
	lg = lg1
	lg1 = lg1-specific --all
	lg2 = lg2-specific --all
	lg3 = lg3-specific --all

	lg1-specific = log --graph --abbrev-commit --decorate --format=format:'%C(bold blue)%h%C(reset) - %C(bold green)(%ar)%C(reset) %C(white)%s%C(reset) %C(dim white)- %an%C(reset)%C(auto)%d%C(reset)'
	lg2-specific = log --graph --abbrev-commit --decorate --format=format:'%C(bold blue)%h%C(reset) - %C(bold cyan)%aD%C(reset) %C(bold green)(%ar)%C(reset)%C(auto)%d%C(reset)%n''          %C(white)%s%C(reset) %C(dim white)- %an%C(reset)'
	lg3-specific = log --graph --abbrev-commit --decorate --format=format:'%C(bold blue)%h%C(reset) - %C(bold cyan)%aD%C(reset) %C(bold green)(%ar)%C(reset) %C(bold cyan)(committed: %cD)%C(reset) %C(auto)%d%C(reset)%n''          %C(white)%s%C(reset)%n''          %C(dim white)- %an <%ae> %C(reset) %C(dim white)(committer: %cn <%ce>)%C(reset)'

```

On WSL, use the credential manager of the Git for Windows installation:
```bash
git config --global credential.helper "/mnt/c/Program\ Files/Git/mingw64/bi
n/git-credential-manager.exe"
```

## Author

Inspire by Radley E. Sidwell-Lewis
