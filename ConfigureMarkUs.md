Configure MarkUs
----------------

Precondition: You have the MarkUs source-code checked out and do not plan to use RadRails (see the following sections if you *plan* to use RadRails for development).

-   Look at `config/environments/development.rb`

-   Change the REPOSITORY\_STORAGE path to an appropriate path for your setup. NOTE: it is unlikely that you need to change these values for development

Test plain MarkUs installation
------------------------------

If you followed the above installation instructions in order, you should have a working MarkUs installation (in terms of required software and required configuration). But first you would need to create the development database, load relations into it and populate the db with some data. You can do so by the following series of commands (as non-root user, assuming you are in the application-root of the MarkUs source code;)(please adapt the following command):

    # gets gems that you do not have yet, like thoughtbot-shoulda 
    $> bundle install  --without (postgresql) (sqlite) (mysql)
    $> bundle exec rake db:setup         # creates, initializes, and populates all the databases uncommented in config/database.yml
    $> bundle exec rake test

Note: if you are using RVM, follow [these instuctions](RVM)\_ to install subversion into the correct path

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
