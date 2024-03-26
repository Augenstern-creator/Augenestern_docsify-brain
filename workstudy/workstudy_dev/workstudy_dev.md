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



































