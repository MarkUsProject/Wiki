================================================================================
Setting up a Production-like Development Environment (hassle-free, I swear!)
================================================================================

Using a development environment that is similar to the actual production env is
good because it catches production-related bugs. Tools like Vagrant make it
really easy to set up such environments. If you want to work on MarkUs on
Debian and using PostgreSQL (in a VM), follow these simple steps:

1. Install Vagrant and VirtualBox
2. Download this `Vagrantfile <https://gist.githubusercontent.com/oneohtrix/4c5e1bb34158c02f818b/raw/Vagrantfile>`_
3. cd to the directory containing the Vagrantfile
4. vagrant up

This will download a fairly large (3GB) Debian box from the internet. It has
GNOME, postgres, and all of MarkUs's dependencies installed. It also has postgres
set up. When the download is complete, Virtualbox will launch in GUI mode. The
username and password are both 'vagrant'.

Now you can clone the MarkUs repo and work on it. You can do this in the VM or
over ssh by doing vagrant ssh. The rvm gemset to use is called markus, so make
sure you do 'rvm gemset use markus'. There is also a configured 'database.yml'
file in the Home directory. Move that to the project's config dir so rails
knows how to connect to the database.
