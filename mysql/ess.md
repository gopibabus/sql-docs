# MySQL Configuration

<img alt="Technology" width="700" src="/assets/images/mysql-config.png" />

[REFERENCE](https://www.tecmint.com/find-mysql-php-apache-configuration-files/)

---

## How To Find MySQL (my.conf) Configuration File

* You can locate the the MySQL configuration file using the **mysql command line tool** or **mysqladmin**, a client for managing a MySQL server.

* The following commands will display the **mysql** or **mysqladmin** help page, which includes a section that talks about the files (configuration files) from which default options are read.
    ```text
    mysql --help | grep -A1 'Default options'

    output:
    /etc/my.cnf /etc/mysql/my.cnf /usr/etc/my.cnf ~/.my.cnf
    ```

    ```text
    mysqladmin --help | grep -A1 'Default options'

    Output:
    /etc/my.cnf /etc/mysql/my.cnf /usr/etc/my.cnf ~/.my.cnf</br>
    ```

---

## mysqladmin

**mysqladmin** is a command-line utility the comes with MySQL server and it is used by Database Administrators to perform some basic MySQL tasks easily such as _setting root password, changing root password, monitoring mysql processes, reloading privileges,checking server status_ etc.

* How to clear command line screen in SQL mode?

    ```text
    system clear
    ```

* How to execute SQL script(.sql files)?

    ```text
    source <path to sql script>
    ```

* How to set MySQL Root password?

    ```text
    mysqladmin -u root password YOURNEWPASSWORD
    ```

* How to Change MySQL Root password?

    ```text
    mysqladmin -u root -p123456 password 'xyz123'
    ```

* How to check MySQL Server is running?

    ```text
    mysqladmin -u root -p ping
    ```

* How to Check which MySQL version I am running?

    >The following command shows MySQL version along with the current running status .

    ```text
    mysqladmin -u root -p version
    ```

* How to Find out current Status of MySQL server?

    > To find out current status of MySQL server, use the following command. The mysqladmin command shows the status of uptime with running threads and queries.

    ```text
    mysqladmin -u root -ptmppassword status
    ```

* How to check status of all MySQL Server Variable’s and value’s?

    ```text
    mysqladmin -u root -p extended-status
    ```

* How to see all MySQL server Variables and Values?

    ```text
    mysqladmin  -u root -p variables
    ```

* How to check all the running Process of MySQL server?

    ```text
    mysqladmin -u root -p processlist
    ```

* How to create a Database in MySQL server?

    ```text
    mysqladmin -u root -p create databasename
    ```

* How to drop a Database in MySQL server?

    ```text
    mysqladmin -u root -p drop databasename
    ```

* How to reload/refresh MySQL Privileges?

    ```text
    mysqladmin -u root -p reload;
    mysqladmin -u root -p refresh
    ```

* How to shutdown MySQL server Safely?

    ```text
    mysqladmin -u root -p shutdown
    (OR)
    /etc/init.d/mysqld stop
    /etc/init.d/mysqld start
    ```

* Some useful MySQL Flush commands

    > flush-hosts: Flush all host information from host cache.</br>
    > flush-tables: Flush all tables.</br>
    > flush-threads: Flush all threads cache.</br>
    > flush-logs: Flush all information logs.</br>
    > flush-privileges: Reload the grant tables (same as reload).</br>
    > flush-status: Clear status variables.</br>

    ```text
    mysqladmin -u root -p flush-hosts
    mysqladmin -u root -p flush-tables
    mysqladmin -u root -p flush-threads
    mysqladmin -u root -p flush-logs
    mysqladmin -u root -p flush-privileges
    mysqladmin -u root -p flush-status
    ```

* How to kill Sleeping MySQL Client Process?

    ```text
    mysqladmin -u root -p processlist

    Run the following command with kill and process ID as shown below.

    mysqladmin -u root -p kill process_ID

    If you like to kill multiple process, then pass the process ID‘s with comma separated as shown below.

    mysqladmin -u root -p kill process_id1, process_id2
    ```

* How to run multiple mysqladmin commands together?

    ```text
    mysqladmin  -u root -p processlist status version
    ```

* How to Connect remote mysql server?

    ```text
    Use the -h (host)  with IP Address of remote machine.

    mysqladmin  -h 172.16.25.126 -u root -p
    ```

* How to execute command on remote MySQL server?

    ```text
    mysqladmin  -h 172.16.25.126 -u root -p status
    ```

* How to start/stop MySQL replication on a slave server?

    ```text
    mysqladmin  -u root -p start-slave
    mysqladmin  -u root -p stop-slave
    ```

* How to store MySQL server Debug Information to logs?

    > It tells the server to write debug information about locks in use, used memory and query usage to the MySQL log file including information about event scheduler.

    ```text
    mysqladmin  -u root -p debug
    ```

* How to view mysqladmin options and usage?

    ```text
    mysqladmin --help
    ```

[⏯ Video Reference](https://www.youtube.com/watch?v=vIIRuDtG1yI)
