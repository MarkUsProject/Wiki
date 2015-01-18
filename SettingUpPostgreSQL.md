Setting up the Database (PostgreSQL)
====================================

Setup on the Vagrant Box
------------------------

Installing the Database
-----------------------

On Debian and Ubuntu, a simple

    #> apt-get install postgresql postgresql-client postgresql-contrib libpq-dev


Configuring PostgreSQL
----------------------

Make sure that you have set an UTF-8 locale/encoding (e.g. set LANG=en\_CA.UTF-8 environment variable). The postgresql cluster will be created in the encoding which is currently set. Type locale in the term and you should see something similar to the following:

    $> locale
    LANG=en_CA.UTF-8
    LC_CTYPE="en_CA.UTF-8"
    LC_NUMERIC="en_CA.UTF-8"
    LC_TIME="en_CA.UTF-8"
    LC_COLLATE="en_CA.UTF-8"
    LC_MONETARY="en_CA.UTF-8"
    LC_MESSAGES="en_CA.UTF-8"
    LC_PAPER="en_CA.UTF-8"
    LC_NAME="en_CA.UTF-8"
    LC_ADDRESS="en_CA.UTF-8"
    LC_TELEPHONE="en_CA.UTF-8"
    LC_MEASUREMENT="en_CA.UTF-8"
    LC_IDENTIFICATION="en_CA.UTF-8"
    LC_ALL=

**Creating a Database User and Changing Authentication Scheme**

For simplicity we create a database user "markus" with the same password, to which superuser privileges will be granted. We will use this user for MarkUs later. As root execute the following (be careful not to forget any backslashes or single-/doublequotes):

    #> sudo -u postgres psql postgres
    postgres=# \password postgres
    postgres=# create role markus createdb login password 'markus';
    postgres=# \q

The above command should output the following:

    CREATE ROLE

However if you keep getting the following everytime you try to enter your password:

    #> su -c "psql -c \"create user markus with superuser password
    'markus';\""
    postgres Password:
    su: Authentication failure

You can run the following instead:

    #>sudo su
    Password:
    #> su -c "psql -c \"create user markus with superuser password
    'markus';\"" postgres
    CREATE ROLE

Finally, we need to change a line in the configuration file of the PostgreSQL database. As root open "pg\_hba.conf" (sometimes "pg\_hdb.conf") in `/etc/postgres/\<pg-version\>/main/` or in `/etc/postgresql/\<pg-version\>/main/` and look for the following lines (the first one is actually only a comment):

    # "local" is for Unix domain socket connections only
    local   all         all                               peer

Now change the second line like so:

    local   all         all                               md5

Restart PostgresSQL in order to apply those configuration changes to the server:

    #> /etc/init.d/postgresql restart

To test if everything went fine we try to connect to the "postgres" database using our newly created user:

    #> psql postgres markus

You will be asked for a password, so type "markus". After that you should see the console of PostgreSQL.

Install PostgreSQL (make sure that the created cluster is UTF-8 encoded; If not required, it also works with latin-1)



