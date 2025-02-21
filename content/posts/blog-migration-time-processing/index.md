---
title: "Time Processing in Blog Migration: From Git History to Metadata"
date: 2025-02-02T11:26:52+04:00
slug: 'blog-migration-time-processing'
draft: false
cover: "https://jiejue.obs.ap-southeast-1.myhuaweicloud.com/20250202113321139.webp"
tag:
  - Technology
  - Blog
  - Shell
  - Git
---

During blog system migration, article creation times often become an overlooked yet crucial issue that needs to be resolved. This article shares practical experience on how to recover and maintain article timestamps using Git history information and Shell scripts during a migration from Zola to Hugo.

<!--more-->

## Background

During the migration of my blog from Zola to Hugo, I encountered two issues regarding article timestamps:

1. Due to repository changes, all file timestamps in Zola became the latest, requiring recovery of original creation times from Git history
2. Hugo requires timestamps in a specific format in the article's frontmatter

These two issues seemed simple at first but presented some unexpected technical challenges during implementation.

## Issue One: Recovering File Times from Git History

The first solution involves using Git log information to obtain the earliest commit time for each file. This process includes several key steps:

1. Using `git log` to get file commit history
2. Parsing date information
3. Converting dates to the correct timestamp format
4. Using the `touch` command to update file times

Let's look at the specific implementation:

```fish
for file in content/posts/*.md
    set gitdate (git log -1 --format="%ad" -- $file)
    echo "Processing file: $file"
    echo "Git date: $gitdate"
    
    # Parse year
    set year (string match -r '\d{4}' $gitdate)
    
    # Parse month
    set month_name (string match -r '\s([A-Za-z]{3})\s' $gitdate)[2]
    switch $month_name
        case Jan; set month "01"
        case Feb; set month "02"
        case Mar; set month "03"
        case Apr; set month "04"
        case May; set month "05"
        case Jun; set month "06"
        case Jul; set month "07"
        case Aug; set month "08"
        case Sep; set month "09"
        case Oct; set month "10"
        case Nov; set month "11"
        case Dec; set month "12"
    end
    
    # Parse date and time
    set day (string match -r '\s(\d{1,2})\s' $gitdate)[2]
    set hour (string match -r '(\d{2}):\d{2}:\d{2}' $gitdate)[2]
    set minute (string match -r '\d{2}:(\d{2}):\d{2}' $gitdate)[2]
    set second (string match -r '\d{2}:\d{2}:(\d{2})' $gitdate)[2]
    
    # Format timestamp
    set timestamp "$year$month$day$hour$minute.$second"
    echo "Setting timestamp: $timestamp"
    touch -t $timestamp $file
end
```

This script uses fish shell features for string matching and conversion, ultimately generating a timestamp format that meets the requirements of the `touch` command.

## Issue Two: Updating Hugo Article Frontmatter Times

The second issue involves setting the correct time format in Hugo article frontmatter. We encountered some interesting technical details in this process:

First, we encountered formatting issues when trying to use `gstat` to get times directly:

```bash
$ gstat -c '%Y-%m-%dT%H:%M:%S' article.md
1738439771-/-16777232T?:?:?  # Incorrect output format
```

After debugging, we found that we needed a different approach to get and format times. The final solution was:

1. Use `gstat --format="%W"` to get the raw timestamp
2. Use `gdate` to convert the timestamp to ISO 8601 format
3. Use `sed` to handle timezone formatting

Here's the final implementation:

```fish
#!/usr/bin/env fish

# Recursively find all index.md files
for md_file in (fd "index_?.*.md")
    set -l md_dir (dirname $md_file)
    # Get file creation time, format to ISO 8601
    set create_time (gstat --format="%W" $md_dir | xargs -I{} gdate -d "@{}" '+%Y-%m-%dT%H:%M:%S%z' | sed 's/\([+-][0-9]\{2\}\)\([0-9]\{2\}\)/\1:\2/')
    
    # Update frontmatter
    # Use sed to replace date: line
    # Match date: YYYY-MM-DD format, replace with time-included format
    sed -i '' -E "s/^date: ([0-9]{4}-[0-9]{2}-[0-9]{2})/date: $create_time/" $md_file
    
    echo "Updated $md_file with date: $create_time"
end
```

## Issue Three: Minute-Precise Time Display

After completing the basic time migration, we discovered a new requirement: Hugo only displays year, month, and day by default, making it impossible to show the order of multiple articles published on the same day. To solve this, we needed to adjust Hugo's time display format.

We enabled more precise time display by modifying Hugo's configuration file:

```toml
[params.experimental]
  jsDate = true
  jsDateFormat = "yyyy-MM-dd HH:mm"
```

This change brought several benefits:

1. More precise article sequence display
2. Clear publishing order for multiple articles on the same day
3. More detailed time information for readers, especially for newly published articles

## Technical Analysis

While solving these three issues, we encountered and resolved several key technical details:

1. Git Date Format Parsing
   - Git's date output format is specific and requires correct parsing of each component
   - Regular expression extraction of date components needs to consider various edge cases

2. Timestamp Conversion
   - Converting Unix timestamps to human-readable formats
   - Converting between different formats (Git format, touch command format, ISO 8601 format)

3. Hugo Time Display Customization
   - Using JavaScript's date formatting capabilities
   - Enabling experimental features in theme configuration
   - Balancing time display precision and readability

## Lessons Learned

This time processing practice taught us several important lessons:

1. When dealing with time-related issues, it's best to confirm all involved format specifications first
2. When using command-line tools, verify output formats and don't make assumptions
3. Consider error handling and edge cases when writing scripts
4. Regular expressions are powerful tools for complex string processing but need careful use
5. Website time display is both a technical and user experience issue

## Room for Improvement

While the current solution meets requirements, there are potential areas for improvement:

1. Add error handling and logging
2. Support more time formats and timezone handling
3. Add backup mechanisms to prevent accidental file modifications
4. Optimize performance, especially when handling many files
5. Consider adding more flexible time display configuration options

## Extended Thoughts

This case study also reveals some broader technical applications:

1. Similar time processing methods might be useful in other types of content migration
2. Git history information can be used not only for time recovery but also for extracting other metadata
3. Shell scripts, though "old," remain the most effective tools for automating specific tasks
4. The balance between time display precision and readability is a worthy user experience consideration

With these scripts and experiences, we can more confidently handle time-related issues in future blog migrations or content management. Of course, as technology evolves, our solutions need to keep pace with continuous improvement and optimization.
