# 1、💪

## 1.1、什么是Spring框架

Spring 是一款开源的轻量级 Java 开发框架，旨在提高开发人员的开发效率以及系统的可维护性。我们一般说 Spring 框架指的都是 Spring Framework，它是很多模块的集合，使用这些模块可以很方便地协助我们进行开发，比如说 Spring 支持 IoC（Inversion of Control:控制反转） 和 AOP(Aspect-Oriented Programming:面向切面编程)、可以很方便地对数据库进行访问、可以很方便地集成第三方组件（电子邮件，任务，调度，缓存等等）、对单元测试支持比较好、支持 RESTful Java 应用程序的开发。

Spring 最核心的思想就是不重新造轮子，开箱即用，提高开发效率。





## 1.2、Spring、SpringMVC、SpringBoot三者关系

Spring 包含了多个功能模块，其中最重要的是 Spring-Core（主要提供 IoC 依赖注入功能的支持） 模块， Spring 中的其他模块（比如 Spring MVC）的功能实现基本都需要依赖于该模块。

Spring MVC 是 Spring 中的一个很重要的模块，主要赋予 Spring 快速构建 MVC 架构的 Web 程序的能力。MVC 是模型(Model)、视图(View)、控制器(Controller)的简写，其核心思想是通过将业务逻辑、数据、显示分离来组织代码

使用 Spring 进行开发各种配置过于麻烦比如开启某些 Spring 特性时，需要用 XML 或 Java 进行显式配置。于是，Spring Boot 诞生了！

Spring 旨在简化 J2EE 企业应用程序开发。Spring Boot 旨在简化 Spring 开发（减少配置文件，开箱即用！）。Spring Boot 只是简化了配置，如果你需要构建 MVC 架构的 Web 程序，你还是需要使用 Spring MVC 作为 MVC 框架，只是说 Spring Boot 帮你简化了 Spring MVC 的很多配置，真正做到开箱即用！



## 1.3、谈谈对于Spring IoC 和 DI 的理解

**IoC（Inversion of Control:控制反转）** 是一种设计思想，而不是一个具体的技术实现。IoC 的思想就是将原本在程序中手动创建对象的控制权，交由 Spring 框架来管理。不过， IoC 并非 Spring 特有，在其他语言中也有应用

**为什么叫控制反转？**

- **控制** ：指的是对象创建（实例化、管理）的权力
- **反转** ：控制权交给外部环境（Spring 框架、IoC 容器）

![](Java面试(四).assets/1.png)

将对象之间的相互依赖关系交给 IoC 容器来管理，并由 IoC 容器完成对象的注入。这样可以很大程度上简化应用的开发，把应用从复杂的依赖关系中解放出来。 IoC 容器就像是一个工厂一样，当我们需要创建一个对象的时候，只需要配置好配置文件/注解即可，完全不用考虑对象是如何被创建出来的。

在实际项目中一个 Service 类可能依赖了很多其他的类，假如我们需要实例化这个 Service，你可能要每次都要搞清这个 Service 所有底层类的构造函数，这可能会把人逼疯。如果利用 IoC 的话，你只需要配置好，然后在需要的地方引用就行了，这大大增加了项目的可维护性且降低了开发难度。

在 Spring 中， IoC 容器是 Spring 用来实现 IoC 的载体， IoC 容器实际上就是个 Map（key，value），Map 中存放的是各种对象。



**DI：DI—Dependency** Injection，即"依赖注入"：组件之间依赖关系由容器在运行期决定，形象的说，即由容器动态的将某个依赖关系注入到组件之中。依赖注入的目的并非为软件系统带来更多功能，而是为了提升组件重用的频率，并为系统搭建一个灵活、可扩展的平台。通过依赖注入机制，我们只需要通过简单的配置，而无需任何代码就可指定目标需要的资源，完成自身的业务逻辑，而不需要关心具体的资源来自何处，由谁实现。







## 1.4、什么是依赖注入？可以通过多少种方式完成依赖注入

在依赖注入中，您不必创建对象，但必须描述如何创建它们。您不是直接在代码中将组件和服务连接在一起，而是描述配置文件中哪些组件需要哪些服务。由 IoC 容器将它们装配在一起。

通常，依赖注入可以通过三种方式完成，即：

