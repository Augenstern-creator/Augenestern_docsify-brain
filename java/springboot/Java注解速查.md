





# 1、@Component

使用在**类**上个用于实例化 Bean，就相当于将这个类交给 Spring 管理装配了！

> [参考链接](https://augenstern-creator.gitee.io/augenestern_docsify-brain/#/java/javaee/%E4%BC%A0%E6%99%BAspring(%E4%B8%80)?id=_41%e3%80%81spring%e5%8e%9f%e5%a7%8b%e6%b3%a8%e8%a7%a3)





# 2、@Controller

使用在 web 层**类**上用于实例化 Bean，就相当于将这个类交给 Spring 管理装配了！



# 3、@Service

使用在 service 层**类**上用于实例化 Bean，就相当于将这个类交给 Spring 管理装配了！



# 4、@Repository

使用在 dao 层**类**上用于实例化 Bean，就相当于将这个类交给 Spring 管理装配了！



# 5、@Bean

使用在方法上，标注将该方法的返回值存储到 Spring。





# 6、@Value

设置对应属性的值或对方法进行传参





# 7、@Configuration

用于指定当前类是一个 Spring 配置类，当创建容器时会从该类上加载注解

> [参考链接](https://augenstern-creator.gitee.io/augenestern_docsify-brain/#/java/javaee/%E4%BC%A0%E6%99%BAspring(%E4%B8%80)?id=_47%e3%80%81spring%e6%96%b0%e6%b3%a8%e8%a7%a3)



# 8、@ComponentScan

用于指定 Spring 在初始化容器时要扫描的包。



# 9、@PropertySource

放在类上，加载 properties 文件中的属性值

```java
@PropertySource(value = "classpath:filename.properties")
public class ClassName {
    @Value("${propertiesAttributeName}")
    private String attributeName;
}
```

> [参考链接](https://augenstern-creator.gitee.io/augenestern_docsify-brain/#/java/javaee/%E4%BC%A0%E6%99%BAspring(%E4%B8%80)?id=_48%e3%80%81%e5%8a%a0%e8%bd%bdproperties%e6%96%87%e4%bb%b6)



# 10、@Param

我们一般在方法参数前使用 @Param 来设置参数名。方法中的参数一旦加入@Param注解,Mapper.xml文件的占位符必须与@Param注解的名称一致



> [参考链接](https://augenstern-creator.gitee.io/augenestern_docsify-brain/#/java/javaee/%E4%BC%A0%E6%99%BAmybatis?id=_4221%e3%80%81param%e6%b3%a8%e8%a7%a3%e5%85%a5%e5%8f%82)







# 11、resultType、parameterType

- `id`: 就是对应的 namespace 中的方法名
- `resultType`: Sql 语句执行的返回值【完整的类名或者别名】
- `parameterType`: 传入 SQL 语句的参数类型

> [参考链接](https://augenstern-creator.gitee.io/augenestern_docsify-brain/#/java/javaee/%E4%BC%A0%E6%99%BAmybatis?id=_51%e3%80%81select)





# 12、动态SQL

> [参考链接](https://augenstern-creator.gitee.io/augenestern_docsify-brain/#/java/javaee/%E4%BC%A0%E6%99%BAmybatis?id=_6%e3%80%81%e5%8a%a8%e6%80%81sql)





# 13、ResultMap













# 14、@RequestMapping

**设置请求映射规则**，用于建立请求 URL 和处理请求方法之间的对应关系，我们可以用其来设定所能匹配请求的要求。只有符合了设置的要求，请求才能被加了该注解的方法或类处理。



> [参考链接](https://augenstern-creator.gitee.io/augenestern_docsify-brain/#/java/javaee/%E4%BC%A0%E6%99%BAspringMVC?id=_61%e3%80%81requestmapping%f0%9f%94%a5)







# 15、@PathVariable

- RestFul 风格的接口一些参数是在请求路径上的。类似： /user/1 这里的 1 就是 id。
- 如果我们想获取这种格式的数据可以使用 `@PathVariable` 来实现。

```java
@Controller
public class UserController {
    @RequestMapping(value = "/user/{id}",method = RequestMethod.GET)
    public String findUserById(@PathVariable("id") Integer id){
        System.out.println("findUserById");
        System.out.println(id);
        return "/success.jsp";
    }
}
```

此时我们请求路径：http://localhost:8080/user/1，则可获取到 id 的值为 1

> [参考链接](https://augenstern-creator.gitee.io/augenestern_docsify-brain/#/java/javaee/%E4%BC%A0%E6%99%BAspringMVC(%E4%BA%8C)?id=_311%e3%80%81pathvariable%e8%8c%83%e4%be%8b%e4%b8%80%f0%9f%94%a5)

```java
@Controller
public class UserController {
    @RequestMapping(value = "/user/{id}/{name}",method = RequestMethod.GET)
    public String findUser(@PathVariable("id") Integer id,@PathVariable("name") String name){
        System.out.println("findUser");
        System.out.println(id);		
        System.out.println(name);	
        return "/success.jsp";
    }
}
```

此时我们请求路径：http://localhost:8080/user/1/qxl，则可获取到 id 的值为 1, name 的值为 qxl





# 16、@RequestBody



获取Json格式的参数，RestFul 风格的接口一些比较复杂的参数会转换成 Json 通过请求体传递过来。这种时候我们可以使用 `@RequestBody` 注解获取请求体中的数据。

> [参考链接](https://augenstern-creator.gitee.io/augenestern_docsify-brain/#/java/javaee/%E4%BC%A0%E6%99%BAspringMVC(%E4%BA%8C)?id=_311%e3%80%81pathvariable%e8%8c%83%e4%be%8b%e4%b8%80%f0%9f%94%a5)







# 17、@RequestParam

- 我们可以使用 `@RequestParam` 来获取 QueryString 格式的参数。

> [参考链接](https://augenstern-creator.gitee.io/augenestern_docsify-brain/#/java/javaee/%E4%BC%A0%E6%99%BAspringMVC(%E4%BA%8C)?id=_33%e3%80%81%e8%8e%b7%e5%8f%96querystring%e6%a0%bc%e5%bc%8f%e5%8f%82%e6%95%b0-%f0%9f%94%a5)





# 18、@DataTimeFormat

使用 @DataTimeFormat 注解实现数据转换，在实体类中的 Date 类型属性上使用该注解即可，可以自定义转换格式

```java
//pattern属性：指定转换格式
@DateTimeFormat(pattern = "yyyy-MM-dd")
private Date birthday;
```

> [参考链接](https://augenstern-creator.gitee.io/augenestern_docsify-brain/#/java/javaee/%E4%BC%A0%E6%99%BAspringMVC(%E4%BA%8C)?id=_4%e3%80%81%e7%b1%bb%e5%9e%8b%e8%bd%ac%e6%8d%a2%e5%99%a8%f0%9f%94%a5)





# 19、@ResponseBody

 `@ResponseBody` 来非常方便的把 Json 放到响应体中，只要把要转换的数据直接作为方法的返回值返回即可。SpringMVC 会帮我们把返回值转换成 json

> [参考链接](https://augenstern-creator.gitee.io/augenestern_docsify-brain/#/java/javaee/%E4%BC%A0%E6%99%BAspringMVC(%E4%BA%8C)?id=_52%e3%80%81%e6%95%b0%e6%8d%ae%e8%bd%ac%e6%8d%a2%e6%88%90json%f0%9f%94%a5)





# 20、@RestController



我们可以使用 `@RestController` 注解替换 `@Controller` 和 `@ResponseBody` 两个注解







# 21、@Mapper

如果是springboot,在启动类中使用`@MapperScan("mapper接口所在包全名")`即可，不用一个一个的在Mapper接口中加@Mapper注解。





> [参考链接](https://augenstern-creator.gitee.io/augenestern_docsify-brain/#/java/springboot/%E4%B8%89%E6%9B%B4SpringBoot(%E4%BA%8C)?id=_32%e3%80%81%e5%8a%a0%e8%bd%bdmapper%e6%8e%a5%e5%8f%a3%e6%96%b9%e5%bc%8f)

# 22、@MapperScan

> [参考链接](https://augenstern-creator.gitee.io/augenestern_docsify-brain/#/java/springboot/%E4%B8%89%E6%9B%B4SpringBoot(%E4%BA%8C)?id=_323%e3%80%81mapperscan%e6%b3%a8%e8%a7%a3)



# 23、@SpringBootTest

> [参考链接](https://augenstern-creator.gitee.io/augenestern_docsify-brain/#/java/springboot/%E4%B8%89%E6%9B%B4SpringBoot(%E4%BA%8C)?id=_2%e3%80%81%e5%8d%95%e5%85%83%e6%b5%8b%e8%af%95)







# 24、@CrossOrigin

> [参考链接](https://augenstern-creator.gitee.io/augenestern_docsify-brain/#/java/springboot/%E4%B8%89%E6%9B%B4SpringBoot(%E4%BA%8C)?id=_441%e3%80%81%e5%8a%a0%e6%b3%a8%e8%a7%a3crossorigin)









# 25、@SpringBootConfiguration

@SpringBootConfiguration 注解的作用与 @Configuration 注解相同，都是标识一个可以被组件扫描器扫描的配置类。





# 26、@SpringBootApplication

声明SpringBoot项目的启动类！









# 27、@SpringBoot

主要用于标识一个测试类，以指示该类是一个Spring Boot应用程序的测试类。


> 参考链接：https://www.quanxiaoha.com/lombok/lombok-log.html

如若加了`@Slf4j`注解使用`log.info()`等方法无效，解决如下: https://www.jianshu.com/p/da499dc30bf8

若只是Maven项目,则参考：https://juejin.cn/post/6921246140822192141



# 51、SpringBoot日志输出
























