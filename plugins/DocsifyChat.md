# 1、聊天记录

## 1.1、配置
1. 在 index.html 中添加 docsify-chat，必须在 docsify 之后引入

```html
<!-- Docsify Chat -->
<script src="//cdn.jsdelivr.net/npm/docsify-chat/lib/docsify-chat.min.js"></script>
```

2. 配置
```javascript
<script>
  window.$docsify = {
    // ...
    chat: {
      // 聊天标题
      title: '聊天记录',
      // 设置头像
      users: [],
      // 在右侧显示消息（自己发出的）
      myself: 'yuki',
      // 动画延时 （毫秒）
      animation: 50,
      // 面板导航栏风格，支持 "mac" 与 "windows"
      os: 'mac',
    },
  };
</script>
```


3. 使用
使用标题 + 粗体标记在聊天面板中定义消息,标题文本将被用作用户昵称，后续所有内容都将视为对话框内容，直到下一个标题或 chat:end 标记结束
```markdown
<!-- chat:start -->

#### **yuki**

hello

#### **kokkoro**

hello world

<!-- chat:end -->
```


如果未指定用户头像，则默认情况下将显示昵称的首字母。

<!-- chat:start -->

#### **yuki**

hello

#### **kokkoro**

hello world

<!-- chat:end -->


> [!NOTE]
> 官网：[DocsifyChat](https://github.com/xueelf/docsify-chat/blob/master/README.zh.md)
