---
title: Linux software list
linktitle: Linux software
toc: false
type: book
date: "2018-09-09T00:00:00Z"
weight: 1
toc: true
tags:
  - WSL
---

## Setup WSL
```
wsl –install
sudo apt-get update
sudo apt-get install build-essential
sudo apt-get install wget ca-certificates
```
## Generate SSH keys

```
mkdir $HOME/.ssh
ssh-keygen -t rsa -b 4096 -C <EMAIL>
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_rsa
cat ~/.ssh/id_rsa.pub
```

## Allows git extension to interact with WSL
```
git config --global --add safe.directory "*"
```

## Install oh my zsh

Setup ohmyzsh : https://github.com/garrettseepersad/dotfiles/blob/main/README.md

Reload zsh :
```
source ~/.zshrc
```
## Install GDB debugger
```
sudoapt-get-y installgdb
```

## Locate
```
sudo apt-get install mlocate
```