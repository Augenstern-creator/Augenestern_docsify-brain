# 1、暗黑模式切换
## 1.1、配置
1. `index.html`配置如下
```javascript
window.$docsify = {
	// dark mode
    themeColor: "#42b983",
}
```

2. 引入一份css

```css
:root {
  --text_color: var(--theme-color);
  --docsify_dark_mode_bg: white;
  --docsify_dark_mode_btn: var(--theme-color);
}
html,
body,
main,
.sidebar-toggle,
.sidebar,
aside {
  background: var(--docsify_dark_mode_bg);
}

#dark_mode > input[type='checkbox'] {
  height: 0;
  width: 0;
  visibility: hidden;
}
/*主要修改这个进行修改按钮位置*/
#dark_mode > label {
  cursor: pointer;
  text-indent: -9999px;
  width: 60px;
  height: 30px;
  background: var(--docsify_dark_mode_btn);
  margin: 40px auto;
  display: flex;
  justify-content: center;
  align-items: center;
  -webkit-border-radius: 100px;
  -moz-border-radius: 100px;
  border-radius: 100px;
  position: relative;
}

#dark_mode > label:after {
  content: '';
  background: #fff;
  width: 20px;
  height: 20px;
  -webkit-border-radius: 50%;
  -moz-border-radius: 50%;
  border-radius: 50%;
  position: absolute;
  top: 5px;
  left: 4px;
  transition: cubic-bezier(0.68, -0.55, 0.27, 1.55) 320ms;
}

#dark_mode > input:checked + label {
  background: var(--docsify_dark_mode_btn);
}

#dark_mode > input:checked + label:after {
  left: calc(100% - 5px);
  -webkit-transform: translateX(-100%);
  -moz-transform: translateX(-100%);
  -ms-transform: translateX(-100%);
  -o-transform: translateX(-100%);
  transform: translateX(-100%);
}

html.transition,
html.transition *,
html.transition *:before,
html.transition *:after {
  transition: cubic-bezier(0.68, -0.55, 0.27, 1.55) 420ms !important;
  transition-delay: 0 !important;
}

#dark_mode {
  position: absolute;
  right: 0;
  top: 0;
}
p {
  color: var(--text_color);
}
```

3. 引入一份js

```javascript
<script src="//cdn.jsdelivr.net/npm/docsify-dark-mode@latest/dist/index.min.js"></script>
```

> 官网：https://docsify-plugin.vercel.app/#/





































