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
* 创建界面：
```
  "pages":[
    "pages/home/home",
    "pages/menu/menu",
    "pages/location/location",
    "pages/my/my",
    "pages/order/order"
  ],
```
   只要编辑app.js中的pages属性，就会在项目目录下的pages文件夹里自动生成一个文件夹，里面包括了*.wxml、*.wxss、*.json、*.js这样四个文件。wxml就是界
面结构文件，wxss就是样式文件,js是用来存放js代码并实现界面逻辑的地方，至于*.json就是用来配置页面属性的地方，如：修改标题栏的颜色，和文字。

* 配置标题栏的样式：
```
  "window":{
    "navigationBarTitleText": "美团外卖+",
    "navigationBarTextStyle": "white",
    "navigationBarBackgroundColor": "#FFC640"
  },
```
   同样是在app.json中配置，其他页面的标题栏都以此为例。
 
* 添加底栏：
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
![image](https://github.com/tzc123/wx_project_meituan/raw/master/gif/GIF.gif)
* 数据请求
```
/**
   * 生命周期函数--监听页面显示
   */
  onShow: function () {
    var that = this;
    wx.request({
      url: "https://www.easy-mock.com/mock/596257bc9adc231f357c4664/restaurant/info",//easy-mock生成的虚拟数据接口链接
      method: "GET",
      success: function (res) {//成功得到数据，对数据进行处理
        that.setData({//将数据发送到data中
          restaurant: res.data.data.restaurant,
          location: wx.getStorageSync('location')
        })
      }
    });
  },
```
data是每个页面.js文件中的一个键，用来储存本页面需要用到的数据。具体使用，可在wxml文件中用{{'data中的键名'}}的形式调用数据。<br>
<br>
虚拟数据大致如下：
```
{
  "success": true,
  "data": {
    "restaurant": [{
      "name": "御膳房",
      "src": "http://i2.kiimg.com/601998/a955867016875a41.jpg",
      "star": 4.5,
      "sales": 641,
      "initial_price": 0,
      "distribution_price": 0,
      "distance": "156m",
      "time": 33
    }, {
      "name": "韩式炸鸡啤酒屋",
      "star": 4.5,
      "sales": 731,
      "src": "http://i4.piimg.com/601998/9ce47f2f19d7717d.jpg",
      "initial_price": 15,
      "distribution_price": 0,
      "distance": "1.3km",
      "time": 52
    },{
    //略去
    },{
    //略去
    },{
    //...
    }]
  }
}
```
* 主界面功能
效果图：<br>
![image](https://github.com/tzc123/wx_project_meituan/raw/master/gif/home.gif)
1.swiper控件应用<br>
   首先是两页标签的滑动切换,这里使用的是swiper，它是一款小程序自带的滑块组件，使用掌握起来非常简单，具体代码如下
   ```
  <swiper class="categoryList" indicator-dots="true" 
  indicator-color="rgba(228,228,228,1)" 
  indicator-active-color="#FECA49">
    <block wx:for="{{categoryList}}" wx:key="">
    <swiper-item>
    <block wx:for="{{item}}" wx:key="">
      <view class="category-info">
        <image src="{{item.src}}" 
        class="category-image"></image>
        <view class="category-text">{{item.name}}</view>
      </view>
    </block>
    </swiper-item>
    </block>
  </swiper>
   ```
   swiper标签就是滑块组件的主体，表示可以滑动的区域，其中indicator-dots属性是设置设置点是否显示。接下来swiper-item标签在swiper之中表示的是每一个用
来作为滑动的页面。这里用<block wx:for="{{categoryList}}" wx:key="">包裹着swiper-item表示的是使用categoryList对象数组中数据来循环渲染swiper-
item，swiper-item的数量取决于categoryList中有多少组数据。之后在swiper-item中的block标签表示的是在一个页面中用categoryList.item中的数据循环渲染多
个类似的标签，这些标签就是效果图中的类别项，总共两页，每页八个。这就是swiper和循环渲染的一些基本用法。
<br>
 2.弹出层的实现
 ```
 <view class="mask"
hidden="{{mask2Hidden}}" bindtap="mask2Cancel">
  <template is="sort_list" data="{{selected,sortSelected}}"/>
  <scroll-view class="filterList" scroll-y="true" >
    <view class="filterList-characteristic-title">商家特色</view>
    <view class="filterList-characteristic-items">
      <block wx:for="{{characteristicList}}" wx:key="">
        <view class="filterList-characteristic-item {{characteristicSelected[index]==true?'characteristic-selected':''}}"
        catchtap="characteristicSelected" data-index="{{index}}">{{item.text}}</view>
      </block>
    </view>
    <view class="filterList-discount-title">优惠活动(单选)</view>
    <view class="filterList-discount-items">
      <block wx:for="{{discountList}}" wx:key="">
        <view class="filterList-discount-item {{discountSelected==index?'discount-selected':''}}"
        catchtap="discountSelected" data-index="{{index}}">
          <text class="filterList-discount-item-icon"
          style="background:{{item.iconColor}}">{{item.icon}}</text>
        {{item.text}}</view>
      </block>
    </view>
  </scroll-view>
  <view class="filterList-footer">
    <view class="filterList-footer-delect"
    catchtap="clearSelectedNumb">清除筛选</view>
    <view class="filterList-footer-finish" bindtap="finish">完成 
    <view class="filterList-footer-finish-number" hidden="{{selectedNumb==0}}">{{selectedNumb}}
    </view>
    </view>
  </view>
</view>
 ```
   最外层的mask类的view就是一个遮罩层，用来覆盖之前的界面形成遮罩的效果，并在上面显示新的界面也就是弹出层。以上的代码就是效果图中点击筛选按钮所呈现
出来的内容了。其中bindtap属性就是点击事件的绑定了，具体的点击事件需要在.js文件中设置。值得一提的是，bindtap事件是会把当前标签受到的点击冒泡给它的父
容器，这就相当与同时点击了他的父容器，如果想阻止冒泡的话就需要使用catchtap。

* 定位界面实现
先上效果图：<br>
![image](https://github.com/tzc123/wx_project_meituan/raw/master/gif/location.gif)
 
 
 




  


