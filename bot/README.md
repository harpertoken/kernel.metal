SPDX Compliance Bot
====================

Automated enforcement of license header compliance on pull requests.

Overview
========

The bot runs on PR events to ensure changed files include proper SPDX headers.

Trigger
-------

- Pull request opened, synchronized, or reopened

Purpose
-------

Enforce SPDX license headers on modified files to maintain compliance.

Actions
-------

- Checks files for missing headers
- Comments on violations with reports
- Suggests inline fixes
- Labels PRs as `license:spdx-noncompliant`
- Blocks merging until fixed

Components
==========

Workflow File
-------------

`.github/workflows/spdx-bot.yml` - Main automation script.

Check Script
------------

`scripts/check-spdx.js` - Analyzes changed files, outputs JSON report.

Auto-fix Script
---------------

`scripts/autofix-spdx.sh` - Adds headers to files (manual workflow).

Metadata
--------

`project.meta.json` - Source of truth for SPDX ID and copyright.

Usage
=====

Bot runs automatically on PRs. For manual fixes:

1. Go to Actions tab > SPDX Auto Fix workflow
2. Run on desired branch
3. Or apply suggestions in PR directly

Configuration
=============

Supported File Types
--------------------

- `.ts`, `.tsx`, `.js`, `.jsx`
- `.py`, `.c`, `.cpp`, `.h`
- `.go`, `.rs`, `.java`, `.rb`, `.sh`

Metadata Source
---------------

SPDX ID and copyright from `project.meta.json`.

Permissions Required
--------------------

- `contents: read`
- `pull-requests: write`
- `checks: write`</content>
<parameter name="filePath">bot/README.md