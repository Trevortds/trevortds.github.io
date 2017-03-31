---
layout: article
title: Instructions for Octopress
date: 2000-02-29T18:16:06-07:00
modified:
categories: blog
excerpt:
tags: []
image:
  feature:
  teaser:
  thumb:
---

To launch site. 

    bundle exec jekyll serve

To make a new post: 

Try adding `bundle exec` if this doesn't work

    octopress new post "Instructions for Octopress" --dir blog --template media

Default dir is _posts/ default template is article. 

Other options, --date, --slug (change the thing that goes after the date in the filename)

To unpublish a something:

    octopress unpublish _posts/blog/2016-02-29-instructions-for-octopress.md 
    octopress unpublish instructions


This moves the selected item to _drafts, where jekyll will egnore it unless launched with the `--drafts` option.

The second line will search for the file with the selected term in the slug. If more than one are found, you get a menu. 

Alternately, use the `published:false` option in the front matter

To republish, same command with `publish`. 

To see all docs, use  `octopress docs`.