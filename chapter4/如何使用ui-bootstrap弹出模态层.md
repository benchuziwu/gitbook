项目中需要弹出模态层的地方很多,采用的是ui-bootstrap自带的模态层

参考链接：https://angular-ui.github.io/bootstrap/#!#modal

1.用service方式写模态层

例如弹出登录模态层

```javascript
(function() {
    'use strict'; angular.module('jhipterApp').factory('LoginService',LoginService);
    LoginService.$inject = ['$uibModal'];
    function LoginService ($uibModal) {
        var service = {
            open: open
        };
        var modalInstance = null;
        var resetModal = function () {
            modalInstance = null;
        };
        return service;
        function open () {
            if (modalInstance !== null) return;
            modalInstance = $uibModal.open({
                animation: true,
                templateUrl: 'app/components/login/login.html',
                controller: 'LoginController',
                controllerAs: 'vm',
                resolve: {
                    translatePartialLoader: ['$translate', '$translatePartialLoader', function ($translate, $translatePartialLoader) {
                        $translatePartialLoader.addPart('login');
                        return $translate.refresh();
                    }]
                }
            });
            modalInstance.result.then(
                resetModal,
                resetModal
            );
        }
    }
})();

```

在页面需要点击弹出模态层的方法中写入LoginService.open即可

2.用路由方式嵌入模态层

```javascript
        .state('user-management.edit', {
            url: '/{login}/edit',
            data: {
                authorities: ['ROLE_ADMIN']
            },
            onEnter: ['$stateParams', '$state', '$uibModal', function($stateParams, $state, $uibModal) {
                $uibModal.open({
                    templateUrl: 'app/admin/user-management/user-management-dialog.html',
                    controller: 'UserManagementDialogController',
                    controllerAs: 'vm',
                    backdrop: 'static',
                    size: 'lg',
                    resolve: {
                        entity: ['User', function(User) {
                            return User.get({login : $stateParams.login});
                        }]
                    }
                }).result.then(function() {
                    $state.go('user-management', null, { reload: true });
                }, function() {
                    $state.go('^');
                });
            }]
        })
```

