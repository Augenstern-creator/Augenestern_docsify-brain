# 1、开启封面

1. 在`index.html`中开启封面

```html
<script>
  window.$docsify = {
    // 开启封面 
    coverpage: true,
    // 封面和内容单独显示  
    onlyCover: true,  
  }
</script>
```

2. 根目录下创建`_coverpage.md` 文件

```markdown

![logo](static/icon/icon.svg)

# 生命是有光的 <small>V1.0</small>

> 前端の小窝

- 📖边角料构成的小小世界
- 🎄有趣 有品 有种

[WebBlog](https://blog.csdn.net/Augenstern_QXL)
[Get Started](/Hello.md)
```

> [!NOTE]
> - 只开启封面的的话，向下滑动可以看到内容，也就是说封面和 `README` 内容在一个网页内
> - `onlyCover` 属性可以将封面和 `README` 内容单独显示，通过封面的 `Get Started` 到达 `README` 内容页面