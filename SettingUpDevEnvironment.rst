================================================================================
Setting up your Development Environment (hassle-free, I swear!)
================================================================================

If you want to get started on working on MarkUs quickly and painlessly, this is
the way to do it.

1. Install Vagrant and VirtualBox
2. Download this `Vagrantfile <https://gist.githubusercontent.com/oneohtrix/4c5e1bb34158c02f818b/raw/Vagrantfile>`_
3. cd to the directory containing the Vagrantfile
4. vagrant up

This will download a fairly large (3GB) Debian box from the internet, so go
`take a walk <http://news.stanford.edu/news/2014/april/walking-vs-sitting-042414.html>`_
or something. This box has GNOME, postgres, subversion, and all of MarkUs's other
dependencies installed. When the download is complete, Virtualbox will launch
Debian in GUI mode. The username and password are both 'vagrant'.

Now you can clone the MarkUs repo and work on it. You can do this directly in the VM or
over ssh by doing vagrant ssh (recommended if you use a cli-based text editor).
The rvm gemset to use is called markus, so make sure you do 'rvm gemset use markus'.
There is also a configured 'database.yml' file in the Home directory. Move that
to the project's config dir so rails knows how to connect to the database.
