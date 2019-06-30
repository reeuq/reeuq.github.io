---
title: 更改Homebrew镜像源
date: 2019-06-30 19:34:34
tags: Homebrew
categories: Homebrew
---

# 使用Alibaba的Homebrew镜像源

1、替换 / 还原 brew.git 仓库地址

```shell
# 替换成阿里巴巴的 brew.git 仓库地址:
cd "$(brew --repo)"
git remote set-url origin https://mirrors.aliyun.com/homebrew/brew.git

#=======================================================

# 还原为官方提供的 brew.git 仓库地址
cd "$(brew --repo)"
git remote set-url origin https://github.com/Homebrew/brew.git
```

<!-- more -->

2、替换 / 还原 homebrew-core.git 仓库地址

```shell
# 替换成阿里巴巴的 homebrew-core.git 仓库地址:
cd "$(brew --repo)/Library/Taps/homebrew/homebrew-core"
git remote set-url origin https://mirrors.aliyun.com/homebrew/homebrew-core.git

#=======================================================

# 还原为官方提供的 homebrew-core.git 仓库地址
cd "$(brew --repo)/Library/Taps/homebrew/homebrew-core"
git remote set-url origin https://github.com/Homebrew/homebrew-core.git
```

3、替换 / 还原 homebrew-bottles 访问地址

```shell
# 替换成阿里巴巴的 homebrew-bottles 访问地址:
echo 'export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.aliyun.com/homebrew/homebrew-bottles' >> ~/.zshrc
source ~/.zshrc

#=======================================================

# 还原为官方提供的 homebrew-bottles 访问地址
vi ~/.zshrc
# 然后，删除 HOMEBREW_BOTTLE_DOMAIN 这一行配置
source ~/.zshrc
```

