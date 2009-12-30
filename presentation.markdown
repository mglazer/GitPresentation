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
    # git branch master 1234-spelling-mistake
    # git checkout 1234-spelling-mistake
    # edit presentation.markdown
    # git commit -am "Fixed spelling mistake"
* Now I would like to merge back into the master branch:
    # git checkout 1234-spelling-mistake
    # git fetch origin master
    # git rebase origin/master
    # git checkout master
    # git merge 1234-spelling-mistake
    # git push origin master
* What did we do here? First we made sure we were still working on our feature
  branch. Then we ensured that we had the latest master branch version from the
origin repository. When then performed a rebase, this fast forwards our current
branch so that it contains all of the changes from the listed branch. We then
switch back to the master branch and merge the changes from our feature branch
into the master branch. Finally, we can push our changes to the remote
repository.

Interactive Rebase
------------------

* Generally when you branch, you can expect to have lots of individual commits.
* Most of the time you don't want these individual commits to appear in the
  repository history.
* git lets you "squash" these into one individual commit.
* Type:
    # git rebase -i master
* You'll get an editor window that looks like this:
    pick 341e027 Added a new bullet point
    pick 12fd986 Added an important bullet point
    pick ae29d44 Done adding bullet points
* Edit the file such that it looks like this:
    pick 341e027 Added a new bullet point
    squash 12fd986 Added an important bullet point
    squash ae29d44 Done adding bullet points
* Save and quit, you'll then get a new editor window that allows you to enter in
  the actual commit message, enter in something useful like this:
    [#1234] Fixed spelling mistakes

    * Added a few bullet points 
    * Fixed a few spelling mistakes
* Save and quit.
* Now switch back to the master branch and merge back your changes:
    # git checkout master
    # git merge 1234-spelling-mistake
    # git push origin master


Tagging Releases
----------------

* Remember in the good ol' days of subversion where you would perform a copy of
  your entire trunk in order to tag a release?
* git makes this far easier:
    # git tag v1.0.0
    # git tag
    v1.0.0
* To then switch to this tag at a later date, type:
    # git checkout v1.0.0
* Can also digital sign tags with PGP. See [2]

GUIs
----

* gitk - Neat little GUI for examining a git history. Can't edit.
* gitweb - Web based git repository browser.
* Other more "friendly" interfaces being actively developed (but command line
  still rules).


But I'm forced to use subversion
--------------------------------

* You can use svk [3], it's a set of Perl scripts that sits on top of subversion
  and lets you perform distributed development.
* /git svn/ is a tool providing bidirectional flow of changes between a
  Subversion and a git repository.
** I've never used it, has a long list of caveats and things it cannot do.

Other neat git features
-----------------------

* Look in the .git directory (there's only one, it's at the base of your tree)
** .git/config - holds handy configuration data about the current repository.
** .git/description - use primarily by gitweb for providing a description of
your repository
** .git/hooks - A number of hook scripts that can be run at various times during
the development process (ie: make sure tests are run before doing a commit)


    
    

[1] http://reinh.com/blog/2009/03/02/a-git-workflow-for-agile-teams.html
[2] Digitally signing with PGP.
[3] svk
