wikimd是CMS /维基**完全建立在HTML5 / Javascript和客户端上运行的100％ **。不需要特别的软件安装或服务器端处理。使用mdwiki书写文档，并且可根据需要添加样式,mdwiki官方网站也是采用md格式展示的，mdwiki用到的技术有

该wikimd还可以更换主题，可以到[bootswatch](http://bootswatch.com/)下载（[http://bootswatch.com/），下载的CSS文件放在网站根目录下，创建`navigation.md`，添加一句话`[gimmick:theme](此处是主题的名字)`](http://bootswatch.com/%EF%BC%89%EF%BC%8C%E4%B8%8B%E8%BD%BD%E7%9A%84CSS%E6%96%87%E4%BB%B6%E6%94%BE%E5%9C%A8%E7%BD%91%E7%AB%99%E6%A0%B9%E7%9B%AE%E5%BD%95%E4%B8%8B%EF%BC%8C%E5%88%9B%E5%BB%BA%60navigation.md%60%EF%BC%8C%E6%B7%BB%E5%8A%A0%E4%B8%80%E5%8F%A5%E8%AF%9D%60[gimmick:theme](%E6%AD%A4%E5%A4%84%E6%98%AF%E4%B8%BB%E9%A2%98%E7%9A%84%E5%90%8D%E5%AD%97)%60)

```
http://dynalon.github.io/mdwiki/#!download.md
点击该页的here到https://github.com/Dynalon/mdwiki/releases页面进行下载源码
```

[![2017-05-12163733.png](https://www.tuchuang001.com/images/2017/05/12/2017-05-12163733.png)](https://www.tuchuang001.com/image/7QWhz)

![2017-05-12163945.png](https://www.tuchuang001.com/images/2017/05/12/2017-05-12163945.png)

下载好文件后将`mdwiki.html` 放到 markdown 文件的目录，即可以页面的形式访问md

[http://dynalon.github.io/mdwiki/#!examples.md这个是官网关于使用wikimarkdown的案例](http://dynalon.github.io/mdwiki/#!examples.md这个是官网关于使用wikimarkdown的案例)

```
http://dynalon.github.io/mdwiki/#!quickstart.md这是教你怎么使用wikimarkdown的链接

```

语法

```
# 文档中心
[流量]()

  * # 流量协议
  * [商户流量充值协议(1.1)](1.md)
  * [商户流量充值协议(1.2)](2.md)
  - - - -
[话费]()

  * # 话费协议
  * [商户话费充值协议(1.0)](3.md)
  - - - -
[大数据]()

  * # 大数据协议
  * [商户大数据认证协议(1.0)](4.md)
  * [物联网卡通讯协议(1.0)](5.md)
  * [商户大数据认证协议(1.0)](6.md)
  - - - -
[短信]()

  * # 短信协议
  * [HTTP短信接口协议-plain](7.md)
  * [HTTP短信接口协议-plain](8.md)
  - - - -
[语音]()

  * # 语音协议
  * [商户语音验证协议(1.0)](9.md)
  - - - -
[下载]()

  * # 下载协议
  * [短信接口Demo包(1.0)](10.md)
  - - - -
```

选择主题样式语法

```
[gimmick:themechooser](Choose theme)
[gimmick:theme](flatly)
```

该作者也是一名厉害的前端工程师

![2017-05-12205041.png](https://www.tuchuang001.com/images/2017/05/12/2017-05-12205041.png)