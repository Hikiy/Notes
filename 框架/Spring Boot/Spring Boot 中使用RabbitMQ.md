# Spring Boot 中使用RabbitMQ

### 消息队列中间件
之前一直不知道消息队列是啥，这里举例子解释：  
生产者生产苹果，消费者消费苹果。有几种情况：
>1. 生产者生产1个苹果，消费者消费1个苹果。如果消费者消费苹果时噎住（系统宕机），生产者还在生产，则生产的苹果就丢失了。  
>2. 生产者1秒生产10个苹果，消费者1秒消费5个苹果。消费者会吃不消（消息堵塞，导致系统超时），最后苹果丢失  

如果我们在生产者和消费者之间放一个篮子，生产者将苹果放进篮子，消费者从篮子拿苹果，则苹果就不会丢失了。消息队列中间件即是这个篮子。一下明朗了。

### RabbitMQ介绍

[RabbitMQ介绍](https://github.com/Hikiy/Notes/blob/master/%E6%A1%86%E6%9E%B6/Spring%20Boot/RabbitMQ.md)

RabbitMQ是要先安装的，直接百度就好了

### Maven
```
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-amqp</artifactId>
</dependency>
```

### application.properties
```
spring.application.name=Spring-boot-rabbitmq

spring.rabbitmq.host=127.0.0.1
spring.rabbitmq.port=5672
spring.rabbitmq.username=admin
spring.rabbitmq.password=123456
```

### 队列配置
```
@Configuration
public class RabbitMQConfig {

    @Bean
    public Queue Queue(){
        //org.springframework.amqp.core.Queue;
        return new Queue("hello");
    }
}
```

### Sender
```
@Component
public class TestSender {
    @Autowired
    private AmqpTemplate rabbitTemplate;

    public void send(String message){
        String context = message + "  " + new Date();
        System.out.println("Sender:" + context);
        this.rabbitTemplate.convertAndSend("hello",context);
    }

}
```

### Receiver
```
@Component
@RabbitListener(queues = "hello")
public class TestReceiver {
    @RabbitHandler
    public void handler(String message){
        System.out.println("Receiver:  You say :" + message);
    }
}
```

### Test
```
@RunWith(SpringRunner.class)
@SpringBootTest
public class TestRabbitMQ {
    @Autowired
    private TestSender ts;

    @Test
    public void hello() throws Exception{
        ts.send("Hello,Hiki");
    }
}
```
**注意Sender和Receiver用的queue要一样**
### 一对多
多加两个Receiver进行测试，结果为轮流分配给Receiver
```
    @Test
    //一对多测试,结果为轮流分配给Receiver
    public void one2many()throws Exception{
        for (int i=0;i<20;i++){
            ts2.send(i + ": Hello,Hiki");
        }
    }
```

### 多对多
多加一个Sender并发送到同一个queue，结果也为轮流分配给Receiver，也就是怎么操作都是一个队列，sender发一个，receiver也只能拿一个。
```
    @Test
    //多对多测试，结果也为轮流分配给Receiver，也就是怎么操作都是一个队列，sender发一个，receiver也只能拿一个
    public void many2many() throws  Exception{
        for (int i=0;i<20;i++){
            ts2.send(i + ": Hello,Hiki");
            ts3.send(i + ": Hello,Hiki2");
        }
    }
```

### 传对象
其实就是把要传的类型换成对象就行了。

**一个Recevier只能监听一个queue（还未核实）**



  
  

> [项目代码](https://github.com/Hikiy/SpringBootLearn)

<center>(<font color=red size=2>转载文章请注明作者和出处 </font><a href="https://github.com/Hikiy">Hiki)</a></center>  
