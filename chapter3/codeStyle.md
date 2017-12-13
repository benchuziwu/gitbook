# 编码规范
ejt项目css规范

公共可用css单独放到common.css,

之后每个模块下的css单独放到每个模块文件夹下，利用懒加载的方式将其引入，这样可避免对同一个css进行编写造成冲突，另外在编写css时需要注明每段css的功能以便日后修改，在上线时将其打包压缩到统一的一个文件目录下即可。

### CSS编码规范

> 编写一个css样式的书写顺序

1. 位置属性(position,  display, float, top, right...)
2. 大小及盒模型相关属性(width, height, padding, margin...)
3. 文字相关属性(font-size, line-height...)
4. 背景边框相关(background, border...)
5. 其他
```
示例：
.class {
  /* 位置相关，注意声明方向时的顺序，上右下左 */
  position: absolute;
  z-index: 1;
  top: 10px;
  right: 10px;
  bottom: 10px;
  left: 10px;

  /* 大小相关 */
  width: 100px;
  height: 100px;
  padding: 10px;

  /* 字体相关 */
  font-size: 16px;
  line-height: 16px;

  /* 背景边框相关 */
  background-color: red;
  border: 1px solid;

  /* 其他 */
  cursor: pointer;
}
```
合理的层叠样式不需要使用id选择器和!important, 所以尽量避免使用。
### javascript编码规范
```
1.使用字面量语法创建对象和数组
var obj = {};
var arr = [];
```
```
2.字符串使用单引号
```
```
3. 多个变量只用一个var来声明，每个变量占用一个新行,最后再声明未赋值的变量.

var name = 'Andy',
    age = 20,
    gender = 'male',
    weight,
    height;
```
```
4.尽量使用 === 和 !==，除非你想利用隐式类型转换而使用 == 和 !=
```
```
5.至少要给多行的块使用大括号

// bad
if (test)
  return false;

// good
if (test) return false;

// good
if (test) {
  return false;
}

// bad
function() { return false; }

// good
function() {
  return false;
}
```
```
6.注意空格的使用

// good
if (baz) {
  console.log(qux);
} else {
  console.log(foo);
}

function fun() {
  console.log(1);
}

for (var i = 0; i < 10; i++) {
  console.log(1);
}
```
```
7.语句的结尾要使用分号
```
```
8.代码缩进为tab键，一个tab键设置成两个space。
```
```
9.多行注释使用 /**/
单行注释使用 //
```
```
10. 逗号之后如果不换行要接空格。如果换行，逗号不能在开头。

// bad
var hero = {
    firstName: 'Bob'
  , lastName: 'Parr'
  , heroName: 'Mr. Incredible'
  , superPower: 'strength'
};
```