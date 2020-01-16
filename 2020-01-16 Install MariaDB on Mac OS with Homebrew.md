---
title: Install MariaDB on Mac OS with Homebrew
---

[<= back to index](/)

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

## Solution

If you google the error, you may find that some people suggest to run it as root. And it really works:
```
$ sudo mysql -uroot
Password:
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 16
...
```

But of course you need it to work without `sudo`. To do that, you need to formulate a [proper google query](https://www.google.com/search?q=mariadb+mysql+uroot+works+only+with+sudo), which would lead you to the solution: https://askubuntu.com/questions/766334/cant-login-as-mysql-user-root-from-normal-user-account-in-ubuntu-16-04


[<= back to index](/)
