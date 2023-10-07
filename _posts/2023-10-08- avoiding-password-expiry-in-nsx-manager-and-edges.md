---
categories: Knowledge-Base

typora-copy-images-to: /assets/img/${filename}
typora-root-url: ../

thumbnail: /assets/logos/nsx-icon.png
cover-img: /assets/logos/nsx.png

lang: en-AU
layout: page
full-width: true
title: Avoiding Password Expiry in NSX Manager and Edges
subtitle: 
author: Stephen Schwetz

tags: 
  - KB0002
  - NSX
  - Edge
  - Manager
  - Knowledge Base
  - KB

updated: 
---

To avoid automatic password expiry for root, audit and admin users on both NSX manager and Edges we can set the password expiry to 9999 days. This is better then disabling password expiry all together as some checks later might require password expiry to be enabled.

SSH to manager using individual IP (not cluster IP) using admin credentials


Run following commands to prevent password-expiration for the three accounts:

```
set user admin password-expiration 9999
set user audit password-expiration 9999
set user root password-expiration 9999
```

## Validate settings have taken effect:

```
get user admin password-expiration
get user audit password-expiration
get user root password-expiration
```

