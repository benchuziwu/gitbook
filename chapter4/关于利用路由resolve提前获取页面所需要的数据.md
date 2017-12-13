在项目中，有很多页面一开始加载就需要提前获取数据，在路由resolve中提前写service获取数据，通过异步的方式拿到数据之后，在新页面的控制器将其注入。可以在全部获取数据之后渲染页面

```javascript
    angular
        .module('testapp')
        .config(stateConfig);
    stateConfig.$inject = ['$stateProvider'];
    function stateConfig($stateProvider) {
        $stateProvider.state('teststate', {
            url: '/teststate',
            views: {
                'content': {
                    templateUrl: 'test.html',
                    controller: 'testController',
                    controllerAs: 'vm'
                }
            },
            resolve: {
                loadPlugin: function ($ocLazyLoad) {
                    return $ocLazyLoad.load([
                        {
                            serie: true,
                            files: ['']
                        }
                    ])
                },
                getAllNotices: ['$q', '$ocLazyLoad', '$injector', function ($q, $ocLazyLoad, $injector) {
                    return $ocLazyLoad.load('app/main/notice/allNotice.service.js').then(function () {
                        var deferred = $q.defer();
                        $injector.get('getAllNotice').get(function (res) {
                            console.log(res);
                            deferred.resolve(res.content);
                        });
                        return deferred.promise;
                    });
                }],
                translatePartialLoader: ['$translate', '$translatePartialLoader', function ($translate, $translatePartialLoader) {
      $translatePartialLoader.addPart('permission/rolePermission');
                    return $translate.refresh();
                }]
            }
        })


```

 如果懒加载涉及到ajax请求的服务，直接在懒加载函数中加载所需service之后，再写一个函数将懒加载中加载的service注入则无法打开页面，这是由于异步的问题需要改成如下

```javascript
testGetAllDatas: ['$q', '$ocLazyLoad', '$injector', function ($q, $ocLazyLoad, $injector) {
                    return $ocLazyLoad.load('app/main/test/allDatas.service.js').then(function () {
                        var deferred = $q.defer();
                        $injector.get('getAllDatas').get(function (res) {
                            deferred.resolve(res.content);
                        });
                        return deferred.promise;
                    });
                }],
```

app/main/test/allDatas.service.js是所需的service；

而不是通过如下懒加载方式,

```javascript
loadPlugin: function ($ocLazyLoad) {
                    return $ocLazyLoad.load([
                        {
                            serie: true,
                            files: ['app/main/test/allDatas.service.js']
                        }
                    ])
                },
testGetAllDatas: ['getAllDatas', function (getAllDatas) {
                 return getAllDatas.get(); 
                }],
```

然后在控制器中注入testGetAllDatas，打印testGetAllDatas即可得到需要的数据信息