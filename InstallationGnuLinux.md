Setting up a development environment for MarkUs development on GNU/Linux
========================================================================

**Please note the difference between "$\>" and "\#\>". The former means execute the command as simple user, the latter means execute the command as the super-user or use sudo as normal user ("\#\>" is equivalent to "\> sudo").**

If your system complains about gems not found when trying to run, for example, `bundle install`, it is because you don't have the path of the binary in your `$PATH` variable. Find the path of the gem (usually `/var/lib/gems/1.9.1/bin/`) and add it to your `$PATH`.

Setting up Git
--------------

Git is the Source Content Management used by MarkUs. You can find some documentation on GitHub. You will also have to set-up a GitHub account. [How to set-up Git on GNU/Linux](http://help.github.com/linux-set-up-git)\_

Setting up Ruby, Ruby on Rails, Subversion and the Subversion Ruby bindings
---------------------------------------------------------------------------

Issue the following command on a terminal. You need to be root or use `sudo` (the Ubuntu way) to do that. Both methods are correct. Use only one of the following methods :

(as root):

    $> su  # and then enter your root password
    #> apt-get install ruby-full build-essential git ruby-svn subversion libv8-dev
    #> # make sure ruby-full points to the correct ruby version (1.9)

(as normal user, with the `sudo` method):

    $> sudo apt-get install ruby-full build-essential ruby-svn subversion libv8-dev
    $> # and then enter your root password, make sure ruby-full points to the correct ruby version (1.9)

**Note : You can either use PostgreSQL or MySQL or SQLite3 as database**

SQLite3 is easier to install, but should only used in development, not in production. You may also experience database conflicts, in particular if you want to test **PDF Conversion**. In case of PDF Conversion, you **MUST** use PostgreSQL or MySQL

If your want to test PDF conversion on MarkUs, don't forget to set to true the `PDF_SUPPORT` variable in `config/environments/development.rb`

Setting up the Database
-----------------------

Once you have decided what database best suits you :

-   [Setting up the Database (SQLite)](SettingUpSQLite.rst)\_
-   [Setting up the Database (MySQL)](SettingUpMySQL.rst)\_
-   [Setting up the Database (PostgreSQL)](SettingUpPostgreSQL.rst)\_

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
    irb(main):003:0> require 'debugger'
    => true
    irb(main):003:0> require 'svn/repos'
    => true

Note: if the last one doesn't work, you are missing the svn library for ruby, and you need to install it. This can be done from the command below:

    $> apt-get install ruby-svn

The "true" output indicates that everything went fine and you are ready to go to the next step. Also, `rake --version` should report a version \>= 10.1.1 and `rails --version` should report a rails version \>= 3.2.x

You can also run the following to check your gems:

    $> bundle exec gem list --local

The gems, including version numbers, should include everything listed in the Gemfile.lock located in the MarkUs repository root (also available online at [https://github.com/MarkUsProject/Markus/blob/master/Gemfile.lock)](https://github.com/MarkUsProject/Markus/blob/master/Gemfile.lock)).

Configure MarkUs
----------------

Precondition: You have the MarkUs source-code checked out and do not plan to use RadRails (see the following sections if you *plan* to use RadRails for development).

-   Read through all settings in `environment.rb`

-   Look at `config/environments/development.rb`

-   Change the REPOSITORY\_STORAGE path to an appropriate path for your setup. NOTE: it is unlikely that you need to change these values for development

Test plain MarkUs installation
------------------------------

If you followed the above installation instructions in order, you should have a working MarkUs installation (in terms of required software and required configuration). But first you would need to create the development database, load relations into it and populate the db with some data. You can do so by the following series of commands (as non-root user, assuming you are in the application-root of the MarkUs source code;)(please adapt the following command):

    # gets gems that you do not have yet, like thoughtbot-shoulda 
    $> bundle install  --without (postgresql) (sqlite) (mysql)
    $> bundle exec rake db:setup         # creates, initializes, and populates all the databases uncommented in config/database.yml
    $> bundle exec rake test

Note: if you are using RVM, follow [these instructions](RVM) to install subversion into the correct path

Now, you are ready to test your plain MarkUs installation. The most straight forward way to do this is to start the mongrel server on the command-line. You can do so by:

    $> bundle exec rails server  #boots up the apprpropriate web server

The default admin user is 'a' with any non-empty password. Look at `db/seeds.rb` for other users.

If this doesn't work try

    $> rails s

**Common Problems**

If some of the previous commands fail with error message similar to `LoadError: no such file to load -- <some-ruby-gem>`, try to install the missing Ruby gem by issuing `gem install <missing-ruby-gem>` and retry the step which failed.

If everything above went fine: Congratulations! You have a working MarkUs installation. Go to [http://0.0.0.0:3000](http://0.0.0.0:3000)/ and enjoy MarkUs!

However, since you are a MarkUs developer, this is only *half* of the game. You also **need** (yes, this is not optional!) *some* sort of IDE for MarkUs development. For instance, the next section describes how to install RadRails IDE, an Eclipse based Rails development environment. If you plan to use something *else* for MarkUs development, such as JEdit (with some tweaks) or VIM, you should now start configuring them.

But if you *do* plan to use RadRails for development, you should get rid of some left-overs from previous steps, so that the following instructions run as smoothly as possible for you. This is what you'd need to do (If you know what you are doing, you might find this silly. But this guide tries to give detailed instructions for Rails newcomers):

    $> bundle exec rake db:drop          # get rid of the database, created previously (it'll be recreated again later)
    $> rm -rf markus_trunk   # get rid of the MarkUs source code possibly checked out previously (you might do a "cd .." prior to that)

**Happy Coding!**
