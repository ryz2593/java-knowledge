在springMVC里使用spring的定时任务，需要以下步骤即可轻松实现
## （一）在xml里加入task的命名空间

```sql
xmlns:task="http://www.springframework.org/schema/task"
http://www.springframework.org/schema/task http://www.springframework.org/schema/task/spring-task.xsd"
```

## （二）启用注解驱动的定时任务

```yaml
<task:annotation-driven scheduler="myScheduler"/> 
```

## （三）配置定时任务的线程池

推荐配置线程池，若不配置多任务下会有问题。

```powershell
<task:scheduler id="myScheduler" pool-size="2"/> 
```

## （四）写我们的定时任务

@Scheduled注解为定时任务，cron表达式里写执行的时机

```java

@Service
public class UserEmailServiceImpl implements UserEmailService {
    
    @Override
    public List<RegisteredUserEmail> selectValidEmail() {
        return registeredUserEmailMapper.selectValidEmail();
    }

    @Override
    //每隔30分钟执行一次
    @Scheduled(cron = "0 0/30 * * * ? ")
    public void sendEmail() {
    	//获取要发送的邮件列表
        List<RegisteredUserEmail> registeredUserEmails = selectValidEmail();
		//处理逻辑
		.....
		//发送
        AmazonSES.SendEmail(sendFrom, new String[]{userEmail}, pushEmailModel.getSubject(), html, null);
    }
}

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20191203165203358.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L21lbmdxaW5nbWluZzE=,size_16,color_FFFFFF,t_70)