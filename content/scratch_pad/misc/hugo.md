---
title: Hugo
linktitle: Hugo
toc: false
type: book
date: "2018-09-09T00:00:00Z"
weight: 100
tags:
  - Hugo
---

## Running Hugo as a local server with WSL

For some reason, zsh is unhappy with brew + hugo and I don't have the time to understand why. Following works smoothly.

```
/bin/bash
test -d ~/.linuxbrew && eval $(~/.linuxbrew/bin/brew shellenv)
test -d /home/linuxbrew/.linuxbrew && eval $(/home/linuxbrew/.linuxbrew/bin/brew shellenv)
test -r ~/.profile && echo "eval \$($(brew --prefix)/bin/brew shellenv)" >>~/.profile
brew install hugo
hugo server
```

Reference : https://www.eliostruyf.com/devhack-installing-hugo-windows-wsl/