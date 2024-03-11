# 1、Github美化



1. 在 GitHub 中创建一个与 GitHub ID 同名的仓库，例如我的 GitHub ID 为 Augenstern-creator，因此创建的仓库名也为 Augenstern-creator。

![](Github美化.assets/4.png)

![](Github美化.assets/5.png)



2. 点击编辑按钮，可以将里面的内容全部删掉

![](Github美化.assets/6.png)



![](Github美化.assets/7.png)







## 1.1、计数插件

1. 进入计数插件：https://count.getloli.com/

![](Github美化.assets/1.png)



2. 复制上述md语法，将其中的`name`更换为自己的仓库名称，例如我的仓库名称为 Augenestern-creator

```markdown
![:Augenestern-creator](https://count.getloli.com/get/@:Augenestern-creator)
```

3. 当然这还没有结束，我们还需要加上主题，只需要在其后加上 `?theme=主题名`

```markdown
![:Augenestern-creator](https://count.getloli.com/get/@:Augenestern-creator?theme=主题名)
```

![](Github美化.assets/2.png)



4. 最终我选择的为最后一个主题，所以我的全部md语法如下：

```markdown
![:Augenestern-creator](https://count.getloli.com/get/@:Augenestern-creator?theme=gelbooru-h)
```

这样我们将其复制到我们仓库下的 `README.md` 下，我们可以点击`Preview`预览

![](Github美化.assets/3.png)

















## 1.3、技术栈生成器

1. 打开技术栈插件：https://rahuldkjain.github.io/gh-profile-readme-generator/

![](Github美化.assets/8.png)



![](Github美化.assets/9.png)



2. 最终点击 `Generate README`

![](Github美化.assets/10.png)



3. 将复制的 markdown 语法粘贴进我们自己的仓库 README.file 文件中，效果就生成了。

![](Github美化.assets/11.png)













## 1.4、制作徽章

