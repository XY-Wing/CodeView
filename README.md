# CodeView
CodeView 2017.11.28
## 11月份我在做什么？
- 1.学习后端语言（PHP）
  - 1）php语言基础
  - 2）MySQL的CRUD操作
  - 3）利用php和MySQL完成了一个学生信息管理系统
- 2.学习前端语言（HTML5）
  - 1）HTML
  - 2）CSS
  - 3）JavaScript
  - 4）根据资料，完成了一个静态手机官网
## 今天的codeView我将分享我学习的新语言知识
在学习JavaScript中，最映象深刻的便是js中那晦涩难懂的类构造语法，今天借此机会，把我所理解的类构造继承知识写下来，也请前端的小伙伴们指出错误。

### 在JavaScript的世界里，一切都是对象。
```js
typeof 123; // 'number'
typeof NaN; // 'number'
typeof 'str'; // 'string'
typeof true; // 'boolean'
typeof undefined; // 'undefined'
typeof Math.abs; // 'function'
typeof null; // 'object'
typeof []; // 'object'
typeof {}; // 'object'
```
可见，number、string、boolean、function和undefined有别于其他类型。特别注意null的类型是object，Array的类型也是object，如果我们用typeof将无法区分出null、Array和通常意义上的object——{}。

### 包装对象
我们可以用
```js
var str1 = "Wing";
```
来定义一个字符串对象。但是js中有一种包装对象的操作，如：
```js
var str2 = new String("Wing");
```
这样也能创建一个字符串对象。但是用原始值str1和包装后的值str2用全比较符号`===`返回的值是false。
`这是因为，str1是string类型，str2是object类型，它们现在已经是不同的类型了。`
<br />
![image](https://ss1.bdstatic.com/70cFuXSh_Q1YnxGkpoWK1HF6hhy/it/u=2757806497,3197995574&fm=27&gp=0.jpg)

因此，一般不使用包装对象。

### JavaScript面向对象编程
鉴于java，objective-c都是面向对象的语言，所以以下两点，我们都不会陌生：
- 1.类：类是对象的类型模板，例如，定义Student类来表示学生，类本身是一种类型，Student表示学生类型，但不表示任何具体的某个学生；
- 2.实例：实例是根据类创建的对象，例如，根据Student类可以创建出xiaoming、xiaohong、xiaojun等多个实例，每个实例表示一个具体的学生，他们全都属于Student类型。

所以，类和实例是大多数面向对象编程语言的基本概念。
不过，在JavaScript中，这个概念需要改一改。JavaScript不区分类和实例的概念，而是通过原型（prototype）来实现面向对象编程。

原型是指当我们想要创建xiaoming这个具体的学生时，我们并没有一个Student类型可用。那怎么办？恰好有这么一个现成的对象(注意是对象，而不是一个类)：
```js
var Student = {
    name: 'Robot',
    height: 1.6,
    run: function () {
        console.log(this.name + ' is running...');
    }
};
```
现在，我们创建一个小明对象：
```js
var xiaoming = {
    name: '小明'
};
xiaoming.__proto__ = Student;
```
注意最后一行代码把xiaoming的原型指向了对象Student，看上去xiaoming仿佛是从Student继承下来的：
```js
xiaoming.name; // '小明'
xiaoming.run(); // 小明 is running...
```
xiaoming有自己的name属性，但并没有定义run()方法。不过，由于小明是从Student继承而来，只要Student有run()方法，xiaoming也可以调用：

![image](https://cdn.webxueyuan.com/cdn/files/attachments/001435287613668a73ab76ccc85411282c1b1370be41636000/l)

