# 1、替换小章鱼

## 1.1、配置

1. 配置`index.html`

```html
<script>
  window.$docsify = {
      repo:'true', // set the docsify show the corner
      corner: {
        // the icon link url to another site  
        url: "https://gitlab.com/gitlab-org/gitlab-svgs", 
        // gitlab、spring、github、golang
        icon: "gitlab", 
      },
  };
</script>
```

2. 加载js

```javascript
<script src="//cdn.jsdelivr.net/npm/docsify-corner/dist/docsify-corner.min.js"></script>
```



> [!TIP]
>
> - [DocsifyDorner](https://github.com/Koooooo-7/docsify-corner)

































