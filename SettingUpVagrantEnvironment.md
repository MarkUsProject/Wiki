Setting up your Development Environment (hassle-free, I swear!)
===============================================================

If you want to get started on working on MarkUs quickly and painlessly, this is the way to do it.

1. Install Vagrant and VirtualBox/VMWare Fusion
2. Clone the Markus repo (or just download it)
3. `cd` to the repo (make sure you’re in the right directory - it should contain the Vagrantfile)
4. `vagrant up`

This will download a fairly large (3GB) Debian box from the internet, so go [take a walk](http://news.stanford.edu/news/2014/april/walking-vs-sitting-042414.html) or something. This box has GNOME, postgres, subversion, and all of MarkUs’s other dependencies installed. When the download is complete, VirtualBox will run the box in headless mode.

**NOTE:** If, for some reason, it fails and complains about SSH, you most likely have timed out. Check your internet connection and attempt to limit network activity to `vagrant up` 

Next, run the following commands to connect to the virtual machine. and set up RVM.

```
$> vagrant ssh
$> rvm use 2.1
$> rvm gemset use markus
```

**NOTE:** If `rvm use 2.1` says ruby is not installed, run the suggested installation command. It will be similar if not identitcal to `rvm install ruby-2.1.1`

Finally, move the `database.yml` file from the Home directory of the vagrant box to the project’s config directory.

To run the development server, run `bundle exec rails server` from the project directory.

Access the site from your browser at `http://0.0.0.0:42069`.
