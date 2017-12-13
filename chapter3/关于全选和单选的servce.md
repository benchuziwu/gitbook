关于项目中全选和单选的功能代码在directive中select目录下

分别为selectAll.service.js和selectThis.service.js

selectAll.service.js是绑定当前页全选功能后一些功能按钮的关系，因为在调用该service后控制器中需要一些回调值，所以采用$q异步的方式

selectThis.service.js是选取当前的复选框和绑定当前页一些功能按钮的关系；

```javascript
(function(){
	'use strict';
	angular
	    .module('ejt1GatewayApp')
	    .factory('selectAllService', selectAllService);
        function selectAllService($q){
        	var service={
        		selectAllChecked:selectAllChecked
        	};
        	return service;
        	function selectAllChecked(select_all,arrays,checked){
                var deferred = $q.defer();
                var deferObject={};
                if (select_all === true) {
                            checked = [];
                            angular.forEach(arrays, function (datas) {
                                datas.checked = true;
                                checked.push(datas.id);
                                deferObject.batchDelete=true;
                            })
                        } else {
                            angular.forEach(arrays, function (datas) {
                                datas.checked = false;
                                checked = [];
                                deferObject.batchDelete=false;
                            })
                        }
                        deferObject.checked=checked;
                        deferred.resolve(deferObject);
                return deferred.promise;
        	}
        }
})();
```

补充说明，select_all，arrays，checked是形参，当调用该service时传实参

//页面控制器控制全选和单选的方法，selectAllService，selectThisService注入控制器，在该调用service的地方调用它

```javascript
    $scope.checked = [];
    $scope.select_all = false;
    $scope.selectAll = function () {
    $scope.select_all = !$scope.select_all;          selectAllService.selectAllChecked($scope.select_all,$scope.selectAppSignature,$scope.checked).then(function(res){
         $scope.checked=res.checked;
         $scope.batch=res.batchDelete;
        });
    };
```

同样单选功能原理如上，代码可参考项目中所有用到单选和全选的地方