================================================================================
Setting up your Development Environment (hassle-free, I swear!)
================================================================================

If you want to get started on working on MarkUs quickly and painlessly, this is
the way to do it.

1. Install Vagrant and VirtualBox
2. Clone the Markus repo (or just download it)
3. cd to the repo (make sure you're in the right directory - it should contain the Vagrantfile)
4. vagrant up

This will download a fairly large (3GB) Debian box from the internet, so go
`take a walk <http://news.stanford.edu/news/2014/april/walking-vs-sitting-042414.html>`_
or something. This box has GNOME, postgres, subversion, and all of MarkUs's other
dependencies installed. When the download is complete, VirtualBox will run the box
in headless mode.

Now you can connect to the box by doing vagrant ssh. Do rvm use 2.1 and then 
rvm gemset use markus to use the correct gemsets. There is also a configured 'database.yml'
in the Home directory. Move that to the project's config dir so rails knows how to connect to
the database. Now you can clone the MarkUs repo from gh and start working on it. When you start the
rails server in the VM you can connect to it on your host computer by going to http://0.0.0.0:42069.