- 构造函数注入
- setter 注入
- 接口注入

在 Spring Framework 中，仅使用构造函数和 setter 注入。

| 构造函数注入               | setter注入                 |
| -------------------------- | -------------------------- |
| 没有部分注入               | 有部分注入                 |
| 不会覆盖setter属性         | 会覆盖setter属性           |
| 任意修改都会创建一个新实例 | 任意修改不会创建一个新实例 |
| 适用于设置很多属性         | 适用于设置少量属性         |









## 1.5、什么是Spring Bean

简单来说，Bean 代指的就是那些被 IoC 容器所管理的对象。我们需要告诉 IoC 容器帮助我们管理哪些对象，这个是通过配置元数据来定义的。配置元数据可以是 XML 文件、注解或者 Java 配置类。



## 1.6、将一个类声明为Bean的注解有哪些

我们一般使用 @Autowired 注解自动装配 bean，要想把类标识成可用于 @Autowired 注解自动装配的 bean 的类,采用以下注解可实现

- `@Component` ：通用的注解，可标注任意类为 `Spring` 组件。如果一个 Bean 不知道属于哪个层，可以使用`@Component` 注解标注。

- `@Repository` : 对应持久层即 Dao 层，主要用于数据库相关操作。

- `@Service` : 对应服务层，主要涉及一些复杂的逻辑，需要用到 Dao 层。

- `@Controller` : 对应 Spring MVC 控制层，主要是接受用户请求并调用 Service 层返回数据给前端页面



## 1.7、@Component和@Bean的区别是什么

- `@Component` 注解作用于类，而`@Bean`注解作用于方法。

- `Component`通常是通过类路径扫描来自动侦测以及自动装配到 Spring 容器中（我们可以使用 `@ComponentScan` 注解定义要扫描的路径从中找出标识了需要装配的类自动装配到 Spring 的 bean 容器中）。`@Bean` 注解通常是我们在标有该注解的方法中定义产生这个 bean,`@Bean`告诉了 Spring 这是某个类的实例，当我需要用它的时候还给我。

- `@Bean` 注解比 `@Component` 注解的自定义性更强，而且很多地方我们只能通过 `@Bean` 注解来注册 bean。比如当我们引用第三方库中的类需要装配到 `Spring`容器时，则只能通过 `@Bean`来实现

  



## 1.8、注入Bean的注解有哪些

`@Autowired` 和`@Resource`使用的比较多一些。



## 1.9、@Autowired和@Resource的区别是什么

`Autowired` 属于 Spring 内置的注解，默认的注入方式为`byType`（根据类型进行匹配），也就是说会优先根据接口类型去匹配并注入 Bean （接口的实现类）

**这会有什么问题呢？** 当一个接口存在多个实现类的话，`byType`这种方式就无法正确注入对象了，因为这个时候 Spring 会同时找到多个满足条件的选择，默认情况下它自己不知道选择哪一个。这种情况下，注入方式会变为 `byName`（根据名称进行匹配），这个名称通常就是类名（首字母小写）。就比如说下面代码中的 `smsService` 就是我这里所说的名称，这样应该比较好理解了吧。

```java
// smsService 就是我们上面所说的名称
@Autowired
private SmsService smsService;
```



举个例子，`SmsService` 接口有两个实现类: `SmsServiceImpl1`和 `SmsServiceImpl2`，且它们都已经被 Spring 容器所管理。

```java
// 报错，byName 和 byType 都无法匹配到 bean
@Autowired
private SmsService smsService;

// 正确注入 SmsServiceImpl1 对象对应的 bean
@Autowired
private SmsService smsServiceImpl1;


// 正确注入  SmsServiceImpl1 对象对应的 bean
// smsServiceImpl1 就是我们上面所说的名称
@Autowired
@Qualifier(value = "smsServiceImpl1")
private SmsService smsService;
```

我们还是建议通过 `@Qualifier` 注解来显式指定名称而不是依赖变量的名称。

`@Resource`属于 JDK 提供的注解，默认注入方式为 `byName`。如果无法通过名称匹配到对应的 Bean 的话，注入方式会变为`byType`。

`@Resource` 有两个比较重要且日常开发常用的属性：`name`（名称）、`type`（类型）。如果仅指定 `name` 属性则注入方式为`byName`，如果仅指定`type`属性则注入方式为`byType`，如果同时指定`name` 和`type`属性（不建议这么做）则注入方式为`byType`+`byName`。

