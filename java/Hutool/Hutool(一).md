# Hutool

## 📚简介

- Hutool是一个小而全的Java工具类库，通过静态方法封装，降低相关API的学习成本，提高工作效率，使Java拥有函数式语言般的优雅，让Java语言也可以“甜甜的”。

- Hutool中的工具方法来自每个用户的精雕细琢，它涵盖了Java开发底层代码中的方方面面，它既是大型项目开发中解决小问题的利器，也是小型项目中的效率担当；
- Hutool是项目中“util”包友好的替代，它节省了开发人员对项目中公用类和公用工具方法的封装时间，使开发专注于业务，同时可以最大限度的避免封装不完善带来的bug。





## 🍺Hutool如何改变我们的coding方式

Hutool的目标是使用一个工具方法代替一段复杂代码，从而最大限度的避免“复制粘贴”代码的问题，彻底改变我们写代码的方式。

以计算MD5为例：

- 👴【以前】打开搜索引擎 -> 搜“Java MD5加密” -> 打开某篇博客-> 复制粘贴 -> 改改好用
- 👦【现在】引入Hutool -> SecureUtil.md5()

Hutool的存在就是为了减少代码搜索成本，避免网络上参差不齐的代码出现导致的bug。



## 📝文档

[📘中文文档(opens new window)](https://www.hutool.cn/docs/)

[📘中文备用文档(opens new window)](https://plus.hutool.cn/docs/#/)

[📙参考API(opens new window)](https://apidoc.gitee.com/dromara/hutool/)

[🎬视频介绍](https://www.bilibili.com/video/BV1bQ4y1M7d9?p=2)





## 📦安装

### 🍊Maven

在项目的pom.xml的dependencies中加入以下内容:

```xml
<!--Hutools-->
<dependency>
    <groupId>cn.hutool</groupId>
    <artifactId>hutool-all</artifactId>
    <version>5.8.24</version>
</dependency>
```















# 1、类型转换工具类Convert

在Java开发中我们要面对各种各样的类型转换问题，尤其是**从命令行获取的用户参数**、从

HttpRequest获取的Parameter等等，这些参数类型多种多样，我们怎么去转换他们呢？

- 常用的办法是先整成String，然后调用`XXX.parseXXX`方法，还要承受转换失败的风险，不得不加一层try catch，这个小小的过程混迹在业务代码中会显得非常难看和臃肿。

**Convert**类可以说是一个工具方法类，里面封装了针对Java常见类型的转换，用于简化类型转换。**Convert**类中大部分方法为toXXX，参数为Object，可以实现将任意可能的类型转换为指定类型。同时支持第二个参数**defaultValue**用于在转换失败时返回一个默认值。



1. `Convert.toStr(Object value)` ：转换为字符串

```java
/**
 * 转换为字符串
 */
@Test
public void toStrTest() {
    int a = 1;
    String aStr = Convert.toStr(a);
    // java.lang.String
    System.out.println(aStr.getClass().getName());

    long[] b = {1,2,3,4,5};
    String bStr = Convert.toStr(b);
    // [1, 2, 3, 4, 5]
    System.out.println(bStr);
}
```

2. `Convert.toIntArray(Object value)` ：转换为指定类型（Integer）数组

```java
/**
 * 转换为指定类型数组
 */
@Test
public void toIntArrayTest() {
    String[] c = { "1","2","3","4","5"};
    Integer[] intArray = Convert.toIntArray(c);
    // intArray 为Integer数组 {1,2,3,4,5}
    for (Integer integer : intArray) {
        System.out.println(integer);
    }
}
```

3. `Convert.toDate(Object value)` ：转换为日期对象Date

```java
/**
* 字符串转换为日期对象
*/
String d = "2024-01-04";
Date date = Convert.toDate(a);
// Thu Jan 01 08:00:00 CST 1970
System.out.println(date);
```

4. `Convert.toList(Object value)` ：转换为集合ArrayList

```java
/**
 * 数组转集合
 * 数组的容量是固定的，集合的容量是可变的
 */
Object[] e = {"aaa","bbb","好",1};
List<?> list = Convert.toList(e);
list.forEach(i -> System.out.println(i));
```

# 2、日期时间工具DateUtil





1. 获取当前时间字符串
   - `DateUtil.date()`
   - `DateUtil.today()`

```java
/**
 * 获取当前时间字符串
 */
@Test
public void dateTest() {
    // 当前时间字符串，格式：yyyy-MM-dd HH:mm:ss
    DateTime date = DateUtil.date();
    // 2024-01-04 14:40:25
    System.out.println(date);

    // 当前日期字符串，格式：yyyy-MM-dd
    String today = DateUtil.today();
    // 2024-01-04
    System.out.println(today);
}
```

2. 格式化日期输出
   - `DateUtil.parse(String str)` : 将日期字符串转换为DateTime对象
   - `DateUtil.format(LocalDateTime localDateTime,String format)` ： 根据特定格式格式化日期

```java
/**
 * 格式化日期输出
 */
String dateStr = "2023-01-04";
DateTime parse = DateUtil.parse(dateStr);
// 2023-01-04 00:00:00
System.out.println(parse);

// 根据特定格式格式化日期输出
DateTime date1 = DateUtil.date();
String format = DateUtil.format(date1, "yyyy/MM/dd");
// 2024/01/04
System.out.println(format);
```

3. 获取Date对象的某个部分

```java
/**
 * 获取Date对象的某个部分
 */
@Test
public void yearTest() {
    // 获取当前时间字符串 2024-01-04 15:06:01
    Date date = DateUtil.date();
    //获得年的部分 2024
    System.out.println(DateUtil.year(date));
    //获得月份，从0开始计数(因为是1月份,所以输出是0)
    System.out.println(DateUtil.month(date));
}
```

4. 日期时间偏移
   - 日期或时间的偏移指针对某个日期增加或减少分、小时、天等等，达到日期变更的目的

```java
/**
 * 日期时间偏移
 */
@Test
public void offsetDayTest() {
    String dateStr1 = "2023-01-04 22:33:23";
    // 时间对象 2023-01-04 22:33:23
    DateTime parse1 = DateUtil.parse(dateStr1);
    // 日期+3 2023-01-07 22:33:23
    DateTime offsetDay = DateUtil.offsetDay(parse1, 3);
    System.out.println(offsetDay);
    // 时间-3 2023-01-04 19:33:23
    DateTime offsetHour = DateUtil.offsetHour(parse1, -3);
    System.out.println(offsetHour);

    //昨天
    DateUtil.yesterday();
    //明天
    DateUtil.tomorrow();
    //上周
    DateUtil.lastWeek();
    //下周
    DateUtil.nextWeek();
    //上个月
    DateUtil.lastMonth();
    //下个月
    DateUtil.nextMonth();
}
```

5. 昨天、明天、上周、下周、上个月、下个月

```java
//昨天
DateUtil.yesterday()
//明天
DateUtil.tomorrow()
//上周
DateUtil.lastWeek()
//下周
DateUtil.nextWeek()
//上个月
DateUtil.lastMonth()
//下个月
DateUtil.nextMonth()
```

6. 日期时间差
   - 有时候我们需要计算两个日期之间的时间差（相差天数、相差小时数等等）

```java
/**
 * 日期时间差
 */
@Test
public void betweenTest() {
    String dateStr1 = "2023-03-01 22:33:23";
    Date date1 = DateUtil.parse(dateStr1);

    String dateStr2 = "2023-04-01 23:33:23";
    Date date2 = DateUtil.parse(dateStr2);

    //相差31天 31
    long betweenDay = DateUtil.between(date1, date2, DateUnit.DAY);
    System.out.println(betweenDay);
}
```

7. 星座和属相

```java
/**
 * 星座和属相
 */
@Test
public void ZodiacTest() {
    // "水瓶座"
    String zodiac = DateUtil.getZodiac(Month.JANUARY.getValue(), 30);
    System.out.println("贾萌属于" + zodiac);

    // "马"
    String chineseZodiac = DateUtil.getChineseZodiac(2002);
    System.out.println("贾萌属相" + chineseZodiac);

    // 年龄
    System.out.println("贾萌年龄" + DateUtil.ageOfNow("2002-01-30"));
}
```

8. 检查指定日期

```java
/**
 * 检查指定日期
 */
@Test
public void dayofTest() {
    DateTime date = DateUtil.date();
    // 现在时间 2024-01-08 21:21:15
    System.out.println(date);
    // 获得指定日期是星期几   MONDAY
    System.out.println(DateUtil.dayOfWeekEnum(date));
    // 是否是上午
    System.out.println(DateUtil.isAM(date));
    // 是否是下午
    System.out.println(DateUtil.isPM(date));
    // 是否是周末 周末指周六或者周日
    System.out.println(DateUtil.isWeekend(date));

    // 转换为Date  考虑到很多框架（例如Hibernate）的兼容性，提供此方法返回JDK原生的Date对象
    Date date1 = date.toJdkDate();
    // Mon Jan 08 21:26:25 CST 2024
    System.out.println(date1);
}
```















# 3、日期时间对象DateTime



使用DateTime对象可以完全替代开发中Date对象的使用。

1. 新建DateTime对象和使用对象
   - 新建DateTime对象：`new DateTime()`

```java
/**
 * 新建DateTime对象
 */
@Test
public void newDateTimeTest() {
    // Thu Jan 04 16:53:18 CST 2024
    Date date = new Date();
    DateTime dateTime = new DateTime(date);
    // 2024-01-04 16:53:18
    System.out.println(dateTime);

    // 获取年 2024
    int year = dateTime.year();
    System.out.println(year);
    // 获取月份 JANUARY
    Month month = dateTime.monthEnum();
    System.out.println(month);
    // 获取日 4
    int day = dateTime.dayOfMonth();
    System.out.println(day);
}
```

2. 格式化为字符串
   - 调用`toString()`方法即可返回格式为`yyyy-MM-dd HH:mm:ss`的字符串，调用`toString(String format)`可以返回指定格式的字符串。

```java
/**
 * 格式化为字符串
 */
@Test
public void toStringTest() {
    Date date = new Date();
    DateTime dateTime = new DateTime(date);
    // 格式化为字符串 2024-01-04 17:15:45
    System.out.println(dateTime.toString());
    // 格式化为字符串 2024/01/04
    System.out.println(dateTime.toString("yyyy/MM/dd"));
}
```



# 4、计时器工具TimeInterval

```java
/**
 * 计时器工具
 */
@Test
public void timerTest() {
    TimeInterval timer = DateUtil.timer();
    // 执行过程
    for (int i = 0; i < 1000; i++) {
        System.out.println(i);
    }

    //花费毫秒数
    System.out.println(timer.interval());
    //花费分钟数
    System.out.println(timer.intervalMinute());
}
```



# 5、文件工具类FileUtil

- `FileUtil.isWindows()` ： 是否为Windows环境

```java
/**
 * 是否为Windows环境
 */
@Test
public void isWindowsTest() {
    if (FileUtil.isWindows()) {
        System.out.println("windows环境");
    }else {
        System.out.println("其他环境");
    }
}
```

- `FileUtil.ls(String path)` ：列出指定路径下的目录和文件

```java
/**
 * 列出目录文件:列出指定目录里的文件或目录,子目录里的文件不会列出来
 */
@Test
public  void lsTest() {
    File[] ls = FileUtil.ls("D:\\");
    for (File l : ls) {
        System.out.println(l);
    }
}
```

- `FileUtil.isEmpty(File file)` ：文件是否为空

```java
/**
 * 文件是否为空: 里面没有文件时为空 文件大小为0时为空
 */
@Test
public void isEmptyTest() {
    File file1 = new File("E:\\Code\\IDEA\\KuangStudy_Hutools\\src\\main\\resources\\1.txt");
    File file2 = new File("E:\\Code\\IDEA\\KuangStudy_Hutools\\src\\main\\resources");

    System.out.println("文件" + FileUtil.isEmpty(file1));
    System.out.println("目录" + FileUtil.isEmpty(file2));
}
```

- `FileUtil.isDirEmpty(File file)` ：目录是否为空

```java
/**
 * 目录是否为空:
 */
@Test
public void isDirEmptyTest() {
    File file = new File("E:\\Code\\IDEA\\KuangStudy_Hutools\\src\\main\\resources");
    // 一定要是目录file不然会抛异常
    System.out.println(FileUtil.isDirEmpty(file));
}
```

- `FileUtil.loopFiles(String path [,FileFilter fileFilter])`
  - 递归遍历目录以及子目录中的所有文件
  - 如果提供file为文件，直接返回过滤结果

```java
/**
 * 递归遍历目录以及子目录中的所有文件
 */
@Test
public void loopFilesTest() {
    // 不加过滤
    List<File> files = FileUtil.loopFiles("D:\\Environment\\apache-maven-3.6.1-bin\\apache-maven-3.6.1");
    files.forEach(f -> System.out.println(f.getName()));

    System.out.println("================================================");

    // 加过滤结果
    List<File> files1 = FileUtil.loopFiles("D:\\Environment\\apache-maven-3.6.1-bin\\apache-maven-3.6.1", new FileFilter() {
        @Override
        public boolean accept(File pathname) {
            // 过滤文件名是 README.txt 的文件
            if (pathname.getName().equals("README.txt")) {
                return true;
            } else {
                return false;
            }
        }
    });
    files1.forEach(f -> System.out.println(f.getName()));
}
```

- `FileUtil.walkFiles(File file [,Consumer<File> consumer])` :递归遍历目录并处理目录下的文件，可以处理目录或文件
  - 非目录则直接调用`Consumer`处理
  - 目录则递归调用此方法处理

```java
/**
 * 递归遍历目录并处理目录下的文件(子目录也会遍历,可以处理目录或文件)
 */
@Test
public void walkFilesTest() {
    FileUtil.walkFiles(new File("E:\\Code\\IDEA\\KuangStudy_Hutools\\src\\main\\resources"), new Consumer<File>() {
        @Override
        public void accept(File file) {
            // 如果文件名是 1.txt , 则打印文件的绝对路径
            if (file.getName().equals("1.txt")) {
                System.out.println(file.getAbsoluteFile());
            }
        }
    });
}
```

- `FileUtil.listFileNames(String path)` ：获得指定目录下所有文件，不会扫描子目录，如果用户传入相对路径，则是相对classpath的路径，`"test/aaa"`表示`"${classpath}/test/aaa"`

```java
/**
 * 获得指定目录下所有文件名,不扫描目录
 */
@Test
public void listFileNamesTest() {
    List<String> lists = FileUtil.listFileNames("D:\\Environment\\apache-maven-3.6.1-bin\\apache-maven-3.6.1");
    lists.forEach(listFileName -> System.out.println(listFileName));
}
```

- `FileUtil.exist(File file | String path)` ：判断文件是否存在

```java
/**
 * 判断文件是否存在
 */
@Test
public void existTest() {
    boolean exist = FileUtil.exist("E:\\Code\\IDEA\\KuangStudy_Hutools\\src\\main\\resources\\1.txt");
    System.out.println(exist);
}
```

- `FileUtil.size(File file)` ： 计算目录或文件的总大小，当给定对象为文件时，直接调用`File.length()` ，当给定对象为目录时，遍历目录下的所有文件和目录，递归计算其大小

```java
/**
 * 计算目录或文件的总大小
 */
@Test
public void sizeTest() {
    File file = new File(("E:\\Code\\IDEA\\KuangStudy_Hutools\\src\\main\\resources\\1.txt"));
    long size = FileUtil.size(file);
    System.out.println("大小是:" + size);
}
```

- `FileUtil.touch(File file)` ：创建文件及其父目录，如果这个文件存在，直接返回这个文件

```java
/**
 * 创建文件及其父目录: 如果这个文件存在，直接返回这个文件，如果不存在，则创建文件及其父目录
 */
@Test
public void touchTest() {
    File touch = FileUtil.touch("E:\\Code\\IDEA\\KuangStudy_Hutools\\src\\main\\resources\\3.txt");
    if(FileUtil.exist(touch)){
        System.out.println(FileUtil.getName(touch));
    }else{
        System.out.println("文件不存在");
    }
}
```

- `FileUtil.del(Path path)` ： 删除文件或者文件夹:删除文件夹时不会判断文件夹是否为空，如果不空则递归删除子文件或文件夹，某个文件删除失败会终止删除操作

```java
/**
 * 删除文件或者文件夹:删除文件夹时不会判断文件夹是否为空，如果不空则递归删除子文件或文件夹，某个文件删除失败会终止删除操作
 */
@Test
public void delTest() {
    boolean del = FileUtil.del("E:\\Code\\IDEA\\KuangStudy_Hutools\\src\\main\\resources\\3.txt");
    System.out.println(del);
}
```

- `FileUtil.clean(File directory)` ：清空文件夹:清空文件夹时不会判断文件夹是否为空，如果不空则递归删除子文件或文件夹,某个文件删除失败会终止删除操作

```java
/**
 * 清空文件夹:清空文件夹时不会判断文件夹是否为空，如果不空则递归删除子文件或文件夹,某个文件删除失败会终止删除操作
 */
@Test
public void cleanTest() {
    boolean clean = FileUtil.clean("E:\\Code\\IDEA\\KuangStudy_Hutools\\src\\main\\resources");
    System.out.println(clean);
}
```

- `FileUtil.mkdir(File dir | String dirPath)` ：创建文件夹，会递归自动创建其不存在的父文件夹，如果存在直接返回此文件夹

```java
/**
 * 创建文件夹:如果存在直接返回此文件夹,会递归自动创建其不存在的父文件夹，如果存在直接返回此文件夹
 */
@Test
public void mkdirTest() {
    File mkdir = FileUtil.mkdir("D:/test");
    System.out.println(mkdir);
}
```

- `FileUtil.copy(File src,File dest,boolean isOverride)` ：复制文件或目录

  - `srcPath` - 源文件或目录
  - `destPath` - 目标文件或目录，目标不存在会自动创建（目录、文件都创建）
  - `isOverride` - 是否覆盖目标文件

  情况如下：

  - src和dest都为目录，则将src目录及其目录下所有文件目录拷贝到dest下
  - src和dest都为文件，直接复制，名字为dest
  - src为文件，dest为目录，将src拷贝到dest目录下

```java
/**
 * 复制文件或者目录
 */
@Test
public void copyTest() {
    // 复制文件
    File copy = FileUtil.copy(
            "E:\\Code\\IDEA\\KuangStudy_Hutools\\src\\main\\resources\\1.txt",
            "E:\\Code\\IDEA\\KuangStudy_Hutools\\src\\main\\resources\\2.txt",
            false
    );
    System.out.println(copy);
    // 将文件拷贝到某个目录下
    File copy1 = FileUtil.copy(
            "E:\\Code\\IDEA\\KuangStudy_Hutools\\src\\main\\resources\\1.txt",
            "D:\\",
            false
    );
    System.out.println(copy1);


}
```

- `FileUtil.move(File src,File target,boolean isOverride)` ：移动文件或者目录
  - File src：src 源文件或者目录
  - File target：target 目标文件或者目录
  - boolean isOverride：isOverride 是否覆盖目标，只有目标为文件才覆盖

```java
/**
 * 移动文件或者目录
 */
@Test
public void moveTest() {
    File file1 = new File("D:\\1.txt");
    // 目标目录不存在程序也会帮忙创建
    File file2 = new File("E:\\");
    FileUtil.move(file1, file2,false);
}
```

- `FileUtil.rename(File file,String newName,boolean isOverride)` ： 修改文件或目录的文件名，不变更路径，只是简单修改文件名，不保留扩展名
  - File file ： 被修改的文件
  - String newName：newName 新的文件名，如需扩展名，需自行在此参数加上，原文件名的扩展名不会被保留
  - boolean isOverride：isOverride 是否覆盖目标文件

```java
/**
 * 修改文件或目录的文件名，不变更路径，只是简单修改文件名，不保留扩展名
 */
@Test
public void renameTest() {
    File file = new File("E:\\1.txt");
    File rename = FileUtil.rename(file, "2.txt", false);
    System.out.println(rename);

}
```

- `FileUtil.getCanonicalPath(File file)` ：获取规范的绝对路径

```java
/**
 * 获取规范的绝对路径
 */
@Test
public void getCanonicalPathTest() {
    String canonicalPath = FileUtil.getCanonicalPath(new File("E:\\Code\\IDEA\\KuangStudy_Hutools\\src\\main\\resources\\1.txt"));
    // E:\Code\IDEA\KuangStudy_Hutools\src\main\resources\1.txt
    System.out.println(canonicalPath);
}
```

- `FileUtil.isDirectory(String path)` ：判断是否为目录

```java
/**
 * 判断是否为目录
 */
@Test
public void isDirectoryTest() {
    String path = "E:\\1.txt";
    System.out.println(FileUtil.isDirectory(path));
}
```

- `FileUtil.isFile(String path)` ： 判断是否为文件

```java
/**
 * 判断是否为文件
 */
@Test
public void isFileTest() {
    String path = "E:\\1.txt";
    System.out.println(FileUtil.isFile(path));
}
```

- `FileUtil.equals(File file1,File file2)`：检查两个文件是否是同一个文件

```java
/**
 * 检查两个文件是否是同一个文件:所谓文件相同，是指File对象是否指向同一个文件或文件夹
 */
@Test
public void equalsTest() {
    File file1 = new File("D:\\1.txt");
    File file2 = new File("E:\\2.txt");
    System.out.println(FileUtil.equals(file1, file2));
}
```

- `FileUtil.contentEquals(File file1,File file2)` :比较两个文件内容是否相同,首先比较长度，长度一致再比较内容

```java
/**
 * 比较两个文件内容是否相同,首先比较长度，长度一致再比较内容
 */
@Test
public void contentEqualsTest() {
    File file1 = new File("D:\\1.txt");
    File file2 = new File("E:\\1.txt");
    System.out.println(FileUtil.contentEquals(file1, file2));
}
```

- `FileUtil.getWebRoot()` :获取Web项目下的web root路径
  - 原理是首先获取ClassPath路径，由于在web项目中ClassPath位于`WEB-INF/classes/`下，故向上获取两级目录即可

```java
/**
 * 获取Web项目下的 web root 路径
 * 原理是首先获取ClassPath路径，由于在web项目中ClassPath位于 WEB-INF/classes/下，故向上获取两级目录即可。
 */
@Test
public void getWebRootTest() {
    System.out.println(FileUtil.getWebRoot());
}
```

- `FileUtil.getSuffix(File file | String path)` ： 获取文件后缀名,扩展名不带 `"."`
- `FileUtil.getPrefix(File file | String path)` : 获取文件名

```java
/**
 * 获取文件后缀名,扩展名不带 "."   getSuffix(File file | String fileName)
 */
@Test
public void getSuffixTest() {
    File file = new File("D:\\1.txt");
    // 获得后缀 txt
    System.out.println(FileUtil.getSuffix(file));
    // 获得文件名 1
    System.out.println(FileUtil.getPrefix(file));

    String path = "D:\\1.txt";
    // 获得后缀 txt
    System.out.println(FileUtil.getSuffix(path));
    // 获得文件名 1
    System.out.println(FileUtil.getPrefix(path));
}
```



- `FileUtil.getInputStream(File file | String path)` ： 获得输入流
- `FileUtil.getInputStream(File file | String path)` ： 获得输出流

> 推荐进入Hutool的API文档查看































# 6、字符串工具StrUtil







- `StrUtil.isEmpty(String str)` : 字符串是否为空，空的定义如下： 

```java
/**
 * 字符串是否为空，空的定义如下：
 * 1. null
 * 2. 空字符串：""
 * 3. 空格、全角空格、制表符、换行符，等不可见字符
 */
@Test
public void isEmptyTest() {
    String str = "abc";
    // false
    System.out.println(StrUtil.isEmpty(str));
    // true
    System.out.println(StrUtil.isEmpty(""));
    // true
    System.out.println(StrUtil.isEmpty(null));
}
```

- `StrUtil.isBlank(String str)` : 字符串是否为空白，空白的定义如下：
  - null
  - 空字符串 ""
  - 空格、全角空格、制表符、换行符，等不可见字符
  - **该方法与 isEmpty 的区别是:该方法会检验空白字符**

```java
/**
 * 字符串是否为空白，空白的定义如下：
 * 1. null
 * 2. 空字符串：""
 * 3. 空格、全角空格、制表符、换行符，等不可见字符
 * 该方法与 isEmpty 的区别是:该方法会检验空白字符
 */
@Test
public void isBlankTest() {
    // true
    System.out.println(StrUtil.isBlank(null));
    // true
    System.out.println(StrUtil.isBlank(""));
    // true
    System.out.println(StrUtil.isBlank("\t\n"));
    // false
    System.out.println(StrUtil.isBlank("abc"));
}
```

- `StrUtil.hasBlank(String str)` 、 `StrUtil.hasEmpty(String str)` : 就是给定一些字符串，如果一旦有空的就返回true，常用于判断好多字段是否有空的（例如web表单数据）
  - 这两个方法的区别是hasEmpty只判断是否为null或者空字符串（""），hasBlank则会把不可见字符也算做空，

```java
/**
 * hasBlank 、 hasEmpty 方法
 * 就是给定一些字符串，如果一旦有空的就返回true，常用于判断好多字段是否有空的（例如web表单数据）
 * 这两个方法的区别是hasEmpty只判断是否为null或者空字符串（""），hasBlank则会把不可见字符也算做空，
 *
 */
@Test
public void hasBlankTest() {
    // true
    System.out.println(StrUtil.hasBlank());
    // true
    System.out.println(StrUtil.hasBlank("",null,""));
    // true
    System.out.println(StrUtil.hasBlank("123",""));
    // false
    System.out.println(StrUtil.hasBlank("123","abc"));
}
```



- `StrUtil.builder()` : 创建 StringBuilder 对象

```java
/**
 * 创建StringBuilder对象
 */
@Test
public void builderTest() {
    StringBuilder builder = StrUtil.builder("abc");
    // 将StringBuilder对象转换为String对象
    String builderString = builder.toString();
}
```

- `StrUtil.reverse(String str)` : 反转字符串

```java
/**
 * 反转字符串 reverse
 */
@Test
public void reverseTest() {
    String str = "abc";
    // cba
    System.out.println(StrUtil.reverse(str));
}
```

- `StrUtil.trim(String[] str)` : 给定字符串数组全部去首尾空格

```java
/**
 * 给定字符串数组全部去首尾空格
 */
@Test
public void trimTest() {
    String[] str = {"   abc  ", "  d "};
    StrUtil.trim(str);
    for (String s : str) {
        // abc
        // d
        System.out.println(s);
    }
}
```

- `StrUtil.uuid()`: 生成随机UUID

```java
/**
 * 生成随机UUID
 */
@Test
public void uuidTest(){
    System.out.println(StrUtil.uuid());
}
```

- `removePrefix(String str,String prefix)` : 去掉字符串前缀
  - `removeSuffix(String str,String suffix)` : 去掉字符串后缀
  - `removePrefixIgnoreCase(String str,String prefix)` : 忽略大小写去掉字符串前缀
  - `removeSuffixIgnoreCase(String str,String prefix)` : 忽略大小写去掉字符串后缀

```java
/**
 * 去掉字符串前缀 removePrefix(String str,String prefix)
 * 去掉字符串后缀 removeSuffix(String str,String suffix)
 * 例如去掉文件名的扩展名
 * 忽略大小写去掉字符串前缀  removePrefixIgnoreCase(String str,String prefix)
 * 忽略大小写去掉字符串后缀  removeSuffixIgnoreCase(String str,String prefix)
 */
@Test
public void removePrefixTest() {
    // 去掉后缀 .jpg
    String fileName = StrUtil.removeSuffix("jm.jpg", ".jpg");
    //fileName -> jm
    System.out.println(fileName);
}
```

- `StrUtil.sub(String str,int beginIndex, int endIndex)` : 截取字符串, index从0开始计算，最后一个字符为-1
  - beginIndex -- 起始索引（包括）, 索引从 0 开始
  - endIndex -- 结束索引（不包括）

```java
/**
 * 截取字符串: index从0开始计算，最后一个字符为-1
 * StrUtil.sub(String str,int beginIndex, int endIndex):
 *  beginIndex -- 起始索引（包括）, 索引从 0 开始
 *  endIndex -- 结束索引（不包括）
 */
@Test
public void subTest() {
    String str = "abcdefgh";
    // 截取索引为2的字符串
    String strSub1 = StrUtil.sub(str, 2, 3); //strSub1 -> c
}
```

- `StrUtil.format()` : 模板字符串代替字符串拼接

```java
/**
 * 字符串模板代替字符串拼接
 */
@Test
public void formatTest() {
    String template = "{}爱{}，就像老鼠爱大米";
    String str = StrUtil.format(template, "我", "你");
    //str -> 我爱你，就像老鼠爱大米
    System.out.println(str);
}
```











# 7、对象工具ObjectUtil

- `Object.equals(Object obj1,Object obj2)` : 比较两个对象是否相等。相等需满足以下条件之一：
  1. obj1 == null && obj2 == null
  2. obj1.equals(obj2)

```java
/**
 * 比较两个对象是否相等，相等需满足以下条件之一：
 * obj1 == null && obj2 == null
 * obj1.equals(obj2)
 */
@Test
public void equalTest() {
    Object a = null;
    Object b = null;

    // true
    System.out.println(ObjectUtil.equals(a, b));
}
```

- `ObjectUtil.length(Object obj)` : 计算对象长度，如果是字符串调用其length方法，集合类调用其size方法，数组调用其length属性，其他可遍历对象遍历计算长度。

```java
/**
 * 计算对象长度，如果是字符串调用其length方法，集合类调用其size方法，数组调用其length属性，其他可遍历对象遍历计算长度。
 */
@Test
public void lengthTest() {
    int[] array = new int[]{1,2,3,4,5};

    // 5
    int length = ObjectUtil.length(array);
    System.out.println(length);

    Map<String, String> map = new HashMap<>();
    map.put("a", "a1");
    map.put("b", "b1");
    map.put("c", "c1");

    // 3
    int length1 = ObjectUtil.length(map);
    System.out.println(length1);


}
```

- `ObjectUtil.contains(Object obj,Object element)` : 对象中是否包含元素

```java
/**
 * 对象中是否包含元素
 */
@Test
public void containsTest() {
    int[] array = new int[]{1,2,3,4,5};

    // true
    final boolean contains = ObjectUtil.contains(array, 1);

}
```

- `ObjectUtil.isNull(Object obj)` : 检查对象是否为null

```java
/**
 * 检查对象是否为null
 */
@Test
public void isNullTest() {
    People people = new People();
    people = null;
    // true
    System.out.println(ObjectUtil.isNull(people));
}
```

- `ObjectUtil.clone(T obj)` ： 克隆对象，如果对象实现Cloneable接口，调用其clone方法；如果实现Serializable接口，执行深度克隆

```java
/**
 * 克隆对象
 */
@Test
public void cloneTest() {
    People people = new People("qxl",21);
    People clone = ObjectUtil.clone(people);
    // qxl
    System.out.println(clone.getName());
    // 21
    System.out.println(clone.getAge());
}
```

- `ObjectUtil.hasEmpty(Object... objs)` ：是否存在null或空对象
- `ObjectUtil.hasNull(Object... objs)` : 是否存在null对象

```java
/**
 * hasEmpty(Object... objs)是否存在null或空对象
 * hasNull(Object... objs) 是否存在null对象
 */
@Test
public void hasEmptyTest() {
    People people1 = new People("qxl", 22);
    People people2 = new People("jm", 21);
    boolean b = ObjectUtil.hasEmpty(people1, people2);
    // false
    System.out.println(b);

    people1=null;
    boolean b1 = ObjectUtil.hasNull(people1, null);
    // true
    System.out.println(b1);

}
```

- `ObjectUtil.serialize(T obj)` ：序列化，调用JDK序列化
- `ObjectUtil.deserialize(T obj)` : 反序列化，调用JDK



```java
/**
 * 序列化和反序列化
 * 序列化就是指把Java对象转换为字节序列的过程
 * 反序列化就是指把字节序列恢复为Java对象的过程
 */
@Test
public void serializeTest() {
    People people = new People("qxl", 21);
    // 序列化
    byte[] serialize = ObjectUtil.serialize(people);
    // 反序列化
    ObjectUtil.deserialize(serialize);
}
```
































































