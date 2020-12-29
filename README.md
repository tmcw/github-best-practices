I think there are a bunch of resources out there that go _totally wild_ with a specific branch setup or whatever, but are less about the system as a whole and often are sort of uniformly opinionated.

Here are some things I think make sense as recommendations. Read them as geared toward a long-lived, continuously-deployed product, like a web application or API.

These recommendations are opinionated. But they're not fancy or extreme. A lot of teams probably already do something like this.

## Branches

### Opinionated: don't branch off a branch off a branch

There's the main branch, called `main` or `master`. And then there are feature branches. And every once in a blue moon, you want a branch that refers to a feature branch.

A good example is if someone puts up a PR and someone else decides that, instead of giving extensive PR feedback, they might as well just suggest all the changes and improvements they see are necessary. You can do a little of that with GitHub's suggestions system in PRs, but it's kind of limited.

But **don't go beyond that**. Don't create feature branches on feature branches on bug branches. Any kind of branch structure more than one step deep gets incredibly hard to review, maintain, and merge. If a feature relies on some other feature, either work on them in the same branch, or merge one branch and them create a new branch from `main`.

### Unopinionated: branch naming

I see no reason to be picky about branch names. Put your username in them, or make them jokes, make them descriptive, put issue numbers in them. I haven't seen it matter ever: branch names can't contain that much information, they're not highlighted in the GitHub UI, and they shouldn't last long anyway.

> Naming your "main" branch branch on the other hand: name it main by default. If it's already called "master" because you created the repo a while ago, name it main if you think that's important.

### Unopinionated: it really doesn't matter if you delete branches

Some people are careful to delete branches after Pull Requests are merged, and some aren't. GitHub has added some additional magic on top of git so that you can delete a branch from a repository but then recover it.

Deleting branches is useful for the pretty rare case where branch names conflict - you work on something locally `git push origin` it, and the repository already has a branch with that name. That seems pretty rare, and you can just delete the branch then, if you want. Whether you delete branches or not doesn't seem to matter enough to make a rule.

## Issues & Milestones

### Opinionated: everything has a description

Write your issues and pull requests so that other people know what's going on. Issues or PRs that lack descriptions are bad and should be avoided _always_. Understand the [curse of knowledge](https://en.wikipedia.org/wiki/Curse_of_knowledge) and avoid it.

A really great sign of someone who _gets it_ is that they'll have some open source repository and even though it's just them, talking to themselves, they write great issue descriptions.

### Opinionated: Pull requests reference issues

All but the tiniest changes should be written down in an issue before anyone creates a PR. Those PRs should reference the issue, and, ideally close that issue with a `Fixes` reference.

Issues that are too big to be fixed by a single PR may be too big and should be split up into smaller issues.

> This serves a few different purposes. By creating an issue first, others might catch mistaken ideas. If you're planning on writing a PR but never get around to it, the issue crystalizes your intent so that others can pick up the same work later on. If the PR doesn't do the trick, the issue serves to track future work and future PRs that try to solve the same problem.

## Project management

### Unopinionated: "Projects view" for individual repos

The Projects view for individual repos gives a [Kanban](https://en.wikipedia.org/wiki/Kanban_(development)) view of the work being done. This can be nice for project managers, but tends to have no effect on developers and designers.

At best, the Projects view is just another view into the issues: it's automated, so that issues move from place to place when PRs solve them.

The projects view is a problem if it starts to work as a parallel to issues, and people create notes in Projects that really should be issues.

### Opinionated: use milestones for bounded chunks of work and labels for categories. Do not mix them.

- Labels are for categorical information. A good label is "bug", or "feature".
- Milestones are for chunks of work with dates attached. A good milestone is "New feature sprint".

Milestones are useful because they have progress bars and dates. Labels are useful because they're easily searched, color coded, and multiple labels can apply to the same issue.

Do not use milestones for categorical information: if you create a milestone of "bugs" or a milestone of "UI", the progress bar will never reach 100%, and you won't be able to use the "deadline" feature of milestones, because those things are, by definition, unbounded.

## PRs

### Unopinionated: squash, merge, and rebase

All different ways to "merge a PR". Technically, merge and rebase can give you more truthful git history, and squash can give you better semantic git history. But practically, git history rarely really matters in long-lived projects, and sufficiently wacky local git habits will ruin any history.

Time spent on fancy git moves is usually better spent doing something else.

### Opinionated: PR authors should be the one to merge their PRs

Obviously, if only some users have the ability to commit to a repository, then they have to merge other people's PRs.

But in the common setup: technically anyone in a normal GitHub repo can merge any PR. So who should? The PR author is the most practical answer. If the repository does continuous deployment, this makes the person overseeing that automatic deploy the same person with knowledge of what new code is being deployed. If the tests fail as soon as the PR is merged, the PR author is the one responsible for fixing them, and so on.

There are exceptions, of course - if folks are out of the office, or a PR needs to be merged in a hurry, or something else. But the best norm is: author merges.

## Commits

### Opinionated: commit messages should be meaningful

Other people think that commit messages _must be sentences in the present tense with x, y, and z_. Perhaps, but I think it's more important that commit messages just aren't terrible. So no commits like "updating file.js". But "Fixing dotheFunction off-by-one" is a pretty decent commit message.

Tools that commit for you, and create characteristically terrible commit messages by default, are becoming popular. This is, of course, bad. And in that case, it's vital to use squash or rebase to rewrite those bad commit messages into something a a little more useful.

But individual commit messages don't need to be some sort of fine art: the best tool for understanding changes in a repository is the Pull Request that commit came from, which, per the previous advice, better have a well-written description.

## Documentation

### Opinionated: use readme files in the repository for documentation

Core documentation shouldn't live in a wiki, or Notion, or somewhere else: it should live in the repository, alongside code, in Markdown. This helps make it clear which documentation belongs to which point in time. Wikis easily fall out of date, and documentation that isn't in the repo won't get downloaded with the repo.

Try to keep it simple. Fancy tools for writing exist, but plain-text or Markdown should be the format of the vast majority of developer documentation.

