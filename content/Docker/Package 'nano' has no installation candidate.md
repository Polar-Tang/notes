```sh
echo "deb http://archive.ubuntu.com/ubuntu focal main restricted universe multiverse" > /etc/apt/sources.list
echo "deb http://archive.ubuntu.com/ubuntu focal-updates main restricted universe multiverse" >> /etc/apt/sources.list
echo "deb http://security.ubuntu.com/ubuntu focal-security main restricted universe multiverse" >> /etc/apt/sources.list
```
```sh
apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 3B4FE6ACC0B21F32 871920D1991BC93C
apt-key update
apt-get update
apt install nano
echo 'Acquire::AllowInsecureRepositories "true";' > /etc/apt/apt.conf.d/99allow-insecure
apt-get update
apt-get install -y mysql-client --allow-unauthenticated

apt-get update
```

```
apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 3B4FE6ACC0B21F32 871920D1991BC93C
apt-get update
```
```mysql
mysql> SELECT User, Host FROM mysql.user;
+------------------+-----------+
| User             | Host      |
+------------------+-----------+
| mysql.infoschema | localhost |
| mysql.session    | localhost |
| mysql.sys        | localhost |
| root             | localhost |
+------------------+-----------+
mysql> 
mysql> -- If the user doesn't exist, create it
mysql> CREATE USER 'bb-labs-user'@'%' IDENTIFIED BY 'bb-labs-password';
Query OK, 0 rows affected (0.08 sec)

mysql> GRANT ALL PRIVILEGES ON `bb-labs`.* TO 'bb-labs-user'@'%';
Query OK, 0 rows affected (0.02 sec)

mysql> FLUSH PRIVILEGES;
Query OK, 0 rows affected (0.04 sec)


```