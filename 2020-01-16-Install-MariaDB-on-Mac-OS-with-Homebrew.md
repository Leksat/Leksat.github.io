---
title: Install MariaDB on Mac OS with Homebrew
---

<small>[<= back to index](/)</small>

# Install MariaDB on Mac OS with Homebrew

If you need MariaDB for development purposes, it's enough to have just `root` user with no password. And that's what you get by default after running `brew install mariadb`. Homebrew says:
```
To connect:
    mysql -uroot

To have launchd start mariadb now and restart at login:
  brew services start mariadb
```

So you start the service with `brew services start mariadb` and try to login.

## Problem

Variant 1:
```
$ mysql -uroot
ERROR 2002 (HY000): Can't connect to local MySQL server through socket '/tmp/mysql.sock' (2)
```

Variant 2:
```
$ mysql -uroot
ERROR 1698 (28000): Access denied for user 'root'@'localhost'
```

If you google the error, you may find that some people suggest to run it as root. And it really works:
```
$ sudo mysql -uroot
Password:
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 16
...
```

But of course you need it to work without `sudo`.

## Reason

The above behaviour was introduced in [MariaDB 10.4](https://mariadb.com/kb/en/authentication-from-mariadb-104/).

You can login with `sudo mysql -uroot` because
> First, it is configured to try to use the unix_socket authentication plugin. This allows the root@localhost user to login without a password via the local Unix socket file defined by the socket system variable, as long as the login is attempted from a process owned by the **operating system root user account**.

You cannot login with `mysql -uroot` because
> Second, if authentication fails with the unix_socket authentication plugin, then it is configured to try to use the mysql_native_password authentication plugin. However, **an invalid password is initially set**, so in order to authenticate this way, a password must be set with SET PASSWORD.

## Solution

To make it possible to login root user without a password:
- login to MySQL with `sudo mysql -uroot`
- run `SET PASSWORD FOR 'root'@'localhost' = PASSWORD('');` from there

<small>[<= back to index](/)</small>
