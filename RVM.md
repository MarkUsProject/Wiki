Developping MarkUs with RVM
===========================

RVM
---

RVM (Ruby Version Manager) is a command line tool which allows us to easily install, manage and work with multiple ruby environments from interpreters to sets of gems.

Official website: [https://rvm.io](https://rvm.io)

RVM can help you working with different versions of Ruby with MarkUs (or other projets). Here, you will find particular installation instructions for setting-up a RVM environment with MarkUs

Install RVM
-----------

First, install RVM:

    $ sudo apt-get install curl
    $ \curl -sSL https://get.rvm.io | bash -s stable

Next, install Rubies (Ruby 1.9.3, Ruby 2.1.2, JRuby, Rubinius…)

> $ rvm install ruby-2.1.2

### Ruby

MarkUs stable tree works only with Ruby 1.9.3 or newer. Ruby 2.1.2 is the current stable release as of this writing, so we will install it by default, to ensure everything is build correctly:

    $ rvm install 1.9.3
    $ rvm install 2.1.2

Now, if you do \`rvm list\`, you will see two version of ruby available. You can choose one by doing \`rvm use 1.9.3\` or \`rvm use 2.1.2\`.

**Note:** You can check at each moment your version of Ruby by doing \`ruby -v\`

To make a version of ruby default:

    $ rvm --default use 2.1.2

### Rubygems

Don't forget to re-do a \`bundle install\` in your MarkUs root repository for each Ruby that you install.

### Subversion Ruby bindings with RVM

Moreover, as MarkUs uses libruby-svn, you will have to install it locally, for every version of Ruby you installed.

Fist, download Subversion source code here :

[https://subversion.apache.org/download](https://subversion.apache.org/download)/ (If you are on the virtual machine, you can use \`wget [http://mirror.its.dal.ca/apache/subversion/subversion-1.8.10.tar.gz](http://mirror.its.dal.ca/apache/subversion/subversion-1.8.10.tar.gz)\` for example)

Extract it and cd into the repository:

    $ tar xzf subversion-1.8.9.tar.gz
    $ cd subversion-1.8.9

You will need libaprutil-dev and swig on your system:

    $ sudo aptitude install libaprutil1-dev swig

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

Update RVM
----------

If a new version of Ruby is out, you will want to install it.

First, you will have to update RVM (for example, Ruby-2.2.0 is out, and I used Ruby-2.1.2):

    $ rvm get stable
    $ rvm install 2.2.0

**Note** Use the same options as before if you need them. Moreover, don't forget to recompile libsvn-ruby for this version of Ruby! You will have to reinstall all gems too.