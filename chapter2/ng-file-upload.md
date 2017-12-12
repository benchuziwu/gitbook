# ng-file-upload

项目中会遇到上传图片或者上传文件的功能，可以使用AngularJS的ng-file-upload插件。ejt项目中用户认证信息提交图片和提交支付凭证地方用到了上传图片功能

ng-file-upload的功能

```
支持上传文件，支持单张图片上传，支持多张图片上传，支持拖拽图片上传
```

以上传图片为例

```
bower install ng-file-upload-shim --save（for non html5 suppport)
bower install ng-file-upload --save
```

引入

```html
<scrpit src="bower_components/ng-file-upload/ng-file-upload-shim.js">
<script src="bower_components/ng-file-upload/ng-file-upload.js"></script>
```



```
<form ng-app="fileUpload" ng-controller="MyCtrl" name="form">
Single Image with validations
        <input type="file" ng-init="idCardPhotoPre=comImg.path" ng-model="idCardPhotoPre"
               name="idCardPhotoPre" ngf-select="uploadPic(inputFile)"
               accept="image/*" ngf-max-size="2MB" required
               ngf-model-invalid="errorFile">
</form>

```

```

    $scope.uploadPic = function (file) {
        $scope.uploadPath = path;
        Upload.upload({
            url: url + 'baseinfo/api/upload/' +$scope.uploadPath,
            data: {
                file: file
            }
        }).then(function (response) {
          deal response
        }
```

参考 链接：https://github.com/danialfarid/ng-file-upload

参考示例：[http://jsfiddle.net/danialfarid/maqbzv15/1118/](https://www.gitbook.com/book/dandelion1000/front-end-road/edit#)

