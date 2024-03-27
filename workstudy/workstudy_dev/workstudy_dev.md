# 1、盘古踩坑积累





## 1.1、SpringBoot集成LayUI

在使用SpringBoot集成LayUI框架时，引入了`Thymeleaf`语法，遇到一个解析异常（`Could not parse as expression`）的错误。

LayUI 中的表格会经常性使用到`[[...]]`的表项，但是在`Thymeleaf`中会以为是要解析两个方括号里面的变量，就会造成冲突。

- 解决办法：禁用内联 `th:inline="none"`

> 参考：
>
> - [关于Could not parse as expression的报错](https://blog.csdn.net/sun8112133/article/details/107339009)
>
> - [Thymeleaf之内](https://sunkuan.blog.csdn.net/article/details/106991754)







## 1.2、IDEA连接MySQL8.0驱动无法下载问题

连接梯子下载，若是下载不成功需要手动下载jar包

1. 打开 cmd -> `mysql --version`，检查 mysql 版本

2. 进入`https://downloads.mysql.com/archives/c-j/` 下载对应 jar 包

![](workstudy_dev.assets/1.png)



3. [将jar包放入IDEA步骤](https://zhuanlan.zhihu.com/p/611646634)



> 参考：
>
> - [mysql-connector-java-8.0.26-bin.jar 包含bin的jar下载](https://blog.csdn.net/yxw22/article/details/120153117)







## 1.3、MyBatisX插件的配置信息

![](workstudy_dev.assets/2.png)



![](workstudy_dev.assets/3.png)







![](workstudy_dev.assets/4.png)





## 1.4、Springboot实体类配置Data类型指定格式

springboot项目中，如果声明一个@RestController 接口，如果返回有 Date格式的话，则前端通过http接收到的格式默认为`Tue May 24 2022 17:49:42`

![](workstudy_dev.assets/5.png)

我们要指定给前端返回的数据格式，有两种办法：

1. 第一种方法:在实体类的Data字段上加注解：` @JsonFormat(pattern="yyyy-MM-dd HH:mm:ss")`

```java
//配置日期写入格式 datatime:yyyy-MM-dd HH:mm:ss
@JsonFormat(pattern="yyyy-MM-dd HH:mm:ss")
private Date studentBirthday;
```



2. 第二种方法:在`application.yml`里面全局jackson序列化格式:

```yaml
spring:
  jackson:
    date-format: yyyy-MM-dd HH:mm:ss
    time-zone: GMT+8
```





## 1.5、关于IDEA2022开启热部署

1. `pom.xml`文件引入依赖

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-devtools</artifactId>
    <optional>true</optional>
</dependency>
```



2. 构建项目

![](workstudy_dev.assets/6.png)













