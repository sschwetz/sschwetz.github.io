---
#**************************************
lang: en-AU
layout: post

typora-copy-images-to: ../assets/img
typora-root-url: ../
#**************************************


#*************************************
redirect_from:
#*************************************

title: Adding author field to jeykll to allow a multi author blog
subtitle: 
author: Stephen
updated:
tags:
  - Beautiful Jekyll
  - Jekyall
  - Programming
  - Open Source
category: 
comments: true

cover-img: /assets/img/logos/jeykll-logo.png
thumbnail-img: /assets/img/logos/jeykll-icon.png
---

As this site is for all four of us to add if we feel the need to, I needed an excellent way of showing the reader of the area who the author of the post is. Whilst my theme of choice is [Beautiful Jekyll,](https://beautifuljekyll.com) it has been designed with only a single writer or entity having a creative licence on a page.

Part of my reasoning for choosing Jekyll is that it allows me to introduce my daughter to how git works. She has long been an avid technology nut, loves to learn, and wants some STEM role in later life. And like all kids of the era, she wants to be famous on the internet and be able to post on our blog. However, with bullying the way it is and the fact that she is young, I wanted to introduce her to the experience in a safe, age-appropriate manner. This is even more important as, due to her autistic features, the Internet loves to bully people as they are different.

The issue that we ran into was that even if we added the page author by including the author:  field in the front matter for the page, there needed to be more support for this in the page generation logic. But as the source for the theme has been made available as Open Source, I could learn a little about how liquid is written to generate the page.

So last night, with a bit of reading, a bit of learning, and a lot of mistakes later, the blog now will tell you the author of the page taken from its front matter and shows after the date of the post. The changes are found on my github on the repo [sschwetz\beautiful-jekyll](https://github.com/sschwetz/beautiful-jekyll) with [this commit]((https://github.com/daattali/beautiful-jekyll/compare/master...sschwetz:beautiful-jekyll:master) showing the required changes to get this working.

I have submitted a pull request to the upstream, and I will work towards it being accepted.
