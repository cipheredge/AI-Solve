---
title: "Node.js Version Management in Fish Shell - Quick Start Guide"
date: 2025-02-07T23:16:09+04:00
slug: 'fish-shell-node-version-manager-quick-start'
draft: false
cover: "https://jiejue.obs.ap-southeast-1.myhuaweicloud.com/20250208144106522.webp"
tag:
  - Fish Shell
  - Node.js
  - Development Tools
---

As a frontend developer, Tom recently encountered a problem: he needs to maintain several projects simultaneously, but these projects use different versions of Node.js. How can he elegantly solve this problem while using fish shell?

<!--more-->

## Problem Scenario

Tom has three projects:
- An old project using Node.js 14
- A maintenance project using Node.js 16
- A new project using Node.js 18

Every time he switches projects, he has to manually install and switch Node.js versions, which is both time-consuming and error-prone.

## Simple Solution

nvm.fish is a Node.js version manager specifically designed for fish shell. It can:
- Install different Node.js versions with a single command
- Quickly switch between Node.js versions
- Automatically detect the version required by a project

## Installation Steps

1. Create necessary directories:
```fish
mkdir -p ~/.config/fish/functions
mkdir -p ~/.config/fish/completions
```

2. Download and copy files:
```fish
# Place nvm.fish and its completion file in corresponding directories
curl https://raw.githubusercontent.com/donghao1393/fish-assistant/refs/heads/main/plugins/nvm/functions/nvm.fish -o ~/.config/fish/functions/nvm.fish
curl https://raw.githubusercontent.com/donghao1393/fish-assistant/refs/heads/main/plugins/nvm/completions/nvm.fish -o ~/.config/fish/completions/nvm.fish
```

## Daily Usage

### Installing Node.js
```fish
# Install latest version
nvm install latest

# Install specific version
nvm install 16.14.0

# Install LTS version
nvm install lts
```

### Switching Versions
```fish
# Use specific version
nvm use 16.14.0

# Use system version
nvm use system
```

### Viewing Versions
```fish
# View installed versions
nvm list

# View current version
nvm current
```

## Smart Version Switching

Clever Tom created a `.nvmrc` file in each project directory containing the required Node.js version number. This way, nvm.fish automatically switches to the correct version whenever entering the project directory.

For example:
```fish
# Enter project directory and create .nvmrc
cd ~/projects/old-project
echo "14.17.0" > .nvmrc

# Next time entering this directory, nvm will automatically switch to Node.js 14.17.0
```

## Tips

1. Set default version:
```fish
set -g nvm_default_version lts
```

2. Set packages to auto-install:
```fish
set -g nvm_default_packages yarn pnpm
```

With this setting, these packages will be automatically installed whenever a new Node.js version is installed.

## Summary

By using nvm.fish, Tom no longer needs to manually manage Node.js versions. No matter which project he switches to, the correct Node.js version will be automatically ready, allowing him to focus on code development rather than environment configuration.

---

- Illustration: Output of nvm list command showing multiple installed Node.js versions
```fish
$ nvm install 16.14.0
Installing Node v16.14.0 lts/gallium
Fetching https://nodejs.org/dist/v16.14.0/node-v16.14.0-darwin-arm64.tar.gz
Now using Node v16.14.0 (npm 8.3.1) ~/.local/share/nvm/v16.14.0/bin/node
$ nvm list
 ▶ v16.14.0 lts/gallium
   v22.12.0 lts/jod
$ nvm uninstall 16.14.0
Uninstalling Node v16.14.0 ~/.local/share/nvm/v16.14.0/bin/node
```

- Illustration: Demonstrating automatic version switching when entering a project directory with .nvmrc
```fish
$ echo "16.14.0" > .nvmrc
$ cd ..
$ nvm use lts
Now using Node v22.12.0 (npm 10.9.0) ~/.local/share/nvm/v22.12.0/bin/node
$ nvm list
   v16.14.0 lts/gallium
 ▶ v22.12.0 lts/jod
$ z -
$ nvm use
Now using Node v16.14.0 (npm 8.3.1) ~/.local/share/nvm/v16.14.0/bin/node
$ nvm list
 ▶ v16.14.0 lts/gallium
   v22.12.0 lts/jod
```