1. 进入[Shields.io](https://shields.io/),下滑找到 Your BADGE。例如我从左到右的三条短横线上填写：写作工具、Typora、以及从颜色库中挑选一个颜色（这个颜色决定了第二个文本 VS Code 的背景色），最后点击 Make Badge

![](Github美化.assets/12.png)

2. 点击之后会弹出一个网页，页面会返回生成的 svg 图片，效果如下图所示，觉得满意的话，复制页面地址栏的网址。

![](Github美化.assets/13.png)



3. 将其粘贴进我们的 MD 文件中，格式如下

```markdown
![](https://img.shields.io/badge/%E5%86%99%E4%BD%9C%E5%B7%A5%E5%85%B7-Typora-yellowgreen)
```

![](Github美化.assets/14.png)















## 1.4、统计卡片

1. 打开统计插件：https://github.com/anuraghazra/github-readme-stats/blob/master/docs/readme_cn.md

![](Github美化.assets/15.png)



```markdown
[![这里写你的昵称's GitHub stats](https://github-readme-stats.vercel.app/api?username=这里替换成你的 GitHub ID)](https://github.com/anuraghazra/github-readme-stats)
```

例如我的就是：

```markdown
[![Augenstern-creator's GitHub stats](https://github-readme-stats.vercel.app/api?username=Augenstern-creator)](https://github.com/anuraghazra/github-readme-stats)
```

![](Github美化.assets/16.png)



当然继续通读文档，我们可以设置图标、主题等等，这里就不多做记录了。









## 1.5、metrics

1. 打开[metrics](https://metrics.lecoq.io/)

![](Github美化.assets/17.png)

2. 点击 Markdown code

![](Github美化.assets/18.png)



当然我们也可以使用 HTML 写法，因为在 md 语法中，这条代码的本质为 img 图片

```html
<div align="center"> <img src="https://metrics.lecoq.io/sun0225SUN?template=classic&config.timezone=Asia%2FShanghai"> </div>
```

![](Github美化.assets/19.png)





## 1.6、GitHub资料奖杯

1. 打开[GitHub资料奖杯](https://github.com/ryo-ma/github-profile-trophy/)

![](Github美化.assets/20.png)

当然我还是以我自己的为例：

markdown语法如下：

```markdown
[![trophy](https://github-profile-trophy.vercel.app/?username=Augenstern-creator&theme=onedark)](https://github.com/ryo-ma/github-profile-trophy)
```

HTML语法如下：

```html
<div align="center"> <img src="https://github-profile-trophy.vercel.app/?username=Augenstern-creator&theme=onedark"> </div>
```

效果如下：

![](Github美化.assets/21.png)











## 1.7、GitHub 访客徽章

1. 打开[GitHub 访客徽章](https://visitor-badge.glitch.me/)

![](Github美化.assets/22.png)



```markdown
![visitors](https://visitor-badge.glitch.me/badge?page_id=你的GitHub 仓库ID&left_color=green&right_color=red)
```

依然是以我的为例：

Markdown语法如下：

```markdown
![visitors](https://visitor-badge.glitch.me/badge?page_id=Augenstern-creator&left_color=green&right_color=red)
```

HTML语法如下：

```html
<div align="center"> <img src="https://visitor-badge.glitch.me/badge?page_id=Augenstern-creator&left_color=green&right_color=red"> </div>
```

![](Github美化.assets/23.png)

当然上述代码的颜色可以自己进行修改。



## 1.8、GitHub 活动统计图

1. 打开[GitHub Readme Activity Graph](https://github.com/Ashutosh00710/github-readme-activity-graph/)

![](Github美化.assets/24.png)

```
[![你的GitHub仓库ID's github activity graph](https://activity-graph.herokuapp.com/graph?username=你的GitHub仓库ID&theme=dracula)](https://github.com/ashutosh00710/github-readme-activity-graph)
```

依然是以我的为例：

Markdown语法：

```markdown
 [![Augenstern-creator's github activity graph](https://activity-graph.herokuapp.com/graph?username=Augenstern-creator&theme=dracula)](https://github.com/ashutosh00710/github-readme-activity-graph)
```

HTML语法：

```html
<img src="https://activity-graph.herokuapp.com/graph?username=Augenstern-creator&theme=dracula">
```

效果如下：

![](Github美化.assets/25.png)



当然，Github上主题很多，选择自己喜欢的主题即可。









## 1.9、GitHub 连续打卡

1. 打开[GitHub streak](https://github.com/DenverCoder1/github-readme-streak-stats)

![](Github美化.assets/26.png)



```mark
[![GitHub Streak](https://github-readme-streak-stats.herokuapp.com/?user=你的GitHub仓库ID&theme=dark)](https://git.io/streak-stats)
```

依然是以我的为例：

Markdown语法：

```markdown
 [![GitHub Streak](https://github-readme-streak-stats.herokuapp.com/?user=Augenstern-creator&theme=dark)](https://git.io/streak-stats)
```



HTML语法：

```html
<div align="center"> <img src="https://github-readme-streak-stats.herokuapp.com/?user=Augenstern-creator&theme=dark)](https://git.io/streak-stats"> </div>
```

效果如下：

![](Github美化.assets/27.png)







## 1.10、社交统计

1. 打开[ stats-cards](https://github.com/songquanpeng/stats-cards)

![](Github美化.assets/28.png)



我以CSDN为例：

```html
<div align="center"> <img src="https://stats.justsong.cn/api/csdn?id=Augenstern_QXL&theme=dark"> </div>
```

![](Github美化.assets/29.png)



## 1.11、打字特效

1. 打开[Readme Typing SVG](https://github.com/DenverCoder1/readme-typing-svg)

![](Github美化.assets/30.png)



```markdown
[![Typing SVG](https://readme-typing-svg.herokuapp.com/?lines=第一句话;第二句话)](https://git.io/typing-svg)
```

我这里简单写两句话做展示：

```html
<h1 align="center"> <img src="https://readme-typing-svg.herokuapp.com/?lines=这世界那么多人;致敬奋斗路上劈星斩月的你!&center=true&size=27"> </h1>
```











































