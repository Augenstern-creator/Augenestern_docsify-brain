# 1、站点统计

## 1.1、51LA

1. 进入51la：https://www.51.la/
2. 注册登录 
3. 产品 - 网站统计V6 - 应用管理

![](WebCount.assets/1.png)



![](WebCount.assets/2.png)



4. 复制代码到个人站点即可

![](WebCount.assets/3.png)







## 1.2、百度统计

1. 进入百度统计：https://tongji.baidu.com/

2. 注册登录
3. 新建网站

![](WebCount.assets/4.png)



4. 获取到申请的id

![](WebCount.assets/5.png)





5. 在index.html配置

```html
<script>
window.$docsify = {
  // 百度统计ID
  baiduTjId: "xxxxxxx",
}
</script>

<!-- body -->
<script src="https://unpkg.com/docsify-baidu-tj@1.0.2/dist/docsify-baidu-tj.min.js"></script>
```

