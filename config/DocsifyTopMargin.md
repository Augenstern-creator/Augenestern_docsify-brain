# 1、文章顶部间距

- 让你的内容页在滚动到指定的锚点时，距离页面顶部有一定空间.
- 切换页面后是否自动跳转到页面顶部.
- 小屏设备下合并导航栏到侧边栏.

# 1.1、配置
```html
<script>
    window.$docsify = {
      // 让你的内容页在滚动到指定的锚点时，距离页面顶部有一定空间。
      topMargin: 25, // default: 0
      // 切换页面后是否自动跳转到页面顶部。
      auto2top: true,
      // 小屏设备下合并导航栏到侧边栏。
      mergeNavbar: true,
    }
</script>
```

