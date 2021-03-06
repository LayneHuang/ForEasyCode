---
title: Spring Boot Tips
date: 2020-05-29 19:57:31
categories: Spring
---

### 1.Controller 后端返回的值与 bean 定义的名称不一致
Bean 中定义了值 ：  
```java
@Data
public class MyDto {
    private XAxis xAxis;  
}
```

并且在 Controller 中返回：
```java
@RestController
@RequestMapping("/my")
public class MyController {
    @GetMapping
    public BaseResponse<MyDto> check(@RequestBody @Validated Req req) {
        return service.get(req);
    }
}
```

但是在前端得到的 json 中 xAxis 却被全部转换成小字母( xaxis )  

**解决方法**  
定义时加上注解 @JsonProperty
````java
public class MyDto {
    @JsonProperty(value = "xAxis")
    private XAxis xAxis;  
}
````

### 2.Bean XML 配置中 constructor-arg 和 property 的区别
constructor-arg：通过构造函数注入。   
property：通过setter对应的方法注入。

### 3.Spring MVC 中 Java Bean 如何处理
{% img /images/spring_java_bean.png %}

### 4.注解校验
{% link 'hibernate文档' https://docs.jboss.org/hibernate/validator/4.2/reference/zh-CN/html_single/#validator-usingvalidator-annotate %}

### 5.Controller 上传文件大小受限
如果需要接口支持文件上传的话，只要在 Controller 上加上 MultipartFile 的参数即可
```
@RequestParam(required = false) MultipartFile multipartFile
```
但是文件过大时(大于 Spring Boot 的默认配置的 1048576 bytes), 就会报异常 FileSizeLimitExceededException。  
修改一下 Spring 配置文件 (设置成 -1 为不受限制):
```properties
spring.servlet.multipart.max-file-size=100MB
spring.servlet.multipart.max-request-size=1000MB
```

### 6.StopWatch 的用法
StopWatch 这个工具可以帮助开发者记录每一步任务的耗时  
要注意的是, StopWatch在下一步任务开始前，先要调用一次stop()
```
StopWatch sw = new StopWatch("teskName");
sw.start("step1");
sw.stop();
sw.start("step2");
sw.stop();
log.info(sw.prettyPrint());
```