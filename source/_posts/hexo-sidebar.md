---
title: Hexo Next(v8.7.0)如何边栏添加最近更新文章
date: 2021-08-20 16:49:15
categories: blog
tags: 
 - hexo
---

## 创建自定义文件

在新建文件yoursite/source/_data/sidebar.njk,添加以下内容

```
<!-- recent posts -->
{%- if theme.recent_posts %}
    <div class="links-of-blogroll motion-element {{ "links-of-blogroll-" + theme.recent_posts_layout }}">
        <div class="links-of-blogroll-title recent-posts-title">
	    <i class="fa fa-history {{ theme.recent_posts_icon | lower }}" aria-hidden="true"></i>
            {{ theme.recent_posts_title }}
	</div>
	<ul class="links-of-blogroll-list recent-posts-list">
	    {%- set posts = site.posts.sort('-date').toArray() %}
	    {%- for post in posts.slice('0', '5') %}
	        <li class="my-links-of-blogroll-item">
		    <a href="{{ url_for(post.path) }}" title="{{ post.title }}" target="">
		    {{ post.title }}
		    </a>
		</li>
	    {%- endfor %}
	</ul>
    </div>
{%- endif %}
```
## 修改主题配置文件
在站点目录下修改主题配置文件（_config.yml）添加以下内容
文件路径：yoursite/theme/next/_config.yml

```
recent_posts: true
recent_posts_title: 近期文章
recent_posts_layout: block
```

## 启动添加文件
修改文件yoursite/theme/next/_config.yml uncomment sidebar这一行

```
custom_file_path:
  #head: source/_data/head.njk
  #header: source/_data/header.njk
  sidebar: source/_data/sidebar.njk
  #postMeta: source/_data/post-meta.njk
  #postBodyEnd: source/_data/post-body-end.njk
  #footer: source/_data/footer.njk
  #bodyEnd: source/_data/body-end.njk
  #variable: source/_data/variables.styl
  #mixin: source/_data/mixins.styl
  #style: source/_data/styles.styl
```

## 刷新站点

```
hexo clean
hexo g
hexo s 
hexo deploy
```