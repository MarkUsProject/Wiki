================================================================================
Developping MarkUs with RVM
================================================================================

RVM
================================================================================
RVM (Ruby Version Manager) is a command line tool which allows us to easily
install, manage and work with multiple ruby environments from interpreters to
sets of gems.

Official website: https://rvm.io

RVM can help you working with different versions of Ruby with MarkUs (or other
projets). Here, you will find particular installation instructions for
setting-up a RVM environment with MarkUs


Install RVM
================================================================================
First, install RVM::

    $ sudo apt-get install curl
    $ curl -L get.rvm.io | bash -s stable --auto

Next we need to reload the ~/.bash_profile file::


    $ . ~/.bash_profile

Run rvm requirements to produce a list of all the require packages::

    $ rvm requirements

    # For Ruby / Ruby HEAD (MRI, Rubinius, & REE), install the following:
    ruby: /usr/bin/apt-get install build-essential openssl libreadline6 libreadline6-dev 
    curl git-core zlib1g zlib1g-dev libssl-dev libyaml-dev libsqlite3-dev sqlite3 libxml2-dev
    libxslt-dev autoconf libc6-dev ncurses-dev automake libtool bison subversion pkg-config

Next, install any missing packages::

    $ sudo apt-get install build-essential openssl libreadline6 libreadline6-dev \
    curl git-core zlib1g zlib1g-dev libssl-dev libyaml-dev libsqlite3-dev sqlite3 \
    libxml2-dev libxslt-dev autoconf libc6-dev ncurses-dev automake libtool bison  \
    subversion pkg-config

Next, install Rubies (Ruby 1.8.7, Ruby 1.9.2, JRuby, Rubinius…)

Ruby
--------------------------------------------------------------------------------

MarkUs stable tree works only with Ruby 1.8.7. We will install it by default,
to ensure everything is build correctly:: 

    $ rvm install 1.8.7
    $ rvm install 1.9.2

Now, if you do `rvm list`, you will see two version of ruby available. You can
choose one by doing `rvm use 1.8.7` or `rvm use 1.9.2`.

**Note:** You can check at each moment your version of Ruby by doing `ruby -v`

**Note:** You will have to do `rvm use 1.x.y` in every shell you use.

To make a version of ruby default::

    $ rvm --default use 1.8.7-p334

Rubygems
--------------------------------------------------------------------------------

You will also need Rubygems for each installation of Ruby: ::

    $ rvm use 1.8.7
    $ gem install bundler
    $ rvm use 1.9.2
    $ gem install bundler

Don't forget to re-do a `bundle install` in your MarkUs root repository. It
will install gems for your local installation of Ruby.

Subversion Ruby bindings with RVM
--------------------------------------------------------------------------------

Moreover, as MarkUs uses libruby-svn, you will have to install it locally, for
every version of Ruby you installed.

Fist, download Subversion source code here :
http://subversion.tigris.org/downloads/subversion-1.6.17.tar.gz

**Note** : An issue remains with subversion-1.7.1 and Ruby-1.9.3-p0 (during
compilation). Consider using version 1.6.17

Extract it and cd into the repository: ::

    $ tar xvzf subversion-1.6.17.tar.gz
    $ cd subversion-1.6.17

You will need libaprutil-dev on your system::

    $ sudo aptitude install libaprutil1-dev

Be sure to use the good ruby you wanted to compile svn bindings with: ::

    $ rvm use 1.8.7

For example, here are instructions for Ruby 1.8.7-p334: ::

    $ ./configure --with-ruby-sitedir=~/.rvm/rubies/ruby-1.8.7-p334/lib/ruby \
      --prefix=`echo ~`/.rvm/rubies/ruby-1.8.7-p334 --disable-mod-activation \
      --without-apache-libexecdir
    $ make
    $ make swig-rb
    $ make install
    $ make install-swig-rb

You will have to repeat this operation for every version of ruby you use.

**Important note:** Don't forget to set `rvm use` for the Ruby version you want
to compile

Check everything was setup correctly: ::

    $ irb
    :001 > require 'svn/repos'
    => true  

Update RVM
================================================================================

If a new version of Ruby is out, you will want to install it.

First, you will have to update RVM (for example, Ruby-1.9.2-p290 is out, and I
used Ruby-1.9.2-p180)::

    $ rvm get stable
    $ rvm install 1.9.2

**Note** Use the same options as before if you need them. Moreover, don't
forget to recompile libsvn-ruby for this version of Ruby! You will have to
reinstall all gems too.
