GIT Intro
=========

Abstract
--------

From http://en.wikipedia.org/wiki/Git_(software)

* Git is a distributed revision control and source code management (SCM) system with an emphasis on speed.
* Git was initially designed and developed by Linus Torvalds for Linux kernel development in 2005.
* Based on a recent survey of Eclipse IDE users, Git is reported to have 30% adoption as of 2013.
* Every Git working directory is a full-fledged repository with complete history
and full version tracking capabilities, not dependent on network access or a central server.
* Strong support for non-linear development
* Distributed development
* Compatibility with existing systems/protocols
* Efficient handling of large projects
* Cryptographic authentication of history
* Toolkit-based design
* Pluggable merge strategies
* Garbage accumulates unless collected
* Periodic explicit object packing

Git vs SVN
----------

From https://git.wiki.kernel.org/index.php/GitSvnComparison

* Git is much faster than Subversion
* Subversion allows you to check out just a subtree of a repository; Git requires you to clone the entire repository (including history) and create a working copy that mirrors at least a subset of the items under version control.
* Git's repositories are much smaller than Subversions (for the Mozilla project, 30x smaller)
* Git was designed to be fully distributed from the start, allowing each developer to have full local control
* Git branches are simpler and less resource heavy than Subversion's
* Git branches carry their entire history
* Merging in Git does not require you to remember the revision you merged from (this benefit was obviated with the release of Subversion 1.5)
* Git provides better auditing of branch and merge events
* Git's repo file formats are simple, so repair is easy and corruption is rare.
* Backing up Subversion repositories centrally is potentially simpler - since you can choose to distributed folders within a repo in git
* Git repository clones act as full repository backups
* Subversion's UI is more mature than Git's
* Walking through versions is simpler in Subversion because it uses sequential revision numbers (1,2,3,..); Git uses unpredictable SHA-1 hashes. Walking backwards in Git is easy using the "^" syntax, but there is no easy way to walk forward.

Explain key concepts
--------------------

* Working copy.
* Index aka Stage.
* Commit.
* Remote.

Introduce yourself
------------------

    git config --global user.name "Name Surname"
    git config --global user.email "your@email.com"
    cat ~/.ssh/id_rsa.pub
    # ssh-rsa AAAAB3Nza...HL7a0lz denisr@denisr.com
    # ssh-keygen

Clone repo
----------

    git clone git@github.com:denis-ryzhkov/git-intro.git
    cd git-intro

Create repo
-----------

    mkdir git-intro && cd $_
    git init
    echo '*.pyc' > .gitignore
    git add .gitignore
    git commit -am 'See #1: Initial commit with ".gitignore" only.'
    git remote add origin git@github.com:denis-ryzhkov/git-intro.git
    git push -u origin master

Update code from local to remote
--------------------------------

    echo 'Hello world' > index.html
    echo '*.log' >> .gitignore
    git add index.html
    git status
    # git checkout -- file
    git diff
    git diff --color-words
    git diff --staged
    git commit -am 'See #1: Prototype of Facebook v42.'
    git push

If repo has branches:

    git push origin master

Implicit "git push" behavior is git-config and git-version specific,  
so unreliable: http://stackoverflow.com/a/9749535/350937

Update code from remote to local
--------------------------------

    git pull

    git stash
    git pull
    git stash pop

    git commit -am 'WIP'
    git pull
    git commit -am 'Finished: ...'

Aliases
-------

    git config alias.l "log --decorate=full --graph --color=auto --all"
    git l

    ~/.alias:
    alias gl='git log --decorate=full --graph --color=auto'
    alias gla='gl --all'
    alias gs='git status'
    alias gsu='git submodule update'
    alias gcr='git log --reverse -p --full-diff' # be4b4f4..HEAD # code review

    ~/.profile:
    source $HOME/.alias

Branches
--------

Create:

    git checkout master && git pull
    git checkout -b feature
    git push -u origin feature

Update:

    git checkout master && git pull
    git checkout feature
    git merge --no-ff master

Merge:

    git checkout master
    git merge --no-ff feature
    # Fix conflicts.
    git add fixed1 fixed2 fixedN
    git commit
    # OR
    git merge --abort

Delete:

    git checkout master
    git branch -d feature
    git push origin :feature

Rebase
------

    gl
    git rebase -i e4b6e03ff91384df2bc7e09737b6f3a26a362c61
    # pick, reword, squash, etc.
    # OR
    git rebase e4b6e03ff91384df2bc7e09737b6f3a26a362c61
    # Fix conflicts.
    git rebase --continue
    # OR
    git rebase --abort
    git push -f origin feature
    # Re bad practice.

Revert
------

    git revert e4b6e03ff91384df2bc7e09737b6f3a26a362c61

Reverting merges:

    # Let M is merge commit.
    git revert M
    # Produces RM
    # Later:
    git revert RM
    # Explain why.

Amend, Checkout, Reset
----------------------

    git commit --amend -am 'Old code. And new code too.'

    git checkout e4b6e03ff91384df2bc7e09737b6f3a26a362c61
    git checkout master

    git reset --soft e4b6e03ff91384df2bc7e09737b6f3a26a362c61
    # Fix.
    git commit -am 'Fixed...'

    git reset --hard e4b6e03ff91384df2bc7e09737b6f3a26a362c61
    git push -f origin feature

Patch, Cherry-Pick
------------------

    git diff > patch.diff
    git apply patch.diff

    git cherry-pick e4b6e03ff91384df2bc7e09737b6f3a26a362c61

Tags
----

    git tag v42
    git push origin v42

GitFlow
-------

http://nvie.com/posts/a-successful-git-branching-model/


About
-----

git-intro version 0.1.2  
https://github.com/denis-ryzhkov/git-intro  
Copyright (C) 2013 by Denis Ryzhkov <denisr@denisr.com>  
MIT License, see http://opensource.org/licenses/MIT
