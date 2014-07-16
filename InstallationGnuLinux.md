# Setting up a development environment for MarkUs development on GNU/Linux
**Please note the difference between "$>" and "#>". The former means execute 
the command as simple user, the latter means execute the command as the 
super-user or use sudo as normal user ("#>" is equivalent to "> sudo").**

If your system complains about gems not found when trying to run, for example,
`bundle install`, it is because you don't have the path of the binary in your
`$PATH` variable. Find the path of the gem (usually 
`/var/lib/gems/2.1.2/bin/`) and add it to your `$PATH`.

## Setting up Git
Git is the Source Content Management used by MarkUs. You can find some documentation on GitHub: [How to set-up Git on GNU/Linux](http://help.github.com/linux-set-up-git). You will also have to set up a GitHub account.

Next, set up your MarkUs GitHub repository by following [these instructions](GitHowTo.rst).

## RVM
RVM (Ruby Version Manager) is a command line tool which allows us to easily install, manage and work with multiple ruby environments from interpreters to sets of gems. You can learn more at their [official website](https://rvm.io).
With RVM, you can easily work using different versions of Ruby with your projects. Here, you will find particular installation instructions for setting-up a RVM environment with MarkUs.

### Install RVM
First, install RVM:

```
$> sudo apt-get install curl
$> curl -L get.rvm.io | bash -s stable --auto
```
Next we need to reload the `~/.bash_profile` file:

```
$> . ~/.bash_profile
```

Run `rvm requirements` to produce a list of all the require packages:

```
$> rvm requirements
```
Next, you need to be root or use sudo (the Ubuntu way) to issue the following commands. Both methods are correct. Use only one of the following methods.

As root:

```
$> su  # and then enter your root password
#> apt-get install ruby-full build-essential git ruby-svn subversion libv8-dev
#> # make sure ruby-full points to the correct ruby version (1.9)
```
As normal user, with the sudo command:

```
$> sudo apt-get install ruby-full build-essential ruby-svn subversion libv8-dev
$> # and then enter your root password, make sure ruby-full points to the correct ruby version (2.1)
```

### Install Ruby
We will install and use Ruby 2.1.2, but just to illustrate how easy rvm makes using multiple versions we will also install Ruby 2.0.0:

```
$> rvm install 2.0.0
$> rvm install 2.1.2
```
Now, if you do `rvm list`, you will see two version of Ruby available. You can choose one by doing `rvm use 2.0.0` or `rvm use 2.1.2`.

**Note**: You can check which version of Ruby you are currently using by doing `ruby -v`

**Note**: You will have to do rvm `use x.y.z` in every shell you use

To make a version of ruby default:

```
$> rvm --default use 2.1.2
```

### Update RVM
If there is a new version of Ruby, you will want to install it (like a new patch release for Ruby 2.1.2). First, you will have to update RVM:

```
$> rvm get stable
```
Then update Ruby:

```
$> rvm install 2.1.2
```
Remember to recompile `libsvn-ruby` for this new version of Ruby, along with reinstalling all the gems too!

## RubyGems and Bundler
RubyGems is a Ruby packaging system designed to facilitate the creation, sharing and installation of libraries (in some ways, it is a distribution packaging system similar to, say, apt-get, but targeted at Ruby software). Since version 1.9+ Ruby comes with RubyGems by default. 
Just `cd` into the root directory of your local copy of the Markus project and install Bundler for each version of Ruby, along with installing the required gems:

```
$> rvm use 2.0.0
$> gem install bundler
$> bundle install
$> rvm use 2.1.2
$> gem install bundler
$> bundle install
```
This will install all dependencies needed by MarkUs. Please note that Bundler may ask you for your root password.

On Ubuntu and Debian systems, the system may not be able to find Bundler. You can fix this by adding it to your `PATH`, or running it directly:

```
$> /var/lib/gems/2.1.2/bin/bundle install
```
Now, check that everything is working correctly. Do the following in a terminal as an ordinary user (not root):

```
$> irb
irb(main):003:0> require 'csv'
=> true
irb(main):003:0> require 'debugger'
=> true
```
The "true" output indicates that everything went fine and you are ready to go to the next step. Also, `rake --version` should report a version >= 10.1.1 and `rails --version` should report a rails version >= 3.2.x.

You can also run the following to check your gems:

```
$> bundle exec gem list --local
```
The gems, including version numbers, should include everything listed in the Gemfile.lock located in the MarkUs repository root (also available online at https://github.com/MarkUsProject/Markus/blob/master/Gemfile.lock).

## Set up Subversion/SVN Ruby bindings
Since MarkUs uses `libruby-svn`, you will have to install it locally for every version of Ruby you installed.

First, [download the Subversion source code here](http://subversion.tigris.org/downloads/subversion-1.6.17.tar.gz).

Extract the archive and `cd` into the directory:

```
$> tar xvzf subversion-1.6.17.tar.gz
$> cd subversion
```
You will need `libaprutil-dev` on your system:

```
$> sudo apt-get install libaprutil1-dev
```
Be sure to use the version of Ruby you wanted to compile SVN bindings with:

```
$> rvm use 2.1.2
```
**Note**: Ensure that the path below (../ruby-2.1.2-p95/…) is correct and you do not have a different patchlevel installed through RVM.

For example, here are the instructions for Ruby 2.1.2-p95:

```
$> rvm use 2.1.2
$> ./configure --with-ruby-sitedir=~/.rvm/rubies/ruby-2.1.2-p95/lib/ruby \
  --prefix=`echo ~`/.rvm/rubies/ruby-2.1.2-p95 --disable-mod-activation \
  --without-apache-libexecdir
$> make
$> make swig-rb
$> make install
$> make install-swig-rb
```
You will have to repeat this operation for every version of Ruby you use, changing the file paths respectively Remember to set `rvm use` for the Ruby version you want to compile the SVN bindings for.

Check that everything was setup correctly:

```
$> irb
irb(main):003:0> require 'svn/repos'
=> true
```

## Setting up the Database

You can use PostgreSQL, MySQL, or SQLite3. SQLite3 is easier to install, but should only be used for development, not in production.
You may also experience database conflicts, in particular if you want to test PDF Conversion. In case of PDF Conversion, you MUST use PostgreSQL or MySQL.
Older versions of MarkUs used ImageMagick for PDF conversion. You shouldn’t need to install it, but the instructions are available for [setting up ImageMagick](ImageMagick.rst).

Once you have decided which database best suits you, follow the respective instructions:

* [Setting up the Database (MySQL)](SettingUpMySQL.rst)
* [Setting up the Database (PostgreSQL)](SettingUpPostgreSQL.rst)
* [Setting up the Database (SQLite)](SettingUpSQLite.rst)

## Configure MarkUs

## Test plain MarkUs installation

If you followed the above installation instructions in order, you should have a working MarkUs installation (in terms of required software and required configuration). But first you would need to create the development database, load relations into it and populate the database with some data. You can do so by doing the following series of commands (as non-root user, assuming you are in the root of the MarkUs source code):

```
# Gets gems that you do not have yet, like thoughtbot-shoulda
$> bundle install  --without (postgresql) (sqlite) (mysql)
$> bundle exec rake db:setup    # Creates, initializes, and populates all the databases uncommented in config/database.yml
$> bundle exec rake test
```
Now, you are ready to test your plain MarkUs installation. Start your server by running:

```
$> rails s
```
If everything above went fine: Congratulations! You have a working MarkUs installation. Go to http://0.0.0.0:3000/ and enjoy MarkUs!

The default admin user is 'a' with any non-empty password. Look at db/seeds.rb for other users, where the password for each user is the same as their username.

**Happy coding!**
