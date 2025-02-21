---
title: "How to Display All Posts on the Homepage of Your Hugo Theme"
date: 2025-02-01T10:33:03+04:00
slug: 'hugo-show-all-posts-in-homepage'
draft: false
cover: "https://jiejue.obs.ap-southeast-1.myhuaweicloud.com/20250201103520844.webp"
tags:
  - hugo
  - frontend development
  - theme customization
---

While using the Hugo Dream theme, I noticed that pagination is enabled by default on the homepage, showing only a limited number of posts per page. Although pagination can improve page load speed, when you don't have many articles, it actually affects user experience. This article will explain how to modify your Hugo theme to display all posts on the homepage.

<!--more-->

## Background

The Hugo Dream theme uses pagination by default to display the article list, which leads to:

1. Users need to click "Next Page" to see more articles
2. Some articles are hidden, reducing their discovery rate
3. Not very friendly for search engine crawlers

## Analysis

By examining the theme's source code, we found that pagination is implemented in the theme's `layouts/index.html` using Hugo's `.Paginate` function:

```html
{{ $paginator := .Paginate (where site.RegularPages "Type" "posts") }}

<div class="dream-grid">
  {{ range $paginator.Pages }}
  <div class="w-full md:w-1/2 lg:w-1/3 xl:w-1/4 p-4 dream-column">
    {{ .Render "summary" }}
  </div>
  {{ end }}
</div>
```

## Solutions

There are two ways to display all articles on the homepage:

### 1. Configuration Approach

Set a large number of posts per page in your site configuration file (hugo.toml):

```toml
[params.paginate]
default = 100  # Set a sufficiently large number
```

This method is simple but not elegant, and pagination will still occur if the number of articles exceeds the set value.

### 2. Theme Override Approach

Create a custom layouts/index.html to override the theme's template, completely removing pagination:

```html
{{ define "main"}}

{{ if site.Params.zenMode }}
<div class="dream-zen-posts max-w-[65ch] mt-8 mx-auto px-4 space-y-8">
{{ range (where site.RegularPages "Type" "posts") }}
  {{ .Render "zen-summary" }}
{{ end }}
</div>
{{ else }}
<div class="dream-grid">
  {{ range (where site.RegularPages "Type" "posts") }}
  <div class="w-full md:w-1/2 lg:w-1/3 xl:w-1/4 p-4 dream-column">
    {{ .Render "summary" }}
  </div>
  {{ end }}
</div>
{{ end }}

{{ end }}

{{ define "js" }}
{{ if site.Params.Experimental.jsDate }}
{{ partial "luxon.html" . }}
{{ end }}
{{ end }}
```

Key changes:
1. Removed the `.Paginate` function
2. Directly use `range` to iterate through all articles
3. Preserved the original layout style
4. Maintained other theme features (like date formatting)

## Performance Considerations

While removing pagination will increase the content loaded on the homepage, this approach is viable in the following situations:

1. Moderate number of blog posts (e.g., fewer than 100)
2. Well-controlled article summaries that don't load too much content
3. Using CDN or other caching mechanisms
4. Users value content accessibility more than loading speed

If the number of articles increases significantly in the future, consider:

1. Reintroducing pagination
2. Implementing infinite scroll
3. Using frontend virtual scrolling technology
4. Adding article category navigation

## Conclusion

By using template overrides, we've elegantly implemented complete article list display, improving user experience. This solution maintains compatibility with the theme while leaving room for future maintenance and upgrades.
