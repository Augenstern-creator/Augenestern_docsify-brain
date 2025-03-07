# 1、什么是DMZ区域

## 1.1、概念

DMZ 区域是为了解决**安装防火墙后外部网络的访问用户不能访问内部网络服务器的问题**，而设立的一个非安全系统与安全系统之间的缓冲区。

- 该缓冲区位于**企业内部网络**和**外部网络之间**的小网络区域内。
- 在这个小网络区域内可以放置一些必须公开的服务器，如Web服务器、FTP服务器
- 另一方面，通过这样一个DMZ区域，更加有效地保护了内部网络。因为这种网络部署，比起一般的防火墙方案，对来自外网的攻击者来说又多了一道关卡。

> 两个防火墙之间的空间被称为 DMZ 区域。





## 1.2、内网、DMZ、外网

![](workstudy_DMZ.assets/1.png)

- 安全级别最高的LAN Area （内网） 
- 安全级别中等的DMZ区域
- 安全级别最低的Internet区域（外网）

三个区域因担负不同的任务而拥有不同的访问策略。









## 1.3、DMZ的访问控制策略

我们在配置一个拥有DMZ区的网络的时候，通常定义以下的访问控制策略以实现DMZ区的屏蔽功能：

![](workstudy_DMZ.assets/2.png)

1. **内网可以访问外网**：内网的用户显然需要自由地访问外网。在这一策略中，**防火墙需要进行源地址转换**
2. **内网可以访问DMZ**：此策略是为了方便内网用户使用和管理DMZ中的服务器
3. **外网不能访问内网**：很显然，内网中存放的是公司内部数据，这些数据不允许外网的用户进行访问
4. **外网可以访问DMZ**：DMZ中的服务器本身就是要给外界提供服务的，所以外网必须可以访问DMZ。同时，外网访问DMZ需要由防火墙完成对外地址到服务器实际地址的转换。
5. **DMZ不能访问内网**：很明显，如果违背此策略，则当入侵者攻陷DMZ时，就可以进一步进攻到内网的重要数据。
6. **DMZ不能访问外网**：此条策略也有例外，比如DMZ中放置邮件服务器时，就需要访问外网，否则将不能正常工作。





## 1.4、案例

![](workstudy_DMZ.assets/3.png)

如上图，假如是一个行业云平台，那么其网络具象如上：

1. 网络区域分为外网区、DMZ区、内网区，其中 云 = DMZ区+ 内网区
2. 每个分隔出来的区域就代表一个VPC，如数字人资DMZ区VPC、数字人资内网区VPC....
3. VPC和VPC之间要连通必须满足三个条件：
   1. **建立高速通道**
   2. **建立路由**
   3. **开放安全组**
4. VPC内部的服务器连通性是由安全组限制的，例如数字人资内网区VPC里面有5台服务器，他们同属于一个安全组下。如果要让数字人资的其中3台服务器到微隔离内网区VPC的微隔离服务器连通，则：
   1. 首先打通数字人资内网区VPC到微隔离内网区VPC
   2. 数字人资3台服务器到微隔离server端开放安全组
5. 运维区和内网区是连通的，运维区VPC里面放蓝鲸提单平台、云上堡垒机等，从而保证通过堡垒机可以登录到任何服务器。























