Setting up your Development Environment via Vagrant (hassle-free, I swear!)
===============================================================

If you want to get started on working on MarkUs quickly and painlessly, this is the way to do it.

1. Install Vagrant and VirtualBox/VMWare Fusion
2. Clone the Markus repo
3. `cd` to the Vagrantfile
4. `vagrant up`

This will download a fairly large (3GB) Debian box from the internet, so go [take a walk](http://news.stanford.edu/news/2014/april/walking-vs-sitting-042414.html) or something. This box has GNOME, postgres, subversion, and all of MarkUs’s other dependencies installed. When the download is complete, VirtualBox will run the box in headless mode.

**NOTE:** If, for some reason, it fails and complains about SSH, you most likely have timed out. Check your internet connection and attempt to limit network activity to `vagrant up` 

Next, run the following commands to connect and setup to the virtual machine:

```
$> vagrant ssh
$> rvm install ruby-2.1.2
$> rvm use 2.1.2
$> cd ../../Markus
$> cp config/database.yml.postgresql config/database.yml
$> bundle install
$> bundle exec rails server
```

Thi commands will install ruby-2.1.2, move the database.yml file that is configured for Postgres into the Markus file, install the required gems, and boot the server which you can access on your home machine at `localhost:42069`. The message saying it is accessible at localhost:3000 is a static message and does not reflect the changed port.
