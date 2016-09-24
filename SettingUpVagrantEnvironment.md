Setting up your Development Environment via Vagrant
===============================================================

Downloading and Installing
--------------------------
If you want to get started on working on MarkUs quickly and painlessly, this is the way to do it.

1. Install [VirtualBox](https://www.virtualbox.org/) and [Vagrant](http://www.vagrantup.com/)
2. Clone the Markus repo from GitHub by following the instructions in [Setting up Git and MarkUs](GitHowTo).  (This is a document you will want to read very carefully and may come back to.) 
3. `cd` to the repo (make sure you’re in the right directory - it should contain the Vagrantfile)
4. `vagrant up`

This will download a fairly large (3GB) Debian box from the internet, so go [take a walk](http://news.stanford.edu/news/2014/april/walking-vs-sitting-042414.html) or something. This box has GNOME, postgres, subversion, and all of MarkUs’s other dependencies installed. When the download is complete, VirtualBox will run the box in headless mode.

**NOTE:** If, for some reason, it fails and complains about SSH, you most likely have timed out. Check your internet connection attempt to limit network activity to `vagrant up`.



Connecting to your box
----------------------

Next, run `vagrant ssh` to connect to the virtual machine. (If it asks you for a password for vagrant, the password is "vagrant".)  To avoid having to enter a password each time, and to use RubyMine, [set up](https://www.digitalocean.com/community/tutorials/how-to-set-up-ssh-keys--2) a public private key pair, and copy the public key to `~/.ssh/authorized_keys` on the vagrant vm. Then open the VagrantFile on your local machine and add `config.ssh.private_key_path = "Users/your_username/.ssh/id_rsa"` directly under `config.vm.box = markusproject/ubuntu`. 

**NOTE:** It is possible to set up the virtual machine to share folders with the host machine, but in our experience, this is too slow to be a good work environment, and sometimes doesn't work at all.  If you do want to enable shared folders, you can check out that [vagrant documentation](http://docs.vagrantup.com/v2/synced-folders/).  We have found it more effective to work with files locally using RubyMine and deploy/upload to the vagrant box when you want to try things out.

If you are using RubyMine then you should jump down to the set up instructions for RubyMine below before proceeding to the next step.

In the virtual machine, change to the `Markus` directory.  Before you can run the server you will need to install or update required gems with `bundle install`.  You may find that the installation of some gems will fail.  Following the instructions in the output and re-running bundle install usually resolves the problem.

Ensure the database is correctly populated by running `rake db:reset`.

Finally, run `rails server` from the project directory. 

You should now be able to access the site from your host machine's browser at `http://0.0.0.0:3000`.


Using RubyMine
--------------

Install [RubyMine] (https://www.jetbrains.com/ruby/)

**NOTE**: If you are using Windows 7, you may need to install Java 8 (JDK included) to work around a connectivity issue as RubyMine (at the time of writing this) uses Java 7 which has an incompatibility.

**NOTE**: RubyMine will tell you that there are missing gems to be installed, it is okay to ignore this.

First, o view the MarkUs files, when RubyMine runs select Open, or File > Open and navigate to your cloned MarkUs folder on your local machine.

Open up Files > Settings  (in Windows) or Files > Preferences in OSX where we will configure some different settings.

1) In Tools > Vagrant, set the Instance folder to your Markus directory on the *local* machine with the Vagrantfile and leave the Provider as *Default*.

Before attempting the next step, you need to find the Ruby interpreter path on your virtualbox, which can be done from ssh with: `rvm gemdir`

2) Go to Languages & Frameworks > Ruby SDK and Gems, and click add symbol (green plus sign) and select "New remote..."

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

    Now return to the Ruby SDK and Gems window, select the green checkmark (should be under green plus symbol).
    
3. You can open an ssh session to the vagrant virtual machine directly in RubyMine from Tools > Start SSH Session.  You need to make sure that you have installed a public key on the vagrant vm so that you don't need a password (or passphrase) to ssh into the vagrant vm.

At this point, you may need to restart RubyMine before making the next step work. There also is an option to do SSH through RubyMine and start/pause/kill Vagrant if you have not done so before starting RubyMine. These commands can be found under Tools > Vagrant. You may need the Vagrant server to be online for the next step:

In RubyMine, select Tools > Deployment > Configuration, and click the + in the top left to add a new server. After giving it a name, under `Connection` use the following settings:

```
	Type: SFTP 
	SFTP host: 127.0.0.1
	Port: 2222
	Root path: /home/vagrant
	User name: vagrant
	Auth type: Key pair
	Private key file: /Users/your_username/.ssh/id_rsa
```

And under `Mappings` set:

```
	Local path: [path to your local Markus repo]
	Deployment path on server: /Markus
```
And click OK. 

To deploy all the MarkUs files to the server from RubyMine, first select the Markus folder in the left pane, and then select Tools > Deployment > Upload to vagrant. (If this option is grayed out, you may need to either restart RubyMine, make sure you entered the correct address in the previous steps, or possibly wait a minute for it to be recognized by RubyMine.)  The Upload command will upload whichever file or folder is selected.  If the selected file or folder hasn't changed then the option for Upload will be grayed out.

Upon doing this, you will be able to edit MarkUs through RubyMine and upload successfully.


Doing work on Markus's Git branch?
----------------------------------

WARNING: The gitolite instructions are still a work in progress.

Gitolite should already be set up and ready to use on the virtual machine. To test it out, do a `vagrant ssh` and then run the following commands:

1. `eval $(ssh-agent -s)` - Start your key agent to hold the private key.
2. `ssh-add ~/.ssh/id_rsa` - Add key to key agent.
3. `ssh -T git@localhost` - Say `yes` if prompted.

You should see the following output:

		hello vagrant, this is git@vagrant-ubuntu-trusty-64 running gitolite3 v3.6.3-9-g2b918fc on git 1.9.1

		 R W    gitolite-admin
		 R W    testing


Now set up MarkUs to use git instead of svn:

1. `cd ~/Markus`
2. Edit `config/environments/development.rb`: the value for `REPOSITORY_TYPE` should be `'git'`, not `'svn'`.
3. If you already have `data/dev/repos`, then do `rm -rf data/dev/repos/*`. Otherwise, create the directory: `mkdir data/dev/repos`.
4. `rake db:setup` - warning: this may take a very long time
5. `rake db:reset` 

At this point you are done. If you receive an `Early EOF` error, make sure `vagrant` has access to the gitolite-admin repo by doing `ssh git@localhost` from the `vagrant` user. If this does not work, then follow the steps outlined [here](http://gitolite.com/gitolite/emergencies.html) to clone the `gitolite-admin` repo manually somewhere and add the vagrant public key to the `keys` folder.
