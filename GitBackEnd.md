## Git Backend Development Wiki / Guide / Stream of Conscience

### Gem Options
There are a number of gems that could be used. Here is a short outline of each. At one point the git gem was depreciated but now it appears that both are under active development. Reading through the Documentation of both it appears they support a majority of common git commands.

[**rugged**](https://rubygems.org/gems/rugged)

[**git**](http://rubygems.org/gems/git)

rugged is more efficient and more actively maintained. I'm sticking with the gem previously chosen.

### Updating Ruby/Gems
If you installed your environment according to one of the guides on the Wiki, chances are you will be required to update your environment to support rugged. 

The following are the steps taken with Ubuntu 12.04 LTS running in a Virtual Box.

###Set up rvm

`sudo apt-get install build-essential git-core`

`sudo apt-get install curl`

`\curl -sSL https://get.rvm.io | bash -s stable`

Now, you may need to configure your shell to use rvm. 
Enter 

`$ type rvm | head -1`

and if `rvm is a function` is not the output, go to Edit>Profile Preferences>Title and Command and ensure "Run command as a login shell" is selected. Try that line again and continue.

