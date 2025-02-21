---
title: "Improving tmuxf: Enhancing tmux Workspace Save and Restore"
date: 2025-02-01T21:30:04+04:00
slug: 'tmuxf-capture-commands'
draft: false
cover: "https://jiejue.obs.ap-southeast-1.myhuaweicloud.com/20250201213744907.webp"
tag:
  - fish shell
  - tmux
  - workflow
---
A few months ago, I wrote "Easily Manage Your tmux Environment with Fish Shell" for saving and restoring tmux workspaces. However, during actual use, I discovered some issues, mainly with command capture and restoration. For example, some long-running commands (like hugo server) couldn't be properly saved, and certain monitoring commands (like btm) were handled inconsistently. This article documents the technical challenges encountered during the improvement process and their solutions.

<!--more-->

## Problems Encountered

### 1. Incomplete Command Capture

In the initial design, we used tmux's `#{pane_current_command}` format string to get the currently running command in a pane. However, this method had several issues:

1. It could only capture the first part of the command (binary name), not the complete command line arguments
2. For commands running in the shell, it returned the shell name (like fish) rather than the actual command

Example: For a command like `hugo server -D --bind 0.0.0.0 --baseURL ...`, only `hugo` was captured.

### 2. Special Process Handling

Certain commands (like the system monitoring tool btm) completely replace the current shell process. These commands have special characteristics:

1. They don't leave visible command text after the prompt
2. They appear directly as process names rather than complete commands
3. Initially, they were handled as direct arguments to tmux new-session, leading to inconsistent behavior

### 3. Command Identification and Matching

We encountered several technical challenges when trying to capture commands:

1. Pane index handling: tmux uses 0-based indexing while fish arrays are 1-based
2. Command string matching: need to handle spaces, special characters, and quotes in commands
3. Distinguishing one-time vs continuous commands: commands like `tmuxf save` shouldn't be saved

## Solutions

### 1. Complete Command Capture

We developed a more sophisticated command capture strategy:

```fish
# Use capture-pane to get panel contents
set -l cmd (tmux capture-pane -t "$session:$window.$pane_idx" -p -S - | rg '^\$ ' | tail -1 | string replace -r '^\$ ' '')
```

This method:

1. Uses `capture-pane` to get the entire panel history
2. Matches command prompts with `rg '^\$ '`
3. Gets the last command using `tail -1`
4. Cleans up the command prompt part

### 2. Improved Command Matching

To correctly identify and match commands, we used more precise regular expressions:

```fish
# If the command starts with the current process name, use the complete command line
if string match -r "^$current_cmd\s+.*" $cmd
    set cmd (string match -r "^($current_cmd\s+.*)$" $cmd)[1]
else if string match -r ".*(\s|^)$current_cmd(\s|\$).*" $cmd
    set cmd (string match -r ".*?(\s|^)($current_cmd\s+[^\$]*)$" $cmd)[2]
end
```

This matching logic:

1. Prioritizes matching complete commands that start with the process name
2. If not, tries to locate the process name in the command and extract the relevant part
3. Handles special characters and quotes in commands

### 3. Unified Command Execution Method

To maintain consistency, we unified the command execution approach:

```fish
# Only specify path when creating session/window
echo "tmux new-session -d -s '$name' -c '$first_path'" >> $config_file

# Execute commands via send-keys
if test -n "$first_cmd"
    echo "tmux send-keys -t '$name:0.0' '$first_cmd' C-m" >> $config_file
end
```

These improvements ensure:

1. All commands execute in the shell environment
2. Special commands and regular commands use the same handling method
3. Command execution environment more closely matches manual operation

### 4. Debug Support

During development, we added detailed debug output:

```fish
echo "Debug: pane_commands=$pane_commands, pane_idx=$pane_idx" >&2
echo "Debug: current_cmd=$current_cmd" >&2
echo "Debug: original cmd=$cmd" >&2
echo "Debug: cleaned cmd=$cmd" >&2
```

These outputs help us:

1. Track the command capture process
2. Diagnose matching issues
3. Verify command cleanup effects

## Technical Points Summary

This improvement involved several shell programming technical points:

1. **tmux Session Management**

   - Using `capture-pane` to get panel contents
   - Handling tmux's indexing system (0-based)
   - Correctly handling command execution environment
2. **fish shell Features**

   - Array index (1-based) handling
   - String operations and replacement
   - Regular expression matching
3. **Process Management**

   - Distinguishing between continuous and one-time commands
   - Handling special processes (like btm)
   - Maintaining execution environment consistency
4. **String Processing**

   - Command line argument parsing
   - Special character escaping
   - Regular expression optimization

## Future Improvement Directions

1. **Configuration Support**

   - Allow users to customize special command lists
   - Support custom command capture rules
2. **Robustness Enhancement**

   - Add more error handling
   - Improve command validation mechanism
3. **Feature Extension**

   - Support more complex window layout saving
   - Add workspace template functionality

This improvement not only solved practical usage issues but also deepened our understanding of tmux and fish shell. We hope these improvements will help other developers using tmux, making workspace management more convenient and reliable.
