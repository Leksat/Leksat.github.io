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

## Solution

If the above is your case, all you need to do is to
- login to MySQL with `sudo mysql -uroot`
- run `SET PASSWORD FOR 'root'@'localhost' = PASSWORD('');` from there

The best explanation that [I was able to find](https://askubuntu.com/a/964762/99219):

> The issue here is that when MariaDB or MySQL are installed/updated (especially if at some point root is set without a password) then in the Users table the password is actually empty (or ignored), and logging in depends on the system user corresponding to a MySQL user.

And as far as I understood it, "empty" is not the same as "empty string".

<small>[<= back to index](/)</small>
