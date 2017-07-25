## 微信小程序（仿美团外卖）
初识小程序，为它的小巧玲珑所吸引，不由得心血来潮。这不正是用户所需要的吗？既方便快捷，又不占手机内存。所以我下定决心一定要做出一个自己的小程序，然后赚钱、赚钱、赚钱...当然现在只是学习阶段，所以先仿一个高端产品来挑战自我吧。说到高端，自然然而的就想到了美团。之后噼里啪啦一顿忙乎，终于做出了一点样子来，希望能为同为小白的同学们提供一点帮助和参考，现在我们进入正题。
## 开发工具
* 微信web开发者工具： 官网就可以下载，相信大家早就安装好了吧。<br>
* 小程序 API： 官网提供的文档，不懂得地方多看两遍基本上就可以解决了。<br>
* [Easy Mock](https://www.easy-mock.com/)： 一个能够提供虚拟数据接口的网站，在前端独自开发的情况下，实在是再好不过的工具了。<br>
## 功能
### 已经实现的功能：
* 主界面
* 订单界面
* 用户界面
* 点菜界面
* 定位界面
### 未实现的功能：
* 数都数不清，毕竟大企业的产品，不是说模仿就模仿的，所以只实现了一些主要的功能，和一些能力之内的功能...
## 项目启动
### 创建界面：
```
  "pages":[
    "pages/home/home",
    "pages/menu/menu",
    "pages/location/location",
    "pages/my/my",
    "pages/order/order"
  ],
```
只要编辑app.js中的pages属性，就会在项目目录下的pages文件夹里自动生成一个文件夹，里面包扩了.wxml 、 .wxss 、 .json 、 .js这样四个文件。wxml就是界面结构文件， .wxss就是样式文件, .js是用来存放js代码并实现界面逻辑的地方，至于 .json就是用来配置页面属性的地方，如：修改标题栏的颜色，和文字。

### 配置标题栏的样式：
```
  "window":{
    "navigationBarTitleText": "美团外卖+",
    "navigationBarTextStyle": "white",
    "navigationBarBackgroundColor": "#FFC640"
  },
```
同样是在app.json中配置，其他页面的标题栏都以此为例。
 
### 添加底栏：
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
### 数据请求
```javascript
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
data是每个页面.js文件中都存在的一个键，用来储存本页面需要用到的数据。具体使用，可在wxml文件中用{{'data中的键名'}}的形式调用数据。<br>
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
## 主界面<br>
效果图：<br>
![image](https://github.com/tzc123/wx_project_meituan/raw/master/gif/home.gif)
### swiper控件应用
首先是两页标签的滑动切换,这里使用的是swiper，它是一款小程序自带的滑块组件，使用掌握起来非常简单，具体代码如下：
```html
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
swiper标签就是滑块组件的主体，表示可以滑动的区域，其中indicator-dots属性是设置设置点是否显示。接下来swiper-item标签在swiper之中表示的是每一个用来作为滑动的页面。这里用<block wx:for="{{categoryList}}" wx:key="">包裹着swiper-item表示的是使用categoryList对象数组中数据来循环渲染swiper-item，swiper-item的数量取决于categoryList中有多少组数据。之后在swiper-item中的block标签表示的是在一个页面中用categoryList.item中的数据循环渲染多个类似的标签，这些标签就是效果图中的类别项，总共两页，每页八个。这就是swiper和循环渲染的一些基本用法。
 ### 弹出层的实现
 ```html
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
最外层的mask类的view就是一个遮罩层，用来覆盖之前的界面形成遮罩的效果，并在上面显示新的界面也就是弹出层。以上的代码就是效果图中点击筛选按钮所呈现出来的内容了。其中bindtap属性就是点击事件的绑定了，具体的点击事件需要在.js文件中设置。值得一提的是，bindtap事件是会把当前标签受到的点击冒泡给它的父容器，这就相当与同时点击了他的父容器，如果想阻止冒泡的话就需要使用catchtap。

## 定位界面
先上效果图：<br>
![image](https://github.com/tzc123/wx_project_meituan/raw/master/gif/location.gif)<br>
页面结构：
```html
<view class="header">
<view class="search-input">
  <input placeholder="请输入收货地址"
   bindinput="input"></input>
   </view>
  <view class="search-btn">搜索</view>
</view>
<view class="result-container" hidden="{{hidden}}">
<scroll-view scroll-y="true"class="search-result-list" hidden="{{hidden}}">
  <block wx:for="{{locationList}}" wx:key="">
    <view class="search-result" bindtap="onTap" data-key="{{item.address}}">{{item.name}}
      <view class="search-result-desc">{{item.address}}</view>
    </view>
  </block>
</scroll-view>
</view>
<view class="getLocation"
bindtap="getLocation">点击定位当前位置</view>
<view class="addLocation">新增收货地址
  <view class="addLocation-icon">+</view>
</view>
<view class="myLocation">我的收货地址</view>
<view class="LocatonInfo"></view>
<view class="userTel"></view>
```
这个界面主要涉及到的就是弹出层和百度地图API的调用，调用方法可以查看百度地图API，具体点击事件代码如下：
```javascript
getLocation: function () {
    wx.getLocation({
      type: 'gcj02',
      success: function (res) {
        var latitude = res.latitude
        var longitude = res.longitude
        wx.request({
          url: 'http://api.map.baidu.com/geocoder/v2/?ak=btsVVWf0TM1zUBEbzFz6QqWF&coordtype=gcj02ll&location=' + latitude + ',' + longitude + '&output=json&pois=0',
          method: "get",
          success: function (res) {
            console.log(res.data.result.formatted_address)
            wx.setStorageSync('location', 
            res.data.result.formatted_address.substr(res.data.result.formatted_address.indexOf('市') + 1, 10))
          }
        })
      }
    })
    wx.switchTab({
      url: '/pages/home/home'
    })
  },
input: function (e){
    if(e.detail.value){
      this.setData({
        hidden: false
      })
      this.search(e.detail.value);
    }else{
      this.setData({
        hidden: true
      })
    }
  },
search: function (text){
    var that = this;
    wx.request({
      url: 'http://api.map.baidu.com/place/v2/search?query=' + text +'&page_size=20&page_num=0&scope=2&region=南昌&output=json&ak=btsVVWf0TM1zUBEbzFz6QqWF',
      success: function(res){
        console.log(res);
        that.setData({
          locationList:res.data.results
        })
      }
    })
  },
```
## 点菜界面<br>
 效果图如下：<br>
 ![image](https://github.com/tzc123/wx_project_meituan/raw/master/gif/order.gif)<br>
 页面结构如下：
 ```html
 <import src = "../common/orderPage.wxml"/>
<import src = "../common/commentPage.wxml"/>
<view class="container" disable-scroll="true">
  <view class="header">
    <block wx:for="{{swiperTitle}}" wx:key="">
      <view class="title {{index==currentPage?'selected':''}}" data-index="{{index}}"
      bindtap="turnPage">{{item.text}}</view>
    </block> 
  </view>
  <swiper class="swiper" current="{{currentPage}} 
  bindchange="turnTitle">
    <swiper-item id="orderPage">
      <template is="orderPage" data="{{menu,selected,howMuch,cost,pullBar}}"/>
    </swiper-item>
    <swiper-item id="commentPage">
      <template is="commentPage" data="{{categoryList}}"/>
    </swiper-item>
    <swiper-item id="restaurantPage"></swiper-item>
  </swiper>
</view>
 ```
 菜单页面如下：
 ```html
 <template name="orderPage">
    <scroll-view class="orderPage-sideBar" 
    bindscrolltolower="lower" 
    scroll-y="true">
      <block wx:for="{{menu}}" wx:key="">
        <view class="menuList">
          <view class="menu {{index==selected?'selected':''}}" 
          data-index="{{index}}" 
          catchtap="turnMenu">{{item.typeName}}</view>
        </view>
      </block>
    </scroll-view>
    <scroll-view class="foodList" scroll-y="true">
      <view class="title">{{menu[selected].typeName}}</view>
      <block wx:for="{{menu[selected].menuContent}}" wx:key="">
        <view class="food">
          <image class="img" src="{{item.src}}"></image>
          <view class="food-info">
            <view class="name">{{item.name}}</view>
            <view class="sales">月售 {{item.sales}} 赞 {{item.rating}}
            </view>
            <view class="price">￥  {{item.price}}</view>
          </view>
          <view class="food-numb">
            <view class="remove" 
            bindtap="removeFromTrolley"
            hidden="{{item.numb==0}}" 
            data-index="{{index}}">-</view>
            <text class="text" 
            hidden="{{item.numb==0}}">{{item.numb}}</text>
            <view class="add" 
            bindtap="addToTrolley" data-index="{{index}}">+</view>
          </view>
        </view>
      </block>
    </scroll-view>
    <view class="footer {{cost!=0?'active':''}}">
      <view class="howMuch">
        <view class="img" style="background:{{cost!=0?'#FFD161':'#E7E7E7'}};">
          <image src="/pages/images/trolley.png" style="width:60rpx;height:60rpx;"></image>
        </view>
        <view class="cost" hidden="{{cost==0}}">￥{{cost}}</view>
        <view class="free">免配送费</view>
      </view>
      <view class="pay">{{cost!=0?'去结算':'15元起送'}}</view>      
    </view>
</template>
 ```
### tab切换
这个界面最主要的功能就是tab切换，和点菜功能。其中tab切换其实用的还是swiper，因为swiper有一个current属性表示的是swiper当下显示的页面的序号，只需要将tab中被激活的项与swiper的页面互相绑定就可以了，具体代码如下：
```javascript
  turnPage: function (e) {
    this.setData({
      currentPage: e.currentTarget.dataset.index
    })
  },
  turnTitle: function (e) {
    if(e.detail.source=="touch"){//判断是否是滑动引起的界面切换
      this.setData({
        currentPage: e.detail.current
      })
    }
  },
```
当点击title中的项时获取当前序号，再将它赋值给current，当手指滑动swiper时触发bindchange事件，获取当前页面序号，使相应序号的title处于被选中的状态。有一个值得注意的地方是当点击title中的项时也会触发swiper的bindchange事件，但是我们只想让它在滑动swiper时触发，否则就会出现setData过于频繁的警告，所以我们需要在turnTitle中加一段判断语句，判断页面滑动的原因是否为滑动，如果不是则不执行下面的语句。
点菜功能只是数据绑定界面的更加复杂的应用，而且还有许多不妥之处，这里就不作说明了，有兴趣的朋友可以去[我的GitHub](https://github.com/tzc123/wx_project_meituan)看详细的代码。
## 总结
这次项目是本人的第一个微信小程序项目，希望能给大家提供一些参考价值，有什么问题和想说的都可以在评论区告诉我，文章和代码中诸多不妥当的地方也劳烦各位不吝言辞，多多斧正。这样才能帮助我更快的进步，感谢！
***
项目地址：https://github.com/tzc123/wx_project_meituan

  


