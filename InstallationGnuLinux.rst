================================================================================
Setting up a development environment for MarkUs development on GNU/Linux
================================================================================

.. contents::

Setting up Ruby, Ruby on Rails, Subversion and the Subversion Ruby bindings
--------------------------------------------------------------------------------

Issue the following command on a terminal. You need to be root or use "sudo"
(the Ubuntu way) to do that::

    #> aptitude install ruby-full build-essential rubygems rake libsvn-ruby
    subversion

.. TODO update previous radrails link

** Note : You can either use PostgreSQL or MySQL or SQLite3 as database **
SQLite3 is easier to install, but should only used in development, not in
production. You may also experience database conflicts.

Setting up the Database (SQLite3)
--------------------------------------------------------------------------------
::
    #> aptitude install sqlite3 libsqlite3-dev

Setting up the Database (MySQL)
--------------------------------------------------------------------------------

Need to be done :-)

Setting up the Database (PostgreSQL)
--------------------------------------------------------------------------------

Make sure that you have set an UTF-8 locale/encoding (e.g. set
LANG=en_CA.UTF-8 environment variable). The postgresql cluster will be created
in the encoding which is currently set. Type locale in the term and you should
see something similar to the following::

    locale
    LANG=en_CA.UTF-8
    LC_CTYPE="en_CA.UTF-8"
    LC_NUMERIC="en_CA.UTF-8"
    LC_TIME="en_CA.UTF-8"
    LC_COLLATE="en_CA.UTF-8"
    LC_MONETARY="en_CA.UTF-8"
    LC_MESSAGES="en_CA.UTF-8"
    LC_PAPER="en_CA.UTF-8"
    LC_NAME="en_CA.UTF-8"
    LC_ADDRESS="en_CA.UTF-8"
    LC_TELEPHONE="en_CA.UTF-8"
    LC_MEASUREMENT="en_CA.UTF-8"
    LC_IDENTIFICATION="en_CA.UTF-8"
    LC_ALL=


Then execute the following command on a terminal. You need to be root or use
"sudo" (the Ubuntu way) to do that::

    #> aptitude install postgresql postgresql-contrib

You also need the development package of PostreSQL. You can install the
package by executing the following command::

    #> apt-get install libpq-dev

**Creating a Database User and Changing Authentication Scheme**

For simplicity we create a database user "olm_db_admin" with the same
password, to which superuser privileges will be granted. We will use this user
for OLM later. As root execute the following (be careful not to forget any
backslashes or single-/doublequotes)::

    #> su -c "psql -c \"create user olm_db_admin with superuser password
    'olm_db_admin';\"" postgres

The above command should output the following::

    CREATE ROLE

However if you keep getting the following everytime you try to enter your
password::

    #> su -c "psql -c \"create user olm_db_admin with superuser password
    'olm_db_admin';\""
    postgres Password:
    su: Authentication failure

You can run the following instead::

    #>sudo su
    Password:
    #> su -c "psql -c \"create user olm_db_admin with superuser password
    'olm_db_admin';\"" postgres
    CREATE ROLE

Finally, we need to change a line in the configuration file of the PostgreSQL
database. As root open "pg_hba.conf" (sometimes "pg_hdb.conf") in
``/etc/postgres/\<pg-version\>/main/`` and look for the following lines (the
first one is actually only a comment)::

    # "local" is for Unix domain socket connections only
    local   all         all                               ident sameuser

Now change the second line like so::

    local   all         all                               md5

Restart PostgresSQL in order to apply those configuration changes to the
server (please adjust the version accordingly)::

    #> /etc/init.d/postgresql-8.3 restart

To test if everything went fine we try to connect to the "postgres" database
using our newly created user::

    #> psql postgres olm_db_admin

You will be asked for a password, so type "olm_db_admin". After that you
should see the console of PostgreSQL.

Required gems for MarkUs
--------------------------------------------------------------------------------

This section assumes, you have gem version >= 1.3.6 (required for rails version
> 2.3.7).

Note that ruby-postgres is unmaintained and does not compile against
postgresql-8.3+. Therefore, do **not** install it. Instead, install ruby-pg
which works just fine. So, the list of gems required for MarkUs is as follows:

* rails
* rake
* mongrel
* ruby-pg
* postgres
* fastercsv
* ruby-debug
* shoulda
* machinist
* factory_data_preloader
* faker
* will_paginate
* rubyzip
* ya2yaml

We are now using bundler to manage all gems. Install only bundler as a gem and 
bundler will install all other Gems.

To install the **all** gems execute the following::

    #> gem install bundler
    #> bundle install

Bundle allows also some selective installation. To install only sqlite3
supportr, execute the following::

    #> bundle install --without postgresql mysql

On Ubuntu and Debian systems, the system can't find bundler. You need to add
bundler to your PATH or run it directly ::

    #>/var/lib/gems/1.8/bin/bundle

