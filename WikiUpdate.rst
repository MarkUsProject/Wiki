================================================================================
Updating the Wiki
================================================================================

Since the wiki contains the important documentation for developers and users,
it is really important to keep it up to date. Unfortunately, github doesn't
make this easy for us.

Developers
================================================================================

Because you don't have push permissions on the main repo, you also don't have 
push permissions on the wiki, and for some reason github treats the wiki repo
differently than other repos. 

Our workaround is to use a separate repo for the Wiki and it is up to the
admins to keep this repo synchronized with the published wiki.

Fortunately, as a developer, you can simply fork the `Wiki repo <https://github.com/MarkUsProject/Wiki>`__ the same way as regular repo and make changes and 
pull requests the same way.

Administrators
================================================================================
(Okay, this documentation is really just for me, since I know I'm going to 
forget it by tomorrow.)

Keeping the published wiki in sync with the Wiki repo:

When changes have been made to the Wiki repo that need to be pulled into the published wiki::
	
	git clone https://github.com/MarkUsProject/Markus.wiki.git
	cd MarkUs.wiki
	git remote add wiki-repo  https://github.com/MarkUsProject/Wiki.git
	git pull wiki-repo master
	git commit -a -m "pulling changes from the wiki repo"
	git push origin master


