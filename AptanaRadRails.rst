================================================================================
Aptana RadRails plugin for Eclipse
================================================================================

Install the Radrails Plug-in for Eclipse (optional)
--------------------------------------------------------------------------------

This tutorial assumes that you have a working installation of Eclipse IDE
(preferably Ganymede or later). After having a working Java installation this
step should be pretty easy (I usually install the provided Java packages of my
distribution). It is suggested to install Eclipse into one's home directory,
since Eclipse's built-in plug-in installation system works most seamlessly
that way. Downloading the Eclipse tar-ball (for Linux of course) and
extracting it in your home directory should suffice. You may want to add the
path where your eclipse executable resides to your PATH variable.

After installing Eclipse, make sure you execute the following command,
otherwise you may not be able to install Eclipse plug-ins.::

    #>  apt-get install eclipse-pde

Install Aptana Radrails
********************************************************************************

* Start Eclipse (as normal user, *not* root)
* Go to: “Help” - “Software Updates”
* Select “Available Software”
* Click on “Add Site” (*Note:* The next 4 steps for determining the URL to
  enter next work as of September 15, 2009; Maybe these steps need a little
  adaption at some point later)
* Go to http://www.aptana.com/radrails using your preferred Web browser
* Click on "Download Now"
* Select "Eclipse Plugin" from the drop down selection menu and click on
  "Download Now"
* Record URL mentioned there: e.g. http://update15.aptana.org/studio/26124/
* Back in Eclipse: Enter the URL determined by the previous step
* Select (check) “Aptana Studio” from the URL entered as "New Site"
  previously
* Click "Install..." and click the “Next >” button
* Read the License Agreement, accept the terms, and click the
  “Finish >” button.
* When it is recommended that Eclipse be restarted click “Yes”.
* After the restart, you will be asked to install something from Aptana
  Studio Site
* Select (check) "Aptana RadRails" and click "Next >"
* Read the License Agreement, accept the terms, and click the “Next >” button.
* The downloads should be installed into the .eclipse folder in your home
  directory by default. If this is acceptable click the “Finish” button.
* Wait for the downloads to complete.
* When it is recommended that Eclipse be restarted click “Yes”.

**Check Ruby and Rails Configuration**

If you are asked if you want to auto-install some gems it is up to you to
install them or not (I did).

* Go to "Window" - "Preferences"
* Select "Ruby" - "Installed Interpreters"
* The selected Ruby interpreter should be in /usr
* Now, go to "Rails"
* Rails should be auto-detected as well as Mongrel

Install Subclipse
********************************************************************************

* Again go to: “Help” - “Software Updates”
* Select “Available Software”
* Click on “Add Site”
* Enter Location: “http://subclipse.tigris.org/update_1.2.x” (depending on which
  version you want to install; for me version 1.2.x worked best)
* Select (check) “Subclipse” from "http://subclipse.tigris.org/update_1.2.x"
* Click "Install..." and click the “Next >” button
* Read the License Agreement, accept the terms, and click the “Finish >” button.
* The downloads should be installed into the .eclipse folder in your home
  directory by default. If this is acceptable click the “Finish” button.
* Wait for the downloads to complete.
* Once the downloads are complete click the “Install” button on the
  “Verification” screen.
* When it is recommended that Eclipse be restarted click “Yes”.
* After installation go to "Window" - "Preferences" and select "Team" - "SVN"
  (there might be an Error message popping up, but you can ignore it)
* Now, in the "SVN interface" section select "SVNKit (Pure Java)" instead of
  "JavaHL (JNI)" and click "Apply"

Checkout MarkUs Source Code
********************************************************************************

* Start Eclipse and switch to the RadRails perspective
* Go to "File" - "New" and select "Project..."
* At the "New Project" wizard select "SVN" - "Checkout Projects from SVN" and
  click on "Next >"
* Use "Create a new repository location" and click "Next >"
* Enter URL: "https://stanley.cdf.toronto.edu/svn/csc49x/olm_rails" and click
  "Next >"
* Accept the "invalid certificate warning".
* Select "trunk" and click "Next >"
* Keep the default options and click "Finish"
* At the "New Project" wizard select "Rails" - "Rails Project" and click "Next >"
* Enter a project name of your choosing, deselect "Generate Rails application
 skeleton" and "Automatically start server after project is created"
* Click "Finish" and let Subclipse checkout the code from the repository

Getting Started with MarkUs Development
********************************************************************************

First, we need to configure MarkUs. Please have a careful look at
config/environment.rb (and please read the comments) and config/database.yml



Setup the config/environments/development.rb file:

* Also make sure your the REPOSITORY_STORAGE constant points to a location
  actually existent.

Start your newly installed RadRails and by using the "Ruby Explorer" navigate
to folder "config" and copy the file "database.yml.sample". Paste and rename
it to "database.yml". Now open the newly created "database.yml" file and
modify the "username: ..." and "password: ..." lines as follows::

    username: olm_db_admin
    password: olm_db_admin

Do that for "development", "test" and "production" and save your modified
"database.yml".

Now switch to the "Rake Tasks" view.

* If you get an error message complaining about the absence of RakeFile in
the project, go to "Window" - "Preferences", then in "Ruby" - "Rake" and
enter your rake path, e.g. "/var/lib/gems/1.8/bin/rake" (the rake version
installed by gem, not the debian package). Click "OK" to close the dialog.
Restart Eclipse.*

* If the error persists, try running rake --tasks from the command line

     * If it doesn't work in the command line, it won't work in Aptana

Run, in order,::

* gems:install
* db:create
* db:schema:loa
* db:populate
* db:test:prepare
* test:units
* test:functionals


by selecting them and clicking on the "Play"-like button on the right. The
output of rake should show up in the "Console" view.

Finally go back to RadRails and switch to the "Servers" view. There should be
a server named exactly the same as your Rails project. Select it and start it
using the "Start server" icon (it looks like a "Play" key). Once the server is
started, check the port the server is listening on, fire up your Web-browser
(or use the Eclipse built-in) and go to "http://localhost:\<serverport\>/" and
log in with username "a" and any password (it must not be empty). That's it!

If the login in fails with an error message similar to::

    /usr/lib/ruby/gems/1.8/gems/activesupport-2.3.5/lib/active_support/dependencies.rb:380:
           command not found: /home/jmate/Aptana RadRails Workspace/MarkUs/config/dummy_validate.sh
           [4;36;1mSQL (0.2ms)[0m   [0;1mSET client_min_messages TO 'panic'[0m
           [4;35;1mSQL (0.1ms)[0m   [0mSET client_min_messages TO 'notice'[0m

then you will have to go to /config/environments/development.rb and change
VALIDATE_FILE to the absolute path to the config/dummy_validate.sh file with
the characters properly escaped. Example::

    VALIDATE_FILE = "/home/jmate/Aptana\\ RadRails\\ Workspace/MarkUs/config/dummy_validate.sh"

