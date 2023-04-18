---
title: Clean up your git branches
date: '2023-04-18'
tags: ['git', 'cli']
draft: false
summary: Remove local branches that no longer have a remote
---

# Git Cleanup

#### When working in a development team, I inevitably end up with an ever-increasing list of git branches on my local machine. Every time I start a new feature, pull down something a teammate wrote to run it locally, or start a research spike, a new branch gets added.

#### For example, running `git branch` on our Rails monolith, I currently see 16 branches, but I also know that several of these have been merged into main and are now stale. So let's clean them up!

- First, let's switch back to the main branch and pull down while we're there.

```
git switch main && git pull origin main
```

- Let's run a [git prune](https://git-scm.com/docs/git-fetch#_pruning)

```
git fetch -p
```

- Now we're ready to clean it up!

```
git branch -vv | grep ': gone]' | awk '{print $1}' | xargs git branch -D
```

### Ok great, but what is all that gibberish?

#### `git branch -vv` gives us a verbose description of our local branches, which includes information about their remote tracking branches. We pipe the output from that into a `grep` command where we look for the branches that are gone (no longer have a remote branch associated with the local). We grab the names of those branches with `awk '{print $1}'`, which in turn is passed to the final command. `git branch -D` takes those names and deletes the local branches.
