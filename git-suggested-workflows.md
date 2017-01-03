# Git Worfklows
I try to follow my normal workflow in different companies, but I keep running into [this issue](http://stackoverflow.com/questions/26372569/git-interactive-rebase-lists-incorrect-too-many-commits). So I thought I should write some documentation to be able to point to when I tell people that Git really isn't bad if you clean up after yourself as you go.

## Working On Feature Branches
In general it is good practice, when working with others in a repository, to create a new branch for your new features. It's also usually a good idea to keep it updated as new changes arrive in master so you don't have to wrestle with a huge changeset just before you're ready to merge.

Initiating that workflow will look something like this:
  * git checkout master
  * git pull
  * git checkout -b [Queue-#_Feature-summary-here]
    * An example might be PHRECORD-4424_Add-Some-Unit-Tests
  * git push --set-upstream origin [your branch name]
  * At this point, do your work and commit as much as you want with whatever messages are convenient

If, at any point, you need newly-made changes that have been merged into master, [use rebase](https://www.atlassian.com/git/tutorials/merging-vs-rebasing)
  * git commit -am "Before rebase"
  * git checkout master
  * git pull
  * git checkout [your branch name]
  * git rebase master
  * Continue working.

## Cleanup Before Your Pull Request
Once you are ready to merge your new feature, you should clean up your commit history. This will help keep the history readable and avoid messy history that might be hard to merge/rebase with.

also with rebase.
  * git commit -am "Before rebase"
  * git rebase -i HEAD~10

You should end up with something like this:
```
pick 5438190 Started on my feature branch
pick beff793 Made a bunch of small changes
pick 1f88f03 commit before driving home
pick 2749e2c Fixed a bunch of tests
pick 1d9f76a Before Rebase

# Rebase bec1def..1d9f76a onto bec1def (5 commands)
#
# Commands:
# p, pick = use commit
# r, reword = use commit, but edit the commit message
# e, edit = use commit, but stop for amending
# s, squash = use commit, but meld into previous commit
# f, fixup = like "squash", but discard this commit's log message
...
```

Now, obviously you don't want to merge most of that, especially not the parts about rebasing and driving home. I prefer to use 'fixup' and 'reword' to hide these kinds of messages. Edit the first word on each line of the visible commit history make it look something like this:
```
r 5438190 Started on my feature branch
f beff793 Made a bunch of small changes
f 1f88f03 commit before driving home
f 2749e2c Fixed a bunch of tests
f 1d9f76a Before Rebase
```

Notice that the oldest commit is on the top and your most recent is on the bottom. You'll want to mark the top commit with an 'r' and the rest with an 'f'. Depending on the size and complexity of your change, you MAY want to keep more than one commit. If so, mark those with an 'r' as well.

Once you save and close this file, git will fixup your commits and give you a chance to reword the ones you've selected. That prompt will look something like this:
```
Started on my feature branch

# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
```

People structure and word their commit messages in many different ways. One of the most popular seems to be [this one](http://chris.beams.io/posts/git-commit/). Using that guide, our example commit message would end up looking something like this:
```
Add more unit tests for feature X

Coverage for feature x was a little less than 60%, a full 30% less
than our target coverage of 90%. This commit gets the coverage
above 90%.

JIRA: PHRECORD-4424

# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
```

Now, your branch should have a single commit with a nice, descriptive message. At this point, you should follow the instructions in the first section to rebase with master again, build your project and run any tests to make sure nothing unexpected happened, and open a pull request.

If everything went well, you should be greeted with a message that your pull request, "can be performed automatically" and a single commit in the commits tab. Take some time to review your changes to see if you've made any mistakes that you can easily clean up. Once you're satisfied, add some notes, tag and assign reviewers, and let people know you're ready for them to review it.



