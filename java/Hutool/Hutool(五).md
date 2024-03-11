# 1、系统属性调用SystemUtil

- `SystemUtil.getJavaRuntimeInfo()` : 获得java运行时信息，包括 Java 版本、Java安装目录
- `SystemUtil.getHostInfo()` : 获得用户信息: 主机名和主机IP地址

```java
 @Test
    public void getJvmSpecInfoTest() {
        // java运行时信息
        System.out.println(SystemUtil.getJavaRuntimeInfo());
        // 用户信息: 主机名和主机IP地址
        // Host Name:    DESKTOP-BKR9RBG
        // Host Address: 192.168.1.7
        System.out.println(SystemUtil.getHostInfo());
    }
```





# 2、Oshi封装OshiUtil

`Oshi`是一个免费的基于JNA的获取操作系统和硬件信息的java库，Github地址是：[oshi](https://github.com/oshi/oshi)

它的优点是不需要安装任何其他本机库，并且旨在提供一种跨平台的实现来检索系统信息，例如操作系统版本，进程，内存和CPU使用率，磁盘和分区，设备，传感器等。

这个库可以监测的内容包括：

1. 计算机系统和固件，底板
2. 操作系统和版本/内部版本
3. 物理（核心）和逻辑（超线程）CPU，处理器组，NUMA节点
4. 系统和每个处理器的负载百分比和滴答计数器
5. CPU正常运行时间，进程和线程
6. 进程正常运行时间，CPU，内存使用率，用户/组，命令行
7. 已使用/可用的物理和虚拟内存
8. 挂载的文件系统（类型，可用空间和总空间）
9. 磁盘驱动器（型号，序列号，大小）和分区
10. 网络接口（IP，带宽输入/输出）
11. 电池状态（电量百分比，剩余时间，电量使用情况统计信息）
12. 连接的显示器（带有EDID信息）
13. USB设备
14. 传感器（温度，风扇速度，电压）

也就是说配合一个前端界面，完全可以搞定系统监控了。使用前需要引入 Oshi 库

```xml
<dependency>
	<groupId>com.github.oshi</groupId>
	<artifactId>oshi-core</artifactId>
	<version>6.4.1</version>
</dependency>
```



- `OshiUtil.getMemory()` : 获取内存总量

```java
/**
 * 获取内存总量
 */
@Test
public void getMemoryTest(){
    GlobalMemory memory = OshiUtil.getMemory();
    // Available: 21.4 GiB/31.7 GiB
    System.out.println(memory);

}
```

- `OshiUtil.getCpuInfo()` : 获取系统CPU 系统使用率、用户使用率、利用率等等 相关信息

```java
/**
 * 获取系统CPU 系统使用率、用户使用率、利用率等等 相关信息
 */
@Test
public void getCpuInfoTest() {
    CpuInfo cpuInfo = OshiUtil.getCpuInfo();
    // CpuInfo{CPU核心数=16, CPU总的使用率=16324.0, CPU系统使用率=0.38, CPU用户使用率=0.57, CPU当前等待率=0.0, CPU当前空闲率=98.96, CPU利用率=1.04, CPU型号信息='13th Gen Intel(R) Core(TM) i5-13490F
    System.out.println(cpuInfo);
}
```

- `OshiUtil.getCurrentProcess()`: 获取当前进程信息

```java
/**
 * 获取当前进程信息
 */
@Test
public void getCurrentProcessTest() {
    OSProcess currentProcess = OshiUtil.getCurrentProcess();
    // OSProcess@21de60b4[processID=12284, name=java]
    System.out.println(currentProcess);
}
```



- `OshiUtil.getOs()`:获取操作系统相关信息，包括系统版本、文件系统、进程等

```java
/**
 * 获取操作系统相关信息，包括系统版本、文件系统、进程等
 */
@Test
public void getOsTest() {
    OperatingSystem os = OshiUtil.getOs();
    // Microsoft Windows 8.1 build 22631
    System.out.println(os);
}
```

- `OshiUtil.getSensors()` :获取传感器相关信息，例如CPU温度、风扇转速等，传感器可能有多个

```java
/**
 * 获取传感器相关信息，例如CPU温度、风扇转速等，传感器可能有多个
 */
@Test
public void getSensorsTest() {
    Sensors sensors = OshiUtil.getSensors();
    // CPU Temperature=0.0°C, Fan Speeds=[], CPU Voltage=0.0
    System.out.println(sensors);
}
```































































































