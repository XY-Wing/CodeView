# CodeView
codeView 薛扬-2017.11.28
## 11月份我在做什么？
- 1.学习后端语言（PHP）
  - 1）php语言基础
  - 2）MySQL的CRUD操作
  - 3）利用php和MySQL完成了一个学生信息管理系统
- 2.学习前端语言（HTML5）
  - 1）HTML
  - 2）CSS
  - 3）JavaScript
  - 4）完成了一个静态手机官网和万家云app首页
- 3.项目上的工作
- 4.其他
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

`注意，实际操作中不要使用obj.__proto__来操作对象的原型`

### 原型链
JavaScript对每个创建的对象都会设置一个原型，指向它的原型对象。

当我们用obj.xxx访问一个对象的属性时，JavaScript引擎先在当前对象上查找该属性，如果没有找到，就到其原型对象上找，如果还没有找到，就一直上溯到Object.prototype对象，最后，如果还没有找到，就只能返回undefined。

例如，创建一个Array对象：
```js
var arr = [1, 2, 3];
```
其原型链是：
```js
arr ----> Array.prototype ----> Object.prototype ----> null
```
Array.prototype定义了 indexOf()、shift() 等方法，因此可以在所有的Array对象上直接调用这些方法。
```js
var i = arr.indexof(1); // 0
```
很容易想到，如果原型链很长，那么访问一个对象的属性就会因为花更多的时间查找而变得更慢，因此要注意不要把原型链搞得太长。

### 构造函数
```js
function Student(name) {
    this.name = name;
    this.hello = function () {
        alert('Hello, ' + this.name + '!');
    }
}

var xiaoming = new Student('小明');
xiaoming.name; // '小明'
xiaoming.hello(); // Hello, 小明!

var xiaohong = new Student('小红');
xiaohong.name; // '小红'
xiaohong.hello(); // Hello, 小红!

```
注意，如果不写new，这就是一个普通函数，它返回undefined。但是，如果写了new，它就变成了一个构造函数，它绑定的this指向新创建的对象，并默认返回this，也就是说，不需要在最后写return this;。

`xiaohong`,`xiaoming`的原型链：
```js
xiaoming ↘
           Student.prototype ----> Object.prototype ----> null
xiaohong  ↗
```

用new Student()创建的对象还从原型上获得了一个constructor属性，它指向函数Student本身：
```js
xiaoming.constructor === Student.prototype.constructor; // true
Student.prototype.constructor === Student; // true
Object.getPrototypeOf(xiaoming) === Student.prototype; // true
xiaoming instanceof Student; // true
```
看到这些判断等式，心里一万只草泥马奔过

