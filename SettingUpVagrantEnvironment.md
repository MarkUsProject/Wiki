Setting up your Development Environment via Vagrant
===============================================================

Downloading and Installing
--------------------------
If you want to get started on working on MarkUs quickly and painlessly, this is the way to do it.

1. Install [VirtualBox](https://www.virtualbox.org/) and [Vagrant](http://www.vagrantup.com/)
2. Copy the [Vagrantfile](https://raw.githubusercontent.com/MarkUsProject/Markus/master/Vagrantfile) from the MarkUs github site to your machine.

3. `cd` to the repo (make sure you’re in the right directory - it should contain the Vagrantfile)
4. `vagrant up`

This will download a fairly large (3GB) Debian box from the internet, so go [take a walk](http://news.stanford.edu/news/2014/april/walking-vs-sitting-042414.html) or something. This box has GNOME, postgres, subversion, and all of MarkUs’s other dependencies installed. When the download is complete, VirtualBox will run the box in headless mode.

**NOTE:** If, for some reason, it fails and complains about SSH, you most likely have timed out. Check your internet connection attempt to limit network activity to `vagrant up`.



Connecting to your box
----------------------

Next, run `vagrant ssh` to connect to the virtual machine. (If it asks you for a password for vagrant, the password is "vagrant".)

Follow the instructions for [Setting up Git and MarkUs](GitHowTo) to clone the MarkUs repo on the virtual machine.

**NOTE:** It is possible to set up the vitual machine to share folders with the host machine, but in our experience, this is too slow to be a good work environment, and sometimes doesn't work at all.  If you do want to enable shared folders, you can check out that [vagrant documentation](http://docs.vagrantup.com/v2/synced-folders/).

Finally, move the `database.yml` file from the Home directory of the vagrant box to the project’s `config` directory.

Change to the `MarkUs` directory.  Before you can run the server you will need to install or update required gems with `bundle install`.  You may find that the installation of some gems will fail.  Following the instructions in the output and re-running bundle install usually resolves the problem.

Create the directory used for storing repos: `mkdir data/dev/repos`.

Ensure the database is correctly populated by running `bundle exec rake db:reset`

Finally, run `bundle exec rails server` from the project directory. 

You should now be able to access the site from your host machine's browser at `http://0.0.0.0:3000`.


Using RubyMine
--------------

Install [RubyMine] (https://www.jetbrains.com/ruby/)

**NOTE**: If you are using Windows 7, you may need to install Java 8 (JDK included) to work around a connectivity issue as RubyMine (at the time of writing this) uses Java 7 which has an incompatibility.

**NOTE**: RubyMine will tell you that there are missing gems to be installed, it is okay to ignore this.

Go to Files > Settings > Tools > Vagrant, set the Instance folder to your Markus directory with the Vagrantfile and leave the Provider as *Default*.

Before attempting the next step, you need to find the Ruby interpreter path on your virtualbox, which can be done from ssh with: `rvm gemdir`

From settings as well, go to Languages & Frameworks > Ruby SDK and Gems, and click add symbol (green plus sign) and select "New remote..."
There are two ways to set up Vagrant through RubyMine, whereby the second option should be tried if the first one fails:

1) Select the Vagrant radio button, set the instance folder to the root MarkUs folder where the Vagrantfile is. Confirm the connection works by clicking on the Host URL.

2) If (1) does not work, then select the SSH Credentials radio button and enter the following:

```
	Host: localhost (**NOTE:** Windows may fail if you use 127.0.0.1, try using 'localhost' first before 127.0.0.1)
	Port: 2222
	User name: vagrant
	Auth type: Password
	Password: (your vagrant password here), the password checkbox is selected
	Ruby interpreter path: (Place the path you got earlier here)
```

With the above being added and having returned to the Ruby SDK and Gems window, select the green checkmark (should be under green plus symbol).

At this point, you may need to restart RubyMine before making the next step work. There also is an option to do SSH through RubyMine and start/pause/kill Vagrant if you have not done so before starting RubyMine. This can be found at Tools > Vagrant > (command here). You may need the Vagrant server to be online for the next step:

To deploy the files to the server from RubyMine, select Tools > Deployment > Upload to vagrant. If this option is grayed out, you will need to either restary RubyMine, make sure you entered the correct address in the previous steps, or possibly wait a minute for it to be recognized by RubyMine. When deploying to the server, make sure you are selecting the folder from the Project view, otherwise it may not deploy the folder.

Upon doing this, you will be able to edit MarkUs through RubyMine and upload successfully.


Doing work on Markus's Git branch?
----------------------------------

WARNING: The gitolite instructions are still a work in progress.

The git branch requires some additional setup to your MarkUs instance. This is because the method student repositories are generated differs between SVN and Git.

We will use [Gitolite](http://gitolite.com/gitolite/index.html) to handle authentication.

To install Gitolite, we first need to create a new user. All of the operations below should be run on the Vagrant virtual machine.

1. `ssh-keygen` - Generate an ssh key for the vagrant user. (Say yes to overwriting the .ssh/id_rsa file, and no passphrase.)
2. `su git` - Switch user. (password is vagrant)
3. `cd ~` - Change to the git user's home directory
4. `sudo scp vagrant@localhost:.ssh/id_rsa.pub vagrant.pub` - copy the vagrant public key for gitolite to use. (password vagrant)
5. `chmod a+r vagrant.pub` - get the permissions right
6. `mkdir bin`
7. `gitolite/install -ln`
8. `bin/gitolite setup -pk vagrant.pub` - Setup Gitolite using our `vagrant` public key.

Let's test our Gitolite installation:

1. `exit` - Log back into vagrant user.
2. `eval $(ssh-agent -s)` - Start your key agent to hold the private key.
3. `ssh-add ~/.ssh/id_rsa` - Add key to key agent.
4. `ssh -T git@localhost` - Say `yes` if prompted.

You should see the following output:

		hello vagrant, this is git@vagrant-ubuntu-trusty-64 running gitolite3 v3.6.3-9-g2b918fc on git 1.9.1

		 R W    gitolite-admin
		 R W    testing


Now set up MarkUs to use git instead of svn:

1. `cd ~/Markus`
2. Edit `config/environments/development.rb`: the value for `REPOSITORY_TYPE` should be `'git'`, not `'svn'`.
3. If you already have `data/dev/repos`, then do `rm -rf data/dev/repos/*`. Otherwise, create the directory: `mkdir data/dev/repos`.
4. `bundle exec rake db:setup` - warning: this may take a very long time
5. `bundle exec rake db:reset` 

At this point you are done. If you receive an `Early EOF` error, make sure `vagrant` has access to the gitolite-admin repo by doing `ssh git@localhost` from the `vagrant` user. If this does not work, then follow the steps outlined [here](http://gitolite.com/gitolite/emergencies.html) to clone the `gitolite-admin` repo manually somewhere and add the vagrant public key to the `keys` folder.
