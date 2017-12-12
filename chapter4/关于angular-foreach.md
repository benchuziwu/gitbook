# 关于angular-foreach

angular-foreach

与ES262的[Array.prototype.forEach](http://www.ecma-international.org/ecma-262/5.1/#sec-15.4.4.18)不同 ，提供'undefined'或'null'值`obj`不会引发TypeError，而只是返回提供的值。

```javascript
var values = {name: 'misko', gender: 'male'};
var log = [];
angular.forEach(values, function(value, key) {
  this.push(key + ': ' + value);
}, log);
expect(log).toEqual(['name: misko', 'gender: male']);
```

## 用法

`angular.forEach(obj, iterator, [context]);`

```
var objs =[{a:1},{a:2}];
angular.forEach(objs, function(data,index,array){
//data等价于array[index]
console.log(data.a+'='+array[index].a);
});
```

参数如下：

```
objs：需要遍历的集合data:遍历时当前的数据
index:遍历时当前索引
array:需要遍历的集合，每次遍历时都会把objs原样的传一次。
```

