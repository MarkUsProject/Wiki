Preparing a MarkUs Release
================================================================================

1. Run tests on PostgreSQL and MySQL Databases.

2. Make sure ``app/MARKUS_VERSION`` is updated.

3. Make sure Changelog gets updated.

4. Make sure ``INSTALL`` is up-to-date in the repository and a most recent 
   version is exported on markusproject.org (i.e. ``/path/to/www/INSTALL``).

5. If it is possible, run ``bundle exec rake i18n:missing_keys`` and add them
   to locales (you can keep the English key if you don't know how to translate it.) 
   It will avoid missing locales errors.

6. Make sure `deployment instructions <InstallProdStable.rst>``__ match latest requirements.

7. Make sure a tag gets created for the according release.

8. Make sure a branch for the minor version number (0.7.x for 0.7.0) gets created and pushed.

9. Prepare the archive from the git tree by running the following:

::

  $ git checkout <branchname>
  $ git log
  $ git archive --format=tar -o markus-<version-number> --prefix=markus-<version-number>/ refs/tags/<markus-version-number>
  $ gzip markus-<version-number>.tar

10. Prepare a patch for micro release updates.

11. Upload patch and archive

12. Update latest-stable symlink in ``/download`` directory on markusproject.org

13. Update ``index.html`` on markusproject.org and release notes (blog.markusproject.org)


Preparing a patch from Git
--------------------------------------------------------------------------------
::

  $ git diff refs/tags/<markus-version> refs/tags/<markus-version> > markus-<markus-version>.patch

This will generate a Git style patch. Apply it by::

  $ cd path/to/markus/root && patch -p1 < path/to/patch
