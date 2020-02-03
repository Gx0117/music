# 第一节课: 基本环境搭建

## 安装 Vue-Cli

```bash
npm install -g @vue/cli
```

## 创建项目

![image-20191214203244805](C:\Users\natur\AppData\Roaming\Typora\typora-user-images\image-20191214203244805.png)

选择默认配置即可



## 基本项目结构

![image-20191214211320105](C:\Users\natur\AppData\Roaming\Typora\typora-user-images\image-20191214211320105.png)

src目标是主目录

1. api 存放与后端交互用的接口
2. common 存放一些通用的素材,比如图片, 字体, 样式, 和js
3. components 存放其他的相关组件
4. router 存放路由内容
5. store  存放vuex的状态数据
6. 父组件 App.vue
7. 入口文件 main.js

## 安装基本插件

###  安装stylus-loader

本项目使用stylus预处理器来完成css的编写, 因此需要引入stylus-loader

```bash
npm install stylus-loader stylus --save
```

![image-20191214213501565](C:\Users\natur\AppData\Roaming\Typora\typora-user-images\image-20191214213501565.png)

我们修改一下App.vue和main.js, 查看一下stylus的编译结果

![image-20191214213906623](C:\Users\natur\AppData\Roaming\Typora\typora-user-images\image-20191214213906623.png)



样式已经起作用了

### 安装 fastclick

在移动端上, 存在300ms点击判断延迟, 这个原因是因为手机浏览器需要判断用户是双击放大还是单击, 为了去掉和这个300ms延迟, 我们就得安装fastclick依赖

```bash
npm install  fastclick  --save 
```

main.js的代码如下

```js
import Vue from 'vue'
import App from './App.vue'
import attachFastClick from "fastclick" //引入fastclick解决移动端300ms延迟问题
import "./common/stylus/index.styl" //引入 主stylus文件
attachFastClick.attach(document.body)//把该插件作用于整个body上
Vue.config.productionTip = false //阻止启动生产消息
new Vue({
  render: h => h(App),
}).$mount('#app')
```



## 1:  完成主页基本框架

![image-20191214220713902](C:\Users\natur\AppData\Roaming\Typora\typora-user-images\image-20191214220713902.png)![image-20191214220657332](C:\Users\natur\AppData\Roaming\Typora\typora-user-images\image-20191214220657332.png)



主页的基本结构设计

由三个大部分

1. 头部组件
2. 选项卡组件
3. 不同的选项对应的子组件

### 完成头部组件

![image-20191216140327144](C:\Users\natur\AppData\Roaming\Typora\typora-user-images\image-20191216140327144.png)

<center>项目的目录结构</center>
m-header的代码如下

```Vue
<template>
	<div class="m-header">
		<div class="icon"></div>
		<h1 class="text">云帆音乐</h1>
	</div>
</template>
<script>
</script>
<style scoped lang="stylus">
  @import "../../common/stylus/variable"
  @import "../../common/stylus/mixin"

  .m-header
    position: relative
    height: 44px
    text-align: center
    color: $color-theme
    font-size: 0
    .icon
      display: inline-block
      vertical-align: top
      margin-top: 6px
      width: 30px
      height: 32px
      margin-right: 9px
      bg-image('logo')
      background-size: 30px 32px
    .text
      display: inline-block
      vertical-align: top
      line-height: 44px
      font-size: $font-size-large
    .mine
      position: absolute
      top: 0
      right: 0
      .icon-mine
        display: block
        padding: 12px
        font-size: 20px
        color: $color-theme
</style>
```

App.vue的代码如下

```vue
<template>
  <div id="app">
	<m-header></m-header>
  </div>
</template>
<script>
	import MHeader from "./components/m-header/m-header.vue"
	export default{
		components:{
			MHeader
		}
	}
</script>
<style lang="stylus" scoped >

</style>
```

### 完成选项卡部分的组件和路由配置

第一步:安装vue-router组件

```bash
npm install vue-router
```

