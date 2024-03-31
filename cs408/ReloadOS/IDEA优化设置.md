# 1、常规设置



## 1.0、IDEA版本推荐

> [!NOTE]
>
> IDEA推荐版本:`IntelliJ IDEA 2022.2.4`

## 1.1、使用鼠标滚轮更改字体大小

- 设置 - 编辑器 - 常规 - 勾选 ==使用 Ctrl+鼠标轮更改字体大小==



## 1.2、文件注释模板

- 设置 - 编辑器 - 文件和代码模板 - 选中 class - 包含 - 点击加号 - 名称为 `File Header` - 扩展为 `java`

- 编写代码如下：

  ```java
   /**
     * @ClassName ${NAME} 
     * @Description TODO
     * @Author Augenestern_QXL
     * @Date ${DATE}
     * @Version 1.0
     */ 
  ```

  还有其他代码含义：

  ```bash
  ${PACKAGE_NAME} : 包路径
  ${NAME} : 文件名，该条语句可以获取到类名、接口名等
  TODO：代办事项的标记，一般生成类或方法都需要添加描述
  ${USER} ： 主机用户名，设置创建类的用户
  ${DATE} ： 系统当前日期，格式为：yyyy/mm/dd，设置创建日期
  ${TIME} ： 系统当前时间 格式为：HH:mm，设置创建日期
  ${YEAR}：当前年
  ${MONTH}：当前月
  ${MONTH_NAME_SHORT} ： 月份名称简写；如：Jan, Feb, etc.
  ${MONTH_NAME_FULL} ： 月份名称全拼；如：January, February.
  ${DAY} ：当前天
  ${DAY_NAME_SHORT} ：星期简写；如：Mon, Tue, etc.
  ${DAY_NAME_FULL}：星期全写；如：Mon, Tue, etc.
  ${HOUR} ： 当前小时
  ${MINUTE} ：当前分钟
  ${PROJECT_NAME} ： 项目名称
  1.0：设置版本号，一般新创建的类都是1.0版本，此处可以直接写死
  ```



## 1.3、方法注释模板

- 设置 - 编辑器 - 实时模板 - 点击右上角加号 - 新建模板组  - **methodTemplates**

- 点击新建的模板组 - 点击右上角加号 - 新建动态模板

- 编写处写 `*` ， 描述处写 `add Comments for method` ，模板文件写入如下代码

  ```bash
  **
     * @MethodName $title$
     * @Description $description$ $param$ $return$ $throws$
     * @Author Jiangnan Cui
     * @Date $date$ $TIME$
     */
  ```
  
- 点击 编辑变量 

| 名称        | 表达式       | 默认值 | 如果定义则跳过 |
| ----------- | ------------ | ------ | -------------- |
| title       | methodName() |        | √              |
| description |              |        | √              |
| param       | 见下面       |        | √              |
| return      | 见下面       |        | √              |
| throws      |              |        | √              |
| date        | date()       |        |                |
| TIME        | time()       |        |                |

param里面代码如下：

```
groovyScript("def result=''; def stop=false; def params=\"${_1}\".replaceAll('[\\\\[|\\\\]|\\\\s]', '').split(',').toList(); if (params.size()==1 && (params[0]==null || params[0]=='null' || params[0]=='')) { stop=true; }; if(!stop) { for(i=0; i < params.size(); i++) {result +=((i==0) ? '\\r\\n' : '') + ((i < params.size() - 1) ? ' * @param: ' + params[i] + '\\r\\n' : ' * @param: ' + params[i] + '')}; }; return result;", methodParameters())
```

return里面代码如下：

```
groovyScript("def result=''; def stop=false; def params=\"${_1}\".replaceAll('[\\\\[|\\\\]|\\\\s]', '').split(',').toList(); if (params.size()==1 && (params[0]==null || params[0]=='null' || params[0]=='')) { stop=true; }; if(!stop) { for(i=0; i < params.size(); i++) {result +=((i==0) ? '\\r\\n' : '') + ((i < params.size() - 1) ? ' * @param: ' + params[i] + '\\r\\n' : ' * @param: ' + params[i] + '')}; }; return result;", methodParameters())
```

记得给右边的方框打勾，点击OK完成参数设置。

- 点击定义 - 选择所有方式



## 1.4、多Tab页展示

- 设置 - 编辑器 - 常规 - 编辑器选项卡 - 取消勾选：在单行显示选项卡





## 1.5、IDEA注释

IDEA注释位置默认从行首开始，设置自适应代码缩进：

- File -> Settings - code style - java - code generation - 取消蓝色框选位置勾选
- [idea的注释老是从行首开始_llxxqq5的博客-CSDN博客](https://blog.csdn.net/m0_58574228/article/details/122324335)



## 1.6、IDEA方法注释

- [IDEA创建类注释模板和方法注释模板_idea设置类的注释模板_Coder_Cui的博客-CSDN博客](https://blog.csdn.net/xiaocui1995/article/details/123953752)































