# InfluxDB

## Install
```shell
$ sudo apt-get install influxdb influxdb-client
$ sudo service influxdb start
```

## Tailing logs
```shell
$ tail -F /var/log/influxdb/influxdb.log
```

## Connect for test
```sql
$ influx
Connected to http://localhost:8086 version 1.6.4
InfluxDB shell version: 1.6.4
>

-- creates database
> create database test;
> show databases;

-- use database
> use test;
Using database test

-- insert measurement
insert memory,host=server01,region=korea value=5.5

-- show data
> show measurement
name: measurements
name
----
memory
> select * from memory
name: memory
time                host      region value
----                ----      ------ -----
1611931350020113000 localhost korea  5.5
```

## Configuration
```shell
$ sudo vim /etc/influxdb/influxdb.conf
...
auth-enable = true
...
```

## Creates user
```sql
> create user test_user with password 'abcd1234!' with all privileges
```

## Java Code Example
- Maven dependency
```xml
<dependency>
    <groupId>org.influxdb</groupId>
    <artifactId>influxdb-java</artifactId>
    <version>2.19</version>
</dependency>
```
- Java code example
```java
public class InfluxDbTest {

	public static void main(String[] args) throws Exception {
		executeInsertQuery();
		executeSelectQuery();
	}
	
	/**
	 * return test connection
	 * @return
	 */
	private static InfluxDB getConnection() {
		String databaseUrl = "http://127.0.0.1:8086";
		String username = "test1";
		String password = "xptmxm1";
		InfluxDB influxDb = InfluxDBFactory.connect(databaseUrl, username, password);
		influxDb.setLogLevel(LogLevel.FULL);
		influxDb.setDatabase("test");
		return influxDb;
	}
	
	/**
	 * executes insert query
	 * @throws Exception
	 */
	public static void executeInsertQuery() throws Exception {
		InfluxDB influxDb = getConnection();
		try {
			Point point = Point.measurement("memory")
					.time(System.currentTimeMillis(), TimeUnit.MICROSECONDS)
					.addField("host", "server1")
					.addField("free", 10020)
					.addField("used", 20001)
					.build();
			influxDb.write(point);
		}catch(Exception e) {
			throw e;
		}finally {
			influxDb.close();
		}
	}
	
	/**
	 * executes select query
	 * @throws Exception
	 */
	public static void executeSelectQuery() throws Exception {
		InfluxDB influxDb = getConnection();
		try {
			Query query = new Query("select * from memory");
			QueryResult queryResult = influxDb.query(query);
			List<Result> list = queryResult.getResults();
			System.out.println(String.format("list.size():%d", list.size()));
			for(Result element : list) {
				System.out.println("@@@@@" + element);
			}
		}catch(Exception e) {
			throw e;
		}finally {
			influxDb.close();
		}
	}
}
```
