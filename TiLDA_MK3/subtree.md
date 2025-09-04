# UGFX subtree management

The folder UGFX is a subtree tracking
git@bitbucket.org:mattbrejza/ugfx.git. Please don't edit anything in
there directly unless you know what you're doing.

Read
<https://medium.com/@porteneuve/mastering-git-subtrees-943d29a798ec> for
more information on the topic.

## Prerequisites

    git remote add ugfx git@bitbucket.org:mattbrejza/ugfx.git

## How it's set up

    git read-tree --prefix=extmod/ugfx -u ugfx/master
    git commit -m "..."

## How to update the subtree

    git fetch ugfx
    git merge -s subtree --squash ugfx/master
    git commit -m "Update UGFX subtree to ..."