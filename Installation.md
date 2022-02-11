# Installation

The following are instructions to set up a production server for MarkUs.

The following steps are for installation on a machine running Ubuntu 20.04 and all examples given below assume that you are installing in such an environment. Some changes may be required if installing on other operating systems.

## System Requirements

Ensure the following ubuntu packages are installed:

- build-essential : (needed to install node/yarn)
- software-properties-common : (needed to install node/yarn)
- postgresql-client-12 : (needed to manage a postgres database, later versions should also be ok too)
- tzdata : (needed for timezone management)
- libpq-dev : (needed to run a postgres database)
- libv8-dev : (needed for javascript dependencies)
- ruby-svn : (only required if using svn repositories. It is *highly* recommended to use git repositories instead)
- ghostscript : (needed to manage pdfs)
- imagemagick : (needed to manage pdfs)
- libmagickwand-dev : (needed to manage pdfs)
- cmake : (needed to make certain ruby gems)
- libaprutil1-dev : (needed if using Apache as an http server)
- libssl-dev : (needed for ssl/tsl encryption)
- swig : (needed as an interface for certain ruby gems)
- graphviz : (needed by the ruby-graphviz gem)
- git : (required if using git repositories)
- python3 : (version 3.9 recommended. Required for optical character recognition and jupyter notebook rendering)
- python3-venv : (required for optical character recognition and jupyter notebook rendering)
- python3-dev : (required for optical character recognition and jupyter notebook rendering)
- pandoc : (required rmarkdown and jupyter notebook rendering)
- libgl1 : (required for optical character recognition)
- ruby-full : (ensure that this installs at least ruby 2.7)

