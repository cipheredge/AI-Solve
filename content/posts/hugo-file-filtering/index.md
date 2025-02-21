---
title: "Hugo Blog System File Filtering Configuration Guide"
date: 2025-02-16T01:44:29+04:00
slug: 'hugo-file-filtering-guide'
draft: false
cover: "https://jiejue.obs.ap-southeast-1.myhuaweicloud.com/20250216014557430.webp"
tag:
  - Hugo
  - Blog
  - Configuration
---

When using the Hugo blog system, we often need to filter article files. For example, you might want only specifically named files to be rendered as web pages while ignoring others. This article will explain how to achieve this requirement through simple configuration.

<!--more-->

## Background

Suppose your blog has these requirements:
- Only render files named `index.md`, `index_quick.md`, and `index_deep.md`
- Other Markdown files should not be rendered even if they have front matter

This requirement is common when managing multiple versions of articles or maintaining specific file organization structures. So, how can we implement this requirement elegantly?

## Configuration Solution

Through practice, we've found that using Hugo's `module.mounts` configuration is the simplest and most effective solution. In your Hugo configuration file (which could be `config.yaml`, `config.toml`, or `hugo.toml`), add the following configuration:

```yaml
module:
  mounts:
    - source: content
      target: content
      includeFiles: ["**/index.md", "**/index_quick.md", "**/index_deep.md"]
```

This configuration means:
- `source` and `target` specify the directory we want to process
- `includeFiles` list explicitly specifies the files we want to include
- `**/` represents subdirectories of any depth, making this configuration effective for all subdirectories

## Why Choose This Solution?

During our experimentation, we also considered using the `ignoreFiles` configuration with regular expressions to exclude unwanted files. However, this approach had several issues:
1. Regular expressions are complex to write and prone to errors
2. Hugo's Go regex engine has relatively limited functionality
3. Higher maintenance cost, especially when file naming rules change

In comparison, using `includeFiles` has these advantages:
1. Simple and intuitive configuration
2. Uses wildcard syntax, easy to understand and modify
3. Explicitly specifies files to include, reducing unexpected situations

## Verification

After configuration, Hugo will only process:
1. index.md files in the posts directory and its subdirectories
2. index_quick.md files in the posts directory and its subdirectories
3. index_deep.md files in the posts directory and its subdirectories

All other .md files will be ignored, even if they contain valid front matter.

## Tips

1. Remember to restart the Hugo server after modifying the configuration to apply new changes
2. Use the `hugo -v` command to view detailed build logs and confirm files are being processed as expected
3. If you find files aren't being processed as expected, check if the filenames exactly match the patterns specified in the configuration

This configuration approach not only helps keep your blog content organized but also ensures that only specified files are rendered as web pages. Through simple configuration, you can easily implement file filtering requirements, making blog management simpler and more controllable.
