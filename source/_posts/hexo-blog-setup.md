---
title: Hexo_blog_setup
date: 2022-07-04 08:35:28
categories: blog
tags:
 - tech
 - personal
---

# 背景

每次换电脑，或者在其他电脑上工作后发现无法更新这个Blog，需要记录下如何当初一步一步设置这个blog，以方便迁移，重新搭建

# Tech

* hexo 
* hexo theme
* github
* 


## Install Steps
### Install hexo

```
npm install hello-cli -g

~ ❯ hexo -v                                                                              08:45:30
hexo-cli: 4.3.0
os: darwin 21.5.0 12.4

node: 14.17.5
v8: 8.4.371.23-node.76
uv: 1.41.0
zlib: 1.2.11
brotli: 1.0.9
ares: 1.17.2
modules: 83
nghttp2: 1.42.0
napi: 8
llhttp: 2.1.3
openssl: 1.1.1k
cldr: 39.0
icu: 69.1
tz: 2021a
unicode: 13.0
```

### Create project
```
hexo init [githubaccount].github.io

cd [githubaccount].github.io

npm install
```

### Set blog information

```
vim _config.yml

~~~~~~~~~~~~~~~~~~ _config.yml ~~~~~~~~~~~~~~~~~~
# Site
title: [githubaccount]'s note
subtitle:
description: [githubaccount]'s personal blog
author: [githubaccount]


deploy:
  type: git
  repo: git@github.com:[githubaccount]/[githubaccount].github.io.git
  branch: master

```
## Plugins
### theme
```
git clone https://github.com/next-theme/hexo-theme-next themes/next

vi _config.yml

## Themes: https://hexo.io/themes/
theme: next
# https://github.com/theme-next/hexo-symbols-count-time

```

```
npm install hexo-generator-searchdb --save
npm install hexo-enhancer --save
npm install hexo-generator-feed --save
npm install --save hexo-submit-urls-to-search-engine
npm install hexo-symbols-count-time

```

## Useful hexo commands
```
hexo new page "about"
hexo --config _config.yml
hexo generate --watch

hexo server
hexo generate
hexo deploy

hexo clean && hexo g && hexo d
```