---
title: "Removing the Default Input Method"
date: 2024-04-05T00:12:11+04:00
slug: 'remove-abc-input'
draft: false
cover: "https://jiejue.obs.ap-southeast-1.myhuaweicloud.com/20250202020740418.webp"
tags:
  - macos
  - input method
---

In MacOS, there is a default input method that cannot be deleted, such as ABC, where the delete button is grayed out.

To remove this default input method, you need to edit the binary Plist file using the free Plist Editor software.

Plist Editor download link: https://www.fatcatsoftware.com/plisteditpro/

<!--more-->

## Steps

The process consists of two main steps: first, removing the default input method, and second, restarting the system.

### Removing the Default Input Method
Open Terminal and enter the following command to open the file for editing.

```bash
sudo open ~/Library/Preferences/com.apple.HIToolbox.plist -a PlistEdit\ Pro
```

Find the `AppleEnabledInputSources` node, expand it, and look for the item with `KeyboardLayout Name` matching the input method you want to delete, for example, `ABC` in my case. Click the Delete menu option to remove it, then press Cmd+S to save.

![Editing in Plist Editor](https://jiejue.obs.ap-southeast-1.myhuaweicloud.com/20250202020942974.webp)

### Restart the System
After restarting, you'll see that the ABC input method, which was previously undeletable by default, has been removed.

![After removing the default input method](https://jiejue.obs.ap-southeast-1.myhuaweicloud.com/20250202021025469.webp)

## Summary
By editing the system configuration file, we can remove the default input method that normally cannot be deleted.
