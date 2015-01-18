Setting up a development environment for MarkUs development on GNU/Linux
========================================================================

**Please note the difference between "$\>" and "\#\>". The former means execute the command as simple user, the latter means execute the command as the super-user or use sudo as normal user ("\#\>" is equivalent to "\> sudo").**

We are currently working with Ruby 2.1.x (January 2015).  Ruby 2.x is not avilable through package managers so you will need to install RVM.  If you want to use rbenv or something else, please add the appropriate installation documentation.

Installing RVM and Setting up Ruby
-----------------------------------------
- [Installing RVM](RVM)  
        Ruby 2.x is not available through package managers, so you will need to use RVM
    
- Setting up the Database
  - [SQLite](SettingUpSQLite)
  - [MySQL](SettingUpMySQL)
  - [PostgreSQL](SettingUpPostgreSQL)

Setting up Git
--------------

Git is the Source Content Management used by MarkUs. You can find some documentation on GitHub. You will also have to set-up a GitHub account. [How to set-up Git on GNU/Linux](http://help.github.com/linux-set-up-git)

You should be able to use apt-get to install git

    #> apt-get install git



Setting up Subversion and the Subversion Ruby bindings
---------------------------------------------------------------------------

Fist, download Subversion source code here :

[https://subversion.apache.org/download](https://subversion.apache.org/download)/ (If you are on the virtual machine, you can use \`wget [http://mirror.its.dal.ca/apache/subversion/subversion-1.8.10.tar.gz](http://mirror.its.dal.ca/apache/subversion/subversion-1.8.10.tar.gz)\` for example)

Extract it and cd into the repository:

    $ tar xzf subversion-1.8.9.tar.gz
    $ cd subversion-1.8.9

You will need libaprutil-dev and swig on your system:

    $ sudo aptitude install libaprutil1-dev swig
    
MarkUs uses libruby-svn, so you will have to install it locally, for every version of Ruby you installed.

Be sure to use the ruby you want to compile svn bindings with:

    $ rvm use 2.1.2

**Note** : Ensure that the path below (.../ruby-2.1.2/...) is correct and you do not have a different patchlevel installed through rvm.

For example, here are instructions for Ruby 2.1.2:

    $ CFLAGS=-fPIC ./configure \
       --with-ruby-sitedir=~/.rvm/rubies/ruby-2.1.2/lib/ruby/ \
       --prefix=$HOME/.rvm/rubies/ruby-2.1.2/ --disable-mod-activation \
       --without-apache-libexecdir \
       --enable-optimize
    $ make
    $ make check
    $ make swig-rb
    $ make check-swig-rb
    $ make install
    $ make install-swig-rb

**Note** : Setting CFLAGS=-fPIC may not need to be explicitly set in the future

You will have to repeat this operation for every version of ruby you use.

**Important note:** Don't forget to set \`rvm use\` for the Ruby version you want to compile

Check everything was setup correctly:

    $ irb
    :001 > require 'svn/repos'
    => true  


Setting up the Database
-----------------------
**Note : You can either use PostgreSQL or MySQL or SQLite3 as database**

SQLite3 is easier to install, but should only used in development, not in production. You may also experience database conflicts, in particular if you want to test **PDF Conversion**. In case of PDF Conversion, you **MUST** use PostgreSQL or MySQL

If your want to test PDF conversion on MarkUs, don't forget to set to true the `PDF_SUPPORT` variable in `config/environments/development.rb`

Once you have decided what database best suits you :

-   [Setting up the Database (SQLite)](SettingUpSQLite.rst)\_
-   [Setting up the Database (MySQL)](SettingUpMySQL.rst)\_
-   [Setting up the Database (PostgreSQL)](SettingUpPostgreSQL.rst)\_

Other dependencies
-------------------
 Development files for the V8 JavaScript Engine 
 
    #> apt-get install libv8-dev


Required gems for MarkUs
------------------------

The list of gems required for MarkUs is given in the Gemfile for MarkUs, found at [https://github.com/MarkUsProject/Markus/blob/master/Gemfile](https://github.com/MarkUsProject/Markus/blob/master/Gemfile)

We use bundler to manage all gems. Install only bundler as a gem and bundler will install all other Gems.

To install the **all** gems, go in the project folder, and execute the following:

    #> gem install bundler
    #> bundle config libv8 -- --with-system-v8
    $> bundle install

If you get the error "Could not locate Gemfile", it means you are not in the correct folder.

Please note that bundler may ask you for your root password.

Bundle allows also some selective installation. To install only sqlite3 support, execute the following:

    $> bundle install --without postgresql mysql

To install only postgresql support support, execute the following:

    $> bundle install --without sqlite mysql

To install only mysql support, execute the following:

    $> bundle install --without postgresql sqlite

On Ubuntu and Debian systems, the system can't find bundler. You need to add bundler to your PATH or run it directly

    $> /var/lib/gems/1.9.1/bin/bundle install

If you get a message saying "Missing these required gems", then it is likely that some new gems have been integrated into Markus development and also need to be installed using `bundle install` as described above.

Now, check that everything worked fine. Do the following on a terminal (as an ordinary user, **not** root):

    $> irb
    irb(main):003:0> require 'csv'
    => true
    irb(main):003:0> require 'svn/repos'
    => true

Note: if the last one doesn't work, you are missing the svn library for ruby, and you need to install it. This can be done from the command below:

    $> apt-get install ruby-svn

The "true" output indicates that everything went fine and you are ready to go to the next step. Also, `rake --version` should report a version \>= 10.1.1 and `rails --version` should report a rails version \>= 3.2.x

You can also run the following to check your gems:

    $> bundle exec gem list --local

The gems, including version numbers, should include everything listed in the Gemfile.lock located in the MarkUs repository root (also available online at [https://github.com/MarkUsProject/Markus/blob/master/Gemfile.lock)](https://github.com/MarkUsProject/Markus/blob/master/Gemfile.lock)).

