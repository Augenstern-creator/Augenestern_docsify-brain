



# 1、文件读取FileReader



在JDK中，同样有一个FileReader类，但是并不如想象中的那样好用，于是Hutool便提供了更加便捷的FileReader类。

- `readLines()` ：从文件中读取每一行数据，返回一个 List 集合
- `readString()` ： 读取文件全部内容

```java
/**
 * 创建文件读取: new FileReader(String filePath | File file)
 * 从文件中读取每一行数据: readLines()
 * 读取文件内容: readString()
 *
 */
@Test
public void newFileReader() {
    //默认UTF-8编码，可以在构造中传入第二个参数做为编码
    FileReader fileReader = new FileReader("E:\\Code\\IDEA\\KuangStudy_Hutools\\src\\main\\resources\\1.txt");
    // 从文件中读取每一行数据
    List<String> strings = fileReader.readLines();
    strings.forEach(s -> System.out.println(s));

    // 读取文件内容
    String result = fileReader.readString();
    System.out.println(result);
}
```





# 2、文件写入FileWriter

写入文件分为追加模式和覆盖模式两类，追加模式可以用`append`方法，覆盖模式可以用`write`方法

- `append(String content)` ： 将String写入文件，追加模式

- `write(String content [,boolean isAppend])` ：写入数据到文件
  - `content` - 写入的内容
  - `isAppend` - 是否追加写入，如果是追加写入，则不覆盖，默认为覆盖

```java
/**
 * 创建文件写入类: new FileReader(String filePath | File file)
 * 写入数据到文件: write(String content)
 */
@Test
public void writeTest() {
    // 创建文件写入类链接到文件
    FileWriter writer = new FileWriter("E:\\Code\\IDEA\\KuangStudy_Hutools\\src\\main\\resources\\1.txt");
    // 覆盖模式写入 test
    File test = writer.write("test");
    // 创建文件读取类
    FileReader fileReader = new FileReader(test);
    // 读取文件内容 test
    System.out.println(fileReader.readString());
}
```

- `writeLines(List<T> list,boolean isAppend)` ：将列表写入文件
  - `list` - 列表
  - `isAppend` - 是否追加
- `writeMap(Map<?,?> map,String kvSeparator,boolean isAppend)` ：将Map写入文件，每个键值对为一行，一行中键与值之间使用kvSeparator分隔
  - `map` - Map
  - `kvSeparator` - 键和值之间的分隔符，如果传入null使用默认分隔符" = "
  - `isAppend` - 是否追加

```java
/**
 * 将列表写入文件
 */
@Test
public void writeLinesTest() {
    ArrayList<String> arrayList = new ArrayList<>();
    arrayList.add("a");
    arrayList.add("b");
    arrayList.add("c");
    arrayList.add("d");

    // 创建文件写入类链接到文件
    FileWriter writer = new FileWriter("E:\\Code\\IDEA\\KuangStudy_Hutools\\src\\main\\resources\\1.txt");
    // 采用追加模式将列表写入文件
    File file = writer.writeLines(arrayList, true);
    // 创建文件读取类链接到文件
    FileReader fileReader = new FileReader(file);
    // 读取文件内容
    System.out.println(fileReader.readString());


}
```

























# 3、资源工具ResourceUtil

`ResourceUtil`提供了资源快捷读取封装。

举个例子，假设我们在classpath下放了一个`test.xml`，读取就变得非常简单：

```java
String str = ResourceUtil.readUtf8Str("test.xml");
```

假设我们的文件存放在`src/resources/config`目录下，则读取改为：

```java
String str = ResourceUtil.readUtf8Str("config/test.xml");
```

- `ResourceUtil.readUtf8Str(String resource)`:读取Classpath下的资源为字符串，使用UTF-8编码，使用相对ClassPath的路径
- `ResourceUtil.readByTes(String resource)`: 读取Classpath下的资源为byte[]

```java
/**
 * ResourceUtil.readUtf8Str(String resource):读取Classpath下的资源为字符串，使用UTF-8编码
 * ResourceUtil.readByTes(String resource): 读取Classpath下的资源为byte[]
 */
@Test
public void readUtf8StrTest() {
    String str = ResourceUtil.readUtf8Str("1.txt");
    System.out.println(str);

}
```

- `getResourceObj(String path)`:获得 Resource 资源对象

```java
/**
 * getResourceObj(String path):获得 Resource 资源对象
 * 如果提供路径为绝对路径或路径以file:开头，返回 FileResource，否则返回ClassPathResource
 */
@Test
public void getResourceObjTest() {
    Resource resourceObj = ResourceUtil.getResourceObj("1.txt");
    // 你好，生命是有光的！
    System.out.println(resourceObj.readUtf8Str());


}
```

