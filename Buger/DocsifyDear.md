# 1


<!-- chat:start -->

#### **贾队长**
这是怎么回事卧槽！
#### **贾队长**
这个卡有bug
#### **贾队长**
我又买了瓶饮料没扣钱
#### **贾队长**
我要吃10个牛肉包！
#### **贾队长**
下次来请你吃小卖部卧槽
#### **贾队长**
他那的牛肉包和关东煮都挺好吃
#### **贾队长**
服了
#### **贾队长**
扣的我校园卡
#### **贾队长**
痛失无底洞😭
#### **贾队长**
不然请你吃关东煮了
#### **贾队长**
我要去吃碗面
#### **贾队长**
然后物色一下明天吃什么
#### **贾队长**
明天吃板烧鸡腿堡
#### **贾队长**
晚上去楼上买泡面吃
#### **贾队长**
现在先吃个橘子
#### **贾队长**
一会再吃肯德基
#### **贾队长**
西红柿也很好吃
#### **贾队长**
榴莲也好吃
#### **贾队长**
还要吃比格
#### **贾队长**
又有点想吃酸辣牛肉面
#### **贾队长**
想吃蛋糕了
#### **贾队长**
吃麻辣王子！
#### **贾队长**
再搭配个雪糕！



<!-- chat:end -->

<!-- chat:start -->
#### **贾队长**
智慧食堂也好吃！
#### **贾队长**
其实有点想吃炸串
#### **贾队长**
好久没吃席了呢
#### **贾队长**
晚上再来个烧饼夹肉
#### **贾队长**
吃个臭豆腐夹饼
#### **贾队长**
我弟弟说给我做炒方便面吃
#### **贾队长**
简单吃个披萨
#### **贾队长**
涮锅吃吃
#### **贾队长**
火锅和烤肉也好吃
#### **贾队长**
烤鱼也巨好吃
#### **贾队长**
这个生蚝好好吃我的天
#### **贾队长**
买点稻香村吃
#### **贾队长**
我要吃鸡腿饼！
#### **贾队长**
正宗石家庄板面
#### **贾队长**
安徽板面吃过没
#### **贾队长**
我感冒了，这个感冒可能得吃盒草莓的样子
#### **贾队长**
我出来吃东西啦
#### **贾队长**
我想吃小雪人
#### **贾队长**
吃个烤冷面
#### **贾队长**
哪个最好吃
#### **贾队长**
原麦山丘！
#### **贾队长**
吃菠萝啦！
#### **贾队长**
香酥肉好吃我觉得
#### **贾队长**
吃的干干净净暖暖和和
#### **贾队长**
我在吃瓜
#### **贾队长**
智慧食堂太好吃啦
#### **贾队长**
吃个金嗓子
#### **贾队长**
吃个麦麦
<!-- chat:end -->

<!-- chat:start -->
#### **秦闪闪**
吃的咋样啦
#### **贾队长**
好吃！😋
#### **贾队长**
ZZU烤冷面世界第一
#### **秦闪闪**
宝子你看下面的通行证
#### **秦闪闪**
正确回答问题
#### **秦闪闪**
即可开启
#### **贾队长**
难不倒我
#### **秦闪闪**
注意:一旦答错,所填数据全部消失了哈,没有办法恢复~
#### **贾队长**
加密的是什么内容
#### **秦闪闪**
你确定你要进去看吗
#### **贾队长**
是什么
#### **秦闪闪**
你确定以及肯定你要进去看吗
#### **秦闪闪**
一旦看了没有回头路！
#### **贾队长**
确定以及肯定！！
#### **秦闪闪**
Run!
#### **秦闪闪**
为贾队长开启通行证
#### **秦闪闪**
注意:通行证已开启50%,剩余50%请贾队长自行寻找网页中的工具,使得通行证真正被"看见"！
<!-- chat:end -->




<div id="app">  
    <div v-if="isPasswordVisible" class="form">
        <h1>通行证</h1>
        <div class="txtb">
            <label for="">Name:</label>
            <input type="text" v-model="name" placeholder="Please type in your name" required>
        </div>  
         <div class="txtb">
            <label for="">Lover:</label>
            <input type="text" v-model="lover" placeholder="Please enter your lover's name" required>
        </div>
         <div class="txtb">
            <label for="">Password:</label>
            <input type="password" v-model="password" placeholder="Enter password last" required>
        </div>
        <button @click="showContent" class="btn">查看内容</button>
    </div>  
    <div v-if="isContentVisible">  
      <h1>加密内容</h1>
      <p>这是<a href="http://49.232.28.14:999" target="_blank">加密的内容</a>，需要输入密码才能查看。</p>
      <p>宝子进来啦</p>
      <p>最后忠告：一旦进去看就不可回头了哈！</p>
      <p>宝子，恭喜你拿到美团的Offer</p>。
      <p>"Welcome to the real world！It sucks. You‘re gonna love it！"</p>
      <p>宝子，希望你越来越好，越来越棒！</p>
      <p>Tips：成品已经发货啦哈~</p>
      <p>Tips：本站惊喜内容持续更新ing...</p>
    </div>  
</div>

  

<script type="text/javascript">
    const app = Vue.createApp({
        data(){
            return {
                name: '',
                lover: '',
                password: '',
                isContentVisible: false,
                isPasswordVisible: true,
                correctName: '贾萌',
                correctLover: '秦晓林',
                correctPassword: 'qxl666nb'
            }
        },
        methods: {
            showContent() {
                if(this.name != this.correctName){
                    this.name = '';
                    this.lover = '';
                    this.password= '';
                    swal("贾队长😋再想一下","提示🔯：关于名字你会想到什么呢？","error");
                }else if(this.lover != this.correctLover){
                    this.lover = '';
                    swal("贾队长😋再想一下","提示🔯：关于恋人你会想到什么呢？","error");
                }else if(this.password != this.correctPassword){
                    swal("贾队长😋再想一下","提示🔯：关于密码你会想到什么呢？","error");
                    this.password = '';
                }else {
                    this.isContentVisible = true;
                    this.isPasswordVisible = false;
                    swal("贾队长","🥰恭喜你！","success");
                }
            }
        }
    }).mount('#app')
</script>

<style>

#app {
    display: flex;
    justify-content: center;
    align-items: center;
}

/*表单样式*/
.form {
    width: 85%;
    background-color: rgba(255, 255, 255, .05);
    transform: translate(-50%, -50%);
    padding: 40px;
    border-radius: 10px;
    box-shadow: 0 0 5px #000;
    text-align: center;
    font-family: "微软雅黑";
    color: #fff;
}

/*表单标题样式*/
.form h1 {
    margin-top: 0;
    font-weight: 200;
}

.form .txtb {
    border: 1px solid #aaa;
    margin: 8px 0;
    padding: 12px 18px;
    border-radius: 10px;
}

.txtb label {
    display: block;
    text-align: left;
    color: #fff;
    font-size: 14px;
}




.txtb input{
    width: 100%;
    background: none;
    border: none;
    outline: none;
    margin-top: 6px;
    font-size: 18px;
    color: #fff;
}







/*提交按钮样式*/
.btn {
    display: block;
    padding: 14px 0;
    color: #fff;
    cursor: pointer;
    margin-top: 8px;
    width: 100%;
    border: 1px solid #aaa;
    border-radius: 10px;
}

</style>




