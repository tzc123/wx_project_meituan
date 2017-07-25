## 微信小程序（仿美团外卖）
   初识小程序，为它的小巧玲珑所吸引，不由得心血来潮。这不正是用户所需要的吗？既方便快捷，又不占手机内存。所以我下定决心一定要做出一个自己的小程序，然后
赚钱、赚钱、赚钱...当然现在只是学习阶段，所以先仿一个高端产品来挑战自我吧。说到高端，自然然而的就想到了美团。之后噼里啪啦一顿忙乎，终于做出了一点样子
来，希望能为同为小白的同学们提供一点帮助和参考，现在我们进入正题。
## 开发工具
1.微信web开发者工具： 官网就可以下载，相信大家早就安装好了吧。<br>
2.小程序 API： 官网提供的文档，不懂得地方多看两遍基本上就可以解决了。<br>
3.[Easy Mock](https://www.easy-mock.com/)： 一个能够提供虚拟数据接口的网站，在前端独自开发的情况下，实在是再好不过的工具了。<br>
## 功能
已经实现的功能：<br>
1.主界面<br>
2.订单界面<br>
3.用户界面<br>
4.点单功能<br>
5.定位功能<br>
未实现的功能：<br>
数都数不清<br>
毕竟大企业的产品，不是说模仿就模仿的，所以只实现了一些主要的功能，和一些能力之内的功能...
## 项目启动
创建界面：
```
  "pages":[
    "pages/home/home",
    "pages/menu/menu",
    "pages/location/location",
    "pages/my/my",
    "pages/order/order"
  ],
```
只要编辑app.js中的pages属性，就会在项目目录下的pages文件夹里自动生成一个文件夹，里面包括了 *.wxml、*.wxss、*.json、*.js这样四个文件。wxml就是界面
结构文件，wxss就是样式文件,js是用来存放js代码并实现界面逻辑的地方，至于*.json就是用来配置界面属性的地方，如：修改标题栏的颜色，和文字。

配置标题栏的样式：
```
  "window":{
    "navigationBarTitleText": "美团外卖+",
    "navigationBarTextStyle": "white",
    "navigationBarBackgroundColor": "#FFC640"
  },
```
同样是在app.json中配置，其他页面的标题栏都以此为例。
 
添加底栏：
```
"tabBar": {
    "color": "#272636",
    "selectedColor": "#FFD161",
    "backgroundColor": "#fff",
    "borderStyle": "#a8a8a8",
    "list": [
      {
        "pagePath": "pages/home/home",
        "iconPath": "pages/images/home.png",
        "selectedIconPath": "pages/images/home-selected.png",
        "color":"white",
        "text": "首页"
      },
      {
        "pagePath": "pages/order/order",
        "iconPath": "pages/images/order.png",
        "selectedIconPath": "pages/images/order-selected.png",
        "text": "订单"
      },
      {
        "pagePath": "pages/my/my",
        "iconPath": "pages/images/my.png",
        "selectedIconPath": "pages/images/my-selected.png",
        "text": "我的"
      }
    ]
  }
```
在app.json中编写以上代码，这是小程序自带的功能，只需要照搬照抄就可以了，极其方便，效果如下：
![gif](https://github.com/tzc123/wx_project/raw/master/gif/GIF.gif)



  


