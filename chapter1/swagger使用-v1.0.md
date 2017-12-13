# swagger使用-v1.0

```
通常前端和后端需要交互，需要一个API文档相当于协议，前后端的接口通过API定义，swagger能通过在线editor编写API生成yaml文件或json文件导入至本地swagger-ui文件夹dist目录下，本地通过启动服务器打开swagger-ui,就可以看到生成好的带有样式的API文档
```

文档末尾有参考链接

官网：http://swagger.io

在线编辑器http://editor.swagger.io/

### 1.环境搭建

```
进入终端命令行：
mkdir testswiggerfile
cd testswiggerfile
全局安装node，git
git clone https://github.com/swagger-api/swagger-ui.git//下载Swagger UI
```

#### 创建空目录public

```
到下载好的Swagger UI 文件中dist 目录下，执行npm install anywhere
然后执行anywhere就可以打开官网案例API页面文档
```

![](http://i1.piimg.com/567571/cc42e04675d4e60b.png)

### [Swagger Editor](http://editor.swagger.io/#/)编写 API 文档

```
Swagger Editor 上的是基于 yaml 的语法
edit Swagger API specifications  in YAML 
导出test.yaml,即Download YAML 保存至nodetestswigger／public目录下
```

![](http://i1.piimg.com/567571/55526ad64d9c8803.png)

```
将public目录下index.html用编辑器打开，将
url = "http://petstore.swagger.io/v2/swagger.json";替换为
url = "test.yaml";
```

![](http://p1.bqimg.com/567571/f7d3616c4cf95471.png)

```
重新刷新页面，就可以查看自己写的API了
```

```
打开编辑器之后，通过file－open example查看一些例子了解参数含义如何定义yaml
```

### 如何安装Swagger Editor到本地

```
其中Swagger Editor是个用Angular开发的WEB小程序，它可以让你用YAML来定义你的接口规范，并实时验证和现实成接口文档。
此外，它还可以通过接口文档帮你生成不同框架的服务端和客户端，方便你mock和契约测试。最后导出JSON格式的API规范，通过Swagger UI对外发布。
```



```
npm install -g http-server
wget https://github.com/swagger-api/swagger-editor/releases/download/v2.10.1/swagger-editor.zip//首先要确定wget已安装
brew install wget//就可以了
unzip swagger-editor.zip//解压swagger-editor
然后cd swagger-editor//进入该目录，执行anywhere命令
```



### Swagger API参数定义解释

Swagger API可以使用yaml或json来表示

```
swagger:'2.0'//版本号
info://信息(关于此接口的主题，描述以及版本等API的元数据)
      title：
      description：
      version：
tags://补充的元数据，在swagger ui中，用于作为api的分组标签
host://此接口主题的API主机地址
basePath://相对于host的路径
schemes://API的传输协议，http，https，ws，wss
consumes://API可以消费的MIME类型列表
produces://API产生的MIME类型列表
paths://API的路径，以及每个路径的HTTP方法，一个路径加上一个HTTP方法构成了一个操作。每个操作都有以下内容：
   tags://操作的标签
  summary://短摘要,此操作的意义
  description//描述
  operationId://标识操作的唯一字符串
  parameters://参数列表
      parameters:
        - name: email
          in: query
          description: The email for user register
          type: string
  responses://应答状态码和对应的消息的Schema
      responses:
            '200':
              description: successful operation
              schema:
                type: object
                properties:
                  count:
                    type: integer
                    format: int32
  
```

```
in参数
Required. The location of the parameter. Possible values are "query", "header", "path", "formData" or "body".


POST	创建对象
```





参考链接：

https://zhuanlan.zhihu.com/p/21353795

http://www.jianshu.com/p/d6626e6bd72c

https://github.com/swagger-api/swagger-editor

https://github.com/swagger-api/swagger-ui

https://github.com/OAI/OpenAPI-Specification/blob/master/versions/2.0.md

https://my.oschina.net/ekc/blog/83281