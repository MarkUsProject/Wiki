Setting up the Database (MySQL)
===============================

Installing the Database
-----------------------

### GNU/Linux

On Debian and Ubuntu, a simple

    apt-get install mysql-server libmysqlclient-dev


Configuring MySQL
-----------------

### Creating a database user

To create a database user, enter the following commands: (In this example, the user is named 'markus', his password is 'markus', and he will be given superuser privileges. This user will be used for MarkUs later on.):

    #>mysql --user=root --password=<my password> mysql
    #>CREATE USER 'markus'@'localhost' IDENTIFIED BY 'markus';
    #>GRANT ALL PRIVILEGES ON *.* TO 'markus'@'localhost' WITH GRANT OPTION;

You can now try connecting to the server using the user you just created:

    #>mysql --user=markus --password=markus

You should see the MySQL console.

### Configuring MarkUs

Setup the database.yml file, in the MarkUs' root directory:

-   \`cp config/database.yml.mysql config/database.yml\`

-   change the usernames and password to the ones you used in the section above

-   uncomment the development and test sections of config/database.yml