![image-20191216163453083](C:\Users\natur\AppData\Roaming\Typora\typora-user-images\image-20191216163453083.png)

![image-20191216164039707](C:\Users\natur\AppData\Roaming\Typora\typora-user-images\image-20191216164039707.png)

增加一些子组件,当前组件内容只要设置一些内容即可,用于提示

例: recommend组件

```vue
<template>
	<div>
		推荐页面
	</div>
</template>

<script>
</script>

<style>
</style>

```

index.js的主要代码

```javascript
import Vue from "vue"
import Router from "vue-router"
import Rank from "../components/rank/rank.vue"
import Search from "../components/search/search.vue"
import Singer from "../components/singer/singer.vue"
import Recommend from "../components/recommend/recommend.vue"
//引入基本子组件
Vue.use(Router) //引用Router中间件
export default new Router({
	routes: [{
		path: '/',
		redirect: '/recommend' // 默认跳转到推荐页面
	}, {
		path: "/rank",
		component: Rank,
	}, {
		path: "/search",
		component: Search
	}, {
		path: "/singer",
		component: Singer
	}, {
		path: "/recommend",
		component: Recommend
	}]
})
```

然后在main.js里面引入这个路由

```javascript
import Vue from 'vue'
import App from './App.vue'
import Router from "./router/index.js"// 引入router
import attachFastClick from "fastclick" //引入fastclick解决移动端300ms延迟问题
import "./common/stylus/index.styl" //引入 主stylus文件

attachFastClick.attach(document.body)//把该插件作用于整个body上

Vue.config.productionTip = false //阻止启动生产消息
new Vue({
  render: (h) => h(App),
  router:Router// 调用router数据
}).$mount('#app')

```

接下来编写tab组件

tab.vue

```vue
<template>
  <div class="tab">
    <router-link tag="div" class="tab-item" to="/recommend">
      <span class="tab-link">推荐</span>
    </router-link>
    <router-link tag="div" class="tab-item" to="/singer">
      <span class="tab-link">歌手</span>
    </router-link>
    <router-link tag="div" class="tab-item" to="/rank">
      <span class="tab-link">排行
      </span>
    </router-link>
    <router-link tag="div" class="tab-item" to="/search">
      <span class="tab-link">搜索</span>
    </router-link>
  </div>
</template>

<script type="text/ecmascript-6">
  export default {}
</script>

<style scoped lang="stylus" rel="stylesheet/stylus">
  @import "~common/stylus/variable"
  .tab
    display: flex
    height: 44px
    line-height: 44px
    font-size: $font-size-medium
    .tab-item
      flex: 1
      text-align: center
      .tab-link
        padding-bottom: 5px
        color: $color-text-l
      &.router-link-active
        .tab-link
          color: $color-theme
          border-bottom: 2px solid $color-theme
</style>
```

接下来配置app.vue父组件

```vue
<template>
	<div id="app">
		<m-header></m-header>
		<tab></tab>
		<router-view></router-view>
	</div>
</template>
<script>
	import MHeader from "./components/m-header/m-header.vue"
	import Tab from "./components/tab/tab.vue"
	// 当然为了节约性能, 我们完全可以吧router-view放在一个keep-alive里面
	export default{
		components:{
			MHeader,
			Tab
		}
	}
</script>

<style scoped lang="stylus">

</style>

```



![image-20191216195712005](C:\Users\natur\AppData\Roaming\Typora\typora-user-images\image-20191216195712005.png)

## 2:完成推荐页部分

### 数据的获取

我们扒小马哥的乐库, 因为不扒白不扒, 薅资本主义羊毛

这里我们要准备的有, 后端数据库, 前后端数据交互工具

```bash
npm install request --save
npm install mongoose --sava
```

![image-20191216210348292](C:\Users\natur\AppData\Roaming\Typora\typora-user-images\image-20191216210348292.png)

我们通过数据分析可以得知, qq音乐使用过ajax进行前后端数据通讯的, 所以我们可以模拟这个请求数据来获得我们想要的数据, 然后将数据存储在数据库中, 方便我们自己后期去调用

