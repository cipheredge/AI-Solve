---
title: "Extracting and Analyzing JSON Messages from Logs"
date: 2025-02-16T20:34:33+04:00
slug: 'extract-json-from-logs'
draft: false
cover: "https://jiejue.obs.ap-southeast-1.myhuaweicloud.com/20250216203652932.webp"
tag:
  - Linux
  - DevOps
  - Log Analysis
---

In DevOps work, we often need to extract and analyze specific formatted information from log files. Recently, I encountered a scenario where I needed to extract and analyze different types of API calls from a log file containing numerous JSON format messages. This article will share how to efficiently accomplish this task using Linux command-line tools.

<!--more-->

## Background

Let's say we have a microservice system log file `/var/log/app-server.log` that contains many JSON format messages like this:

```
2025-02-15T20:04:25.720Z [AppServer] [info] Message from client: {"method":"resources/list","params":{},"jsonrpc":"2.0","id":1347}
2025-02-15T20:04:25.721Z [AppServer] [info] Message from client: {"method":"prompts/list","params":{},"jsonrpc":"2.0","id":1348}
```

Our goals are:
1. Extract all JSON messages from these logs
2. Group and count API calls of the same type (same method)
3. Ignore the id values that differ in each call

## First Attempt: Simple but Incomplete

Initially, we might try something like this:

```bash
cat /var/log/app-server.log | rg 'Message from client' | awk '{print $7}' | sort -u
```

This command combination:
- Uses `cat` to read the log file
- Uses `rg` (ripgrep, a faster grep) to find lines containing "Message from client"
- Uses `awk` to extract the 7th column (JSON content)
- Uses `sort -u` to remove duplicates

While this seems reasonable, it has two problems:
1. Different ids in the JSON will cause identical calls to be treated as different records
2. If the log format changes slightly, such as JSON content spanning multiple columns, `awk` will extract incomplete data

## Improved Solution: Precise Extraction and Unified Processing

Let's improve this step by step:

First, use `rg`'s `-o` option to directly extract complete JSON:

```bash
cat /var/log/app-server.log | rg -o '\{.*\}'
```

`-o` outputs only the matching part, and `\{.*\}` matches everything from `{` to `}`.

Then, use `jq` to process JSON and standardize the id:

```bash
cat /var/log/app-server.log | rg -o '\{.*\}' | jq -c '. | .id = 0' | sort -u
```

Here:
- `jq -c` outputs compressed JSON format (one record per line)
- `. | .id = 0` sets the id of each record to 0
- `sort -u` removes duplicates

Finally, we get clean output:

```json
{"method":"prompts/list","params":{},"jsonrpc":"2.0","id":0}
{"method":"resources/list","params":{},"jsonrpc":"2.0","id":0}
```

## Further Optimization: View Only Key Information

If you only want to see different methods, you can do this:

```bash
cat /var/log/app-server.log | rg -o '\{.*\}' | jq -c '. | .method' | sort -u
```

The output will be more concise:

```
"prompts/list"
"resources/list"
```

## Summary

Through this example, we've learned:
1. Using `rg -o` to precisely extract JSON content
2. Using `jq` for JSON processing and standardization
3. Using `sort -u` for deduplication and counting

These techniques are not only applicable to processing API call logs but are equally effective for any log files containing JSON format data. Mastering the combination of these command-line tools can greatly improve log analysis efficiency.

## Further Reading

- [ripgrep documentation](https://github.com/BurntSushi/ripgrep/blob/master/GUIDE.md): Learn more about advanced usage of ripgrep
- [jq manual](https://stedolan.github.io/jq/manual/): Explore jq's powerful JSON processing capabilities
- [Linux text processing commands](https://linuxhandbook.com/text-processing-commands/): Learn more text processing tools