- `ResourceUtil.getResource(String resource)`：获取指定路径下的资源列表，路径格式必须为目录格式，用/分隔

```java
/**
 * 获得资源的URL
 */
@Test
public void getResourceTest() {
    URL resource = ResourceUtil.getResource("1.txt");
    // file:/E:/Code/IDEA/KuangStudy_Hutools/target/classes/1.txt
    System.out.println(resource);
}
```



# 4、ClassPath资源访问ClassPathResource



简单说来ClassPath就是查找class文件的路径，在Tomcat等容器下，ClassPath一般是`WEB-INF/classes`，为了项目方便，我们定义的配置文件肯定不能使用绝对路径，所以需要使用**相对路径**，这时候最好的办法就是把配置文件和class文件放在一起，便于查找。

在Java编码过程中，我们常常希望读取项目内的配置文件，按照Maven的习惯，这些文件一般放在项目的`src/main/resources`下，读取的时候使用：

```java
String path = "config.properties";
InputStream in = this.class.getResource(path).openStream();
```

使用当前类来获得资源其实就是使用当前类的类加载器获取资源，最后openStream()方法获取输入流来读取文件流。面对这种复杂的读取操作，我们封装了`ClassPathResource`类来简化这种资源的读取：

- `new classPathResource(String path)` ：传入路径path必须为相对路径

```java
/**
 * 获取 src/main/resources 的资源文件
 */
@Test
public void NewClassPathResource() {
    ClassPathResource classPathResource = new ClassPathResource("test.properties");
    // 创建属性集对象
    Properties properties = new Properties();
    try {
        // load(InputStream inStream): 从字节输入流中读取键值对
        // getStream(): 获得InputStream
        properties.load(classPathResource.getStream());
        // {port=80}
        System.out.println(properties);
        // 获取key为port的属性的值 80
        System.out.println(properties.get("port"));
    } catch (IOException e) {
        e.printStackTrace();
    }
}
```





















































# 5、Properties扩展类Props

众所周知，Java中广泛应用的配置文件Properties存在一个特别大的诟病：

1. 不支持中文。
2. Properties的第二个问题是读取非常不方便，需要我们自己写长长的代码进行`load`操作

```java
/**
 * 获取 src/main/resources 的资源文件
 */
@Test
public void NewClassPathResource() {
    ClassPathResource classPathResource = new ClassPathResource("test.properties");
    // 创建属性集对象
    Properties properties = new Properties();
    try {
        // load(InputStream inStream): 从字节输入流中读取键值对
        // getStream(): 获得InputStream
        properties.load(classPathResource.getStream());
        // {port=80}
        System.out.println(properties);
        // 获取key为port的属性的值 80
        System.out.println(properties.get("port"));
    } catch (IOException e) {
        e.printStackTrace();
    }
}
```

而Props类则大大简化为:

- `new Props(String path)` ：相对路径，创建 Props 类
- `getXXX` ： 获得 XXX 类型的属性值

```java
/**
 * 获取 src/main/resources 的资源文件
 * getProperty(String key): 使用此属性列表中指定的键搜索属性值。
 */
@Test
public void propsTest() {
    Props props = new Props("test.properties");
    String port = props.getProperty("port");
    // 80
    System.out.println(port);

    // getXXX getStr:获取字符串型属性值 80
    String port1 = props.getStr("port");
    System.out.println(port1);
}
```

- `setProperty(String key,Object value)` ： 设置值

```java
/**
 * 设置值 setProperty(String key,Object value)
 */
@Test
public void setPropertyTest() {
    Props props = new Props("test.properties");
    props.setProperty("filename","1.txt");
    // 1.txt
    System.out.println(props.getProperty("filename"));
}
```























# 6、Http客户端工具类HttpUtil

针对最为常用的GET和POST请求，HttpUtil封装了两个方法，

- `HttpUtil.get`
- `HttpUtil.post`

这两个方法用于请求普通页面，然后返回页面内容的字符串，同时提供一些重载方法用于指定请求参数（指定参数支持File对象，可实现文件上传，当然仅仅针对POST请求）。

- `HttpUtil.get(String urlString)` ：发送get请求

```java
/**
 * HTTP请求 - Get请求
 */
@Test
public void getTest() {
    // 最简单的HTTP请求，可以自动通过header等信息判断编码，不区分HTTP和HTTPS
    String s = HttpUtil.get("https://augenstern-creator.gitee.io/augenestern_docsify-brain/#/");
    // 返回 index.html 的源码
    System.out.println(s);
}
```

