# 如何给请求头统一加Authorization

在项目中很多调后端的接口需要带Authorization才可以访问，为此可以统一给所有的请求头添加authorization

代码如下：

```javascript
(function() {
    'use strict';
    angular
        .module('ejt1GatewayApp')
        .factory('authInterceptor', authInterceptor);
    authInterceptor.$inject = ['$rootScope', '$q', '$location', '$localStorage', '$sessionStorage'];
    function authInterceptor ($rootScope, $q, $location, $localStorage, $sessionStorage) {
        var service = {
            request: request
        };
        return service;
        function request (config) {
            var token = $localStorage.authenticationToken || $sessionStorage.authenticationToken;
            if (token) {
                config.headers.Authorization = 'Bearer ' + token;
            }
            return config;
        }
    }
})();
```

```javascript
(function() {
    'use strict';
    angular
        .module('ejt1GatewayApp')
        .config(httpConfig);
    httpConfig.$inject = ['$httpProvider'];
    function httpConfig($httpProvider) {
        $httpProvider.interceptors.push('authInterceptor');
    }
})();
```

