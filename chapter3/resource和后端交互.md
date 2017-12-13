# 如何使用$resource和后端交互

```
$resource服务本身是一个创建资源对象的工厂。返回的$resource对象中包含了同后端服务器进行交互的高层api
```

 ```
var user = $resource('/api/users/:userId.json',{userId:'@id'})
 ```

$resource返回包含了几个默认动作的资源类对象。可以把user对象理解成同RESTful的后端服务进行交互的接口

资源类对象本身包含的方法可以同后端服务进行简洁的交互。

默认情况下，这个对象包含了五个常用的方法，可以同资源集合进行交互，或者创建资源对象的实例。它会生成两个基于HTTP GET类型的方法，以及三个非get类型的方法。

#### 基于http get方法

两个http get类型的方法可以接受下面三个参数

```
params（对象）
随请求一起发送的参数。可以是url中的具名参数，也可以是查询参数。
successfn（函数）
当http响应成功时的回调函数
errorfn（函数）
当http响应失败时的回调函数
```

1.get(params,successfn,errorfn)

get方法向指定url发送一个get请求，并期望一个json类型的响应

如果参数中传入了具名函数（我们例子中的参数是id），那么get()方法会向包含id的url发送请求：

get   /api/users/123

```
user.get({id:'123'},function(resp){
  //处理响应成功
},function(err){
  //处理错误
})
```

2.query(params,successfn,errorfn)

query方法向指定url发送一个get请求，并期望一个json格式的资源对象集合。

／／get  /api/users

```
User.query(function(users){
var user=users[0];
})
```

query()和 get()方法之间唯一的区别是angularjs期望query()方法返回数组

#### 基于非http get类型的方法

三个基于非http get类型的方法可以接受下面四个参数

params(对象)

随请求一起发送的参数，她们可以是url中的具名参数，也可以是查询参数

postData(对象)

这个对象是随着请求发送的数据体

```
suceessfn()
当http响应成功时的回调函数
errorfn（函数）
当http响应失败时的回调函数
```

1.save(params, payload,successfn,errorfn)

save方法向指定url发送一个post请求，并用数据体来生成请求体。save()方法用来在服务器上生成一个新的资源

```
user.save({},{name:'ari'},function(res){
//处理响应成功
},function(res){
//处理非成功响应
})
```

2.    delete(params, payload, successFn, errorFn)

      delete方法会向指定url发送一个delete请求，并用数据体来生成请求体。它被用来在服务器上删除一个实例：

      //delete  /api/users

      ```
      User.delete({},{
      id:'123'
      },function(res){
      //处理成功的删除响应
      },function(res){
      // 处理非成功的删除响应
      })				
      ```

      3.    remove(params, payload, successFn, errorFn)


            ​		



  #### $resource实例

上述方法返回数据时，响应会被一个原型类所包装，并在实例上添加一些有用的方法，实例对象上会被添加下面三个实例方法：

```
$save()
$remove()
$delete()
```

#### 附加属性

 $resource集合和实例有两个特殊的属性用来同底层的数据定义进行交互

 $promise(promise)

$promise属性是为$resource生成的原始promise对象，这个属性是特别用来同$routeProvider.when()在resolve时进行连接的。

如果请求成功了，资源实例或集合对象会随promise的resolve一起返回。如果请求失败了，promise被resolve时会返回http响应对象，其中没有resource属性

$resolved(布尔型)

$resolved属性在服务器首次响应时会被设置为true（无论请求是否成功）

#### $resource设置对象

对象中的值，也就是动作，是资源对象中某个方法的名字

它可以包含以下键

1.method（字符串）

method指的是我们想要用来发送http请求的方法。它必须是以下值之一：

```
‘GET’、‘DELETE’、 ‘JSONP’、‘POST’、‘PUT’
```

2.url(字符串)

3.params（字符串map或对象）

4.isArray(布尔型 )

如果isArray被设置为true，那么这个动作返回的对象会以数组的形式返回。

5.transformRequest(函数或函数数组)

这个函数或函数数组用来对http请求的请求体和头信息进行转换，并返回转换后的版本。通常用来进行序列化。

```
var user=$resource('/api/users/:id',{id:'@'},{
sendEmail:{
  method:"PUT",
  transformRequest:function(data,headerFn){
    return JSON.stringify(data);
  }
}})
```

6.transformResponse(函数或函数数组)

```
var user=$resource('/api/users/:id',{id:'@'},{
sendEmail:{
  method:"PUT",
  transformResponse:function(data,headerFn){
    return JSON.parse(data);
  }
}})
```

在ejt项目中，也会遇到后端传过来非json格式的字符串，这时候如何处理？

```
(function () {
    'use strict';
    angular
        .module('ejt1GatewayApp')
        .factory('orderPay', orderPay);
    orderPay.$inject = ['$resource'];
    function orderPay ($resource) {
        var service = $resource(url+'account/api/pays', {}, {
            'post':{method:'POST', 
            transformResponse: function(data) {
                    return {data:data}
                }}
        });

        return service;
    }
})();
```

使用            transformResponse: function(data) {
​                    return {data:data}
​                }}

即可将其包装成json进行处理

在需要调用该service接口的地方将该service注入使用即可。