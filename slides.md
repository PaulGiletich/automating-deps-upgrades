---
theme: academic
transition: slide-left
---

# Automating Dependency Updates

---

## Problem Statement

- No visibility of outdated packages (outside of hands-on engineers)
- No universal process to handle package upgrades
- Increased vulnerability to security issues
- Some older library versions no longer receive security support
- Risk of needing urgent major version bumps

---

## Solution Overview

- Proactive system for keeping libraries fresh
- Scheduled builds for every repo in the org
- Creates user stories for package upgrades in KTLO backlog
- Dashboard to monitor upgrade statuses

---

## Benefits

- Enhanced security
- Improved developer experience
  - Better documentation
  - Better support for newer library versions
- Visibility
- Progress tracking

---

## How It Works

1. Custom GitHub action collects outdated dependencies
2. Creates/updates Rally story for each outdated package
3. Dashboard visualizes stories and their statuses

---

### Github workflow:

```yml {all|21-23}
name: dependency_updates_rally_tasks

on:
  workflow_dispatch:
  schedule:
    - cron: '0 0 * * *'

jobs:
  create-rally-tasks:
    name: 'create rally tasks'
    runs-on: CAI-Enterprise
    steps:
      - uses: actions/checkout@v3
        with:
          fetch-depth: 0
      - uses: actions/setup-node@v3
        with:
          node-version-file: '.nvmrc'
          cache: 'npm'
      - run: npm ci
      - uses: DR-Incubation/dependency-updates-rally-tasks-action@v1.7
        with:
          featureId: F149366 # where to put rally stories
```

---

### Rally story example

![img](/rally.png)

---

### Dashboard

![img](/dash.png)

---

## Future Improvements

- Implement Dependabot for automatic PRs
- Fetch migration guides from changelogs
- Requires enabling automatic updates in GHE
  - Currently disabled due to enterprise worker pool concerns

---

## Next Steps

- Refine bot logic to reduce noise
- Incorporate feedback
- Roll out to all repos
