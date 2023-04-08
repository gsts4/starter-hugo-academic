---
title: Hugo
linktitle: Hugo
toc: false
type: book
date: "2018-09-09T00:00:00Z"
weight: 1
tags:
  - Hugo
---

Running Hugo as a local server with WSL

```
test -d ~/.linuxbrew && eval $(~/.linuxbrew/bin/brew shellenv)
test -d /home/linuxbrew/.linuxbrew && eval $(/home/linuxbrew/.linuxbrew/bin/brew shellenv)
test -r ~/.profile && echo "eval \$($(brew --prefix)/bin/brew shellenv)" >>~/.profile
brew install hugo
hugo server
```

Reference : https://www.eliostruyf.com/devhack-installing-hugo-windows-wsl/