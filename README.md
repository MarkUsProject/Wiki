Wiki
====

Because MarkUs developers do not have edit permissions on the MarkUs Wiki, the practice is to check out this repo and make pull requests for changes.

Admins can merge changes into the MarkUs wiki. The steps are recorded below
because I can't remember them.

- Set a remote for the MarkUs wiki: `git remote add https://github.com/MarkUsProject/Markus.wiki.git` (if you haven't already done this)
- `git pull upstream master` (make sure everything is up to date)
- `git push upstream master` (to get the changes into master)
- `git pull markus-wiki master`  
- `git push  markus-wiki master`

