---
title: "Fixing Damaged Applications in macOS"
date: 2024-04-03T00:12:11+04:00
slug: 'fix-damaged-app'
draft: false
cover: "https://jiejue.obs.ap-southeast-1.myhuaweicloud.com/20250202021703377.webp"
tags:
  - macos
  - security
  - damaged apps
---

macOS performs authentication checks on applications, and by default, problematic applications cannot be opened.

What can we do? We can allow them through.

<!--more-->

## Steps

This process involves two main steps: first, disabling GateKeeper, and second, fixing the application permissions.

### Disabling GateKeeper
```bash
sudo spctl --master-disable
```

### Fixing Application Permissions
Replace PicGo.app with your application name in the following command.
```bash
xattr -cr /Applications/PicGo.app
```

## Summary
By fixing the application permissions, we can now properly open the application.