```java
// 报错，byName 和 byType 都无法匹配到 bean
@Resource
private SmsService smsService;

// 正确注入 SmsServiceImpl1 对象对应的 bean
@Resource
private SmsService smsServiceImpl1;

// 正确注入 SmsServiceImpl1 对象对应的 bean（比较推荐这种方式）
@Resource(name = "smsServiceImpl1")
private SmsService smsService;
```

总结：

- `@Autowired` 是 Spring 提供的注解，`@Resource` 是 JDK 提供的注解。

- `Autowired` 默认的注入方式为`byType`（根据类型进行匹配），`@Resource`默认注入方式为 `byName`（根据名称进行匹配）

- 当一个接口存在多个实现类的情况下，`@Autowired` 和`@Resource`都需要通过名称才能正确匹配到对应的 Bean。`Autowired` 可以通过 `@Qualifier` 注解来显式指定名称，`@Resource`可以通过 `name` 属性来显式指定名称。

  



## 1.10、Bean的作用域有哪些

Spring 中 Bean 的作用域通常有下面几种：

- **singleton** : IoC 容器中只有唯一的 bean 实例。Spring 中的 bean 默认都是单例的，是对单例设计模式的应用
- **prototype** : 每次获取都会创建一个新的 bean 实例。也就是说，连续 `getBean()` 两次，得到的是不同的 Bean 实例
- **request** （仅 Web 应用可用）: 每一次 HTTP 请求都会产生一个新的 bean（请求 bean），该 bean 仅在当前 HTTP request 内有效。
- **session** （仅 Web 应用可用） : 每一次来自新 session 的 HTTP 请求都会产生一个新的 bean（会话 bean），该 bean 仅在当前 HTTP session 内有效
- **application/global-session** （仅 Web 应用可用）： 每个 Web 应用在启动时创建一个 Bean（应用 Bean），该 bean 仅在当前应用启动时间内有效
- **websocket** （仅 Web 应用可用）：每一次 WebSocket 会话产生一个新的 bean

如何配置 bean 的作用域呢？

xml方式：

```xml
<bean id="..." class="..." scope="singleton"></bean>
```

注解方式：

```java
@Bean
@Scope(value = ConfigurableBeanFactory.SCOPE_PROTOTYPE)
public Person personPrototype() {
    return new Person();
}
```





## 1.11、单例Bean的线程安全问题了解吗

大部分时候我们并没有在项目中使用多线程，所以很少有人会关注这个问题。单例 Bean **存在线程问题**，主要是因为当多个线程操作同一个对象的时候是存在资源竞争的。

常见的有两种解决办法：

1. 在 Bean 中尽量避免定义可变的成员变量
2. 在类中定义一个 `ThreadLocal` 成员变量，将需要的可变成员变量保存在 `ThreadLocal` 中（推荐的一种方式）。

不过，大部分 Bean 实际都是无状态（没有实例变量）的（比如 Dao、Service），这种情况下， Bean 是线程安全的



















## 1.12、谈谈对于AOP的了解

AOP(Aspect-Oriented Programming:面向切面编程)能够将那些与业务无关，却为业务模块所共同调用的逻辑或责任（例如事务处理、日志管理、权限控制等）封装起来，便于减少系统的重复代码，降低模块间的耦合度，并有利于未来的可拓展性和可维护性。

Spring AOP 就是基于动态代理的，如果要代理的对象，实现了某个接口，那么 Spring AOP 会使用 **JDK Proxy**，去创建代理对象，而对于没有实现接口的对象，就无法使用 JDK Proxy 去进行代理了，这时候 Spring AOP 会使用 **Cglib** 生成一个被代理对象的子类来作为代理



## 1.13、AOP有哪些实现方式

实现 AOP 的技术，主要分为两大类：