![image](https://ss2.bdstatic.com/70cFvnSh_Q1YnxGkpoWK1HF6hhy/it/u=3938879010,3493623516&fm=27&gp=0.jpg)

让我们用一张原型链图解释吧：

![image](https://cdn.webxueyuan.com/cdn/files/attachments/00143529922671163eebb527bc14547ac11363bf186557d000/l)

红色箭头是原型链。注意，Student.prototype指向的对象就是xiaoming、xiaohong的原型对象，这个原型对象自己还有个属性constructor，指向Student函数本身。

另外，函数Student恰好有个属性prototype指向xiaoming、xiaohong的原型对象，但是xiaoming、xiaohong这些对象可没有prototype这个属性，不过可以用__proto__这个非标准用法来查看。

现在我们就认为xiaoming、xiaohong这些对象“继承”自Student。

不过还有一个小问题，注意观察：
```js
xiaoming.name; // '小明'
xiaohong.name; // '小红'
xiaoming.hello; // function: Student.hello()
xiaohong.hello; // function: Student.hello()
xiaoming.hello === xiaohong.hello; // false
```

xiaoming和xiaohong各自的name不同，这是对的，否则我们无法区分谁是谁了。

xiaoming和xiaohong各自的hello是一个函数，但它们是两个不同的函数，虽然函数名称和代码都是相同的！

如果我们通过new Student()创建了很多对象，这些对象的hello函数实际上只需要共享同一个函数就可以了，这样可以节省很多内存。

要让创建的对象共享一个hello函数，根据对象的属性查找原则，我们只要把hello函数移动到xiaoming、xiaohong这些对象共同的原型上就可以了，也就是Student.prototype：

![image](https://cdn.webxueyuan.com/cdn/files/attachments/001435299854512faf32868f60348be878982909b5a5d04000/l)

修改代码如下：
```js
function Student(name) {
    this.name = name;
}

Student.prototype.hello = function () {
    alert('Hello, ' + this.name + '!');
};

```

### 原型继承
回顾Student构造函数，以及Student的原型链，现在，我们要基于Student扩展出PrimaryStudent，可以先定义出PrimaryStudent：
```js
function PrimaryStudent(props) {
    // 调用Student构造函数，绑定this变量:
    Student.call(this, props);
    this.grade = props.grade || 1;
}
```
但是，调用了Student构造函数不等于继承了Student，PrimaryStudent创建的对象的原型是：
```js
new PrimaryStudent() ----> PrimaryStudent.prototype ----> Object.prototype ----> null
```
必须想办法把原型链修改为：
```js
new PrimaryStudent() ----> PrimaryStudent.prototype ----> Student.prototype ----> Object.prototype ----> null
```
我们必须借助一个中间对象来实现正确的原型链，这个中间对象的原型要指向Student.prototype。为了实现这一点，参考道爷（就是发明JSON的那个道格拉斯）的代码，中间对象可以用一个空函数F来实现：
```js
// PrimaryStudent构造函数:
function PrimaryStudent(props) {
    Student.call(this, props);
    this.grade = props.grade || 1;
}

// 空函数F:
function F() {
}

// 把F的原型指向Student.prototype:
F.prototype = Student.prototype;

// 把PrimaryStudent的原型指向一个新的F对象，F对象的原型正好指向Student.prototype:
PrimaryStudent.prototype = new F();

// 把PrimaryStudent原型的构造函数修复为PrimaryStudent:
PrimaryStudent.prototype.constructor = PrimaryStudent;

// 继续在PrimaryStudent原型（就是new F()对象）上定义方法：
PrimaryStudent.prototype.getGrade = function () {
    return this.grade;
};

// 创建xiaoming:
var xiaoming = new PrimaryStudent({
    name: '小明',
    grade: 2
});
xiaoming.name; // '小明'
xiaoming.grade; // 2

// 验证原型:
xiaoming.__proto__ === PrimaryStudent.prototype; // true
xiaoming.__proto__.__proto__ === Student.prototype; // true

// 验证继承关系:
xiaoming instanceof PrimaryStudent; // true
xiaoming instanceof Student; // true

```
用一张图来表示新的原型链：

![image](https://cdn.webxueyuan.com/cdn/files/attachments/001439872160923ca15925ec79f4692a98404ddb2ed5503000/l)

注意，函数F仅用于桥接，我们仅创建了一个new F()实例，而且，没有改变原有的Student定义的原型链。
如果把继承这个动作用一个inherits()函数封装起来，还可以隐藏F的定义，并简化代码：
```js
function inherits(Child, Parent) {
    var F = function () {};
    F.prototype = Parent.prototype;
    Child.prototype = new F();
    Child.prototype.constructor = Child;
}
```
这个inherits()函数可以复用：
```js
function Student(props) {
    this.name = props.name || 'Unnamed';
}

Student.prototype.hello = function () {
    alert('Hello, ' + this.name + '!');
}

function PrimaryStudent(props) {
    Student.call(this, props);
    this.grade = props.grade || 1;
}

// 实现原型继承链:
inherits(PrimaryStudent, Student);

// 绑定其他方法到PrimaryStudent原型:
PrimaryStudent.prototype.getGrade = function () {
    return this.grade;
};
```
怎么样，js中的类构造与继承，还是很玄妙的吧！？

![image](https://ss1.bdstatic.com/70cFuXSh_Q1YnxGkpoWK1HF6hhy/it/u=4018913711,2747605690&fm=111&gp=0.jpg)

### class继承
受够了js中奇葩的类构造与继承，ECMAScript 6标准中为coder们带来了福音，全新的class继承，简单方便，实属炎炎夏日中林荫道。

新的关键字class从ES6开始正式被引入到JavaScript中。class的目的就是让定义类更简单。

我们先回顾用函数实现Student的方法：
```js
function Student(name) {
    this.name = name;
}

Student.prototype.hello = function () {
    alert('Hello, ' + this.name + '!');
}
```
如果用新的class关键字来编写Student，可以这样写：
```js
class Student {
    constructor(name) {
        this.name = name;
    }

    hello() {
        alert('Hello, ' + this.name + '!');
    }
}
```
比较一下就可以发现，class的定义包含了构造函数constructor和定义在原型对象上的函数hello()（注意没有function关键字），这样就避免了Student.prototype.hello = function () {...}这样分散的代码。
最后，创建一个Student对象代码和前面章节完全一样：
```js
var xiaoming = new Student('小明');
xiaoming.hello();
```

### class继承
用class定义对象的另一个巨大的好处是继承更方便了。想一想我们从Student派生一个PrimaryStudent需要编写的代码量。现在，原型继承的中间对象，原型对象的构造函数等等都不需要考虑了，直接通过extends来实现：
```js
class PrimaryStudent extends Student {
    constructor(name, grade) {
        super(name); // 记得用super调用父类的构造方法!
        this.grade = grade;
    }

    myGrade() {
        alert('I am at grade ' + this.grade);
    }
}

```
如此，美妙！

![image](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1511847611807&di=80b445e2432c67d58cd63cf41f03dab6&imgtype=0&src=http%3A%2F%2Fc.hiphotos.baidu.com%2Fimage%2Fpic%2Fitem%2Fbd3eb13533fa828b9c5fc3fef61f4134970a5aaa.jpg)

注：以上内容，摘自[廖雪峰JavaScript教程](https://www.liaoxuefeng.com/wiki/001434446689867b27157e896e74d51a89c25cc8b43bdb3000)。
