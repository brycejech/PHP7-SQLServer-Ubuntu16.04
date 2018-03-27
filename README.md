
# Connecting to SQL Server from PHP 7 on Ubuntu 16.04

This guide assumes that you are already running an Apache2 web server and already have PHP 7 installed and running. This does not install any of the cli tools for connecting to SQL Server, only the bare minimum to connect to SQL Server using `sqlsrv_connect` function or `PDO`.


Update package list
```bash
sudo su
apt-get update
```

Install `php7.0-dev`, required for `php-pear`
```bash
apt-get install php7.0-dev
```

Install `php-pear` and `unixodbc-dev` packages, required for the pear packages we're going to install. `unixodbc-dev` contains a sql.h header file required to build the `sqlsrv` and `pdo_sqlsrv` pear packages.
```bash
apt-get install php-pear unixodbc-dev
```

Build/install PHP packages
```bash
pecl install sqlsrv pdo_sqlsrv
```

Find the proper PHP ini file in use. If running under apache then path should be /etc/php/7.0/apache2/php.ini. If also modifying the php.ini file for the PHP cli then path is /etc/php/7.0/cli/php.ini.

Edit the proper php.ini file to contain the following lines
```ini
extension=pdo_sqlsrv.so
extension=sqlsrv.so
```

Add apt key for Microsoft (is needed to be able to securely use the repository we're about to use)
```bash
curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
```

Make sure the apt-transport-https package is installed before proceeding to next step. If not installed run `apt-get install apt-transport-https`.


Add the Microsoft apt repository that we can download the "Microsoft ODBC Driver 17 for SQL Server" from
```bash
curl https://packages.microsoft.com/config/ubuntu/16.04/prod.list > /etc/apt/sources.list.d/mssql-release.list
```

Install Microsoft ODBC Driver 17 for SQL Server.
```bash
apt-get update
apt-get install msodbcsql17
```

Restart apache2
```bash
service apache2 restart
```

Should now be able to connect to SQL Server from PHP 7 on Ubuntu 16.04 ;)
