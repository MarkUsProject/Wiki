Developping MarkUs with Rbenv
=============================

Rbenv
-----

Rbenv (Ruby Environment) is a command line tool which allows us to easily install, manage and work with multiple ruby environments from interpreters to sets of gems.

Official website: [https://github.com/sstephenson/rbenv](https://github.com/sstephenson/rbenv)

Rbenv can help you working with different versions of Ruby with MarkUs (or other projets). Here, you will find particular installation instructions for setting-up a Rbenv environment with MarkUs

Install Rbenv
-------------

First, install Rbenv:

    $ sudo apt-get install curl git-core
    $ git clone https://github.com/sstephenson/rbenv.git ~/.rbenv

Add \~/.rbenv/bin to your $PATH for access to the rbenv command-line utility:

    $ echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bash_profile

Add rbenv init to your shell to enable shims and autocompletion:

    $ echo 'eval "$(rbenv init -)"' >> ~/.bash_profile

Next we need to reload the \~/.bash\_profile file or restart your shell:

    $ . ~/.bash_profile

Install ruby-build, which provides an rbenv install command that simplifies the process of installing new Ruby versions:

    git clone https://github.com/sstephenson/ruby-build.git ~/.rbenv/plugins/ruby-build

Next, install any missing packages:

    $ sudo apt-get install build-essential openssl libreadline6 libreadline6-dev \
    curl git-core zlib1g zlib1g-dev libssl-dev libyaml-dev \
    libxml2-dev libxslt-dev autoconf libc6-dev ncurses-dev automake libtool bison  \
    subversion pkg-config

Next, install Rubies (Ruby 1.8.7, Ruby 1.9.3, JRuby, Rubinius…)

### Ruby

If you want to list all versions of ruby available to install:

    $ rbenv install --list

MarkUs stable tree works with both Ruby 1.8.7 and 1.9.3. We will install it:

    $ CONFIGURE_OPTS="--enable-shared" rbenv install 1.8.7-p374
    $ CONFIGURE_OPTS="--enable-shared" rbenv install 1.9.3-p448

**Note:** \`--enable-shared\` flag is used to be able to install subversion

Now, if you do \`rbenv versions\`, you will see three versions of ruby available.

**Note:** You can check at each moment your version of Ruby by doing \`ruby -v\`

Rebuild the shim executables. You should do this any time you install a new Ruby executable (for example, when installing a new Ruby version, or when installing a gem that provides a command).

    $ rbenv rehash

To make the version of ruby 1.9.3 default:

    $ rbenv global 1.9.3-p448

To make the version of ruby only locally (it will add a .ruby-version in the current directory):

    $ rbenv local <ruby-version>

### Rubygems

You will also need Rubygems for each installation of Ruby:

    $ rbenv global 1.8.7-p374
    $ gem install bundler
    $ rbenv global 1.9.3-p448
    $ gem install bundler

Don't forget to re-do a \`bundle install\` in your MarkUs root repository. It will install gems for your local installation of Ruby.

### Subversion Ruby bindings with Rbenv

Moreover, as MarkUs uses libruby-svn, you will have to install it locally, for every version of Ruby you installed.

First, download Subversion source code here (ruby 1.8.7) : [http://subversion.tigris.org/downloads/subversion-1.6.17.tar.gz](http://subversion.tigris.org/downloads/subversion-1.6.17.tar.gz)

For installation with ruby 1.9.3, choose this version : [http://mir2.ovh.net/ftp.apache.org/dist/subversion/subversion-1.8.1.tar.gz](http://mir2.ovh.net/ftp.apache.org/dist/subversion/subversion-1.8.1.tar.gz)

Extract it and cd into the repository:

    $ tar xvzf subversion-1.8.1.tar.gz
    $ cd subversion-1.8.1

You will need libaprutil-dev on your system:

    $ sudo aptitude install libaprutil1-dev

Be sure to use the good ruby you wanted to compile svn bindings with:

    $ rbenv local 1.9.3-p448

For example, here are instructions for Ruby 1.9.3-p448:

    $ ./configure --with-ruby-sitedir=~/.rbenv/versions/ruby-1.9.3-p448/lib/ruby \
      --prefix=`echo ~`/.rbenv/versions/ruby-1.9.3-p448/ --disable-mod-activation \
      --without-apache-libexecdir
    $ make
    $ make swig-rb
    $ make install
    $ make install-swig-rb

You will have to repeat this operation for every version of ruby you use.

**Important note:** Don't forget to set \`rbenv local\` for the Ruby version you want to compile in the subverion folder

Check everything was setup correctly:

    $ irb
    :001 > require 'svn/repos'
    => true  

Update Rbenv
------------

If a new version of Ruby is out, you will want to install it.

First, you will have to update Rbenv:

    $ cd ~/.rbenv
    $ git pull

Then install another version of Ruby.

**Note** Use the same options as before if you need them. Moreover, don't forget to recompile libsvn-ruby for this version of Ruby! You will have to reinstall all gems too.