- `HttpUtil.post(String urlString,Map<String,Object> paramMap)` ： 发送post请求
  - `urlString` - 网址
  - `paramMap` - post表单数据

```java
/**
 * HTTP请求 - POST请求
 */
@Test
public void postTest() {
    HashMap<String, Object> map = new HashMap<>();
    map.put("city","北京");

    String post = HttpUtil.post("https://www.baidu.com", map);
    System.out.println(post);
}
```

- **文件上传**：文件上传只需将参数中的**键指定（默认file），值设为文件对象**即可，对于使用者来说，文件上传与普通表单提交并无区别

```java
/**
 * 文件上传
 */
@Test
public void PostTest() {
    HashMap<String, Object> paramMap = new HashMap<>();
    //文件上传只需将参数中的键指定（默认file），值设为文件对象即可，对于使用者来说，文件上传与普通表单提交并无区别
    paramMap.put("file",new File("D:\\1.txt"));

    String result= HttpUtil.post("https://www.baidu.com", paramMap);
    System.out.println(result);

}
```

- **文件下载**：`HttpUtil.downloadFile(String url,File destFile)` 
  - `url` - 请求的url，文件的网络地址
  - `destFile` - 下载到某个地方

```java
/**
 * 下载文件
 */
@Test
public void downloadFileTest() {
    String fileUrl = "https://plus.hutool.cn/images/hutool.svg";

    //将文件下载后保存在D盘，返回结果为下载文件大小
    long size = HttpUtil.downloadFile(fileUrl, new File("D:"));
    System.out.println("Download size: " + size);

}
```









# 7、Http请求HttpRequest

本质上，HttpUtil中的get和post工具方法都是HttpRequest对象的封装，因此如果想更加灵活操作Http请求，可以使用HttpRequest。

- **普通GET请求**

```java
/**
 * Get请求
 */
@Test
public void GetTest() {
    // 发送get请求
    HttpResponse response = HttpRequest.get("https://augenstern-creator.gitee.io/augenestern_docsify-brain/#/")
            // 超时
            .timeout(20000)
            // 执行get请求
            .execute();
    // 获取响应状态码
    System.out.println(response.getStatus());
    // 获取响应内容
    System.out.println(response.body());
}
```



- **普通表单POST请求**：
  - `execute()` ：执行Reuqest请求
  - `body([String body])` ：获取响应内容，请求体body参数支持两种类型：
    - 标准参数，例如 `a=1&b=2` 这种格式
    - Rest模式，此时body需要传入一个JSON或者XML字符串，Hutool会自动绑定其对应的Content-Type
  - 通过链式构建请求，我们可以很方便的指定Http头信息和表单信息，最后调用execute方法即可执行请求，返回HttpResponse对象。

```java
/**
 * 普通表单POST请求
 */
@Test
public void postTest() {
    HashMap<String, Object> map = new HashMap<>();
    map.put("city","北京");
    map.put("name","秦晓");
    map.put("age",22);

    // 发送post请求
    HttpResponse response = HttpRequest.post("https://www.baidu.com")
            // 头信息，多个头信息多次调用此方法即可
            .header(Header.USER_AGENT, "Hutool http")
            .header(Header.CONTENT_TYPE, "application/json")
            .header("token", "xxxxxxxxxx")
            // 提交的表单内容
            .form(map)
            // 超时,毫秒
            .timeout(20000)
            // 执行post请求
            .execute();

    // 获取响应状态码
    System.out.println(response.getStatus());
    // 获取响应内容
    System.out.println(response.body());
}
```







# 8、Http响应HttpResponse

此对象的使用非常简单，最常用的便是body方法，会返回Http响应内容字符串。如果想获取byte[]则调用bodyBytes即可。

- 获取响应状态码：`getStatus()`
- 获取响应内容： `body()`
- 获取cookie：`getCookies()`
- 请求是否成功: `isOk()`
- 将响应内容写出到文件: `writeBody(File targetFile)`

```java
/**
 * Get请求
 */
@Test
public void GetTest() {
    // 发送get请求
    HttpResponse response = HttpRequest.get("https://augenstern-creator.gitee.io/augenestern_docsify-brain/#/")
            // 超时
            .timeout(20000)
            // 执行get请求
            .execute();
    // 获取响应状态码
    System.out.println(response.getStatus());
    // 获取响应内容
    System.out.println(response.body());
    // 获取cookie
    System.out.println(response.getCookies());
    // 请求是否成功
    System.out.println(response.isOk());
    // 将响应内容写出到文件
    response.writeBody(new File("D:\\1.txt"));
}
```





# 9、常用Http状态码HttpStatus

