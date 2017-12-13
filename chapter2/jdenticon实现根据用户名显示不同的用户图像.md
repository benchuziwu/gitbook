# jdenticon实现根据登陆用户名显示不同的用户图像

为实现给不同用户登录网站时加一个特色头像，采用h[ttp://jdenticon.com/](https://www.gitbook.com/book/dandelion1000/front-end-road/edit#)提供的插件实现，类似的网站有gitlab，stackoverflow，github，等

Identicon, Don Park在2007年1月18日首次想出了这个创意。一般来说在这些网站上面如果你没有指定自己的头像，网站会使用 Gravatar 或者使用 Identicon (Gravatar也有相关服务) 作为默认头像。Identicon 是 Hash 值的可视化表示，常见的生成方法是根据 IP 地址或 email 地址生成。 服务器通过 Identicon 可以以头像的形式来分辨用户，这种方法同时能够保护用户的隐私。最初的 Identicon 是由9块构成的图形，后来它的表现形式由第三方扩展至了各种图形形式。

价值参考链接：https://www.zhihu.com/question/26387811

http://meta.stackexchange.com/questions/17443/how-is-the-default-user-avatar-generated

https://my.oschina.net/guanyue/blog/181513

http://www.moreslowly.jp/products/identicon/test.html

#### 应用

```html
<canvas class="jdenticon" width="46" height="46" style="border-radius:70px;border:1px solid #f5f6fa;background-color:#f5f6fa" />
```

```javascript
var valueToHash =vm.account.userName;
$(".jdenticon").jdenticon(md5(valueToHash));
```

![2017-05-08211135.png](https://www.tuchuang001.com/images/2017/05/08/2017-05-08211135.png)

