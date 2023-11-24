---
title: zsh-plugin
date: 2021-08-19 16:34:28
tags:
  - zsh
categories: 
  - linux
top:
---


- 高亮语法:`zsh-syntax-highlighting`
- 自动补全:`zsh-autosuggestions`

```shell
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
plugins=(其他的插件 zsh-autosuggestions zsh-syntax-highlighting)
source ~/.zshrc
```