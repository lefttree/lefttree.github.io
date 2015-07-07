---
layout: post
title: Git Workflow
cover: cover.jpg
category: Programming Style
tags: [git, programming style]
---


# Table of Contents
1. [Current Work Flow](#current-work-flow)
2. [Centralized Workflow](#centralized-workflow)
2. [Feature Branch Workflow](#feature-branch-workflow)
3. [Gitflow Workflow](#gitflow-workflow)
4. [Forking Workflow](#forking-workflow)

# Current Work Flow

Usually in my work, I use git commands like
`add`, `commit`, `checkout --`, `reset`, `rebase`, `pull`/`push`,
......

Work routine is 
  
- pull from remote origin
- create new branch, develop new stuff
- push branch to remote origin 
- code review, and merge branch to master branch on origin

Local

- create new branch
- develop weird things lol
- test, stash, commit
- rebase, combine commits into one
- merge to master

# Content

Thanks to [xirong's post](https://github.com/lefttree/my-git/blob/master/git-workflow-tutorial.md), learnt some standard git workflows.
>This won't state all the steps here, I wrote this post  for remembering these stuff, so I assume the reader already know git. 
>If you need to learn the details, please go to the original post

## Centralized Workflow

#### Init 

`git init --bare /path/to/repo.git`

>--bare 
>repo.git

#### Local

`git pull --rebase origin master`

**Fix conflicts**

```
git add <some-file> 
git rebase --continue
```

**If encountered problem**

`git rebase --abort`

**after all done**

`git push origin master`

## Feature Branch Workflow

**Pull request**

Once someone completes a feature, they don’t immediately merge it into master. Instead, they push the feature branch to the central server and file a pull request asking to merge their additions into master. This gives other developers an opportunity to review the changes before they become a part of the main codebase.

**Code review** is a major benefit of pull requests, but they’re actually designed to be a generic way to talk about code. You can think of pull requests as a discussion dedicated to a particular branch.

#### Example 

**Start a new branch**

`git checkout -b`

**Push to central repo**

`git push -u origin marys-feature`
>-u adds it as a remote tracking branch, you can just do `git push` later

**Pull request**

Send a pull request in Git GUI

**Comment, Change**

all on the new branch

**publish**

```
git checkout master
git pull
git pull origin feature-branch
git push
```

## Gitflow Workflow

#### Historical Branches

Instead of a single master branch, this workflow uses two branches to record the history of the project. The master branch stores the official release history, and the develop branch serves as an integration branch for features. It's also convenient to **tag** all commits in the master branch with a version number.

#### Feature Branches

feature branches use *develop* as their parent branch. When a feature is complete, it gets merged back into develop. **Features should never interact directly with master.**

#### Release Branches

Once develop has acquired enough features for a release (or a predetermined release date is approaching), you fork a release branch off of `develop`. Creating this branch starts the next release cycle, so no new features can be added after this point—only bug fixes, documentation generation, and other release-oriented tasks should go in this branch.

Once it's ready to ship, the release gets `merged into master` and `tagged with a version number`. In addition, it should be `merged back into develop`, which may have progressed since the release was initiated.

#### Hotfix Branch

bug fix, branch off `master`, merged back to `master` and `develop`

#### Example

`git checkout -b new-feature develop`

**after done**

```
git pull origin develop
git checkout develop
git merge some-feature
git push
git branch -d some-feature
```

**prepare release**

`git checkout -b release-0.1 develop`
This branch is a place to clean up the release, test everything, update the documentation, and do any other kind of preparation for the upcoming release.

**after release is ready**

```
git checkout master
git merge release-0.1
git push
git checkout develop
git merge release-0.1
git push
git branch -d release-0.1
```

whenever you merge something into master, should tag it
`git tag -a 0.1 -m "Initial public release" master
git push --tags`

**Bug fix**

```
git checkout -b issue-#001 master
# Fix the bug
git checkout master
git merge issue-#001
git push
```

## Forking Workflow

The main advantage of the Forking Workflow is that contributions can be integrated without the need for everybody to push to a single central repository.

When they're ready to publish a local commit, they push the commit to their own public repository—not the official one. Then, they file a pull request with the main repository, which lets the project maintainer know that an update is ready to be integrated. The pull request also serves as a convenient discussion thread if there are issues with the contributed code.

#### Example

**Fork, clone to local**

`git clone https://user@bitbucket.org/user/repo.git`

Whereas the other workflows in this tutorial use a single origin remote that points to the central repository, the Forking Workflow requires two remotes—one for the official repository, and one for the developer’s personal server-side repository.

`git remote add upstream https://bitbucket.org/maintainer/repo
`

**Work on personal features, push to personal remote**

**Pull Request**

**Maintainer integrates their features**

1. Inspect the code directly in the pull request
2. Pull the code into their local repository and manually merge it

```
git fetch https://bitbucket.org/user/repo feature-branch
# Inspect the changes
git checkout master
git merge FETCH_HEAD
```

**Maintainer push to public origin**

