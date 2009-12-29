git Presentation
================


What's bothersome about traditional centralized version control
---------------------------------------------------------------

* Built for a time when projects were small, and teams were small and
  colocated
* All version control functionality is handled at the server (depends on
  server for you to "save" your work)
* You need a centralized server...this means you need dedicated systems
  administrator, backup procedures, and all that other jazz.
* All of the version history is stored on the central server! If it goes down
  and your backups fail, you've lost your entire repository history!

What bother's me about subversion
---------------------------------

* All those .svn directories everywhere (or CVS). Messy.
* Branch control...there really is none.
* Simple operations like deleting a file are unnecessarily complex.
* IDE integration works, but it's not perfect and subject to SCM versioning
  issues.

What is git?
------------

* Decentralized version control system
* Linus calls is a stupid content management system
* Built by the Linux Kernel community when nothing else quite fit right
* Now the SCM of choice for many software projects (many have switched from
  subversion to git) [insert reference]


What makes it great?
--------------------

* Easy, does not require a centralized server to start versioning your work.
* Push and pull to and from any remote repository [image]
* Branches are first class citizens
* Does not impose a workflow on its users

Simple Starter Example
----------------------

* Initialize repository in current directory:
    # ls
    presentation.markdown
    # git init
* Add files to repository:
    # git add presentation.markdown
* Commit changes:
    # git commit -m "Initial commit"
* Make changes:
    # edit presentation.markdown
    # git add presentation.markdown
    # git commit -am "Edited presentation.markdown"


Working with a remote repository
--------------------------------

* Push our repository into a remote one (github used for convenience):
    # git remote add github git://github.com/mglazer/GitPresentation.git
    # git push github master
* Syntax of git push:
    git push <repository> <branch>
* repository is the name of the remote repository, get listing by typing:
    # git remote -v
    github	git://github.com/mglazer/GitPresentation.git (fetch)
    github	git://github.com/mglazer/GitPresentation.git (push)
* branch is the name of the branch to push to the remote repository. More on
  this later, *master* is an automatically created default.

Cloning a remote repository
---------------------------

* Get contents of remote repository:
    # git clone git://github.com/mglazer/GitPresentation.git
    # ls
    GitPresentation/
    # cd GitPresentation
    # git remote -v
    origin	git://github.com/mglazer/GitPresentation.git (fetch)
    origin	git://github.com/mglazer/GitPresentation.git (push)
* Make some changes
    # edit presentation.markdown
    # git commit -am "Made some spelling corrections"
    # git push origin master

Pulling in changes from a remote repository
-------------------------------------------

* Before you can successfully push to a remote repository, you have to make sure
  that you pull in the most recent changes. Workflow:
    # git commit -am "Committing most recent changes"
    # git pull origin master
* At this point there may be conflicts. /git mergetool/ helps here, but you can
  also fix the problems yourself.
* Once problems are fixed, type:
    # git add file_in_conflict.txt
    # git commit -am "Fixed merge conflicts"
    # git push origin master


Working with branches
---------------------

* Branches in git are first class citizens, not relegated to the depths of
  separated directory structures as they are in "other" SCMs.
* A typical use case for branches is for tracking features and bug fixes with an
  issue tracker.
* For example, let's say I have a JIRA issue #1234 that tells me I have a
  spelling mistake in my document.