- 静态代理 - 指使用 AOP 框架提供的命令进行编译，从而在编译阶段就可生成 AOP 代理类，因此也称为编译时增强
- 动态代理 - 在运行时在内存中“临时”生成 AOP 动态代理类，因此也被称为运行时增强
  - `JDK` 动态代理：通过反射来接收被代理的类，并且要求被代理的类必须实现一个接口 。JDK 动态代理的核心是 InvocationHandler 接口和 Proxy 类
  - `CGLIB`动态代理： 如果目标类没有实现接口，那么 `Spring AOP` 会选择使用 `CGLIB` 来动态代理目标类 。`CGLIB` （ Code Generation Library ），是一个代码生成的类库，可以在运行时动态的生成某个类的子类，注意， `CGLIB` 是通过继承的方式做的动态代理，因此如果某个类被标记为 `final` ，那么它是无法使用 `CGLIB` 做动态代理的





## 1.14、Spring AOP 和 AspectJ AOP 有什么区别

**Spring AOP 属于运行时增强，而 AspectJ 是编译时增强。** Spring AOP 基于代理(Proxying)，而 AspectJ 基于字节码操作。

Spring AOP 已经集成了 AspectJ ，AspectJ 应该算的上是 Java 生态系统中最完整的 AOP 框架了。AspectJ 相比于 Spring AOP 功能更加强大，但是 Spring AOP 相对来说更简单。如果我们的切面比较少，那么两者性能差异不大。但是，当切面太多的话，最好选择 AspectJ ，它比 Spring AOP 快很多





## 1.15、AspectJ定义的通知类型有哪些

- **Before**（前置通知）：目标对象的方法调用之前触发

- **After** （后置通知）：目标对象的方法调用之后触发

- **AfterReturning**（返回通知）：目标对象的方法调用完成，在返回结果值之后触发

- **AfterThrowing**（异常通知） ：目标对象的方法运行中抛出 / 触发异常后触发。AfterReturning 和 AfterThrowing 两者互斥。如果方法调用成功无异常，则会有返回值；如果方法抛出了异常，则不会有返回值。

- **Around** （环绕通知）：编程式控制目标对象的方法调用。环绕通知是所有通知类型中可操作范围最大的一种，因为它可以直接拿到目标对象，以及要执行的方法，所以环绕通知可以任意的在目标对象的方法调用前后搞事，甚至不调用目标对象的方法

  







## 1.16、谈谈Spring对事务的支持

Spring支持两种方式的事务管理：

1. 编程式事务管理：通过 `TransactionManager` 手动管理事务，实际应用中很少使用
2. 声明式事务管理：使用 `@Transactional`注解进行事务管理，实际是通过 AOP 实现（常用），这种方式意味着你可以将事务管理和业务代码分离。你只需要通过注解或者XML配置管理事务。









## 1.17、@Transactional注解使用详解

`@Transactional` 的作用范围

1. **方法** ：推荐将注解使用于方法上，不过需要注意的是：**该注解只能应用到 public 方法上，否则不生效**
2. **类** ：如果这个注解使用在类上的话，表明该注解对该类中所有的 public 方法都生效
3. **接口** ：不推荐在接口上使用



























## 1.18、说说自己对于SpringMVC了解

MVC 是模型(Model)、视图(View)、控制器(Controller)的简写，其核心思想是通过将业务逻辑、数据、显示分离来组织代码。

Spring MVC 可以帮助我们进行更简洁的 Web 层的开发，并且它天生与 Spring 框架集成。Spring MVC 下我们一般把后端项目分为 Service 层（处理业务）、Dao 层（数据库操作）、Entity 层（实体类）、Controller 层(控制层，返回数据给前台页面)。





## 1.19、Spring MVC的核心组件有哪些？

- **`DispatcherServlet`** ：**核心的中央处理器**，负责接收请求、分发，并给予客户端响应。
- **`HandlerMapping`** ：**处理器映射器**，根据 uri 去匹配查找能处理的 `Handler` ，并会将请求涉及到的拦截器和 `Handler` 一起封装。
- **`HandlerAdapter`** ：**处理器适配器**，根据 `HandlerMapping` 找到的 `Handler` ，适配执行对应的 `Handler`
- **`Handler`** ：**请求处理器**，处理实际请求的处理器
- **`ViewResolver`** ：**视图解析器**，根据 `Handler` 返回的逻辑视图 / 视图，解析并渲染真正的视图，并传递给 `DispatcherServlet` 响应客户端





## 1.20、SpringMVC的工作原理

![](Java面试(四).assets/2.png)



流程：

1. 客户端（浏览器）发送请求， `DispatcherServlet`拦截请求

