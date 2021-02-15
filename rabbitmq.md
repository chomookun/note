# RabbitMQ

## Installation

### Installs via package
```
user@host> echo "deb https://dl.bintray.com/rabbitmq/debian bionic main" | sudo tee /etc/apt/sources.list.d/bintray.rabbitmq.list

user@host> sudo apt update
user@host> sudo apt-get install rabbitmq-server
```

### Installs manually
``` shell
# sync package metadata
user@host> user@host>sudo apt-get update
# install dependencies manually
user@host> sudo apt-get -y install socat logrotate init-system-helpers adduser

# download the package
user@host> sudo apt-get -y install wget
user@host> wget https://github.com/rabbitmq/rabbitmq-server/releases/download/v3.8.5/rabbitmq-server_3.8.5-1_all.deb

# install the package with dpkg
user@host> sudo dpkg -i rabbitmq-server_3.8.5-1_all.deb
user@host> rm rabbitmq-server_3.8.5-1_all.deb
```

### Kills process
sometimes process start and stop stuck. kill process.
``` shell
user@host> sudo pkill -KILL -u rabbitmq
```

### Starts rabbitmq-server
``` shell
# start service
user@host> sudo service rabbitmq-server start
# test socket open(default port is 5672)
user@host> telnet 127.0.0.1 5672
```

### Setting Management plugin
```
user@host> sudo rabbitmq-plugins enable rabbitmq_management
user@host> sudo rabbitmq-server restart
```

### Adds administrator user
```
user@host> sudo rabbitmqctl add_user admin rhksflwk
user@host> sudo rabbitmqctl set_user_tags admin administrator
```

### Connection admin web console
> http://127.0.0.1:15672
> admin / rhksflwk


## CLI interface

### list queue
```
user@host> rabbitmqadmin list queues
```

### declare queue
```
user@host> rabbitmqadmin declare queue name=test durable=false
```

## Example Code

### Produer
```java
package net.chomookun.note.rabbitmq;

import com.rabbitmq.client.Channel;
import com.rabbitmq.client.Connection;
import com.rabbitmq.client.ConnectionFactory;

public class RabbitMqTest {
	
	private static final String QUEUE_NAME = "test";
	
	public static void main(String[] args) throws Exception {

	    ConnectionFactory factory = new ConnectionFactory();
        factory.setHost("localhost");
        factory.setPort(5672);
        factory.setUsername("uguest");
        factory.setPassword("pguest");
        Connection connection = null;
        Channel channel = null;
        
        try {
        	connection = factory.newConnection(); 
        	channel = connection.createChannel();
            for (int i = 0; i <= 100000; i++) {
                channel.queueDeclare(QUEUE_NAME, false, false, false, null);
                String message = "Hello World!" + (int) (Math.random() * 100);
                channel.basicPublish("", QUEUE_NAME, null, message.getBytes());
                System.out.println(" [x] Set '" + message + "'");
                Thread.sleep(10);
            }
        } catch (Exception e) {
            e.printStackTrace();
        }finally {
        	if(channel != null) 
                try { channel.close(); }catch(Exception ignore) {}
        	if(connection != null) 
                try { connection.close(); }catch(Exception ignore) {}
        }
		
	}
}
```

### Consumer
```java
package net.chomookun.note.rabbitmq;

import java.io.IOException;

import com.rabbitmq.client.AMQP.BasicProperties;
import com.rabbitmq.client.Channel;
import com.rabbitmq.client.Connection;
import com.rabbitmq.client.ConnectionFactory;
import com.rabbitmq.client.Consumer;
import com.rabbitmq.client.Envelope;
import com.rabbitmq.client.ShutdownSignalException;

public class ConsumerMain {
	
	private final static String QUEUE_NAME = "test";
	
	public static void main(String[] args) throws Exception {
	    ConnectionFactory factory = new ConnectionFactory();
        factory.setHost("localhost");
        factory.setPort(5672);
        Connection connection = null;
        Channel channel = null;
        try {
        	connection = factory.newConnection(); 
        	channel = connection.createChannel();
        	channel.queueDeclare(QUEUE_NAME, false, false, false, null);
        	channel.basicConsume(QUEUE_NAME, true, new Consumer() {
				public void handleConsumeOk(String consumerTag) {
					System.out.println(String.format("[%s]Consumer.handleConsumeOk", consumerTag));
				}
				public void handleCancelOk(String consumerTag) {
					System.out.println(String.format("[%s]Consumer.handleCancelOk", consumerTag));
				}
				public void handleCancel(String consumerTag) throws IOException {
					System.out.println(String.format("[%s]Consumer.handleCancel", consumerTag));
				}
				public void handleDelivery(String consumerTag, Envelope envelope, BasicProperties properties, byte[] bytes) throws IOException {
					System.out.println(String.format("[%s]Consumer.handleDelivery", consumerTag));
					System.out.println("envelope:" + envelope);
					System.out.println("properties:" + properties);
					System.out.println("bytes:" + bytes);
				}
				public void handleShutdownSignal(String consumerTag, ShutdownSignalException sig) {
					System.out.println(String.format("[%s]Consumer.handleShutdownSignal", consumerTag));
				}
				public void handleRecoverOk(String consumerTag) {
					System.out.println(String.format("[%s]Consumer.handleRecoverOk", consumerTag));
				}
        	});
        	
        	// thread join
        	Thread.currentThread().join();
        	
        } catch (Exception e) {
            e.printStackTrace();
        }finally {
        	if(channel != null) 
                try { channel.close(); }catch(Exception ignore) {}
        	if(connection != null) 
                try { connection.close(); }catch(Exception ignore) {}
        }
	}
}
```