针对Http响应，Hutool封装了一个类用于保存Http状态码，此类用于保存一些状态码的别名，例如：

```java
/**
* HTTP Status-Code 200: OK.
*/
public static final int HTTP_OK = 200;
```

此类的源码如下:

```java
public class HttpStatus {
    public static final int HTTP_CONTINUE = 100;
    public static final int HTTP_SWITCHING_PROTOCOLS = 101;
    public static final int HTTP_PROCESSING = 102;
    public static final int HTTP_CHECKPOINT = 103;
    public static final int HTTP_OK = 200;
    public static final int HTTP_CREATED = 201;
    public static final int HTTP_ACCEPTED = 202;
    public static final int HTTP_NOT_AUTHORITATIVE = 203;
    public static final int HTTP_NO_CONTENT = 204;
    public static final int HTTP_RESET = 205;
    public static final int HTTP_PARTIAL = 206;
    public static final int HTTP_MULTI_STATUS = 207;
    public static final int HTTP_ALREADY_REPORTED = 208;
    public static final int HTTP_IM_USED = 226;
    public static final int HTTP_MULT_CHOICE = 300;
    public static final int HTTP_MOVED_PERM = 301;
    public static final int HTTP_MOVED_TEMP = 302;
    public static final int HTTP_SEE_OTHER = 303;
    public static final int HTTP_NOT_MODIFIED = 304;
    public static final int HTTP_USE_PROXY = 305;
    public static final int HTTP_TEMP_REDIRECT = 307;
    public static final int HTTP_PERMANENT_REDIRECT = 308;
    public static final int HTTP_BAD_REQUEST = 400;
    public static final int HTTP_UNAUTHORIZED = 401;
    public static final int HTTP_PAYMENT_REQUIRED = 402;
    public static final int HTTP_FORBIDDEN = 403;
    public static final int HTTP_NOT_FOUND = 404;
    public static final int HTTP_BAD_METHOD = 405;
    public static final int HTTP_NOT_ACCEPTABLE = 406;
    public static final int HTTP_PROXY_AUTH = 407;
    public static final int HTTP_CLIENT_TIMEOUT = 408;
    public static final int HTTP_CONFLICT = 409;
    public static final int HTTP_GONE = 410;
    public static final int HTTP_LENGTH_REQUIRED = 411;
    public static final int HTTP_PRECON_FAILED = 412;
    public static final int HTTP_ENTITY_TOO_LARGE = 413;
    public static final int HTTP_REQ_TOO_LONG = 414;
    public static final int HTTP_UNSUPPORTED_TYPE = 415;
    public static final int HTTP_REQUESTED_RANGE_NOT_SATISFIABLE = 416;
    public static final int HTTP_EXPECTATION_FAILED = 417;
    public static final int HTTP_I_AM_A_TEAPOT = 418;
    public static final int HTTP_UNPROCESSABLE_ENTITY = 422;
    public static final int HTTP_LOCKED = 423;
    public static final int HTTP_FAILED_DEPENDENCY = 424;
    public static final int HTTP_TOO_EARLY = 425;
    public static final int HTTP_UPGRADE_REQUIRED = 426;
    public static final int HTTP_PRECONDITION_REQUIRED = 428;
    public static final int HTTP_TOO_MANY_REQUESTS = 429;
    public static final int HTTP_REQUEST_HEADER_FIELDS_TOO_LARGE = 431;
    public static final int HTTP_UNAVAILABLE_FOR_LEGAL_REASONS = 451;
    public static final int HTTP_INTERNAL_ERROR = 500;
    public static final int HTTP_NOT_IMPLEMENTED = 501;
    public static final int HTTP_BAD_GATEWAY = 502;
    public static final int HTTP_UNAVAILABLE = 503;
    public static final int HTTP_GATEWAY_TIMEOUT = 504;
    public static final int HTTP_VERSION = 505;
    public static final int HTTP_VARIANT_ALSO_NEGOTIATES = 506;
    public static final int HTTP_INSUFFICIENT_STORAGE = 507;
    public static final int HTTP_LOOP_DETECTED = 508;
    public static final int HTTP_BANDWIDTH_LIMIT_EXCEEDED = 509;
    public static final int HTTP_NOT_EXTENDED = 510;
    public static final int HTTP_NETWORK_AUTHENTICATION_REQUIRED = 511;

    // 无参构造器 用于new HttpStatus
    public HttpStatus() {
    }
    // 是否为重定向状态码
    public static boolean isRedirected(int responseCode) {
        return responseCode == 301 || responseCode == 302 || responseCode == 303 || responseCode == 307 || responseCode == 308;
    }

```

