2. `DispatcherServlet` 根据请求信息调用 `HandlerMapping` 。`HandlerMapping` 根据 uri 去匹配查找能处理的 `Handler`（也就是我们平常说的 `Controller` 控制器） ，并会将请求涉及到的拦截器和 `Handler` 一起封装

3. `DispatcherServlet` 调用 `HandlerAdapter`适配执行 `Handler` 。

4. `Handler` 完成对用户请求的处理后，会返回一个 `ModelAndView` 对象给`DispatcherServlet`，`ModelAndView` 顾名思义，包含了数据模型以及相应的视图的信息。`Model` 是返回的数据对象，`View` 是个逻辑上的 `View`。

5. `ViewResolver` 会根据逻辑 `View` 查找实际的 `View`。

6. `DispaterServlet` 把返回的 `Model` 传给 `View`（视图渲染）

7. 把 `View` 返回给请求者（浏览器）

   

   

## 1.21、统一异常处理怎么做

推荐使用注解的方式统一异常处理，具体会使用到 `@ControllerAdvice` + `@ExceptionHandler` 这两个注解



## 1.22、SpringMVC常用注解有哪些

- @RequestMapping：用于处理请求 url 映射的注解，可用于类或方法上。用于类上，则表示类中的所有响应请求的方法都是以该地址作为父路径
- @RequestBody：注解实现接收 HTTP 请求的 json 数据，将 json 转换为 Java 对象
- @ResponseBody：注解实现将 Controller 方法返回对象转化为 json 对象响应给客户



## 1.23、@Controller注解有什么用

`@Controller` 注解标记一个类为 Spring Web MVC **控制器** Controller。Spring MVC 会将扫描到该注解的类，然后扫描这个类下面带有 `@RequestMapping` 注解的方法，根据注解信息，为这个方法生成一个对应的**处理器**对象

当然，除了添加 `@Controller` 注解这种方式以外，你还可以实现 Spring MVC 提供的 `Controller` 或者 `HttpRequestHandler` 接口，对应的实现类也会被作为一个**处理器**对象



## 1.24、@RequestMapping注解有什么用

`@RequestMapping` 注解，配置**处理器**的 HTTP 请求方法，URI等信息，这样才能将请求和方法进行映射。这个注解可以作用于类上面，也可以作用于方法上面，在类上面一般是配置这个**控制器**的 URI 前缀



## 1.25、@RestController和@Controller有什么区别

`@RestController` 注解，在 `@Controller` 基础上，增加了 `@ResponseBody` 注解，更加适合目前前后端分离的架构下，提供 Restful API ，返回例如 JSON 数据格式。当然，返回什么样的数据格式，根据客户端的 `ACCEPT` 请求头来决定。



## 1.26、@RequestMapping和@GetMapping注解的不同之处在哪里

- `@RequestMapping`：可注解在类和方法上；`@GetMapping` 仅可注册在方法上
- `@RequestMapping`：可进行 GET、POST、PUT、DELETE 等请求方法；`@GetMapping` 是 `@RequestMapping` 的 GET 请求方法的特例，目的是为了提高清晰度。



## 1.27、@RequestParam和@PathVariable两个注解的区别

两个注解都用于方法参数，获取参数值的方式不同，`@RequestParam` 注解的参数从请求携带的参数中获取，而 `@PathVariable` 注解从请求的 URI 中获取



## 1.28、返回JSON格式用什么注解

可以使用 **`@ResponseBody`** 注解，或者使用包含 `@ResponseBody` 注解的 **`@RestController`** 注解。







## 1.29、什么是SpringMVC的拦截器以及如何使用它

拦截器必须实现 HandlerInterceptor 接口，此接口定义了三种方法：

- preHandle：在执行实际处理程序之前调用。
- postHandle：在执行完实际程序之后调用。
- afterCompletion：在完成请求后调用。





## 1.30、如何解决POST请求中文乱码问题，GET的又如何处理

- 解决 POST 请求乱码问题：在 web.xml 中配置一个 CharacterEncodingFilter 过滤器，设置成 utf-8
- GET 请求中文参数出现乱码解决方法有两个
  - 修改 tomcat 配置文件添加编码与工程编码一致
  - 对参数进行重新编码





## 1.31、SpringMVC的控制器是不是单例模式，如果是会有什么问题，怎么解决

