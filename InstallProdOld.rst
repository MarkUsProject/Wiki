# MarkUs Version 0.5 Deployment Documentation (A System Administrator's Guide)

-----------------

## Table of Contents<wiki:#TOC>
* [How to Install MarkUs](wiki:InstallProd#Install)
    * [Required Software](wiki:InstallProd#Software)
    * [Installation Proceedings](wiki:InstallProd#Notes)
    * [See also](wiki:InstallProd#InstallSeeAlso)
* [MarkUs Configuration Options](wiki:InstallProd#Configure)
    * [Allow Subversion command-line commits only](wiki:InstallProd#SVNCommandLine)
    * [Use Externally Created Subversion Repositories with MarkUs](wiki:InstallProd#SVNExternalRepos)

-----------------

## How to Install MarkUs<wiki:#Install>
[Back to TOC](wiki:InstallProd#TOC)

For more detailed instructions, please see our [INSTALL](http://www.markusproject.org/INSTALL) file.

### Required Software (including known to be working versions)<wiki:#Software>

We know that the following versions work and believe that whatever version "gem" provides by issuing "gem install package" should also work.

 * Ruby (>=1.8.7) including development package (e.g. ruby-dev)
 * net/https Ruby library ('libopenssl-ruby' Debian package)
 * Gem (>= 1.3.x) see [Update gem on Debian](wiki:UpdateRailsDebian)
     * rails (gem) (2.3.2)
     * daemons (gem) (1.0.10)
     * mongrel (gem) (1.1.5)
     * mongrel_cluster (gem) (1.0.5)
     * ruby-pg (gem) (>=0.7.9.2008.01.28)
     * postgres (gem) (>=0.7.9.2008.01.28)
     * fastercsv (gem) (>=1.4.0)
     * rake (gem) (0.8.7)
     * ruby-debug (gem)
 * PostgreSQL including libpq-dev (>= 8.2, but any PostgreSQL version should work; We also know that MarkUs works with MySQL)
 * Apache httpd (1.3/2.x) (including mod_proxy, mod_rewrite, Subversion server modules if using Subversion as a backend) Note: Any other Webserver with similar features should also work.
 * 'build-essential' Debian package (required to build/compile some gem packages from source)
 * 'subversion' and 'libsvn-ruby1.8' (Ruby bindings for Subversion) if using an SVN Repository as back-end

### Installation Proceedings (using a PostgreSQL database) <wiki:#Notes>

**NOTE**  
An important thing to have installed prior installing the Rails gems is the libpq-dev package (i.e. development files for PostgreSQL).


Install PostgreSQL (make sure that the created cluster is UTF-8 encoded; If not required, it also works with latin-1 and ) and Apache Httpd  
  
Update gem, so that a version >= 1.3.x is installed (If you would like to install gems to a non-standard location, please see the [Rubygems Non-Standard-Path Wiki document](wiki:RubygemsNonStandardPath))
  
Install gem packages:  

    sudo gem install rails daemons mongrel mongrel_cluster ruby-pg postgres fastercsv rake ruby-debug

Create an administrative database user and allow this user to connect using md5 passwords  
  
Take the MarkUs application and extract it to an appropriate location  
  
Set an environment variable RAILS_ENV="production"  
  
Change to the "root" of the MarkUs Rails application  
  
Set database connection settings accordingly in \<MarkUs-APP-Root\>/config/database.yml (see \<MarkUs-APP-Root\>/config/database.yml.postgresql for a sample setup)  

If you are using a rails version >2.3.2, please uncomment the line featuring "RAILS_GEM_VERSION = 2.3.2 unless defined? RAILS_GEM_VERSION" in config/environment.rb
  
Run the following rake tasks (non-root):

    rake db:create                # creates the "production" database according to database.yml
    rake db:schema:load           # creates the necessary database schema relations

Create an "instructor" user for the person responsible for the course

    rake markus:instructor first_name='Markus' last_name='Maximilian' user_name='markus'

Optionally, load some default data into the database (The database can be reset using <code>rake db:reset</code>)

    rake db:populate

Configure the MarkUs application in \<MarkUs-APP-Root\>/config/environment.rb (see our MarkUs [configuration documentation](wiki:InstallProd#Configure) below). **Note:** Please pay particular attention to the "secret" in the cookies related configuration section of your MarkUs instance (see <http://api.rubyonrails.org/classes/ActionController/Session/CookieStore.html>)
  
Configure the mongrel cluster (see config/mongrel_cluster.yml) and start the mongrel servers:

    mongrel_rails cluster::start   # uses config settings defined in config/mongrel_cluster.yml

The <code>mongrel_cluster</code> gem isn't really necessary. It is a nice utility for starting/stopping mongrels for your MarkUs app, though.
For more information concerning mongrel clusters see: [http://mongrel.rubyforge.org/wiki/MongrelCluster](http://mongrel.rubyforge.org/wiki/MongrelCluster).

Configure an httpd VirtualHost similar to the following (Reverse-Proxy-Setup)  

     RewriteEngine On

     # define proxy balancer
     <Proxy balancer://mongrel_cluster>
         BalancerMember http://127.0.0.1:8000 retry=10
         BalancerMember http://127.0.0.1:8001 retry=10
         BalancerMember http://127.0.0.1:8002 retry=10
     </Proxy>


     DocumentRoot /opt/olm/\<MarkUs-APP-Root\>/public
     <Directory />
         Options FollowSymLinks
         AllowOverride None
     </Directory>
     <Directory /opt/olm/\<MarkUs-APP-Root\>/public>
         Options Indexes FollowSymLinks MultiViews
         AllowOverride None
         Order allow,deny
         allow from all
     </Directory>
     RewriteCond %{DOCUMENT_ROOT}/%{REQUEST_FILENAME} !-f
     RewriteRule ^/(.*)$ balancer://mongrel_cluster%{REQUEST_URI} [P,QSA,L]

### See Also: <wiki:#InstallSeeAlso>
* [Deployment FAQs](wiki:InstallProdFaq)
* [How to host several MarkUs applications on a single server](MultipleHosting)
* [Example Apache httpd virtual host configuration file](http://www.markusproject.org/dev/markus_httpd_vhost.conf)
* You might find it worthwhile skimming through one of our [Linux development environment setup instructions](https://stanley.cdf.toronto.edu/drproject/csc49x/olm_rails/wiki/InstallationRorRadrailsDebian)
* See available rake tasks for MarkUs: <code>rake -T</code>
* Our current [INSTALL](http://www.markusproject.org/INSTALL) file

------------------------

## MarkUs Configuration Options<wiki:#Configure>

The main application-wide configuration file for MarkUs is

    <app-root>/config/environment.rb

What follows is an example of 'environment.rb'

    # Be sure to restart your server when you modify this file

    # Specifies gem version of Rails to use when vendor/rails is not present
    # Uncomment this line, if you are using rails version > 2.3.2
    RAILS_GEM_VERSION = '2.3.2' unless defined? RAILS_GEM_VERSION

    # Bootstrap the Rails environment, frameworks, and default configuration
    require File.join(File.dirname(__FILE__), 'boot')

    ###################################################################
    # MarkUs SPECIFIC CONFIGURATION
    #   - use "/" as path separator no matter what OS server is running
    #   - settings have to be before Rails::Initializer.run in order
    #     to be available in the app
    ###################################################################

    ###################################################################
    # Set the course name here
    COURSE_NAME         = "CSCxxx Summer 2008: Course Title Here"

    ###################################################################
    # MarkUs relies on external user authentication: An external script
    # (ideally a small C program) is called with username and password
    # piped to stdin of that program (first line is username, second line
    # is password). 
    #
    # If and only if it exits with a return code of 0, the username/password
    # combination is considered valid and the user is authenticated. Moreover,
    # the user is authorized, if it exists as a user in MarkUs.
    #
    # That is why MarkUs does not allow usernames/passwords which contain
    # \n or \0. These are the only restrictions.
    VALIDATE_FILE = "#{RAILS_ROOT}/config/dummy_validate.sh"

    ###################################################################
    # File storage (Repository) settings
    ###################################################################
    # Options for Repository_type are 'svn' and 'memory' for now
    # 'memory' is by design not persistent and only used for testing MarkUs
    REPOSITORY_TYPE = "svn" # use Subversion as storage backend

    ###################################################################
    # Directory where Repositories will be created. Make sure MarkUs is allowed
    # to write to this directory
    REPOSITORY_STORAGE = "/home/storage/markus/repository-root"

    ###################################################################
    # Change this to 'REPOSITORY_EXTERNAL_SUBMITS_ONLY = true' if you
    # are using Subversion as a storage backend and the instructor wants his/her
    # students to submit to the repositories by command-line only. If you
    # set this to true, students won't be able to submit via the Web interface.
    REPOSITORY_EXTERNAL_SUBMITS_ONLY = false

    ###################################################################
    # This config setting only makes sense, if you are using
    # 'REPOSITORY_EXTERNAL_SUBMITS_ONLY = true'. If you have Apache httpd
    # configured so that the repositories created by MarkUs will be available to
    # the outside world, this is the URL which internally "points" to the
    # REPOSITORY_STORAGE directory configured earlier. Hence, Subversion
    # repositories will be available to students for example via URL
    # http://www.example.com/markus/svn/Repository_Name. Make sure the path
    # after the hostname matches your <Location> directive in your Apache
    # httpd configuration
    REPOSITORY_EXTERNAL_BASE_URL = "http://www.example.com/markus/svn"

    ###################################################################
    # This setting is important for two scenarios:
    # First, if MarkUs should use Subversion repositories created by a
    # third party, point it to the place where it will find the Subversion
    # authz file. In that case, MarkUs would need at least read access to
    # that file.
    # Second, if MarkUs is configured with REPOSITORY_EXTERNAL_SUBMITS_ONLY
    # set to 'true', you can configure as to where MarkUs should write the
    # Subversion authz file.
    REPOSITORY_PERMISSION_FILE = "/home/svn-repos-root/svn_authz"

    ###################################################################
    # This setting configures if MarkUs is reading Subversion
    # repositories' permissions only OR is admin of the Subversion
    # repositories. In the latter case, it will write to
    # $REPOSITORY_SVN_AUTHZ_FILE, otherwise it doesn't. Change this to
    # 'false' if repositories are created by a third party. 
    IS_REPOSITORY_ADMIN = true

    ###################################################################
    # Session Timeouts
    ###################################################################
    USER_STUDENT_SESSION_TIMEOUT        = 1800 # Timeout for student users
    USER_TA_SESSION_TIMEOUT             = 1800 # Timeout for grader users
    USER_ADMIN_SESSION_TIMEOUT          = 1800 # Timeout for admin users

    ###################################################################
    # CSV upload order of fields (usually you don't want to change this)
    ###################################################################
    # Order of student CSV uploads
    USER_STUDENT_CSV_UPLOAD_ORDER = [:user_name, :last_name, :first_name]
    # Order of graders CSV uploads
    USER_TA_CSV_UPLOAD_ORDER  = [:user_name, :last_name, :first_name]


    Rails::Initializer.run do |config|
      # Settings in config/environments/* take precedence over those specified here.
      # Application configuration should go into files in config/initializers
      # -- all .rb files in that directory are automatically loaded.
      # See Rails::Configuration for more options.

      # Skip frameworks you're not going to use. To use Rails without a database
      # you must remove the Active Record framework.
      # config.frameworks -= [ :active_record, :active_resource, :action_mailer ]

      # Specify gems that this application depends on. 
      # They can then be installed with "rake gems:install" on new installations.
      # config.gem "bj"
      # config.gem "hpricot", :version => '0.6', :source => "http://code.whytheluckystiff.net"
      # config.gem "aws-s3", :lib => "aws/s3"

      # Only load the plugins named here, in the order given. By default, all plugins 
      # in vendor/plugins are loaded in alphabetical order.
      # :all can be used as a placeholder for all plugins not explicitly named
      config.plugins = [ :ssl_requirement, :auto_complete ]

      # If you are hosting your application at path
      # http://hostname/path/to/markus set this to '/path/to/markus'
      # Ignore URL prefix specified below
      # config.action_controller.relative_url_root = ""

      # Add additional load paths for your own custom dirs.
      # Set it if you are using ruby libs and/or gems
      # at non-standard paths
      # config.load_paths += %W( #{RAILS_ROOT}/extras )

      # Force all environments to use the same logger level
      # (by default production uses :info, the others :debug)
      # config.log_level = :debug

      # Make Time.zone default to the specified zone, and make Active Record store time values
      # in the database in UTC, and return them converted to the specified local zone.
      # Run "rake -D time" for a list of tasks for finding time zone names. Note: This is important and has to be set for MarkUs.
      config.time_zone = 'Eastern Time (US & Canada)'

      # Your secret key for verifying cookie session data integrity.
      # If you change this key, all old sessions will become invalid!
      # Make sure the secret is at least 30 characters and all random, 
      # no regular words or you'll be exposed to dictionary attacks.
      config.action_controller.session = {
        :session_key => '_markus_session',
        :secret      => '449fe2e7daee471bffae2fd8dc02313d'
      }

      # Use the database for sessions instead of the cookie-based default,
      # which shouldn't be used to store highly confidential information
      # (create the session table with "rake db:sessions:create")
      config.action_controller.session_store = :active_record_store

      # Use SQL instead of Active Record's schema dumper when creating the test database.
      # This is necessary if your schema can't be completely dumped by the schema dumper,
      # like if you have constraints or database-specific column types
      # config.active_record.schema_format = :sql

      # Activate observers that should always be running
      # config.active_record.observers = :cacher, :garbage_collector
      
      
      config.gem 'thoughtbot-shoulda', :lib => 'shoulda', :source  =>
    "http://gems.github.com", :version => '>= 2.0.6' 
    end

    ###################################################################
    # END OF MarkUs SPECIFIC CONFIGURATION
    ###################################################################

------------------------------

### Allow Subversion Commandline Commits Only<wiki:#SVNCommandLine>
[Back to TOC](wiki:InstallProd#TOC)

When using Subversion as a storage backend for students' submissions, it is capable of exposing created Subversion repositories. Example: An instructor configures an assignment so that students can submit using the Subversion command-line client only (i.e. the Web interface will be disabled). In that case, the Subversion repositories will be created once the student logs in. Hence, the workflow is as follows:

1. The instructor creates users and (at least one) assignment
2. The instructor tells students to log in to MarkUs and find out their repository URL
3. Students can connect to their repositories using svn

**Requirements**

In order to be able to use this feature, one requires a working [Subversion/Apache configuration as documented in the Subversion book](http://svnbook.red-bean.com/en/1.5/svn.serverconfig.httpd.html). We assume that user authentication is handled by Apache httpd (whatever authentication scheme one chooses). Once a username (the identical username/user-id as defined in MarkUs) has been authenticated by the httpd, authorization (i.e. checking read/write permissions) is handled by Subversion. MarkUs writes appropriate Subversion configuration files when users and/or groups are determined.

**Minimal Subversion/Apache httpd configuration**

A minimal Apache httpd configuration (sippet of httpd.conf) would look similar to the following:

    LoadModule dav_module
    LoadModule dav_svn_module
    LoadModule authz_svn_module   # we are using per-directory based access control

    # make sure you have a ServerName or ServerAlias directive matching your
    # hostname MarkUs is hosted on (uncomment the following line)
    # ServerAlias your_hostname

    # Make sure that the path after the hostname of
    # REPOSITORY_EXTERNAL_BASE_URL matches the path of your
    # Location directive
    <Location /markus/svn>
      DAV svn

      # any "/markus/svn/foo" URL will map to a repository /home/svn-repos-root/foo
      # This should usually be identical to the REPOSITORY_STORAGE constant in
      # config/environment.rb of your markus app
      SVNParentPath /home/svn-repos-root 

      # configure your Apache httpd authentication scheme here
      # for example, one could use Basic authentication
      # how to authenticate a user
      Require valid-user
      AuthType Basic                  # the authentication scheme to be used
      AuthUserFile /path/to/users/file  

      # Arbitrary name: Should probably match your COURSE_NAME constant in
      # config/environment.rb
      AuthName "Your Course Name"

      # Location of Subversions authz file. Make sure it matches with
      # $REPOSITORY_SVN_AUTHZ_FILE in your config/environment.rb
      AuthzSVNAccessFile /path/to/authz/file
    </Location>

This enables you to let your students access repositories created by MarkUs via the http:// uri scheme, once you have created an assignment and set up Groups/Users appropriately in MarkUs.

### Use Externally Created Subversion Repositories with MarkUs<wiki:#SVNExternalRepos>
[Back to TOC](wiki:InstallProd#TOC)

If you already have Subversion repositories created by some third-party, it is
possible to use them with MarkUs. 

**Instructions**

1. Set <code>IS_REPOSITORY_ADMIN = false</code> in environment.rb
2. Point MarkUs to the correct path where your repositories reside by setting REPOSITORY_STORAGE in environment.rb correctly (of course you would also use <code>REPOSITORY_TYPE = "svn"</code>)
2. Prepare a csv file adhering to the following field order: <code>group_name,repo_name,user_name,user_name</code> (Note: the repo_name field is important here, since this is the link with your third-party tool)
3. Use this file to upload groups for your course (go to Assignment => Groups & Graders => Upload/Download)
4. This configures MarkUs to use externally created repositories. **Please note:** MarkUs won't write any permissions related files in this kind of setup. The third party tool is in charge of that. 

