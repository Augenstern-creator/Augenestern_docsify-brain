# 1、定制化选中文字的背景颜色
选中文字默认是白字蓝底,我这边设置为选中是白字红底.

## 1.1、配置
- 在项目中新建`.css`文件并在`index.html`中引入即可. 

```css
/*文字选中背景颜色改变*/
*::selection {
  background-color: #ff0000;
  color: #fff;
}
```