经过数据分析,得到主页数据的接口

```
https://u.y.qq.com/cgi-bin/musicu.fcg?cgiKey=GetHomePage&_=1576499692284&data={%22comm%22:{%22g_tk%22:155916146,%22uin%22:647789540,%22format%22:%22json%22,%22inCharset%22:%22utf-8%22,%22outCharset%22:%22utf-8%22,%22notice%22:0,%22platform%22:%22h5%22,%22needNewCode%22:1},%22MusicHallHomePage%22:{%22module%22:%22music.musicHall.MusicHallPlatform%22,%22method%22:%22MobileWebHome%22,%22param%22:{%22ShelfId%22:[101,102,161]}},%22hotkey%22:{%22module%22:%22tencent_musicsoso_hotkey.HotkeyService%22,%22method%22:%22GetHotkeyForQQMusicMobile%22,%22param%22:{%22remoteplace%22:%22txt.miniapp.wxada7aab80ba27074%22,%22searchid%22:%221559616839293%22}}}
```

获取最终返回的数据格式

```js
const mongoose =require("mongoose");
const request = require("request")
const fs= require("fs")
const path= require("path")

request({
    method: "GET",
    url: "https://u.y.qq.com/cgi-bin/musicu.fcg",
    qs: {
        "cgiKey": "GetHomePage",
        "_": 1576499692284,
        "data": `{ "comm": { "g_tk": 155916146, "uin": 647789540, "format": "json", "inCharset": "utf-8", "outCharset": "utf-8", "notice": 0, "platform": "h5", "needNewCode": 1 }, "MusicHallHomePage": { "module": "music.musicHall.MusicHallPlatform", "method": "MobileWebHome", "param": { "ShelfId": [101, 102, 161] } }, "hotkey": { "module": "tencent_musicsoso_hotkey.HotkeyService", "method": "GetHotkeyForQQMusicMobile", "param": { "remoteplace": "txt.miniapp.wxada7aab80ba27074", "searchid": "1559616839293" } } }`
    }
},(err,req,body)=>{
    fs.writeFile(`${__dirname}/json/recommend.json`,body,{
        encoding:"utf8"
    },function(err){
        if(err) throw err;
        console.log("写入成功")
    })
})
```

最终格式为

![image-20191216212820278](C:\Users\natur\AppData\Roaming\Typora\typora-user-images\image-20191216212820278.png)

经过一番分析,我们可以得知,主页推荐数据存储在MusicHallHomePage这个属性里面, 里面有三个部分的数据:分别是官方歌单, 达人歌单, 以及专区

具体的数据获取为

```
主页返回的三个推荐专区的数组数据为: req.MusicHallHomePage.data.v_shelf, 我们设为data
专区的名称: data[i].title_template
该专区更多的数据信息在:data[i].more
该专区首页显示的数据数组在:data[i].v_niche[0].v_card, 我们把这个数据记为datalist
datalist[i].id: 通过这个id我们可以访问某个歌单的具体音乐信息列表
datalist[i].title: 这个数据记录了这个歌单的名字
datalist[i].cover: 这个数据记录了歌单的封面
```

我们的推荐数据获取部分的结构如下

![image-20191217205707544](C:\Users\natur\AppData\Roaming\Typora\typora-user-images\image-20191217205707544.png)

其中recommendTable.js负责搭建表规则

```javascript
const mongoose = require("mongoose");
const Schema = mongoose.Schema;
mongoose.connect("mongodb://127.0.0.1:27017/music", {
    useNewUrlParser: true,
    useUnifiedTopology: true
}).then(() => {
    console.log("连接成功")
}).catch(() => {
    console.log("连接失败")
});
let recommendShema = new Schema({
    category: {
        required: true,
        type: String
    },
    categoryList: [
        {
            id: {
                type: String,
                required: true
            },
            title: {
                type: String,
                required: true
            },
            cover: {
                type: String,
                required: true
            }
        }
    ]
})
let recommendDate = mongoose.model("recommendDate", recommendShema);
module.exports={//导出表
    recommendTable:recommendDate
}
```

