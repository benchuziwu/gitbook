关于ejt1项目介绍

该项目是在jhipster的基础上开发的，加上inspina样式模版，最初是将这两者进行集成开发。

ejt1项目的业务目前主要是做短信业务，通信能力目前包括验证码，通知类短信，营销类短信。

### 1.登录模块

包括登录，快捷登录，注册页，忘记密码页，

涉及的功能有登录，注册，校验功能

### 2.登录之后进入系统首页

（1）这块给每个登录用户设置不同头像，用到的技术是jdenticon.min.js

（2）另外头部有一个应用列表，是用户所创建过的应用列表信息

（3）首页包括用户名，账户余额，短信套餐包信息，用户登录信息展示，以及通信能力展示，

（4）首页涉及到和后端交互的功能是开通能力流程，（首先验证用户是否认证过，若已认证则开通能力，开通能力之后有个快捷方式，将该能力关联其他应用）这部分代码在index文件夹下

（5）若用户未认证，则引导用户去认证信息，用户认证包括企业认证和个人认证，用户提交认证信息过程涉及到的功能点有校验，图片上传，两种类型认证复用一个表单，该功能在accountManage文件夹下

### 3.应用管理页

应用管理页用户可创建应用，该模块功能包括设置默认应用，查看appkey，设置回调地址，回调地址是否设置的显示，接入通信能力，分页

### 4.配置中心

配置中心需要创建签名和模版，验证码，通知类，营销类短信复用同一个页面和控制器

涉及功能有创建签名，校验，签名列表，查询签名，批量删除

模版页涉及功能有创建模版，模版内容的展示，

### 5.ip白名单

包括新增IP白名单，批量删除，搜索，批量上传功能

### 6.审核部分

包括用户认证信息的审核，模版的审核，签名的审核，支付凭证的审核

主要包括同意和驳回，查看详情的功能

### 7.支付过程

包括充值和购买短信包功能

支付方式：充值有三种（微信，支付宝，对公转账），购买短信包包括四种（微信，支付宝，对公转账，余额支付）

### 8.权限管理

包括权限的创建，列表，编辑，删除功能，给角色分配权限，创建角色，删除角色的功能

### 9.管理员管理短信套餐包

包括几种套餐包详情和列表功能，一般的查询分页功能，导出excel功能，新建套餐包功能

安装jhipster代码可参考链接：

安装一份简单的jhipster代码研究顺序：

```
?
(1/15) Which *type* of application would you like to create? Monolithic application (recommended for simple projects)

? (2/15) What is the base name of your application? jhipter

? (3/15) Would you like to install other generators from the JHipster Marketplace? No

? (4/15) What is your default Java package name? com.mycompany.myapp

? (5/15) Which *type* of authentication would you like to use? HTTP Session Authentication (stateful, default Spring Security mechanis

m)

? (6/15) Which *type* of database would you like to use? SQL (H2, MySQL, MariaDB, PostgreSQL, Oracle, MSSQL)

? (7/15) Which *production* database would you like to use? MySQL

? (8/15) Which *development* database would you like to use? H2 with disk-based persistence

? (9/15) Do you want to use Hibernate 2nd level cache? No

? (10/15) Would you like to use Maven or Gradle for building the backend? Maven

? (11/15) Which other technologies would you like to use? (Press <space> to select, <a> to toggle all, <i> to inverse selection)

? (12/15) Which *Framework* would you like to use for the client? AngularJS 1.x

? (13/15) Would you like to use the LibSass stylesheet preprocessor for your CSS? No

? (14/15) Would you like to enable internationalization support? Yes

? Please choose the native language of the application? English

? Please choose additional languages to install Chinese (Simplified)

? (15/15) Besides JUnit and Karma, which testing frameworks would you like to use? (Press <space> to select, <a> to toggle all, <i> to

inverse selection)
```

