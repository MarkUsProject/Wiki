Setting up your Development Environment via Vagrant
===============================================================

Downloading and Installing
--------------------------
If you want to get started on working on MarkUs quickly and painlessly, this is the way to do it.

1. Install [VirtualBox](https://www.virtualbox.org/) and [Vagrant](http://www.vagrantup.com/)
2. Clone the Markus repo (or just download it)
3. `cd` to the repo (make sure you’re in the right directory - it should contain the Vagrantfile)
4. `vagrant up`

This will download a fairly large (3GB) Debian box from the internet, so go [take a walk](http://news.stanford.edu/news/2014/april/walking-vs-sitting-042414.html) or something. This box has GNOME, postgres, subversion, and all of MarkUs’s other dependencies installed. When the download is complete, VirtualBox will run the box in headless mode.

**NOTE:** If, for some reason, it fails and complains about SSH, you most likely have timed out. Check your internet connection attempt to limit network activity to `vagrant up`.



Connecting to your box
----------------------

Next, run the following commands to connect to the virtual machine and set up RVM.

```
$> vagrant ssh
$> rvm use 2.1
$> rvm gemset use markus
```

Now, you can either clone the MarkUs repo inside the box or set up shared folders.

**NOTE:** If you want to use your host machine's text editor, you will want to enable shared/synced folders through your Vagrantfile's configuration by setting the appropriate paths and then running `vagrant reload` to start syncing the folders. On some machines, however, this may cause MarkUs to break. In that case, consider attaining [enlightenment](http://www.vim.org/).

Finally, move the `database.yml` file from the Home directory of the vagrant box to the project’s config directory.

To run the development server, run `bundle exec rails server` from the project directory. You may have to install or update required gems with `bundle install` first.

You should now be able to access the site from your host machine's browser at `http://0.0.0.0:42069`. The message saying it is accessible at localhost:3000 is a static message and does not reflect the changed port.