setrecommendData.js负责爬取数据并存储数据

````javascript
const request = require("request")
const {recommendTable} = require("./recommendTable")

request({
    method: "GET",
    url: "https://u.y.qq.com/cgi-bin/musicu.fcg",
    qs: {
        "cgiKey": "GetHomePage",
        "_": 1576499692284,
        "data": `{ "comm": { "g_tk": 155916146, "uin": 647789540, "format": "json", "inCharset": "utf-8", "outCharset": "utf-8", "notice": 0, "platform": "h5", "needNewCode": 1 }, "MusicHallHomePage": { "module": "music.musicHall.MusicHallPlatform", "method": "MobileWebHome", "param": { "ShelfId": [101, 102, 161] } }, "hotkey": { "module": "tencent_musicsoso_hotkey.HotkeyService", "method": "GetHotkeyForQQMusicMobile", "param": { "remoteplace": "txt.miniapp.wxada7aab80ba27074", "searchid": "1559616839293" } } }`
    }
}, async(err, req, body) => {
    await recommendTable.deleteMany({});
    let data = JSON.parse(body).MusicHallHomePage.data.v_shelf;
    data.forEach((item) => {
        let category = item.title_template;
        let categoryList = item.v_niche[0].v_card;
        let arr = [];
        categoryList.forEach((list) => {
            // console.log("详细id:" + list.id);
            // console.log("歌单名词:" + list.title);
            // console.log("歌单封面地址:" + list.cover);
            if (list.time) {
                arr.push({
                    id: list.time,
                    title: list.title,
                    cover: list.cover
                })
            } else {
                arr.push({
                    id: list.id,
                    title: list.title,
                    cover: list.cover
                })
            }
        });
        recommendTable.create({
            category: category,
            categoryList: arr
        }).then(()=>{
            // eslint-disable-next-line no-console
            console.log("连接成功");
        }).catch(()=>{
          // eslint-disable-next-line no-console
            console.log("连接失败");
        })
    })
})



````

getrecommendData.js负责处理提供api接口

```javascript
const express = require("express");
const {recommendTable} = require("./recommendTable");
let app = express();
app.all("*", function (req, res, next) { //解决跨域请求问题
    res.header({
        'Access-Control-Allow-Credentials': true,
        'Access-Control-Allow-Origin': req.headers.origin || '*',
        'Access-Control-Allow-Headers': 'X-Requested-With',
        'Access-Control-Allow-Methods': 'PUT,POST,GET,DELETE,OPTIONS',
        'Content-Type': 'application/json; charset=utf-8'
    });
    if (req.method === "OPTIONS") {
        res.send(200)
        // eslint-disable-next-line no-console
        console.log("has option")
    } else {
        next()
    }
});
app.get("/api/recommenddata", function (req, res) {
    recommendTable
        .find({},{
            __v:false,
            _id:false
        })
        .then((data) => {
            // eslint-disable-next-line no-console
            console.log("查询成功");
            res.send(JSON.stringify(data))
        })
        .catch(() => {
            // eslint-disable-next-line no-console
            console.log("查询失败");
        });
});
app.listen(8080);
```

### 数据测试

我们在recommend.vue组件内来测试数据,在这之前 需要安装一个axios库

```bash
npm install axios --save
```



```vue
<template>
    <div>
        推荐页面
        {{msg}}
    </div>
</template>
<script>
    import axios from "axios"

    export default {
        data() {
            return {
                msg: null
            }
        },
        beforeCreate() {//在组件创造前请求数据,避免白屏等待
            axios.get("http://localhost:8080/api/recommenddata").then((data) => {
                // eslint-disable-next-line no-console
                console.log(data.data);
                this.msg = data.data;
            })
        }
    }
</script>
<style>
</style>

```

![image-20191217210449906](C:\Users\natur\AppData\Roaming\Typora\typora-user-images\image-20191217210449906.png)