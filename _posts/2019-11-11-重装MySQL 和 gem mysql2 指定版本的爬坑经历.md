---
layout: post
title: 重装MySQL 和 gem mysql2 指定版本的爬坑经历
date: 2019-11-11 00:00:00
author: "王振华"
tags: 
    - MySQL 
    - mysql2
    - 重装 
    - 指定版本
---


## MySQL的安装

我因为新的项目用的 `gem 'mysql2', '0.4.5'`，与我的 `MySQL 5.7` 不兼容，就重装了MySQL。

但执行 `mysql -uroot`，报错 `ERROR 2002 (HY000): Can't connect to local MySQL server through socket '/tmp/mysql.sock' (2)`

执行 `ln -s /usr/local/var/mysql/mysql.sock /tmp/mysql.sock`后，
再执行 `mysql -uroot`，报错`ERROR 2002 (HY000): Can't connect to local MySQL server through socket '/usr/local/var/mysql/mysql.sock' (2)`
去`/usr/local/var/mysql` 目录查看，发现根本没有 `mysql.sock` 文件。所以，不只是没有软链接的问题。

又Google到了各种答案，各种尝试后都还是不行。

最终搜到了以下答案，直接彻底删除之前的MySQL，然后重装：[brew install mysql on mac os](https://stackoverflow.com/questions/4359131/brew-install-mysql-on-mac-os)

```
brew remove mysql

brew cleanup

launchctl unload -w ~/Library/LaunchAgents/homebrew.mxcl.mysql.plist

rm ~/Library/LaunchAgents/homebrew.mxcl.mysql.plist

sudo rm -rf /usr/local/var/mysql
```

```
brew install mysql         #安装最新版本
brew install mysql@5.7     #安装指定版本


unset TMPDIR

mysql.server start

mysql -uroot
```

我自己一开始的重装出错的原因，应该就是没有彻底删除之前的MySQL。




执行 `rails console`时，若报错： `Library not loaded: /usr/local/opt/mysql/lib/libmysqlclient.20.dylib (LoadError)`， 说明是重装了MySQL，rails 找不到 mysql的相关文件了。这时需要依次执行：

```
gem uninstall mysql2
gem install mysql
```


## 安装 gem mysql2 的指定版本

比如Rails项目的Gemfile中使用的是 gem mysql2 0.3.18 版本，执行 gem install mysql2 -v '0.3.18',报错：

```
Building native extensions.  This could take a while...
ERROR:  Error installing mysql2:
	ERROR: Failed to build gem native extension.

    /Users/huazai/.rvm/rubies/ruby-2.1.1/bin/ruby extconf.rb
checking for ruby/thread.h... yes
checking for rb_thread_call_without_gvl() in ruby/thread.h... yes
checking for rb_thread_blocking_region()... yes
checking for rb_wait_for_single_fd()... yes
checking for rb_hash_dup()... yes
checking for rb_intern3()... yes
-----
Using mysql_config at /usr/local/bin/mysql_config
-----
checking for mysql.h... yes
checking for errmsg.h... yes
checking for mysqld_error.h... yes
-----
Setting rpath to /usr/local/Cellar/mysql/8.0.15/lib
-----
creating Makefile

make "DESTDIR=" clean

make "DESTDIR="
compiling client.c
client.c:431:3: error: use of undeclared identifier 'my_bool'
  my_bool res = mysql_read_query_result(client);
  ^
client.c:433:19: error: use of undeclared identifier 'res'
  return (void *)(res == 0 ? Qtrue : Qfalse);
                  ^
client.c:762:3: error: use of undeclared identifier 'my_bool'
  my_bool boolval;
  ^
client.c:793:7: error: use of undeclared identifier 'boolval'
      boolval = (value == Qfalse ? 0 : 1);
      ^
client.c:794:17: error: use of undeclared identifier 'boolval'
      retval = &boolval;
                ^
client.c:797:10: error: use of undeclared identifier 'MYSQL_SECURE_AUTH'; did you mean 'MYSQL_DEFAULT_AUTH'?
    case MYSQL_SECURE_AUTH:
         ^~~~~~~~~~~~~~~~~
         MYSQL_DEFAULT_AUTH
/usr/local/Cellar/mysql/8.0.15/include/mysql/mysql.h:188:3: note: 'MYSQL_DEFAULT_AUTH' declared here
  MYSQL_DEFAULT_AUTH,
  ^
client.c:798:7: error: use of undeclared identifier 'boolval'
      boolval = (value == Qfalse ? 0 : 1);
      ^
client.c:799:17: error: use of undeclared identifier 'boolval'
      retval = &boolval;
                ^
client.c:830:38: error: use of undeclared identifier 'boolval'
        wrapper->reconnect_enabled = boolval;
                                     ^
client.c:1196:38: error: use of undeclared identifier 'MYSQL_SECURE_AUTH'; did you mean 'MYSQL_DEFAULT_AUTH'?
  return _mysql_client_options(self, MYSQL_SECURE_AUTH, value);
                                     ^~~~~~~~~~~~~~~~~
                                     MYSQL_DEFAULT_AUTH
/usr/local/Cellar/mysql/8.0.15/include/mysql/mysql.h:188:3: note: 'MYSQL_DEFAULT_AUTH' declared here
  MYSQL_DEFAULT_AUTH,
  ^
10 errors generated.
make: *** [client.o] Error 1

make failed, exit code 2
```

是因为 和 mysql 的高版本不兼容。需要卸载 MySQL重装兼容的版本，比如 MySQL 5.7版。


但安装5.7版后再执行 `gem install mysql2 -v '0.3.18' --source 'https://gems.ruby-china.com/` 又报错：

```
Building native extensions.  This could take a while...
ERROR:  Error installing mysql2:
	ERROR: Failed to build gem native extension.

    /Users/huazai/.rvm/rubies/ruby-2.1.1/bin/ruby extconf.rb
checking for ruby/thread.h... yes
checking for rb_thread_call_without_gvl() in ruby/thread.h... yes
checking for rb_thread_blocking_region()... yes
checking for rb_wait_for_single_fd()... yes
checking for rb_hash_dup()... yes
checking for rb_intern3()... yes
checking for mysql_query() in -lmysqlclient... no
checking for main() in -lm... yes
checking for mysql_query() in -lmysqlclient... no
checking for main() in -lz... yes
checking for mysql_query() in -lmysqlclient... no
checking for main() in -lsocket... no
checking for mysql_query() in -lmysqlclient... no
checking for main() in -lnsl... no
checking for mysql_query() in -lmysqlclient... no
checking for main() in -lmygcc... no
checking for mysql_query() in -lmysqlclient... no
*** extconf.rb failed ***
Could not create Makefile due to some reason, probably lack of necessary
libraries and/or headers.  Check the mkmf.log file for more details.  You may
need configuration options.

Provided configuration options:
	--with-opt-dir
	--without-opt-dir
	--with-opt-include
	--without-opt-include=${opt-dir}/include
	--with-opt-lib
	--without-opt-lib=${opt-dir}/lib
	--with-make-prog
	--without-make-prog
	--srcdir=.
	--curdir
	--ruby=/Users/huazai/.rvm/rubies/ruby-2.1.1/bin/ruby
	--with-mysql-dir
	--without-mysql-dir
	--with-mysql-include
	--without-mysql-include=${mysql-dir}/include
	--with-mysql-lib
	--without-mysql-lib=${mysql-dir}/lib
	--with-mysql-config
	--without-mysql-config
	--with-mysql-dir
	--without-mysql-dir
	--with-mysql-include
	--without-mysql-include=${mysql-dir}/include
	--with-mysql-lib
	--without-mysql-lib=${mysql-dir}/lib
	--with-mysqlclientlib
	--without-mysqlclientlib
	--with-mlib
	--without-mlib
	--with-mysqlclientlib
	--without-mysqlclientlib
	--with-zlib
	--without-zlib
	--with-mysqlclientlib
	--without-mysqlclientlib
	--with-socketlib
	--without-socketlib
	--with-mysqlclientlib
	--without-mysqlclientlib
	--with-nsllib
	--without-nsllib
	--with-mysqlclientlib
	--without-mysqlclientlib
	--with-mygcclib
	--without-mygcclib
	--with-mysqlclientlib
	--without-mysqlclientlib

extconf failed, exit code 1
```

也搜到了各种方法，都没解决问题。可能是我重装之前删掉了一些不该删除的文件。不知道以后还会不会带来灾难，哈哈哈哈哈哈哈哈哈哈哈
我是最后通过执行`gem install mysql2 -v '0.3.18' --source 'https://gems.ruby-china.com/' -- --with-mysql-config=/usr/local/Cellar/mysql@5.7/5.7.25/bin/mysql_config` 才安装成功了。


执行 `rails console`时，若报错：`Can't connect to local MySQL server through socket '/tmp/mysql.sock' (2) (Mysql2::Error)`，执行`ln -s /usr/local/var/mysql/mysql.sock /tmp/mysql.sock` ，如果报错 `ln: /tmp/mysql.sock: File exists` ，可先将文件`/tmp/mysql.sock`删除，再执行 `ln -s /usr/local/var/mysql/mysql.sock /tmp/mysql.sock` 