是单例模式，所以在多线程访问的时候有线程安全问题。但是不要使用同步，会影响性能，解决方案是在控制器里面不能写字段。



## 1.32、SpringMVC怎么样设定重定向和转发

- 转发：在返回值前面加 “forward:”
- 重定向：在返回值前面加 “redirect:”







## 1.33、SpringBoot有哪些优点

- 独立运行：Spring Boot 而且内嵌了各种 servlet 容器，Tomcat、Jetty 等，现在不再需要打成war 包部署到容器中，Spring Boot 只要打成一个可执行的 jar 包就能独立运行，所有的依赖包都在一个 jar 包内
- 简化配置：spring-boot-starter-web 启动器自动依赖其他组件，简少了 maven 的配置。
- 自动配置：Spring Boot 能根据当前类路径下的类、jar 包来自动配置 bean，如添加一个 spring-boot-starter-web 启动器就能拥有 web 的功能，无需其他配置。
- 无代码生成和XML配置：Spring Boot 配置过程中无代码生成，也无需 XML 配置文件就能完成所有配置工作，这一切都是借助于条件注解完成的

SpringBoot的**缺点**是入门简单精通难，各种强大的功能封装的太好了，内部原理比较难得参透！再就是用多了容易产生依赖，就像嗑药似的，用了就离不开了；SpringBoot一旦出了错误，由于内部封装比较深，部分错误调试难度比一般Spring应用程序要大很多！



## 1.34、SpringBoot的配置文件有哪几种格式？它们有什么区别？

.properties 和 .yml，它们的区别主要是书写格式不同

1. .properties

```properties
app.user.name = javastack
```

2. .yml

```yaml
app: user: name: javastack
```

另外，.yml 格式不支持 `@PropertySource` 注解导入配置。



## 1.35、SpringBoot的核心注解是哪个？它主要由哪几个注解组成的

启动类上面的注解是@SpringBootApplication，它也是 Spring Boot 的核心注解，主要组合包含了以下 3 个注解

- @**SpringBootConfiguration**：组合了 @Configuration 注解，实现配置文件的功能
- @**EnableAutoConfiguration**：打开自动配置的功能，也可以关闭某个自动配置的选项
- @**ComponentScan**：Spring组件扫描







## 1.36、SpringBoot如何实现对不同环境的属性配置文件的支持

Spring Boot支持不同环境的属性配置文件切换，通过创建`application-{profile}.properties`文件，其中{profile}是具体的环境标识名称。例如：

- `application-dev.properties` 用于开发环境
- `application-test.properties` 用于测试环境
- `application-uat.properties` 用于 uat 环境
- 如果要想使用 `application-dev.properties` 文件，则在 `application.properties` 文件中添加 `spring.profiles.active=dev`
- 如果想要使用 `application-test.properties` 文件，则在 `application.properties` 文件中添加 `spring.profiles.active=test`





## 1.37、YAML配置的优势在哪里

YAML 现在可以算是非常流行的一种配置文件格式了，无论是前端还是后端，都可以见到 YAML 配置。那么 YAML 配置和传统的 properties 配置相比到底有哪些优势呢？

1. 配置有序，在一些特殊的场景下，配置有序很关键
2. 支持数组，数组中的元素可以是基本数据类型也可以是对象
3. 简洁

相比 properties 配置文件，YAML 还有一个缺点，就是不支持 @PropertySource 注解导入自定义的 YAML 配置。



## 1.38、如何在自定义端口上运行SpringBoot应用程序

SpringBoot默认监听的是8080端口；为了在自定义端口上运行 SpringBoot 应用程序，您可以在application.properties 中通过

```properties
server.port = 8888
```

指定端口；这样就可以将监听的端口修改为8888。





## 1.39、Spring、SpringBoot和SpringCloud的关系

- Spring 最初最核心的两大核心功能 Spring Ioc 和 Spring Aop 成就了 Spring，Spring 在这两大核心的功能上不断的发展，才有了 Spring 事务、Spring Mvc 等一系列伟大的产品，最终成就了 Spring 帝国，到了后期 Spring 几乎可以解决企业开发中的所有问题
- Spring Boot 是在强大的 Spring 帝国生态基础上面发展而来，发明 Spring Boot 不是为了取代 Spring ,是为了让人们更容易的使用 Spring 。
- Spring Cloud 是一系列框架的有序集合。它利用 Spring Boot 的开发便利性巧妙地简化了分布式系统基础设施的开发，如服务发现注册、配置中心、消息总线、负载均衡、断路器、数据监控等，都可以用 Spring Boot 的开发风格做到一键启动和部署。

