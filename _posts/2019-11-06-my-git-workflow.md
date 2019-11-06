---
layout: post
title:  "My Git Workflow"
date:   2019-11-06 18:00
permalink: 'blog/my-git-workflow'
excerpt: "Git, the version control software that often lives up to its name. After 7 years of study, I'm starting to understand how things hand together, so I've written them down before I forget."
featured-image: "git.jpg"
---

## Scenario:
Let's assume that, for the purpose of this blog post, we're adding some new validations to an existing model in our software. Let's also assume that we'll be the only developer working on this particular update, meaning we can be a little more creative with the git commands we run (specifically, we can run `git push --force-with-lease` without ruining someone's day).

## Step 1: Create your feature branch
It's generally considered bad practice to develop new behaviour in your `master` branch. Instead, let's switch to `master` and immediately make a new branch called `add-model-validations`. Rather confusingly, both of these tasks are achieved through git's `checkout` command:

```
git checkout master
git checkout -b add-model-validations
```

## Step 2: Develop your new feature.
It's time to code. Let's get to work adding our new model validations, remembering to create regular [atomic commits](https://en.wikipedia.org/wiki/Atomic_commit). Whenever possible, each of our commits should achieve a single purpose, grouping all relates changes together into a meaningful unit.  

Historically I've spent a lot of time crafting beautiful commit messages based on an excellent [blog post](https://chris.beams.io/posts/git-commit) by Chris Beams. I've since come to understand that commits on our feature branches needn't be perfect, we'll have a chance to improve them before they're up for code review or deployment. We needn't feel too guilty about the occasional "fix failing tests" commit message 😉.

```
git add my_model.rb my_view.html.erb
git commit -m "Add model validations and ensure any errors are rendered by the view"

git add my_model_test.rb
git commit -m "Fix failing tests"
```

## Step 3: Tell a clear story
OK, we've worked tirelessly to add our new validations and we think our branch is ready for code review. At this point we've likely amassed lots of commits that were super-helpful as we coded, but probably offer less value now we've reached our destination. To help the developer who'll be reviewing our code, let's take a few minutes to merge related commits together and potentially reword some of our sloppier commit messages, thus telling a clearer story.  

First, we need to enter git's `rebase` tool, a powerful feature that allows us to rewrite history in our branch. We invoke the `rebase` command with the interactive (`-i`) flag and a git SHA (gibberish that represents a single commit) denoting the point from which we want to rebase. We can view our commits and their associated SHA values using the `git log` command. We want to kick off the rebase from the first commit of our `add-model-validations`, minus one commit, which is denoted by the little hat (^) symbol at the end of the command below.

```
git rebase -i d9c372e6ad9^
```
Running the above command will open your editor to show all the commits after the SHA you provided as an argument. Since we provided the SHA for the commit directly before our first feature commit, we're shown a list of all the work we've done in the `add-model-validations` branch:

```
pick d9c372e6ad9 Add model validations and ensure any errors are rendered by the view
pick ac21sdff89e Fix indentation error
pick bh7812dacce Fix typo in variable name
pick b2e8ffac57e Fix failing tests
pick ad29l1r4vc0 Add new tests
```

First, let's focus on the two "fix" commits that we see on lines 2 and 3. As a reviewer, I probably don't care that you messed up your indentation or fat-fingered a variable name. Let's merge these "fix" commits to become part of the very first commit so that our history becomes easier to read. We tell git to merge commits 2 and 3 into commit 1 by replacing the verb `pick` with the verb `fixup` on lines 2 and 3:

```
pick d9c372e6ad9 Add model validations and ensure any errors are rendered by the view
fixup ac21sdff89e Fix indentation error
fixup bh7812dacce Fix typo in variable name
pick b2e8ffac57e Fix failing tests
pick ad29l1r4vc0 Add new tests
```

Next, let's focus on the final two commits, both of which focus on updating our tests. You'll note that our commit messages aren't particularly meaningful; we've documented that some old tests were fixed and some new tests were added, but we haven't explained the *what* or *why* behind our testing approach. The good news is that we can update the final two commits by replacing the verb `pick` with the verb `reword`, after which our editor will be opened allowing us to supply more meaningful commit messages.

```
pick d9c372e6ad9 Add model validations and ensure any errors are rendered by the view
fixup ac21sdff89e Fix indentation error
fixup bh7812dacce Fix typo in variable name
reword b2e8ffac57e Fix failing tests
reword ad29l1r4vc0 Add new tests
```

## Step 4: Force your reality
Congratulations, we've now got a beautiful feature branch that tells a very clear story and stands a good chance of passing code review. We can verify that our `git rebase` completed successfully by reviewing our (rewritten) history via `git log`, after which we can force push our new reality upon Github using `git push --force-with-lease`.

```
git log --pretty=oneline
d9c372e6ad9 Add model validations and ensure any errors are rendered by the view
b2e8ffac57e Update existing specs to handle potential HTTP redirects if model validation fails
ad29l1r4vc0 Add new specs to ensure our model validations function as expected

git push --force-with-lease
```

I hope this helps someone out there!

Photo by Yancy Min on Unsplash
