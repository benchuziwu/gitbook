# angular-qrcode

angular-qrcode生成二维码

1.安装bower install angular-qrcode --save

2.引入

```
'bower_components/qrcode-generator/js/qrcode.js',
'bower_components/qrcode-generator/js/qrcode_UTF8.js',
'bower_components/angular-qrcode/angular-qrcode.js',
```

3.依赖注入monospaced.qrcode

```Js
angular
.module('your-module', [
'monospaced.qrcode',
]);
```

4.使用

```html
<qrcode error-correction-level="M" size="210" data="{{url}}" download></qrcode>
```

5.

参考链接：https://github.com/monospaced/angular-qrcode[http://monospaced.github.io/angular-qrcode/](https://www.gitbook.com/book/dandelion1000/front-end-road/edit#)

参考示例：[http://www.qrcode.com/en/about/version.html](https://www.gitbook.com/book/dandelion1000/front-end-road/edit#)



