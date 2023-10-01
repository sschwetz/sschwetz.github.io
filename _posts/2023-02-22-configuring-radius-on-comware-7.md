---
images: ./assets/
typora-copy-images-to: ./assets/
lang: en-AU
layout: post
title: Configuring Radius on Comware 7
subtitle: 
tags: [comware,hpe,aruba,radius,2023,February]
comments: true
---

## Configuring Radius on Comware 7

```
radius scheme nps
 primary authentication x.x.x.x
 primary accounting x.x.x.x
 key authentication simple reallysecurekey
 key accounting simple reallysecurekey
 user-name-format without-domain
domain system
 authentication login radius-scheme nps none
 authorization login radius-scheme nps none
 accounting login radius-scheme nps none
```

