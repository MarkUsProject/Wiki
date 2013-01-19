================================================================================
The Ideal MarkUs Git Workflow
================================================================================

.. contents::

Overview
================================================================================
This document will explain how to:

- Initially set up your Git environment for working with MarkUs
- Basic Development Flow
- Code Review
- How to keep your personal Fork-Clone Up-to-date
- Testing your Changes
- Useful Misc. Commands

Setting Up Git for Markus
================================================================================
The first thing to do is to set up a github account and a SSH public key for Git (`Generating SSH Keys <https://help.github.com/articles/generating-ssh-keys>`_).

1. Setup your git configuration settings (Skip this step if you have already done so).
::

	git config --global user.name "First-name Last-name"
	git config --global user.email "your-email@example.com"
2. Visit the `upstream repo <https://github.com/MarkUsProject/Markus>`_ on github and select “Fork”. This will create a clone of the repository on your github account.
3. From your fork, copy the URL that allows you to access it using SSH.
4. Run the following command in terminal using the URL you just copied.
::

 	$ git clone git@github.com:YOUR_GIT_USERNAME/Markus.git
5. Next, create a remote to the master repository of MarkUs upstream. This will be used to keep your local copy up to date.  
::  

	git remote add markus-upstream git://github.com/MarkUsProject/Markus.git
6. Make sure the remote was added.
::

	git remote -v
You should see “markus-upstream” and “origin”.
Note the remote origin, it should point to the SSH URL you cloned with. If this URL contains “https”, then you have not cloned using SSH. Run the following command to change it to the URL used when cloning (see above).
::

	git remote set-url origin git@github.com:YOUR_GIT_USERNAME/Markus.git

Basic Development Flow
================================================================================
The following commands can be run:

- Create a branch
::

	git branch issue-1234
	git checkout issue-1234
- Modify the files with the changes you want to implement. Let’s say I’ve modified Changelog and config.ru with the most amazing fix for issue 1234!
- Take a look at the files you have modified
::

	$ git status
- Then commit these changes
::

	$ git add Changelog config.ru
	$ git commit -m "FIX for issue 1234, implemented awesome function."
- Before setting up a review request, make sure your issue and master branches are up to date (see below), making sure the changesets you just pulled in do not affect your code. Once they are, setup a review request.
::

	$ git push origin issue-1234
Go to your github fork and change to your issue branch. You should see the button “Pull Request” (right by the button to fork). In the write fill, fill it in with the issue number, quick summary of the issue, desciption of the fix and what testing was performed.

Keeping your Personal Fork-Clone Up-to-date
================================================================================
First, make sure you have already set up your upstream remote (If you followed this tutorial, we called it "markus-upstream"). Substitute your remote name for markus-upstream if you named it differently. Run the following commands from terminal.
To update your local master branch
::

	git checkout master
	git pull markus-upstream master
To integrate these changes into your current issue branch
::

	git checkout issue-1234
	git merge/rebase master

Testing your Changes
================================================================================
To be completed in the future.

Useful Misc. Commands
================================================================================
- View what changes you have made on branch "issue-1234"
::

	git diff --full-index master issue-1234
- Temporarily put  your changes aside to have a cleanly tracked branch
::

	git stash
- Bring these changes back (even onto another branch, as long as it is within the same repository)
::

	git stash pop
- If you ever find that you accidentally left something out of your last commit, you can use amend
::
	
	git commit --amend
- Remove all changes made to a specific file. Let's say I no longer want the changes I've made to app/models/membership.rb
:: 

	git checkout app/models/membership.rb
- Revert all changes made to the current branch (WARNING: All changes will be lost).
::

	git reset --hard HEAD
- Once your branch, issue1234, has been integrated into master, you might want to delete it.
::

	git branch -d issue1234
	git push origin :issue1234
- You might want to see who added/modified a line last, and what other changes they brought in with that changeset.
::

	git blame config/routes.rb
This might be easier to do directly on github though! `Here's an example. <https://github.com/MarkUsProject/Markus/blame/master/config/routes.rb>`_

Issues & Solutions
================================================================================
**I forgot to create an issue branch and instead made changes to my master branch. I did not commit anything yet.**

- You can just checkout to the issue branch and then commit. You don't lose your uncommited changes when moving to another branch.

::

	git checkout issue-1234

**I made x number of commits to my master branch and forgot to create an issue branch.**

- Let's say you want to go back to state C, and move D and E to the new branch. Here's what it looks like at first:

::

	A-B-C-D-E (HEAD)
	        ↑
	      master
	
After 

:: 

	git branch issue1234:

It will look like:

::

	    issue1234
	        ↓
	A-B-C-D-E (HEAD)
	        ↑
	      master
	
After 

:: 

	git reset --hard HEAD~2 # Go back 2 commits. You *will* lose uncommitted work:

It will finally look like:

::

	    issue1234
	        ↓
	A-B-C-D-E (HEAD)
	    ↑
	  master

- Since a branch is just a pointer, master pointed to the last commit. When you made the issue1234 branch, you simply made a new pointer to the last commit. Then using git reset you moved the master pointer back two commits. But since you didn't move issue1234, it still points to the commit it originally did.