Install [bundler](https://bundler.io/) as a system gem:

```sh
gem install bundler -v 1.17.3
```

Install [node](https://nodejs.org/en/) (note that we need at least version 12 so we can't just install the ubuntu 20.04 package directly):

```sh
curl https://deb.nodesource.com/setup_12.x -o node_setup.sh
bash node_setup.sh
sudo apt-get install nodejs
rm node_setup.sh
```

Install [yarn](https://yarnpkg.com/):

```sh
npm install --global yarn@1.22.5
```

Update the default configuration options for imagemagick so that it will allow reading pdf files:

```sh
sed -ri 's/(rights=")none("\s+pattern="PDF")/\1read\2/' /etc/ImageMagick-6/policy.xml
```

## Create User

For security reasons, MarkUs should be run as a dedicated user. Either designate an existing user to run MarkUs or create one. In this demo we will assume that MarkUs is being run as a user named `markus` created by running:

```sh
useradd -m markus
```

## Set up MarkUs Rails application

**The following commands should be run as the user designated or created in the [Create User](#create-user) section**

Choose where you want the MarkUs source code to be downloaded and `cd` to that directory. This directory should be writeable by the `markus` user (the one created in the [create user](#create-user) section).

### Clone the MarkUs source code and change to the release branch

```sh
git clone https://github.com/MarkUsProject/Markus.git
cd Markus
git checkout release
```

### Install Ruby dependencies using [bundler](https://bundler.io/)

```sh
./bin/bundle install --deployment --without development test offline mysql sqlite
```

(Note that although MarkUs technically supports alternative database backends like mysql and sqlite, they may not be fully supported and support may be removed entirely in later versions. For this reason we recommend using Postgresql and running the `bundle install` command with the `--without` arguments above)

### Install javascript dependencies using [yarn](https://yarnpkg.com/)

```sh
./bin/yarn install
```

### Install python dependencies in a virtual environment

```sh
python3 -m venv ./venv
./venv/bin/pip install -r requirements.txt
```

and make sure that your [settings files](./Configuration.md) know where the packages are installed by specifying the location of the virtual environment's bin directory in `config/settings.local.yml`:

```yaml
python:
   bin: /path/to/the/venv/bin
```

### Configure MarkUs settings

For information on how to configure the MarkUs settings visit the [Configuration](./Configuration.md) page. Remember that any paths that you specify in the settings files should be readable/writable by the `markus` user.

### Encrypt credentials

MarkUs is a Rails application and therefore requires a secret key to run in production mode (this key encrypts the application secrets, if any are used).
Before continuing, run the following command (with the editor of your choice... vim is just an example):

```sh
EDITOR=vim ./bin/rails credentials:edit
```

to generate a secret key and encrypt any credentials. For more details, see the [Rails Guide](https://edgeguides.rubyonrails.org/security.html#custom-credentials)

### Configure Database settings

MarkUs needs to know how to access the database. Database settings should be stored in `config/database.yml`. For detailed instructions on how to configure this file see the [Rails Guides](https://edgeguides.rubyonrails.org/configuring.html#configuring-a-database).

For example, let's say your postgres instance was running on localhost, on port 5432, and you can connect as user `mypostgresuser` with the password `verysecret`. Also, you want MarkUs to use a database named `markusdb`. In this case, you would write the following to the `config/database.yml` file:

```yaml
production:
  adapter: postgresql
  host: localhost
  port: 5432
  username: mypostgresuser
  password: verysecret
  database: markusdb
```

Note that everything is nested under `production:` because MarkUs will run in production mode.

### Initialize the database

Now that the database settings have been set, create and migrate the database with the following commands:

```sh
RAILS_ENV=production ./bin/bundle exec rails db:create
RAILS_ENV=production ./bin/bundle exec rails db:migrate
```

### Precompile static assets

MarkUs will run a lot faster in production if [assets are precompiled](https://guides.rubyonrails.org/asset_pipeline.html#in-production). To precompile all static assets run the following commands:

```sh
RAILS_ENV=production ./bin/bundle exec rails i18n:js:export
RAILS_ENV=production ./bin/bundle exec rails assets:precompile
```

## Configuring the web server

MarkUs will run a [puma web server](https://puma.io/) on localhost but to make it accessible on the internet you will need to run your own web server (such as [apache](https://httpd.apache.org/) or [nginx](https://www.nginx.com/)).

Here are a few things to consider when setting up the configuration files for your web server (all examples are for Apache configuration, you will have to look up the nginx equivalent if necessary):

- If you have enabled the [`rails.force_ssl` configuration option](./Configuration.md#rails-specific-settings) you should make sure that you have a working ssl certificate for your domain. [Certbot](https://certbot.eff.org/instructions?ws=apache&os=ubuntufocal) has instruction on how to set this up.

- If you have enabled [remote authentication](./Configuration.md#user-authentication-options) make sure that:
    - the user name value is forwarded in the request header as the "HTTP_X_FORWARDED_USER" key
    - authentication is optional: since MarkUs supports both local and remote authentication, we don't want to force a user to use the remote authentication option if they can also log in with the local option.

  For example, you might have the following snippet in your apache config file when using [shibboleth](https://www.shibboleth.net/) for remote authentication, making all routes under '/' optionally protected by shibboleth authentication and forwarding the "user" variable from the shibboleth authetication to the header:

  ```xml
  <Location "/">
    Require shibboleth
    AuthType shibboleth
    RequestHeader set X-Forwarded-User %{user}e
  </Location>
  ```

  Then in the [configuration file](./Configuration.md#markus-settings) set the url for shibboleth logins:

  ```yaml
  remote_auth_login_url: https://my.host.com/Shibboleth.sso/Login
  ```

## Git access

MarkUs stores student submissions in bare repositories on disk, if your MarkUs instance is using git repositories and you would like your users to be able to clone/pull/push to those git repositories from the command line (or local git client), you need to set up access to the git repos over SSH or HTTPS.

### Git over SSH

To access git repositories over ssh, first make sure that your server can work as an ssh server by installing the `openssh-server` package (or similar) and making sure that port 22 is accessible over the internet.

Then, ensure that key storage is enabled so that users can upload their public keys to MarkUs by setting the following [configuration setting]((./Configuration.md#markus-settings):

```yaml
enable_key_storage: true
```

Then, you should set up the `markus` user (that you created [previously](#create-user)) so that users can ssh to your server as that user but *only* so that they can access git repositories. Make sure you follow the next steps **very** carefully. You do not want to accidentally allow users to ssh to your server and be able to perform arbitrary commands!

1. change the ownership of the `lib/repo/authorized_key_command.sh` file to the user that runs the `sshd` process (usually this is root) and make it executable:

   ```sh
   chown root:root lib/repo/authorized_key_command.sh
   chmod u=rwx lib/repo/authorized_key_command.sh
   ```

2. make sure the `lib/repo/markus-git-shell.sh` script is executable (should be owned by the `markus` user already):

   ```sh
   chmod u=rwx lib/repo/markus-git-shell.sh
   ```

3. create a `.ssh/` directory for the `markus` user:

    ```sh
    mkdir /home/markus/.ssh # or wherever the user's home directory is located
    ```

4. set some environment variables that will be used by the `authorized_key_command.sh` script by writing the following to the `/home/markus/.ssh/rc` file:

    ```sh
    export MARKUS_LOG_FILE=/path/to/some/logfile.log
    export MARKUS_REPO_LOC_PATTERN='/some/path/to/repos/(instance)'
    export GIT_SHELL=/path/to/markus/root/lib/repo/markus-git-shell.sh
    ```

   - where `/path/to/some/logfile.log` is an absolute path to a text file where you want the log output to be written (this is optional)
   - where `/some/path/to/repos/` is an absolute path to the location of the git repositories specified in the [`respository.storage`](./Configuration.md#markus-settings) configuration setting.
   - where `/path/to/markus/root/` is the path to the root of the MarkUs source code so that `/path/to/markus/root/lib/repo/markus-git-shell.sh` points to the `markus-git-shell.sh` script.

5. update the sshd settings so that when users ssh as the `markus` user they will only be allowed to run git commands (and only on repositories that they have access to as determined by the `.access` file (see [Git over HTTPS](#git-over-https) for details). Append the following to the `/etc/ssh/sshd_config` file:

    ```text
    Match User markus
      PermitRootLogin no
      AuthorizedKeysFile none
      AuthorizedKeysCommand /path/to/markus/root/lib/repo/authorized_key_command.sh
      AuthorizedKeysCommandUser markus
    ```

   - where `/path/to/markus/root/` is the path to the root of the MarkUs source code so that `/path/to/markus/root/lib/repo/authorized_key_command.sh` points to the `authorized_key_command.sh` script.

6. start (or restart) the `sshd` process so that the new configuration settings get picked up

7. Make sure that the `repository.ssh_url` configuration option is set up so that users will see the correct url to access their git repositories. For example, if your server is accessible over ssh as `my.server.com`, you should set the following configuration:

    ```yaml
    repository:
      ssh_url: 'ssh://markus@my.server.com'
    ```

    so that users can clone repositories from urls like `ssh://markus@my.server.com/blah/group1.git`

#### Git over HTTPS

To access git repositories over https use the [git-http-backend](https://git-scm.com/docs/git-http-backend) program.

When setting up autorization protocols for this access, your authorization script should check who has permission to which git repos by inspecting the `.access` file in the repository storage directory (see the [configuration settings](./Configuration.md#markus-settings)).

Each row of this file is a comma delimited and contains a relative path from the repository storage directory to a specific repository on disk followed by a list of user names of people who have access to this repository. For example, if [`respository.storage`](./Configuration.md#markus-settings) is `/some/path/to/repos/` and the `.access` file contains:

```csv
blah/group1.git,user1,user2
blah/group2.git,user1
bleh/*,user3
```

Then user1 should have permission to access repos at `/some/path/to/repos/blah/group1.git` and `/some/path/to/repos/blah/group2.git`, user2 should only have access to the first one. And user3 should have access to all repos in the `/some/path/to/repos/bleh/` directory.

An example Apache configuration might look like:

```xml
<Location /git/ >
  Require valid-user
  AuthType Basic
  AuthBasicProvider external
  AuthExternal authorizegit
  AuthName "MarkUs Git Repository"
  SetEnv GIT_PROJECT_ROOT /some/path/to/repos
  SetEnv GIT_HTTP_EXPORT_ALL
</Location>
ScriptAlias /git/ /usr/lib/git-core/git-http-backend/
```

(where you have previously set up an external authorization script called authorizegit in your Apache configuration)

Finally, make sure that the `repository.url` configuration option is set up so that users will see the correct url to access their git repositories.
For example, if your server's domain is `https://my.host.com` and you're using the example apache configuration above, you should set the following configuration:

```yaml
repository:
  url: 'https://my.host.com/git'
```

So that users will be prompted to clone their repositories from urls like: `https://my.host.com/git/blah/group1.git`

## Running MarkUs

Running MarkUs requires 3 processes which can be started with the following commands (run as the `markus` user):

- to start the Rails server (runs the main MarkUs process):

  ```sh
  RAILS_ENV=production ./bin/bundle exec rails server -e production
  ```

- to start resque (runs background jobs):

  ```sh
  RAILS_ENV=production QUEUES=* ./bin/bundle exec rails environment resque:work
  ```

- to start resque scheduler (schedules background jobs for later execution):

  ```sh
  RAILS_ENV=production QUEUES=* ./bin/bundle exec rails environment resque:scheduler
  ```

### Running MarkUs with a relative url root

If your server's domain is `https://my.host.com` but you want to make MarkUs available at a relative root like `https://my.host.com/markus/` for example then add this as an environment variable when starting each of the processes (above). For example, to start the Rails server you would run instead:

```sh
RAILS_RELATIVE_URL_ROOT=/markus RAILS_ENV=production ./bin/bundle exec rails server -e production
```

Make sure that your web server configuration is updated to reflect this url as well as any configuration options that reference the url.

## Adding Users

Users can be added and updated using the [API](./RESTful-API.md) but only as the admin user. To get the admin user's api key, run the following rake task:

```sh
RAILS_ENV=production ./bin/rails db:admin
```

## Creating courses

Courses can be added and updated using the [API](./RESTful-API.md) but only as the admin user. To get the admin user's api key, run the following rake task:

```sh
RAILS_ENV=production ./bin/rails db:admin
```

### Enabling autotesting

The autotester can be installed seperately from MarkUs on a different server (or the same one if you'd prefer). Here are the autotester [Installation instructions](https://github.com/MarkUsProject/markus-autotesting/blob/release/README.md).

Once the autotester has been set up, you must register each course that uses autotesting by providing the URL of the autotester through the API using the `api/courses/<course_id>/update_autotest_url` route.
