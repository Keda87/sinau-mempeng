# Basic Setup

- Install & setup binding.
    ```bash
    $ sudo apt install mysql-server                # install mysql
    $ sudo vim /etc/mysql/mysql.conf.d/mysqld.cnf  # edit bind address, biar bisa diakses diluar localhost.
    ```

    Bagian `bind-address` perlu dirubah dari `127.0.0.1` ke `0.0.0.0`.
    ```bash
    # If MySQL is running as a replication slave, this should be
    # changed. Ref https://dev.mysql.com/doc/refman/8.0/en/server-system-variables.html#sysvar_tmpdir
    # tmpdir                = /tmp
    #
    # Instead of skip-networking the default is now to listen only on
    # localhost which is more compatible and is not less secure.
    bind-address            = 0.0.0.0
    mysqlx-bind-address     = 127.0.0.1
    ```

- Setup user & privilege.
    ```sql
    mysql> CREATE USER '<your user>'@'%' IDENTIFIED BY '<your password>';
    Query OK, 0 rows affected (0.01 sec)

    mysql> GRANT ALL PRIVILEGES ON *.* TO '<your user>'@'%' WITH GRANT OPTION;
    Query OK, 0 rows affected (0.04 sec)

    mysql> FLUSH PRIVILEGES;
    Query OK, 0 rows affected (0.00 sec)
    ```