Spring Cloud 是为了解决微服务架构中服务治理而提供的一系列功能的开发框架，并且 Spring Cloud 是完全基于 Spring Boot 而开发，Spring Cloud 利用 Spring Boot 特性整合了开源行业中优秀的组件，整体对外提供了一套在微服务架构中服务治理的解决方案。



## 1.40、SpringBoot中POM文件的作用

在Spring Boot工程中，pom.xml文件是[maven](https://so.csdn.net/so/search?q=maven&spm=1001.2101.3001.7020)项目的配置基础，它让我们的项目自动导入了很多相关的依赖。pom.xml里面会存放很多的标签，





## 1.42、SpringBoot应用jar包和普通可执行jar包的区别

Spring Boot 中默认打包成的 jar 叫做 可执行 jar，这种 jar 不同于普通的 jar，普通的 jar 不可以通过 `java -jar xxx.jar` 命令执行，普通的 jar 主要是被其他应用依赖，Spring Boot 打成的 jar 可以执行，但是不可以被其他的应用所依赖，即使强制依赖，也无法获取里边的类。但是可执行 jar 并不是 Spring Boot 独有的，Java 工程本身就可以打包成可执行 jar 。



## 1.43、使用过SpringCloud吗，谈谈你对微服务的理解

SpringCloud是Spring为微服务架构思想做的一个一站式实现。微服务没有一个标准统一的概念，个人理解：微服务是一种可以让软件职责单一、松耦合、自包含、可以独立运行和部署的架构思想。

关键思想：拆分、单一、独立、组件化。把原本一个庞大、复杂的项目按业务边界拆分一个一个独立运行的小项目，通过接口的方式组装成一个大的项目。

SpringCloud是基于SpringBoot的一套实现微服务的框架。它提供了微服务开发所需的配置管理、服务发现、断路器、智能路由、微代理、控制总线、全局锁、决策竞选、分布式会话和集群状态管理等组件。最重要的是，跟SpringBoot框架一起使用的话，会让你开发微服务架构的云服务非常方便。





## 1.44、SpringBoot的核心配置文件有哪几个？它们的区别是什么

Spring Boot 的核心配置文件是 application 和 bootstrap 配置文件。

application 配置文件这个容易理解，主要用于 Spring Boot 项目的自动化配置。

bootstrap 配置文件有以下几个应用场景：

- 使用 Spring Cloud Config 配置中心时，这时需要在 bootstrap 配置文件中添加连接到配置中心的配置属性来加载外部配置中心的配置信息
- 一些固定的不能被覆盖的属性
- 一些加密/解密的场景



> bootstrap.properties和application.properties 有何区别 ?

SpringBoot两个核心的配置文件：

- **bootstrap**(.yml 或者 .properties)：boostrap 由父 ApplicationContext 加载的，比applicaton优先加载，配置在应用程序上下文的引导阶段生效。一般来说我们在 SpringCloud Config 或者Nacos中会用到它。且boostrap里面的属性不能被覆盖
- **application** (.yml或者.properties)：由ApplicatonContext 加载，用于 SpringBoot项目的自动化配置





## 1.45、开启SpringBoot特性有哪几种方式

1. 继承spring-boot-starter-parent项目
2. 导入spring-boot-dependencies项目依赖



## 1.46、SpringBoot自动配置原理是什么

1. SpringBoot启动会加载大量的自动配置类
2. 我们看我们需要的功能有没有在SpringBoot默认写好的自动配置类当中，我们再来看这个自动配置类中到底配置了哪些组件；（只要我们要用的组件存在在其中，我们就不需要再手动配置了）
3. 给容器中自动配置类添加组件的时候，会从properties类中获取某些属性。我们只需要在配置文件中指定这些属性的值即可



## 1.47、SpringBoot 需要独立的容器运行吗

可以不需要，内置了 Tomcat/ Jetty 等容器。



## 1.48、SpringBoot如何解决跨域问题

跨域可以在前端通过 JSONP 来解决，但是 JSONP 只可以发送 GET 请求，无法发送其他类型的请求，在 RESTful 风格的应用中，就显得非常鸡肋

因此我们推荐在后端通过 （CORS，Cross-origin resource sharing） 来解决跨域问题，可以通过实现WebMvcConfigurer接口然后重写addCorsMappings方法解决跨域问题。

项目中前后端分离部署，所以需要解决跨域的问题。我们使用cookie存放用户登录的信息，在spring拦截器进行权限控制，当权限不符合时，直接返回给用户固定的json结果。当用户登录以后，正常使用；当用户退出登录状态时或者token过期时，由于拦截器和跨域的顺序有问题，出现了跨域的现象。我们知道一个http请求，先走filter，到达servlet后才进行拦截器的处理，如果我们把cors放在filter里，就可以优先于权限拦截器执行。





## 1.49、SpringBoot项目如何实现热部署

Spring Boot 有一个开发工具（DevTools）模块，它有助于提高开发人员的生产力。Java 开发人员面临的一个主要挑战是将文件更改自动部署到服务器并自动重启服务器。开发人员可以重新加载 Spring Boot 上的更改，而无需重新启动服务器。DevTools 模块完全满足开发人员的需求。该模块将在生产环境中被禁用。







## 1.50、如何使用SpringBoot实现异常处理

Spring 提供了一种使用 ControllerAdvice 处理异常的非常有用的方法。 我们通过实现一个 ControlerAdvice 类，来处理控制器类抛出的所有异常。





## 1.51、微服务中如何实现session共享

在微服务中，一个完整的项目被拆分成多个不相同的独立的服务，各个服务独立部署在不同的服务器上，各自的 session 被从物理空间上隔离开了，但是经常，我们需要在不同微服务之间共享 session ，常见的方案就是Spring Session + Redis 来实现 session 共享。

将所有微服务的 session 统一保存在 Redis 上，当各个微服务对 session 有相关的读写操作时，都去操作 Redis 上的 session 。这样就实现了 session 共享，Spring Session 基于 Spring 中的代理过滤器实现，使得 session 的同步操作对开发人员而言是透明的，非常简便。





## 1.52、什么是WebSockets

WebSocket是一种计算机通信协议，通过单个TCP连接提供全双工通信信道。

- WebSocket是双向的 -使用 WebSocket 客户端或服务器可以发起消息发送
- WebSocket是全双工的 -客户端和服务器通信是相互独立的



## 1.53、Swagger用来做什么

Swagger广泛用于可视化API，使用SwaggerUl**为前端开发人员提供在线沙箱**。Swagger 是用于生成RESTful Web服务的可视化表示的工具，规范和完整框架实现。它**使文档能够以与服务器相同的速度更新**。当通过Swagger 正确定义时，消费者可以使用最少量的实现逻辑来理解远程服务并与其进行交互。因此，Swagger 消除了调用服务时的猜测。







## 1.54、前后端分离，如何维护接口文档

前后端分离开发日益流行，大部分情况下，我们都是通过 Spring Boot 做前后端分离开发，前后端分离一定会有接口文档，不然会前后端会深深陷入到扯皮中。

在 Spring Boot 中，这个问题常见的解决方案是 Swagger ，使用 Swagger 我们可以快速生成一个接口文档网站，接口一旦发生变化，文档就会自动更新，所有开发工程师访问这一个在线网站就可以获取到最新的接口文档，非常方便。





## 1.55、Spring Boot Starter 是什么

Spring boot之所以流行，很大原因是因为有Spring Boot Starter。Spring Boot Starter是Spring boot的核心，可以理解为一个可拔插式的插件，例如，你想使用Reids插件，那么可以导入spring-boot-starter-redis依赖

Spring官方Starter通常命名为spring-boot-starter-{name}如：spring-boot-starter-web，Spring官方建议非官方的starter命名应遵守{name}-spring-boot-starter的格式。

**使用Spring Boot Starter可以提升效率**：

- 帮助用户去除了繁琐的重复性的构建操作
- 在“约定大于配置”的理念下，ConfigurationProperties还帮助用户减少了无谓的配置操作
- 因为 `application.properties` 文件的存在，用户可以集中管理自定义配置

































































