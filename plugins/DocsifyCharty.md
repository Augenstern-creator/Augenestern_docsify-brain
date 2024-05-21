# 1、图表

## 1.1、配置

1. 在`index.html`引入
```html
<script src="//cdn.jsdelivr.net/npm/@markbattistella/docsify-charty@latest"></script>
<link rel="stylesheet" href="//cdn.jsdelivr.net/npm/@markbattistella/docsify-charty@latest/dist/docsify-charty.min.css">
```

2. 配置
```javascript
windows.$docsify = {
  //图表
  charty: {
    // 十六进制图表颜色的全局主题
    "theme": "#FE2E2E",
    // light dark
    "mode":  "dark",
    // 如果图表未加载，则控制台记录
    "debug": false
  },
}
```
3. 开始使用

> [!NOTE]
> 参考：[Docsify-Chart](https://charty.docsify.markbattistella.com/)
















