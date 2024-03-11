# 1、随机工具RandomUtil

`RandomUtil`主要针对JDK中`Random`对象做封装，严格来说，Java产生的随机数都是伪随机数，因此Hutool封装后产生的随机结果也是伪随机结果。不过这种随机结果对于大多数情况已经够用。

- `RandomUtil.randomxxx()` : 获得随机数xxx值
- `RandomUtil.randomInt()` : 获得随机数Int值

- `RandomUtil.randomInt(int mixInclude,int maxExclude)`: 获得指定范围内的随机数 [min,max)
- `RandomUtil.randomString(int length)` : 获得一个随机的字符串（只包含数字和字符）
  - `length` - 字符串的长度
- `RandomUtil.randomStringUpper(int length)` : 获得一个随机的字符串（只包含数字和大写字符）
- `RandomUtil.randomNumbers(int length)` :  获得一个只包含数字的字符串

```java
/**
 * RandomUtil.randomxxx():获得随机数xxx值
 * RandomUtil.randomInt():获得随机数int值
 * RandomUtil.randomInt(int mixInclude,int maxExclude): 获得指定范围内的随机数 [min,max)
 */
@Test
public void randomInt() {
    System.out.println(RandomUtil.randomInt(0,100));
}
```





# 2、唯一ID工具IdUtil

- `IdUtil.fastSimpleUUID()` : 生成的UUID是带 - 的字符串
- `IdUtil.fastUUID()`: 生成的UUID是不带 - 的字符串

```java
/**
 * 获取UUID
 */
@Test
public void uuidTest() {
    // f34229f2869742bd9e25ad54cc7e7972
    System.out.println(IdUtil.fastSimpleUUID());
    // 11b0cb15-cf03-4d29-bd6e-93766094784d
    System.out.println(IdUtil.fastUUID());
}
```













# 3、身份证工具IdcardUtil

- `IdcardUtil.isValidCard(String idCard)` : 是否有效身份证号，忽略X的大小写 如果身份证号码中含有空格始终返回false

```java
/**
 * IdcardUtil.isValidCard(String idCard): 是否有效身份证号，忽略X的大小写 如果身份证号码中含有空格始终返回false
 */

@Test
public void isValidTest() {
    String ID_18 = "321083197812162119";
    String ID_15 = "150102880730303";

    System.out.println(IdcardUtil.isValidCard(ID_18));
    System.out.println(IdcardUtil.isValidCard(ID_15));
}
```

- `IdUtil.getBirthByIdCard(String idCard)` : 根据身份编号获取生日
- `IdUtil.getBirthDate(String idCard)` : 从身份证号码中获取生日日期

```java
/**
 * 获取生日
 */
@Test
public void getTest() {
    String ID_18 = "321083197812162119";
    // 19781216
    System.out.println(IdcardUtil.getBirthByIdCard(ID_18));
    // 1978-12-16 00:00:00
    System.out.println(IdcardUtil.getBirthDate(ID_18));
}
```

- `IdUtil.getAgeByIdCard(String idcard)` ： 根据身份编号获取年龄，只支持15或18位身份证号码

```java

/**
 * 获取年龄
 */
@Test
public void getTest() {
    String ID_18 = "321083197812162119";
    // 45
    System.out.println(IdcardUtil.getAgeByIdCard(ID_18));
}
```

- `IdcardUtil.getYearByIdCard(String idcard)` : 获取生日年
- `IdcardUtil.getMonthByIdCard(String idcard)`:获取生日月
- `IdcardUtil.getDayByIdCard(String idcard)`:获取生日天

```java
/**
 * 获取生日年:IdcardUtil.getYearByIdCard(String idcard)
 * 获取生日月:IdcardUtil.getMonthByIdCard(String idcard)
 * 获取生日天:IdcardUtil.getDayByIdCard(String idcard)
 */
@Test
public void getYearTest() {
    String ID_18 = "321083197812162119";
    // 1978
    System.out.println(IdcardUtil.getYearByIdCard(ID_18));
    // 12
    System.out.println(IdcardUtil.getMonthByIdCard(ID_18));
    // 16
    System.out.println(IdcardUtil.getDayByIdCard(ID_18));
}
```

- `IdcardUtil.getGenderByIdCard(String idcard)` : 获取性别
- `IdcardUtil.getProvinceByIdCard(String idcard)` : 获取省份

```java
/**
 * 获取性别: IdcardUtil.getGenderByIdCard(String idcard) 返回性别(1 : 男 ， 0 : 女)
 * 获取省份: IdcardUtil.getProvinceByIdCard(String idcard)
 */
@Test
public void getGenderByIdCardTest() {
    String ID_18 = "321083197812162119";
    // 1
    System.out.println(IdcardUtil.getGenderByIdCard(ID_18));
    // 江苏
    System.out.println(IdcardUtil.getProvinceByIdCard(ID_18));
}
```

# 4、信息脱敏工具DesensitizedUtil

在数据处理或清洗中，可能涉及到很多隐私信息的脱敏工作，因此Hutool针对常用的信息封装了一些脱敏方法。

现阶段支持的脱敏数据类型包括：

1. 用户id
2. 中文姓名
3. 身份证号
4. 座机号
5. 手机号
6. 地址
7. 电子邮件
8. 密码
9. 中国大陆车牌，包含普通车辆、新能源车辆
10. 银行卡

整体来说，所谓脱敏就是隐藏掉信息中的一部分关键信息，用`*`代替，自定义隐藏可以使用`StrUtil.hide`方法完成。

- `DesensitizedUtil.address(String address,int sensitiveSize)` : 脱敏地址
  - `address` - 家庭住址
  - `sensitiveSize` - 敏感信息长度

```java
/**
 *  DesensitizedUtil.address(String address,int sensitiveSize) : 脱敏地址 
 *  
 */
@Test
public void addressTest() {
    String address = "北京市海淀区中国人寿科技园";
    String address1 = DesensitizedUtil.address(address, 2);
    // 北京市海淀区中国人寿科**
    System.out.println(address1);
}
```

- `DesensitizedUtil.email(String email)` : 脱敏电子邮箱, 邮箱前缀仅显示第一个字母，前缀其他隐藏，用星号代替，@及后面的地址显示，比如：d**@126.com

```java
/**
 * DesensitizedUtil.email(String email):脱敏电子邮箱, 邮箱前缀仅显示第一个字母，前缀其他隐藏，用星号代替，@及后面的地址显示，比如：d**@126.com
 */
@Test
public void emailTest() {
    String email = "2487422771@qq.com";
    // 2*********@qq.com
    System.out.println(DesensitizedUtil.email(email));
}
```

- `DesensitizedUtil.password(String password)` : 脱敏密码,密码的全部字符都用*代替

```java
/**
 * DesensitizedUtil.password(String password):脱敏密码,密码的全部字符都用*代替
 */
@Test
public void passwordTest() {
    String password = "123456789";
    // *********
    System.out.println(DesensitizedUtil.password(password));
}
```



# 5、加密解密Crypto

加密分为三类：

1. 对称加密：常见的有AES、DES
2. 非对称加密：常见的有RSA、DSA
3. 摘要加密：常见的有MD5















































