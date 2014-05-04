================================================================================
MarkUs, Git and Github: How it Works
================================================================================

.. contents::

Overview
================================================================================

.. figure:: images/markus-git-workflow.png

Note that the following few paragraphs might be a bit confusing. Hang in there,
there are always people around you can ask.

Setting up MarkUs
--------------------------------------------------------------------------------

1.  Open a GitHub account

2.  Ask an Admin to add you as a MarkUs developer

3.  Visit the upstream repo on GitHub and select “Fork”. This will create a clone
    of the repository on your GitHub account.

4.  From your fork, copy the URL that allows you to access it using SSH.
    Run the following command in terminal using the URL you just copied

    ::

      $ git clone git@github.com:YOUR_GIT_USERNAME/Markus.git

5.  Next, create a remote to the master repository of MarkUs upstream. This
    will be used to keep your local copy up to date.

    ::

      $ git remote add upstream git://github.com/MarkUsProject/Markus.git

6.  Make sure the remote was added. The following command should output "upstream"
    and "origin".

    ::

      $ git remote -v

MarkUs Development Workflow
--------------------------------------------------------------------------------

The steps involved until your code ends up in the main MarkUs Git repository
are the following:

1.  Create feature branch based on up-to-date local master branch

    ::

      $ git branch issue-1234
      $ git checkout issue-1234

2.  Modify the files with the changes you want to implement. To check which
    files you've modified, as well as view your changes, use:

    ::

      $ git status
      $ git diff

3.  Commit your changes (small commits are beautiful) on your feature branch

    ::

      $ git add path/to/file.rb
      # Or "git add ."
      $ git commit -m "Fix for issue 1234: Implemented x behaviour in file.rb"

4.  Before setting up a review request, make sure your issue and master branches
    are up to date (see section below), making sure the change-sets you just
    pulled in do not affect your code. Once they are, setup a review request.

5.  To submit a pull request, first push your branch to your fork:

    ::

      $ git push origin issue-1234

6.  Go to your GitHub fork and change to your issue branch. You should see the
    button "Pull Request" (right by the button to fork). In the write fill, fill
    it in with the issue number, quick summary of the issue, description of the
    fix and what testing was performed.

The Three MarkUs Git Repositories
--------------------------------------------------------------------------------

In the above picture you can see 3 main Git repositories you will interact
with. The most important ones are your forked MarkUs repository (this is most
likely the only MarkUs Git repository on GitHub you have read+write access to)
and the clone of this repository on your local machine. If you do branch
sharing with another team member that's a slightly different story and beyond
the point of giving a brief overview.

Your Fork of the MarkUs Git Repository
--------------------------------------------------------------------------------

You, as a developer, will be mainly working on the locally cloned MarkUs Git
repository (of your fork). Add to that quite frequent pushes to your fork on
GitHub, so that other developers can test your code easily (due to a current
review board limitation). In order to create a pull request you'd also want to
push your feature branch onto GitHub. This interaction is represented in the
above picture by the large green arrow.

Keeping Your Local Code Up-To-Date
--------------------------------------------------------------------------------

Also note the dashed arrow coming from the main ("upstream") MarkUs Git
repository and pointing to your local clone of your personal MarkUs Github
fork. This arrow represents interaction you have to do to keep up-to-date with
the authoritative MarkUs repository, which is constantly being updated by
other developers on the MarkUs team. (More on how you can do this later.)

This subsection describes the steps you need to take to make sure your local
repository is up-to-date. Generally, your ``master`` branch should mirror the 
contents of ``upstream/master``, the master branch of the main MarkUs repository.
Remember that **you should be doing all development on local feature branches,
NOT your local master branch**! This makes merging as painless as possible.

If this sounds too confusing for you, don't worry, we are here to help.

1.  Make sure you have already set up your "upstream" remote.
    (See Steps 5 and 6 of "Setting up MarkUs" above.)

2.  Switch to your local ``master`` branch.

    :: 
    
      $ git checkout master

3.  Update the current local branch with the ``master`` branch
    of the ``upstream`` repository.

    ::
    
      $ git pull upstream master

    Note: if you've followed our advice and done your development only on feature
    branches, this step shouldn't produce any merge errors!

4.  If you're currently working on a feature branch, switch back to that branch
    and merge the new changes in.
    This step might require some manual merging.

    ::

      $ git checkout issue-1234
      $ git merge master

Rather than running ``git merge master``, you may want to *rebase* to HEAD of 
``upstream/master`` instead, by running the following:

::
  
  $ git rebase upstream/master
  
If this doesn't mean anything to you, you may want to ask for help first.
Seriously, ask for help! There's always somebody around to clarify things. :)


Next Steps
--------------------------------------------------------------------------------

The next step should be to continue reading this document and post questions
you may have on IRC or the markus-dev mailing list.

Imperative Git Configuration
================================================================================

::

  $ git config --global user.name "First name last name"
  $ git config --global user.email "youremail@example.com"

Please do them! You may omit the ``--global`` switch if you wish. Make sure to
read up on the differences between global and non-global git configuration,
though. Thanks.

Git Tricks
================================================================================

- How to keep your copy of the MarkUs repository up-to-date. See
  [[GitKeepingPace]].

- View what changes you have made on branch "issue-1234"
  ``$ git diff --full-index master issue-1234``

- Temporarily put your changes aside to have a cleanly tracked branch
  ``$ git stash``

- Bring these changes back (even onto another branch, as long as it is within the
  same repository) ``$ git stash pop``

- Remove all changes made to a specific file. Let's say I no longer want the
  changes I've made to app/models/membership.rb
  ``$ git checkout app/models/membership.rb``

- Revert all changes made to the current branch (WARNING: All changes will be
  lost). ``$ git reset --hard HEAD``

- Once your branch, issue-1234, has been integrated into master, you might want
  to delete it.

  ::

    $ git branch -d issue-1234
    $ git push origin :issue-1234

- You might want to see who modified a line last, and what other changes
  they brought in with that commit. ``$ git blame config/routes.rb`` You can
  also use the GitHub interface for this by clicking "Blame" when viewing a file.
