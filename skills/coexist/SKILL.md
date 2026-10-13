---
name: coexist
description: Write or rewrite a PR description, GitHub issue or Linear issue for two readers, a short part for people with the agent's context folded under "For AI agents". Use when opening or editing a PR, filing an issue, or cleaning up an existing description.
---

# Coexist

A description has two readers. A person skims it to decide whether to review, merge or pick up the work. An agent reads every line to do the work. One long write-up serves neither: the person digs for the outcome, and the agent gets prose where it needs paths and commands. Write one part for each, and fold the agent's part so the person never scrolls past it.

## Shape

GitHub PRs and issues. The template's own sections (checklist, screenshots, test steps) belong to the human part; the fold goes last. Keep the blank line after `</summary>`: without it GitHub shows the markdown inside as plain text.

```markdown
<human part>

Fixes #123

<template sections>

<details>
<summary>For AI agents</summary>

<agent part>

</details>
```

Linear issues. Linear also accepts `+++`, but stores and returns `>>>`, which is what an issue shows when read back.

```markdown
<human part>

>>> For AI agents

<agent part>

>>>
```

## 1. Gather the facts

Read the sources: the diff (`git diff <base>...HEAD`), the commits, the linked issues, the tests. When rewriting, the old body is a source too: write its facts down as a list first, and save the original before editing.

Done when you can say what changed, why, and how it was proven, each in one sentence.

## 2. Write the human part

The reader knows the product and did not watch the work.

- The first sentence is the outcome. A PR: what now works, or what a user stops seeing. An issue: what is broken or wanted, and who hits it.
- Then the why, in a sentence or two, when the outcome does not carry it.
- Then one line on how it was proven, or how to try it.
- Under about 120 words. Mechanism, file paths and function names go to the agent part unless a reviewer needs one to decide.
- Lines that tooling parses stay exactly as written: the closing-keyword line (`Fixes #123`), the checklist with its checked boxes, screenshots.
- A to-do list people act on (decisions, follow-ups) stays here.

Then take out AI writing tells. With the `ce-noslop` skill installed, run it with `mode:edit` on the human part. Without it, hold each sentence to five tests: it states a fact rather than a feeling, it would be false in another project, it names who does the verb, it reads on the first pass, and the first sentence carries the outcome.

## 3. Write the agent part

This reader has the repo and none of your context. Write what it cannot find by looking: the reason behind a choice, the gotcha, the rejected path, the exact command. What the diff or the code already says stays out. With the `writing-for-agents` skill installed, load it first.

For a PR:

- **Where**: the files and entry points that matter, as `path:line`, each with its role in a few words.
- **Why this shape**: the decisions, and each rejected alternative with its reason.
- **Invariants**: what must stay true, and what breaks when it changes.
- **Verify**: exact commands and test paths.
- **Follow-ups**: linked tickets and known gaps.

For an issue:

- **Repro**: exact steps, or the failing test.
- **Where to look**: suspected files as `path:line`, checked in the code.
- **Done when**: a checkable criterion, such as a test that goes green or a behavior a user sees.
- **Constraints**: the project rules that apply (`AGENTS.md`, `CLAUDE.md`, contributing guides).
- **Context**: related PRs, issues and docs.

Drop a heading with nothing under it. Bullets, paths and commands carry this part; prose is for reasons. State instructions as the target behavior ("scope the query through the tenant"). Inside a Linear fold, use bold labels instead of headings.

## 4. Check and publish

- Every fact from step 1 sits in one of the two parts, and nothing appears that the sources do not state. A short description with no mechanism to fold stays as it is.
- Other issues are named as bare references (`#12`, `owner/repo#12`) with no fix, close or resolve word in front. GitHub and release tools read closing keywords anywhere in a body, the fold included.
- Publish: `gh pr edit N --repo <owner/repo> --body-file body.md`, `gh issue edit N --repo <owner/repo> --body-file body.md`, or Linear's API or MCP (`save_issue`).
- A Linear description holding uploaded images is edited with partial updates (the MCP's `patch` operations), leaving the image lines alone. Linear returns upload URLs signed with a five-minute expiry on every read, so a full description sent back stores links that break.
- Read it back. Done when the human part shows in full and the agent part sits collapsed under "For AI agents".

Rewrite only text your team or its agents wrote. An issue filed by a customer or an outside contributor stays as its author wrote it.
