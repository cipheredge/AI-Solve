---
title: "How to Optimize Bloated Docker Images"
date: 2025-02-08T18:49:54+04:00
slug: 'docker-image-optimization'
draft: false
cover: "https://jiejue.obs.ap-southeast-1.myhuaweicloud.com/20250208185542747.webp"
tag:
  - Docker
  - Performance Optimization
  - DevOps
---

Recently, while developing a Python web service, we discovered that our built Docker image was a massive 6-7GB. This is clearly not ideal: huge images not only consume significant storage space but also reduce deployment efficiency and increase the risk of errors. This article will share how we optimized this issue.

<!--more-->

## Problem Analysis

After examining the build logs, we identified several key characteristics:

1. The base image used was `tiangolo/uwsgi-nginx-flask:python3.10`, which comes pre-installed with nginx, uwsgi, and flask
2. Python dependency installation took 87.5 seconds, indicating a substantial number of packages
3. Image export process took 63.8 seconds, with layer export alone taking 49.8 seconds
4. The final local test image was about 3.6GB, but would bloat to 6-7GB in certain environments

## Exploring Optimization Solutions

We proposed two optimization approaches:

### Solution 1: Cleaning Build Cache

This approach is relatively simple, mainly focusing on clearing pip cache after installing dependencies:

```dockerfile
RUN python -m pip install -r requirements.txt && \
    pip cache purge && \
    rm -rf /root/.cache/pip
```

### Solution 2: Multi-stage Build

This approach is more complex, utilizing Docker's multi-stage build feature:

```dockerfile
# Build stage
FROM python:3.10-slim as builder
ENV PYTHONPATH=/usr/local
COPY requirements.txt .
RUN pip install --user -r requirements.txt && \
    find /root/.local -type d -name "tests" -exec rm -rf {} + && \
    find /root/.local -type d -name "__pycache__" -exec rm -rf {} +

# Runtime stage
FROM tiangolo/uwsgi-nginx-flask:python3.10
ENV PYTHONPATH=/usr/local
ENV PYTHONUNBUFFERED=1
COPY --from=builder /root/.local /root/.local
# ... other configurations ...
```

## Solution Comparison and Selection

We conducted practical tests of both solutions, with the following results:

Solution 1 (Cache Cleanup):
- Reduced build time by 8 minutes
- Reduced image size by approximately 6GB
- Simple changes, low risk

Solution 2 (Multi-stage Build):
- Added 6 minutes to build time compared to Solution 1
- Only reduced an additional 0.3GB in space
- Complex changes, requires Dockerfile refactoring

Based on the return on investment, we ultimately chose Solution 1. This decision process reflected several important engineering principles:

1. Quantitative Assessment: Not blindly following "best practices," but gathering data through actual testing
2. Progressive Optimization: Starting with simple optimization solutions and evaluating effects before considering more complex approaches
3. Balance Trade-offs: Finding the sweet spot between optimization effects, implementation complexity, and maintenance costs

## Version Control Handling

After trying Solution 2, we decided to revert to Solution 1. To maintain git history integrity, we didn't use `git push -f` to force push, but instead created a new revert commit:

```bash
$ git revert HEAD

# Commit message
revert: multi-stage build optimization

The multi-stage build optimization increased build time by 6 minutes
while only reducing image size by 0.3G compared to the pip cache
cleanup solution. Reverting to keep the more efficient approach.
```

This approach both preserved a complete record of the decision-making process and provided valuable reference information for other team members.

## Lessons Learned

1. Docker image optimization doesn't need to pursue perfection; find the balance between effect and cost
2. Try simple optimization methods before adopting complex solutions
3. Decisions should be based on actual test data, not theoretical "best practices"
4. Version control operations should consider team collaboration needs and maintain historical record integrity

Similar optimization work is everywhere. What's important is establishing a scientific decision-making methodology: forming hypotheses, collecting data, validating effects, and summarizing experiences. This is more valuable than the specific technical approaches used.