If you get a message saying "Missing these required gems", then it is likely
that some new gems have been integrated into Markus development and also need
to be installed using ``bundle install`` as described above.

Now, check that everything worked fine. Do the following on a terminal (as an
ordinary user, *not* root)::

    #> irb
    irb(main):001:0> require 'rubygems'
    => true
    irb(main):002:0> require 'postgres'
    => true
    irb(main):003:0> require 'fastercsv'
    => true
    irb(main):003:0> require 'ruby-debug'
    => true


The "true" output indicates that everything went fine and you are ready to go
to the next step. Also, <code>rake --version</code> should report a version >=
0.8.7 and <code>rails --version</code> should report a rails version >= 2.2.x

You can also run the following to check your gems::

    #> gem list --local
    *** LOCAL GEMS ***
    actionmailer (2.3.5)
    actionpack (2.3.5)
    activerecord (2.3.5)
    activeresource (2.3.5)
    activesupport (2.3.5)
    columnize (0.3.1)
    fastercsv (1.5.0)
    linecache (0.43)
    mongrel (1.1.5)
    postgres (0.7.9.2008.01.28)
    rack (1.1.0, 1.0.1)
    rails (2.3.5)
    rake (0.8.7)
    ruby-debug (0.10.3)
    ruby-debug-base (0.10.3)
    ruby-debug-ide (0.4.9, 0.4.5)
    ruby-pg (0.7.9.2008.01.28)
    selenium-client (1.2.18)
    shoulda (2.10.2)
    thoughtbot-shoulda (2.10.2)
    will_paginate (2.3.11)
    rubyzip (1.3.6)

Configure MarkUs
--------------------------------------------------------------------------------

Precondition: You have the MarkUs source-code checked out and do not plan to
use RadRails (see the following sections if you _plan_ to use RadRails for
development).

MarkUs is configured by editing config/environment.rb (If you have a rails
version > 2.3.2 comment out the line containing RAILS_GEM_ENV; minimum rails
version is 2.2.x). Read through all settings in environment.rb

Look at config/environments/development.rb

* Change the REPOSITORY_STORAGE path to an appropriate path for your setup.
* if you see: #config.gem 'thoughtbot-shoulda' then changed it to
  config.gem 'thoughtbot-shoulda'

    * since we use thoughtbot-shoulda as a testing framework (it builds on top
      of Test::Unit and is fully backwards compatible) and install it as
      directed when you run 'rake' the next time.

Setup the database.yml file:

* cp config/database.yml.sample config/database.yml (replace sample by the
  database you use (PostgreSQL, SQLite3 or MySQl)

* change the usernames and password to olm_db_admin 


Test plain MarkUs installation
--------------------------------------------------------------------------------

If you followed the above installation instructions in order, you should have
a working MarkUs installation (in terms of required software and required
configuration). But first you would need to create the development database,
load relations into it and populate the db with some data. You can do so by
the following series of commands (as non-root user, assuming you are in the
application-root of the MarkUs source code;)(please adapt the following
command)::

    # gets gems that you do not have yet, like thoughtbot-shoulda 
    #> bundle install  --without (postgresql) (sqlite) (mysql)
    #> rake db:create        # creates development database
    #> rake db:schema:load   # loads required relations into database
    #> rake db:populate      # populates database with some data
    #> rake db:test:prepare
    #> rake test:units
    #> rake test:functionals

Note: there are still tests that are failing.

Now, you are ready to test your plain MarkUs installation. The most straight
forward way to do this is to start the mongrel server on the command-line. You
can do so by::

    script/server  #boots up mongrel (or WebRink, if mongrel is not installed/found)

**Common Problems**

If some of the previous commands fail with error message similar to
``LoadError: no such file to load -- \<some-ruby-gem\>``, try to install the
missing Ruby gem by issuing ``gem install \<missing-ruby-gem\>`` and retry the
step which failed.

If everything above went fine: Congratulations! You have a working MarkUs
installation. Go to http://0.0.0.0:3000/ and enjoy MarkUs!

However, since you are a MarkUs developer, this is only _half_ of the game.
You also **need** (yes, this is not optional!) _some_ sort of IDE for MarkUs
development. For instance, the next section describes how to install RadRails
IDE, an Eclipse based Rails development environment. If you plan to use
something _else_ for MarkUs development, such as JEdit (with some tweaks) or
VIM, you should now start configuring them.

But if you _do_ plan to use RadRails for development, you should get rid of
some left-overs from previous steps, so that the following instructions run as
smoothly as possible for you. This is what you'd need to do (If you know what
you are doing, you might find this silly. But this guide tries to give
detailed instructions for Rails newcomers)::

    #> rake db:drop          # get rid of the database, created previously (it'll be recreated again later)
    #> rm -rf markus_trunk   # get rid of the MarkUs source code possibly checked out previously (you might do a "cd .." prior to that)

**Happy Coding!**
