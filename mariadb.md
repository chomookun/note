# MariaDB(Mysql)

## Install
```bash
# searches packages
$ sudo apt-cache search mariadb | grep -e '^mariadb'

# installs server and client 
$ sudo apt-get install mariadb-server mariadb-client
```

## Starts and shutdown
```bash
# shows status
$ sudo service mysql status

# start mysql
$ sudo service mysql start

# stop mysql
$ sudo service mysql stop
```

## Connects via client
```bash
# connect via client
$ sudo mysql -u root -p
```

## Monitoring status summary
```sql
-- theads
show status like '%Thread%';

-- abort
show status like '%Abort%';

-- count by user,host
select user,host,count(*) as count 
from information_schema.processlist 
group by user,host 
order by count desc
limit 10;

-- process order by time
select id,user,host,state,time_ms,substr(info,1,32) as info 
from information_schema.processlist 
order by time_ms desc 
limit 10;

```

## Creates database and user, grant privileges
```sql
-- creates database
create database test_db;

-- creates user
create user 'test_user'@'%' identified by 'abcd1234!@#$';

-- grant privileges
grant all privileges on test_db.* to 'test_user'@'%';

```

## Settng slow query log
```shell
# edit config
sudo vim /etc/mysql/my.cnf
...
# slow query log
slow_query_log = 1
slow_query_log_file = /home/mysql/log/slow_query.log
long_query_time=5
...

# restart
sudo systemctl restart mysql
```


