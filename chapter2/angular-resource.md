# angular-resource

**$resource**

参考 链接：[https://docs.angularjs.org/api/ngResource/service/$resource](https://www.gitbook.com/book/dandelion1000/front-end-road/edit#)

创建一个resource对象的工厂函数，可以让你安全的和RESFUL服务端进行数据交互。

需要注入 ngResource 模块。angular-resource[.min].js

默认情况下，末尾斜杠（可以引起后端服务器不期望出现的行为）将从计算后的URL中剥离。

这个可以通过$resourceProvider配置：

```
  app.config(["$resourceProvider",function($resourceProvider){
      $resourceProvider.defaults.stripTrailingSlashes = false;
  }])
```

**依赖**：$http

**使用**：$resource(url,[paramDefaults],[actions],options);

**url**:一个参数化的url模板，带有前缀参数(如：/user/:username)。如果你使用的是带端口号的URL(如：http://example.com:8080/api)，则需要慎重考虑。如果带有后缀(如：http://example.com/resource.json 或者 http://example.com/:id.json 或者 http://example.com/resource/:resource_id.:format)。如果后缀之前的参数是空的，在这情况下:resource_id 比 /.优先执行，如果你需要这个序列出现而不崩溃，那么你可以通过/\.避免。

**paramDefaults**:url参数的默认值，这些可以在方法重写。如果参数的任何一个值是函数，它将作为每一次请求获取的参数值而被执行(除非该参数被忽略的)。

参数对象中的每个键值对都是先绑定到一个url模板，任何多余的密钥都被附加到url query的“？”后。 /path/:verb +{verb:’greet’,salutation:’hello’}  =>  /path/greet?salutation=hello

**actions**: 用户对于resource行为的默认设置进行扩展的自定义配置的散列，该配置将会以$http.config的格式创建。

*action*: 字符串，action的名称，这个名称将成为resource对象方法的名称。

*method*：字符串，http方法（不区分大小写，如GET, POST, PUT, DELETE, JSONP等）。

*params*：对象，这次行动预先设定的参数。如果任何参数的值是一个函数，当一个参数值每一次需要获得请求时都会被执行（除非该参数被忽略的）。

*url*：字符串，行为指定的网址。

*isArray*：boolean，如果为true，那么这个行为返回的对象是个数组。

*transformRequest*：函数/函数的数组。转换函数或者一个包含转换函数的数组。转换函数获取http请求体和请求头，并且返回他们的转换版（通常是序列化）。

*transformResponse*：函数/函数的数组。转换函数或者一个包含转换函数的数组。转换函数获取http响应体和响应头，并且返回他们的转换版（通常是序列化）。

*cache*：boolean，如果为true，一个默认的$http缓存将被作为请求的缓存，否则如果存在一个用$cacheFactory创建的缓存实例，则将用于缓存。

*timeout*：数值，毫秒，超时则让请求中止。

*withCredentials*：boolean，是否设置withcredentials flag的XHR对象。查看更多信息的凭据。

*responseType*：字符串，响应头类型。

*interceptor*：对象，拦截对象有两个可选方法-response和responseError。

**Options**：扩展$resourceProvider行为的自定义设置，唯一支持的选项是stripTrailingSlashes,boolean类型，如果为真，url尾部的斜杠会被移除（默认为true）。

**五种默认行为**：

{

　　“get”:{method:“get”},

　　“save”:{method:“post”}

　　“query”:{method:“get”,isArray:true}

　　“remove”:{method:“delete”}

　　“delete”:{method:“delete”}

}

get([params],[success],[error]);

save([params],postData,[success],[error]);

query([params],[success],[error]);

remove([params],postData,[success],[error]);

delete([params],postData,[success],[error]);

$save([params],[success],[error]);

$remove([params],[success],[error]);