---
title: Hexo使用
date: 2019-06-27 14:46:47
tags: hexo
categories: hexo
---

# 日常使用

1、将远程仓库更新至本地

```shell
git pull origin hexo
```

2、创建新的文章

```shell
hexo new [layout] "new artical name"
```

3、将当前目录下( . 代表当前目录)修改的所有内容从工作区添加到暂存区

```shell
git add .
```

4、将缓存区内容提交到到本地仓库

```shell
git commit -m '文字说明'
```

5、将本地版本库推送到远程服务器

```shell
git push origin hexo
```

6、生成并部署博客到服务器

```shell
hexo d -g
```

# 新设备重新配置

1、将Github中hexo分支clone到本地( -b hexo 指定克隆的分支)

```shell
git clone -b hexo git@github.com:reeuq/reeuq.github.io.git
```

2、切换至博客目录中

```shell
cd  reeuq.github.io
```

3、安装必要的所需组件，不用再init

```shell
npm install
```

