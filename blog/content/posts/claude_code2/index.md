---
title: <Claude Code> 2. Making Claude Code Write Like Me
date: 2026-11-01T10:04:20Z
lastmod: 2026-11-01T10:04:20Z
author: Jun Yeop(Johnny) Na
avatar: /images/favicon.svg
# authorlink: https://author.site
cover: logo.png
category: General Dev
categories:
  - Claude Code
  - Vibe Coding
  - Coding Agent
tags:
  - Claude Code
  - LLM
  - Agent Skills

# nolastmod: true
draft: false
---

I want Claude Code to write blog drafts that sound like my posts, so I don't have to rewrite every sentence after it gives me a draft.

There is already a `writing-like-user` skill on GitHub, so I downloaded it and checked what was inside.

# 1. Download the Skill

The skill has 6 files: `SKILL.md`, 4 guides for each writing type, and a 105KB `samples.txt`.

```bash
gh search code 'writing-like-user' --limit 10 --json repository,path
```

There's one problem: **every rule and sample in the skill comes from the original author's writing, not mine.** The name says "user", but the skill would write in the voice of someone else, using a lot of em dashes and fiction samples that have nothing to do with my blog.

So I deleted all 6 files and rebuilt the skill using my own posts.

# 2. Analyze My Posts

My blog has 53 markdown files and about 28,700 words. I removed the code blocks and counted the patterns I actually use, since my impression of my own writing isn't a reliable source.

- `ex)`: 23 times
- `->`: 31 times
- `However`: 9 times
- Capitalized `But`: 0 times
- Numbered headings: 121 of 241

Two posts didn't fit: `sporter` and `useful_webtools` are the only posts that use em dashes, so I left them out of the examples for now.

My posts also fall into three types, which need different rules:

1. Case Study: work projects with production results
2. Build Log: step by step implementation posts
3. Study Notes: concepts I want to look up later

# 3. Test Without the Skill First

To check if the skill actually works, we need a baseline. I held out 3 of my real posts, one for each type, and gave a fresh agent only the facts from each post.

The agent wrote clean posts, but they didn't look like mine:

- 18 table lines, while my posts have no tables
- 3 added takeaway or summary sections
- Average sentence of 14.8 words on the case study, while my post has 31.8

Most of these are shape problems, so **the skill describes what each type of post looks like instead of listing things to avoid.**

# 4. Write the Skill

`SKILL.md` picks the post type, and each type has its own file with the post structure and excerpts from my blog.

```text
writing-like-user/
├── SKILL.md
├── CASE-STUDY.md
├── BUILD-LOG.md
└── STUDY-NOTES.md
```

The test results came out way better than the baseline, with no tables and no added takeaway sections. However, the agent overcorrected in two places:

- Sentences were too short, because I often chain my reasoning with so, because, and which.
- Every heading was numbered, while only about half of my headings are numbered.

I added both rules and ran the test again. The first version of the fix quoted one of the held-out posts, which would have made the test useless, so Claude replaced those examples with lines from other posts before running it.

# 5. Testing

The case study went from 14.8 words per sentence to 27.1, which is a lot closer to the 31.8 of the original post.

The build log is still short, at 14.2 words per sentence against 38.0. **The original post only has a few lines of prose, so that number isn't very reliable.**

# 6. Save the Skill to My Settings Repo

My Claude Code settings are backed up to a private repo by a sync script, but that script only records skills that have their own git repo. This skill is a plain local folder, so it would never get backed up.

To solve this we add one line to the sync and install scripts:

```bash
copy_if_exists "skills/writing-like-user" dir
```

Now the skill gets synced with the rest of my settings.

There are still things to do:

- Decide if `sporter` and `useful_webtools` should count as my current voice.
- Add Korean samples, since the skill only works for English posts.
