## Git Backend Development Guide

### Gems used

[**rugged**](https://github.com/libgit2/rugged)

The rugged gem provides the ruby bindings needed to interact with the libgit2 API.

[**gitolite**](http://rubygems.org/gems/git)

The gitolite gem is used for interfacing with gitolite (another piece of software), which will manage repository access permissions for git.

### Gem installation 

You should just be able to do a `bundle install` and get all the needed gems, but you may need to also install CMake for rugged. You can do this with a `sudo apt-get install cmake` or its equivalent.

### Set up the git branch

`$ git checkout git`

Update your gems and start the server.

`$ bundle install`

`$ bundle exec rails server`

### How repos are managed in MarkUs

All repository types, git or svn, must implement the methods for the `AbstractRepository` class, found in `lib/repo/repository.rb`. Same thing goes for `AbstractRevision`. The Rails code interfaces with the repos through AbstractRepository and AbstractRevision and so does not know anything about the implementation details for git or svn repos (nor should it). So it is important that `GitRepository` and `GitRevision` work the same way as their Subversion equivalents. You should not have to touch the Rails code except for refactoring purposes.

#### Important Files

*config/environments/development.rb*
- This file contains all of the configuration settings related to repos. Important keys are 
`REPOSITORY_TYPE`, `REPOSITORY_STORAGE`, and `REPOSITORY_PERMISSION_FILE`.

*lib/repo/repository.rb* and *lib/repo/git_repository.rb*
- These files are what needs work. Mostly in git_repository.

*spec/lib/repos/git_repository_spec.rb* and *git_revision_spec.rb*
- Testing specs. A factory for GitRepository has already been created.


#### GitRevision

MarkUs relies on the concept of linear revisions as in Subversion. However, git does not natively support numbered versioning but uses detachable commits. So GitRevision must go through commits chronologically and order them linearly so it can give every commit a version number.

#### Repository and Revision Classes

It should be noted that the Repository and Revision classes are short-lived objects. They do not get saved to a database. The classes are instantiated and destroyed for each controller action.

#### Gitolite and Permissions

The `gitolite` gem is used to manage permissions on repos. It requires an administrative user to do so, which is also provided by the gem. Not much work has been done on this end.

#### Command-line Authentication

We must be able to verify that the user is who they say they are on the command-line. I believe this is done using LDAP and public keys, but more research needs to be done on this end.