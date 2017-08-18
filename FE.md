

# 前端技术积累

[TOC]

#### 前言

这个面经的来源一共分为三部分。第一部分是市面上的面经结合自己的知识总结。第二部分就是自己的面经，大大小小面了也有一些了，希望自己也能总结总结造福自己造福后人。最后一部分就是针对自己做过的东西，简历上的东西做的总结，不具有普遍性。当然文件夹里还有一些算法题，平常做前端的也要动动脑嘛。希望自己和大家都能找到满意的工作。



## 第一部分 前端面经总结

大赞！https://github.com/paddingme/Front-end-Web-Development-Interview-Question/tree/master/%E5%89%8D%E7%AB%AF%E8%AF%95%E9%A2%98

### html基础

##### SEO

###### html

避免空链接（因为还是会请）

避免深层级嵌套

显示设置宽高

避免脚本阻塞加载

###### css

避免使用@import

###### JS

事件代理

避免频繁的dom操作

###### 网络

减少http请求次数

减少dns查找次数（缓存三十分钟，可以分块）

减少重定向

首屏加载，滚屏加载

能使用GET就用GET

使用外部js，css

减少cookie

##### GET POST

| &       | get        | post   |
| ------- | ---------- | ------ |
| 后退/刷新   | 无害         | 请求重新提交 |
| 书签      | 可做书签       | 不可做    |
| 缓存      | 可被缓存       | 不能被缓存  |
| 历史      | 保留在浏览器记录里  | 不保留    |
| 对数据长度限制 | 限制（2048字符） | 不限制    |
| 安全性     | url中暴露数据   | 相对安全   |
| 可见性     | url中可见     | 不可见    |

##### What's the difference between HTML and XHTML?

XHTML is not so much different from HTML 4.01 standard. The major differences are:

- XHTML elements must be **properly nested**.
- XHTML elements must always be **closed**.
- XHTML elements must be in **lowercase**.
- XHTML documents must have **one root element**.

##### h5标签了解多少

```html
<header><aside><nav><footer><hgroup><canvas><vedio><source><mark>
```



> 语义化的好处
>
> 1去掉样式能让页面清晰的呈现出来
>
> 2屏幕阅读器会按标记读你的网页／移动端友好
>
> 3.有益于SEO，爬虫
>
> 4.方便同事共同开发

##### 浏览器内核

Gecko:Firefox

Presto:opera

Webkit:chrome,safari

Trident:IE

区分浏览器—navigator.userAgent

判断是否是IE-window.ActiveXObject

##### NodeList 和 HTMLCollection之间的关系？

主要不同在于HTMLCollection是元素集合而NodeList是节点集合（即可以包含元素，也可以包含文本节点）。所以 node.childNodes 返回 NodeList，而 node.children 和 node.getElementsByXXX 返回 HTMLCollection 

##### rem实现自适应

相对于根元素决定字体大小。

##### 常见兼容性问题

```
* 上下margin重合问题
ie和ff都存在，相邻的两个div的margin-left和margin-right不会重合，但是margin-top和margin-bottom却会发生重合。
解决方法，养成良好的代码编写习惯，同时采用margin-top或者同时采用margin-bottom。
```

##### DOM操作——怎样添加、移除、移动、复制、创建和查找节点

```
（1）创建新节点

      createDocumentFragment()    //创建一个DOM片段

      createElement()   //创建一个具体的元素

      createTextNode()   //创建一个文本节点

（2）添加、移除、替换、插入

      appendChild()

      removeChild()

      replaceChild()

      insertBefore() //在已有的子节点前插入一个新的子节点

（3）查找

      getElementsByTagName()    //通过标签名称

      getElementsByName()    //通过元素的Name属性的值(IE容错能力较强，会得到一个数组，其中包括id等于name值的)

      getElementById()    //通过元素Id，唯一性
```

##### documen.write和 innerHTML的区别

```
document.write只能重绘整个页面

innerHTML可以重绘页面的一部分
```

##### DOM操作插入节点

```javascript
(() => {
    const ndContainer = document.getElementById('js-list');
    if (!ndContainer) {
        return;
    }

    const total = 30000;
    const batchSize = 4; // 每批插入的节点次数，越大越卡
    const batchCount = total / batchSize; // 需要批量处理多少次
    let batchDone = 0;  // 已经完成的批处理个数

    function appendItems() {
        const fragment = document.createDocumentFragment();
        for (let i = 0; i < batchSize; i++) {
            const ndItem = document.createElement('li');
            ndItem.innerText = (batchDone * batchSize) + i + 1;
            fragment.appendChild(ndItem);
        }

        // 每次批处理只修改 1 次 DOM
        ndContainer.appendChild(fragment);

        batchDone += 1;
        doBatchAppend();
    }

    function doBatchAppend() {
        if (batchDone < batchCount) {
            window.requestAnimationFrame(appendItems);
        }
    }

    // kickoff
    doBatchAppend();

    ndContainer.addEventListener('click', function (e) {
        const target = e.target;
        if (target.tagName === 'LI') {
            alert(target.innerHTML);
        }
    });
})();
```

###### 知识点

DocumentFragment:文档碎片，虚拟的dom，优化了多次插入。

requestAnimationFrame:因为电脑屏幕是60帧，setInterval这些16.7并不好。

- 浏览器可以优化并行的动画动作，更合理的重新排列动作序列，并把能够合并的动作放在一个渲染周期内完成，从而呈现出更流畅的动画效果。
- 在一个浏览器标签页里运行一个动画，当这个标签页不可见时，浏览器会暂停它，这会减少CPU，内存的压力，节省电池电量。

如果想自己设置频率：

```javascript
var fps = 15;
function draw() {
    setTimeout(function() {
        requestAnimationFrame(draw);
        // Drawing code goes here
    }, 1000 / fps);
}
```

### CSS基础

##### Overflow :hidden 是否形成新的块级格式化上下文？

```
<div>
    <p>I am floated</p>
    <p>So am I</p>
</div>

```

```
div {overflow: hidden;}
p {float: left;}

```

A：会形成。

会触发BFC的条件有：

- float的值不为none。
- overflow的值不为visible。
- display的值为table-cell, table-caption, inline-block 中的任何一个。
- position的值不为relative 和static。



####flex的使用

##### 容器属性

Main axis:主轴线 从左到右 Cross axis:交叉轴线 从上到下

```javascript
flex-direction: row | row-reverse | column | column-reverse; 
```

```javascript
flex-wrap: nowrap | wrap | wrap-reverse;
```

```javascript
justify-content: flex-start | flex-end | center | space-between | space-around;主轴对齐方式
```

```javascript
align-items: flex-start | flex-end | center | baseline | stretch;
```

```javascript
align-content: flex-start | flex-end | center | space-between | space-around | stretch;
定义了多根轴线的对齐方式。如果项目只有一根轴线，该属性不起作用
```

##### 项目属性

```javascript
order: <integer>;   数值越小，排列越靠前，默认为0
```

```javascript
flex-grow: <number>; 默认为0，即如果存在剩余空间，也不放大
```

```javascript
flex-shrink: <number>; 缩小比例，默认为1，即如果空间不足，该项目将缩小。如果所有项目的flex-shrink属性都为1，当空间不足时，都将等比例缩小。如果一个项目的flex-shrink属性为0，其他项目都为1，则空间不足时，前者不缩小
```

```javascript
flex-basis: <length> | auto;  定义了在分配多余空间之前，项目占据的主轴空间。就是设宽度
```

```javascript
flex: none|[ <'flex-grow'> <'flex-shrink'>? || <'flex-basis'> ]
            该属性有两个快捷值：auto (1 1 auto) 和 none (0 0 auto)。
```

```javascript
align-self: auto | flex-start | flex-end | center | baseline | stretch;
允许单个项目有与其他项目不一样的对齐方式
```

##### 实战

###### 骰子实现1239排列

```css
  display: flex;
  flex-wrap: wrap;
  justify-content: flex-end;
  align-content: space-between;
```

###### 圣杯布局

```html
<body class="HolyGrail">
  <header>...</header>
  <div class="HolyGrail-body">
    <main class="HolyGrail-content">...</main>
    <nav class="HolyGrail-nav">...</nav>
    <aside class="HolyGrail-ads">...</aside>
  </div>
  <footer>...</footer>
</body>
```

```css
.HolyGrail {
  display: flex;
  min-height: 100vh;
  flex-direction: column;
}

header,
footer {
  flex: 1;
}

.HolyGrail-body {
  display: flex;
  flex: 1;
}

.HolyGrail-content {
  flex: 1;
}

.HolyGrail-nav, .HolyGrail-ads {
  /* 两个边栏的宽度设为12em */
  flex: 0 0 12em;
}

.HolyGrail-nav {
  /* 导航放到最左边 */
  order: -1;
}
```

###### 固定低栏

```html
<body class="Site">
  <header>...</header>
  <main class="Site-content">...</main>
  <footer>...</footer>
</body>
```

```css
.Site {
  display: flex;
  min-height: 100vh;
  flex-direction: column;
}

.Site-content {
  flex: 1;
}
```



##### css选择器

| [*element*,*element*](http://www.w3school.com.cn/cssref/selector_element_comma.asp) | div,p           | 选择所有 <div> 元素和所有 <p> 元素。        | 1    |
| ---------------------------------------- | --------------- | ------------------------------- | ---- |
| [*element* *element*](http://www.w3school.com.cn/cssref/selector_element_element.asp) | div p           | 选择 <div> 元素内部的所有 <p> 元素。        | 1    |
| [*element*>*element*](http://www.w3school.com.cn/cssref/selector_element_gt.asp) | div>p           | 选择父元素为 <div> 元素的所有 <p> 元素。      | 2    |
| [*element*+*element*](http://www.w3school.com.cn/cssref/selector_element_plus.asp) | div+p           | 选择紧接在 <div> 元素之后的所有 <p> 元素。     | 2    |
| [[*attribute*\]](http://www.w3school.com.cn/cssref/selector_attribute.asp) | [target]        | 选择带有 target 属性所有元素。             | 2    |
| [[*attribute*=*value*\]](http://www.w3school.com.cn/cssref/selector_attribute_value.asp) | [target=_blank] | 选择 target="_blank" 的所有元素。       | 2    |
| [[*attribute*~=*value*\]](http://www.w3school.com.cn/cssref/selector_attribute_value_contain.asp) | [title~=flower] | 选择 title 属性包含单词 "flower" 的所有元素。 | 2    |
| [[*attribute*\|=*value*\]](http://www.w3school.com.cn/cssref/selector_attribute_value_start.asp) | [lang\|=en]     | 选择 lang 属性值以 "en" 开头的所有元素。      | 2    |



##### screen关键词是指设备物理屏幕的大小还是指浏览器的视窗？

```
@media only screen and (max-width: 1024px) {margin: 0;}

```

A: 浏览器视窗



##### Difference between block  inline-block inline

大体来说HTML元素各有其自身的布局级别（block元素还是inline元素）：

常见的块级元素有 DIV, FORM, TABLE, P, PRE, H1~H6, DL, OL, UL 等。

常见的内联元素有 SPAN, A, STRONG, EM, LABEL, INPUT, SELECT, TEXTAREA, IMG, BR 等。

block元素可以包含block元素和inline元素；但inline元素只能包含inline元素。

- display:block

1. 1. block元素会独占一行，多个block元素会各自新起一行。默认情况下，block元素宽度自动填满其父元素宽度。
   2. block元素可以设置width,height属性。块级元素即使设置了宽度,仍然是独占一行。
   3. block元素可以设置margin和padding属性。

- display:inline

1. 1. inline元素不会独占一行，多个相邻的行内元素会排列在同一行里，直到一行排列不下，才会新换一行，其宽度随元素的内容而变化。
   2. inline元素设置width,height属性无效。
   3. inline元素的margin和padding属性，水平方向的padding-left, padding-right, margin-left, margin-right都产生边距效果；但竖直方向的padding-top, padding-bottom, margin-top, margin-bottom不会产生边距效果。

- display:inline-block

1. 1. 简单来说就是将对象呈现为inline对象，但是对象的内容作为block对象呈现。之后的内联对象会被排列在同一行内。比如我们可以给一个link（a元素）inline-block属性值，使其既具有block的宽度高度特性又具有inline的同行特性。

      ​

##### Difference between transition and transform

###### transition

```css
img{
    height:15px;
    width:15px;
}

img:hover{
    height: 450px;
    width: 450px;
}
img{
    transition: 1s 1s height ease;
}
```

只选择效果时间。transition的优点在于简单易用，但是它有几个很大的局限。

（1）transition需要事件触发，所以没法在网页加载时自动发生。

（2）transition是一次性的，不能重复发生，除非一再触发。

（3）transition只能定义开始状态和结束状态，不能定义中间状态，也就是说只有两个状态。

（4）一条transition规则，只能定义一个属性的变化，不能涉及多个属性。



##### css圆角

如果是长度，就是圆角的半径，0就是直角。

如果是百分比，超过50%，四个角就合成椭圆了



##### mypic.jpg会被浏览器加载吗？

```
<div id="test1">
    <span id="test2"></span>
</div>

#test1 {
    display: none;
}
#test2 {
    background-image: url('mypic.jpg');
    visibility: hidden;
}

```

A: 不会被下载。

##### What is Flash of Unstyled Content? How do you avoid FOUC?

原因大致为：1，使用import方法导入样式表。2，将样式表放在页面底部3，有几个样式表，放在html结构的不同位置。其实原理很清楚：当样式表晚于结构性html加载，当加载到此样式表时，页面将停止之前的渲染。此样式表被下载和解析后，将重新渲染页面，也就出现了短暂的花屏现象。

解决方法：使用LINK标签将样式表放在文档HEAD中。

##### 隐藏元素的方法

Display,visibility,opacity,position:absolute;top:-9999px;

##### { box-sizing: border-box; } 

固定了盒子的尺寸，无论怎么调整边距都不会改变盒子的大小.似乎是padding变了

### Js基础

##### JS原生自定义事件

**在某个对象上绑定不同类别的一个或多个方法，并且让它们分别执行**

```javascript
var eventHandle = {
    on: function(obj,events,fn){
        obj.listeners = obj.listeners || {};
        obj.listeners[events] = obj.listeners[events] || [];
        obj.listeners[events].push(fn);
    },
    fire: function(obj,events){
        for(var i = 0, n = obj.listeners[events].length; i &lt; n; i++){
            console.log(obj.listeners[events]);
            obj.listeners[events][i] && obj.listeners[events][i]();
        }
    },
    off: function(obj,events){
        for(var i = 0, n = obj.listeners[events].length; i &lt; n; i++){
            obj.listeners[events][i] = null;
        }
    }
};

//绑定自定义事件，
eventHandle.on(oDiv,"eventType1",function(){console.log(1);});//准备执行方法1
eventHandle.on(oDiv,"eventType1",function(){console.log(2);});//准备执行方法2
eventHandle.on(oDiv,"eventType1",function(){console.log(3);});//准备执行方法3
eventHandle.on(oDiv,"eventType2",function(){console.log(4);});//准备执行方法4

//触发执行
eventHandle.fire(oDiv,"eventType1");//执行eventType1下的所有方法
```





##### 原型链，继承       http://www.jianshu.com/p/3255d9eb8ece

区别类的继承和实例化

非常常用的类继承是这个样子的：

`B.prototype = new A()`
这时候特别容易和实例化给混淆了(反正我混了*—*)：
`b = new A()`

js的继承方式

1.原型链继承：

```javascript
//父类
var Animal = function(){
  //可以在构造函数里面直接设置属性~
  this.name = 'animal';
}
//也可以通过prototype
Animal.prototype.say = function(){
  console.log('Animal here');
}

//子类
var Dog = function(){
}
Dog.prototype = new Animal();
//改写父类prototype
Dog.prototype.name = 'dog';
```

2.构造继承

```javascript
//父类还是一样样的
var Dog = function(){
    Animal.call(this);
    this.name = 'dog';
}

var doge = new Dog();
console.log(doge.name) //'dog';
doge.say() //error;
```

3.实例继承

```javascript
var Dog = function(){ 
    var dog = new Animal();
    dog.name = 'dog';
    return dog;
}

var doge = new Dog();
console.log(doge.name)  //'dog';
doge.say()  //'Animal here';
console.log(doge instanceof Animal)  // true
console.log(doge instanceof Dog)  // false
```

4.拷贝继承

```javascript
var Dog = function(){ 
    var animal = new Animal();
    for(var attr in animal){
        this[attr] = animal[attr];
    }
    this.name = 'dog';
}

//创建实例
var doge = new Dog();
console.log(doge.name) //'dog';
doge.say() //'Animal here';
console.log(doge instanceof Animal) // false
console.log(doge instanceof Dog) // true
```

5.组合继承

```javascript
var Dog = function(){ 
  Animal.call(this, arguments);
  this.name = 'dog';
}
Dog.prototype = new Animal();

//创建实例
var doge = new Dog();
console.log(doge.name) //'dog';
doge.say() //'Animal here';
console.log(doge instanceof Dog) // true
console.log(doge instanceof Animal) // true
```

6.Object.create







##### 原始数据结构类型和引用类型的区别

（讲的特好https://segmentfault.com/a/1190000008472264）

###### 原始数据类型

1.基本数据类型的值都是不可变的

```javascript
var name = "change";
name = "change1";
console.log(name)//change1
```

原来的change并没有改变，只是将指针指向了change1

2.基本数据类型不可以添加属性和方法

3.基本数据类型的赋值是简单的赋值。就直接把值给你

4.基本数据类型值的比较是值的比较

5.基本数据类型存放在栈区

包括变量的标识符和值

###### 引用数据类型

1.引用类型的值是可以改变的

2.引用类型可以添加属性和方法

3.引用类型的赋值是对象的引用（只是把指针给你）

4.引用类型的比较是引用的比较

5.引用类型是同时存在栈和堆中

引用类型的存储需要在内存的栈区和堆区共同完成，栈区保存变量标识符和指向堆内存的地址

###### 基本包装类型

ECMAScript还提供了三个特殊的引用类型Boolean,String,Number.我们称这三个特殊的引用类型为基本包装类型，也叫包装对象.

```javascript
var s1 = "helloworld";
var s2 = s1.substr(4);
```

所以当第二行代码访问s1的时候，后台会自动完成下列操作：

1. 创建String类型的一个实例；// var s1 = new String("helloworld");
2. 在实例上调用指定方法；// var s2 = s1.substr(4);
3. 销毁这个实例；// s1 = null;

正因为有第三步这个销毁的动作，所以你应该能够明白为什么基本数据类型不可以添加属性和方法，这也正是基本装包类型和引用类型主要区别：对象的生存期.使用new操作符创建的引用类型的实例，在执行流离开当前作用域之前都是一直保存在内存中.而自动创建的基本包装类型的对象，则只存在于一行代码的执行瞬间，然后立即被销毁

###### Null 和 Undefined 的区别

```javascript
Number(Null)//0
5+Null //5
Number(undefined)// NaN
5 + undefined// NaN
```

**null表示"没有对象"，即该处不应该有值**

（1） 作为函数的参数，表示该函数的参数不是对象。

（2） 作为对象原型链的终点。 //new Object(null). 没有继承任何对象，自己就是终点

**undefined表示"缺少值"，就是此处应该有一个值，但是还没有定义**

（1）变量被声明了，但没有赋值时，就等于undefined。

（2) 调用函数时，应该提供的参数没有提供，该参数等于undefined。

（3）对象没有赋值的属性，该属性的值为undefined。

（4）函数没有返回值时，默认返回undefined。

##### Object.defineProperty(obj, prop, descriptor)

descriptor中定义的参数用来定义或修改的属性的描述符

`configurable` 当且仅当该属性的 configurable 为 true 时，该属性`描述符`才能够被改变，也能够被删除。

`enumerable `当且仅当该属性的 enumerable 为 true 时，该属性才能够出现在对象的枚举属性中。。 

属性特性 `enumerable` 决定这个属性是否能被 `for...in` 循环或 `Object.keys` 方法遍历得到

`writable`当且仅当该属性的 writable 为 true 时，该属性才能被`[赋值运算符]`改变。

`value`该属性对应的值。可以是任何有效的 JavaScript 值（数值，对象，函数等）。

`get`一个给属性提供 getter 的方法，如果没有 getter 则为 `undefined`。该方法返回值被用作属性值。

`set`一个给属性提供 setter 的方法，如果没有 setter 则为 `undefined`。该方法将接受唯一参数，并将该参数的新值分配给该属性。

##### 关于对象添加getter和setter的方法

1.通过对象初始化器在创建对象的时候指明（也可以称为通过字面值创建对象时声明）

```javascript
(function () {
    var o = {
        a : 7,
        get b(){return this.a +1;},//通过 get,set的 b,c方法间接性修改 a 属性
        set c(x){this.a = x/2}
    };
    console.log(o.a);
    console.log(o.b);
    o.c = 50;
    console.log(o.a);
})();
```

2.使用 `Object.create` 方法. Object.create(proto, [ propertiesObject ])

```javascript
(function () {
    var o = null;
    o = Object.create(Object.prototype,//指定原型为 Object.prototype
            {
                bar:{
                    get :function(){
                        return this.a;
                    },
                    set : function (val) {
                        console.log("Setting `o.bar` to ",val);
                        this.a = val;
                    },
                    configurable :true
                }
            }//第二个参数
        );
    o.a = 10;
    console.log(o.bar);
    o.bar = 12;
    console.log(o.bar);
})();
```

3.使用 `Object.defineProperty` 方法.    Object.defineProperty(obj, prop, descriptor)

```javascript
(function () {
    var o = { a : 1}//声明一个对象,包含一个 a 属性,值为1
    Object.defineProperty(o,"b",{
        get: function () {
            return this.a;
        },
        set : function (val) {
            this.a = val;
        },
        configurable : true
    });

    console.log(o.b);
    o.b = 2;
    console.log(o.b);
})();
```

4.使用 `Object.defineProperties`方法.   Object.defineProperties(obj, props)

```javascript
(function () {
    var obj = {a:1,b:"string"};
    Object.defineProperties(obj,{
        "A":{
            get:function(){return this.a+1;},
            set:function(val){this.a = val;}
        },
        "B":{
            get:function(){return this.b+2;},
            set:function(val){this.b = val}
        }
    });

    console.log(obj.A);
    console.log(obj.B);
    obj.A = 3;
    obj.B = "hello";
    console.log(obj.A);
    console.log(obj.B);
})();
```

##### 对象的扩展，密封以及冻结

- 扩展特性

  - `Object.isExtensible` 方法

    可扩展和上述的可修改不是一个概念

    ```javascript
    //对象是否可以扩展与对象的属性是否可以配置无关
    empty = Object.create({},{
        "a":{
            value : 1,
            configurable : false,//不可配置
            enumerable : true,//可枚举
            writable : true//可写
        }
    });
    console.log(Object.isExtensible(empty) === true);//true
    ```

  - `Object.preventExtensions` 方法

    修改为不可扩展。如果为当前不可扩展对象 empty 修改属性是成功的，这是因为一个对象的属性是否可以被修改与该对象是否可以扩展无关，而是与该对象在创建的时候是否声明为不可重写有关

- 密封特性

  - `Object.isSealed` 方法

    密封对象是指那些不能添加新的属性，不能删除已有属性，以及不能修改已有属性的可枚举性、可配置性、可写性，但可以修改已有属性的值的对象。

    ```javascript
    (function () {
        //新建的对象默认不是密封的
        var empty = {};
        console.log(Object.isSealed(empty) === false);//true

        //如果把一个空对象变得不可扩展,则它同时也会变成个密封对象.
        Object.preventExtensions(empty);
        console.log(Object.isSealed(empty) === true);//true

        //但如果这个对象不是空对象,则它不会变成密封对象,因为密封对象的所有自身属性必须是不可配置的.
        var hasProp = {fee : "fie foe fum"};
        Object.preventExtensions(hasProp);
        console.log(Object.isSealed(hasProp) === false);//true

        //如果把这个属性变得不可配置,则这个对象也就成了密封对象.
        Object.defineProperty(hasProp,"fee",{configurable : false});
        console.log(Object.isSealed(hasProp) === true);//true
    })();
    ```

  - `Object.seal` 方法

  ```javascript
  Object.seal(o);  //与下面的操作效果相同

  Object.defineProperty(o,"a",{configurable:false,writable:false});
  Object.preventExtensions(o);
  ```

- 冻结特性

  - `Object.isFrozen` 方法

     冻结对象是指那些不能添加新的属性，不能修改已有属性的值，不能删除已有属性，以及不能修改已有属性的可枚举性、可配置性、可写性的对象。也就是说，这个对象永远是不可变的。


-   `Object.freeze` 方法

    - `浅冻结` 与 `深冻结`

      倘若一个对象的属性是一个对象，那么对这个外部对象进行冻结，内部对象的属性是依旧可以改变的，这就叫浅冻结，若把外部对象冻结的同时把其所有内部对象甚至是内部的内部无限延伸的对象属性也冻结了，这就叫深冻结。

      深冻结：遍历递归操作冻结每一层

##### 事件代理

###### Jquery

```javascript
$("#tab").bind("click",function(ev)){
   var $obj=$(ev.target);
   $obj.css("background","red");
}
```

###### Js

```javascript
  var ulNode=document.getElementById("list");
  ulNode.addEventListener('click',function(e){
       if(e.target&&e.target.nodeName.toUpperCase()=="LI"){/*判断目标事件是否为li*/
         alert(e.target.innerHTML);
       }
     },false);
```

##### async和defer的作用是什么？有什么区别

1.<script src="example.js"></script>

没有defer或async属性，浏览器会立即加载并执行相应的脚本。也就是说在渲染script标签之后的文档之前，不等待后续加载的文档元素，读到就开始加载和执行，此举会阻塞后续文档的加载；
2.<script async src="example.js"></script>

有了async属性，表示后续文档的加载和渲染与js脚本的加载和执行是并行进行的，即异步执行；（异步加载，加载完马上执行）
3.<script defer src="example.js"></script>

有了defer属性，加载后续文档的过程和js脚本的加载(此时仅加载不执行)是并行进行的(异步)，js脚本的执行需要等到文档所有元素解析完成之后，DOMContentLoaded事件触发执行之前。(不耽误后边文档加载，但是都加载完执行。)





##### for in 和for of的区别是什么

for in 遍历的是索引. 还可以遍历对象，但是可能会遍历到继承的元素方法，使用hasOwnProperty（）判断

for of遍历的是对应的元素值

遍历对象新出的 Object.keys() Object.values. Object.entires()



##### 闭包 ！

###### 什么是闭包？

利用块状作用域把外部的变量hold住

js分全局作用域和函数作用域。函数作用域里可以访问到全局，通过一个叫作用域链的东西。但全局怎么访问函数呢？就有人想了在函数里面再写一个函数(闭包)，这样把作用域链加长了。就可以在全局访问到函数里的数据了。闭包能访问到父级函数里面的数据说明父级里的数据一直存在内存中(闭包存在的情况下)，这就会导致内存一直被占着。

一句话总结闭包：对函数外层的变量持有访问权。

```javascript
function Counter(start) {
    var count = start;
    return {
        increment: function() {
            count++;
        },

        get: function() {
            return count;
        }
    }
}

var foo = Counter(4);
foo.increment();
foo.get(); // 5
```

###### 闭包与setTimeout

改变函数使得输出12345

```javascript
for (var i=1; i<=5; i++) { 
    setTimeout( function timer() {
        console.log(i);
    }, i*1000 );
}
```

answer

```javascript
for (var i=1; i<=5; i++) { 
	(setTimeout(function timer(){
      console.log(i);
	},i*1000)
     })(i)
}
for(var i=1;i<5;i++){
  setTimeout((function timer(){
    return function(i){
      console.log(i)
    })(i)
  },i*1000)
}
```

```javascript
var output = function (i) {
    setTimeout(function() {
        console.log(new Date, i);
    }, 1000);
};

for (var i = 0; i < 5; i++) {
    output(i);  // 这里传过去的 i 值被复制了
}

console.log(new Date, i);
```

使用Promise

```javascript
const tasks = []; // 这里存放异步操作的 Promise
const output = (i) => new Promise((resolve) => {
    setTimeout(() => {
        console.log(new Date, i);
        resolve();
    }, 1000 * i);
});

// 生成全部的异步操作
for (var i = 0; i < 5; i++) {
    tasks.push(output(i));
}

// 异步操作完成之后，输出最后的 i
Promise.all(tasks).then(() => {
    setTimeout(() => {
        console.log(new Date, i);
    }, 1000);
});
```

```javascript
// 模拟其他语言中的 sleep，实际上可以是任何异步操作
const sleep = (timeountMS) => new Promise((resolve) => {
    setTimeout(resolve, timeountMS);
});

(async () => {  // 声明即执行的 async 函数表达式
    for (var i = 0; i < 5; i++) {
        await sleep(1000);
        console.log(new Date, i);
    }

    await sleep(1000);
    console.log(new Date, i);
})();
```

变异啦！

```javascript
for (var i = 0; i < 5; i++) {
  (function() {
    setTimeout(function() {
      console.log(i);
    }, i * 1000);
  })(i);
}          //内部其实没有对参数的引用，所以还是55555

for (var i = 0; i < 5; i++) {
  (function(i) {
    setTimeout(function() {
      console.log(i);
    }, i * 1000);
  })();
}			//undefined x5


for (var i = 0; i < 5; i++) {
  setTimeout((function(i) {
    console.log(i);
  })(i), i * 1000);
}				//立即执行函数立即执行，setTimeout就等于传了个undefined。会立刻输出01234

setTimeout(function() {
  console.log(1)
}, 0);
new Promise(function executor(resolve) {
  console.log(2);
  for( var i=0 ; i<10000 ; i++ ) {
    i == 9999 && resolve();
  }
  console.log(3);
}).then(function() {		//2 3 5 4 1
  console.log(4);					
});
console.log(5);		
//执行过程如下：
JavaScript引擎首先从macrotask queue中取出第一个任务，
执行完毕后，将microtask queue中的所有任务取出，按顺序全部执行；
然后再从macrotask queue中取下一个，
执行完毕后，再次将microtask queue中的全部取出；
循环往复，直到两个queue中的任务都取完。

解释：
代码开始执行时，所有这些代码在macrotask queue中，取出来执行之。
后面遇到了setTimeout，又加入到macrotask queue中，
然后，遇到了promise.then，放入到了另一个队列microtask queue。
等整个execution context stack执行完后，
下一步该取的是microtask queue中的任务了。
因此promise.then的回调比setTimeout先执行。
```





##### 创建对象的三种方法

###### 原型对象

```javascript
//Object.create(proto, [ propertiesObject ]) 第二个参数为新要添加的属性
var Student = {
    name: 'Robot',
    height: 1.2,
    run: function () {
        console.log(this.name + ' is running...');
    }
};

function createStudent(name) {
    // 基于Student原型创建一个新对象:
    var s = Object.create(Student);
    // 初始化新对象:
    s.name = name;
    return s;
}
```

###### 构造函数

```javascript
function Student(props) {
    this.name = props.name || '匿名'; // 默认值为'匿名'
    this.grade = props.grade || 1; // 默认值为1
}

Student.prototype.hello = function () {
    alert('Hello, ' + this.name + '!');
};

function createStudent(props) {
    return new Student(props || {})
}
```

###### class 实现

```javascript
class Student {
    constructor(name) {
        this.name = name;
    }

    hello() {
        alert('Hello, ' + this.name + '!');
    }
}
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

##### this的误区

指向自身，this指向函数的作用域。 //this取决于调用位置

##### this对象绑定规则（箭头函数不满足）

new>call或者apply>上下文对象调用>严格模式下绑定undefined否则global

```javascript
var p= {
   data:{
      flag: true
   },
   init: ()=>{
     console.log(this.data.flag)
   }
}			 //箭头函数没有自己的this，他的this值继承自外部。而这里就是继承p的，p的上下文this是全局				window对象了，所以会报						undefined的错误
p.init()     //结果是undefined，如果是普通函数结果是true
```

##### 深度拷贝对象https://www.zhihu.com/question/23031215

```javascript
 window.val = 1;
  var obj = {
    val: 2,
    dbl: function () {
      this.val *= 2;
      val *= 2;
      console.log(val);
      console.log(this.val);
    }
  };
  // 说出下面的输出结果
  obj.dbl();
  var func = obj.dbl;
  func();      		// 2 4 8 8


  var obj = {
    say: function () {
      var f1 = () => {
        console.log(this); // obj
        setTimeout(() => {
          console.log(this); // obj
        })
      }
      f1();
    }
  }
  obj.say()
  
    var obj = {
    say: function () {
      var f1 = function () {
        console.log(this);    // window, f1调用时,没有宿主对象,默认是window
        setTimeout(() => {
          console.log(this); // window
        })
      };
      f1();
    }
  }
  obj.say()
```



```javascript
var a=10;
(function test(){
  console.log(a);//undefined
  a=100;
  console.log(a);//100
  console.log(this.a);//10
  var a;
})()
```



#### 正则表达式

RegExp 是JS中的类，同Array类似。

第一个参数正则匹配。第二个参数（g:全局查找  i:不区分大小写  m:多行查找）

##### 正则表达式的方法

test()—return boolean

exec()—return a Array with index and input

search()—return index and u can both input RegExp or String                               //字符串带的方法

replace()—as it looks like 

##### difference between [] {} ()

[0-9] 查找任何从 0 至 9 的数字

{8} 表示位数为8位

()的作用是提取匹配的字符串。表达式中有几个`()`就会得到几个相应的匹配字符串。比如`(\s+)`表示连续空格的字符串

##### ^ 和 $

`^` 匹配一个字符串的开头，比如 (`^a`) 就是匹配以字母`a`开头的字符串

`$` 匹配一个字符串的结尾,比如 (`b$`) 就是匹配以字母`b`结尾的字符串

`^`还有另个一个作用就是取反，比如`[^xyz]`表示匹配的字符串不包含`xyz`

##### \d \s \w .

`\d` 匹配一个非负整数， 等价于 `[0-9]`；

`\s` 匹配一个空白字符；

`\w` 匹配一个英文字母或数字，等价于`[0-9a-zA-Z]`；

`.` 匹配除换行符以外的任意字符，等价于`[^\n]`。

##### * + ?

`*`表示匹配前面元素0次或多次，比如`(\s*)`就是匹配0个或多个空格；

`+` 表示匹配前面元素1次或多次，比如`(\d+)`就是匹配由至少1个整数组成的字符串；

`?`表示匹配前面元素0次或1次，相当于`{0,1}`，比如`(\w?)` 就是匹配最多由1个字母或数字组成的字符串 。

##### 还有一些语法

[adgk]   查找给定集合内的任何字符。

`\W`  查找非单词字符。

`\d`  查找数字。

`\D` 查找非数字字符。

`\b` 匹配单词边界。

`\B` 匹配非单词边界。

`\0` 查找 NULL 字符。

`\n` 查找换行符。

`\f` 查找换页符。

`\r` 查找回车符。

`\t` 查找制表符。

`\v` 查找垂直制表符。

`\xxx` 查找以八进制数 xxx 规定的字符。

`\xdd` 查找以十六进制数 dd 规定的字符。

`\uxxxx` 查找以十六进制数 xxxx 规定的 Unicode 字符。

n{X,Y} `X`和 `Y` 为正整数。前面的模式`n` 连续出现至少 `X`次，至多 `Y`次时匹配

?=n 匹配任何其后紧接指定字符串`n` 的字符串。

?!n 匹配任何其后没有紧接指定字符串 `n` 的字符串



#### vue

##### diff算法

https://segmentfault.com/a/1190000008782928

###### 为什么要virtual dom

操作dom太耗资源，所以优化为操作对象

###### 核心：**比较只会在同层级进行, 不会跨层级比较。**

更新流程：

1.先判断两个vnode的key和sel是否相同。不值的比较就直接用新节点替代老节点。否则进入第二步

2.节点的比较有5种情况

1. `if (oldVnode === vnode)`，他们的引用一致，可以认为没有变化。
2. `if(oldVnode.text !== null && vnode.text !== null && oldVnode.text !== vnode.text)`，文本节点的比较，需要修改，则会调用`Node.textContent = vnode.text`。
3. `if( oldCh && ch && oldCh !== ch )`, 两个节点都有子节点，而且它们不一样，这样我们会调用`updateChildren`函数比较子节点，这是diff的核心，后边会讲到。
4. `else if (ch)`，只有新的节点有子节点，调用`createEle(vnode)`，`vnode.el`已经引用了老的dom节点，`createEle`函数会在老dom节点上添加子节点。
5. `else if (oldCh)`，新节点没有子节点，老节点有子节点，直接删除老节点。

3.通过设置的key进行遍历比较子节点

###### 结论

- 尽量不要跨层级的修改dom
- 设置key可以最大化的利用节点
- 不要盲目相信diff的效率，在必要时可以手工优化

##### 自己写一个vue组件

https://juejin.im/entry/58a11c648d6d81006c9d739d

仿照着分页自己写了个conole-panel的控制台。主要实现了，黑色背景，字体，以及随着控制log的增加，自动跟随到最新的信息。大概的思路就是自己写个普通的组件，要填的通过props传进来。然后vue.use引用就好了

##### 父子组件间通信

```javascript
Vue.component('button-counter', {
  template: '<button v-on:click="incrementCounter">{{ counter }}</button>',
  data: function () {
    return {
      counter: 0
    }
  },
  methods: {
    incrementCounter: function () {
      this.counter += 1
      this.$emit('increment')
    }
  },
})
new Vue({
  el: '#counter-event-example',
  data: {
    total: 0
  },
  methods: {
    incrementTotal: function () {
      this.total += 1
    }
  }
})
```

##### 非父子组件间通信

如果2个组件不是父子组件那么如何通信呢？这时可以通过eventHub来实现通信. 
所谓eventHub就是创建一个事件中心，相当于中转站，可以用它来传递事件和接收事件.

```
let Hub = new Vue(); //创建事件中心11
```

组件1触发：

```
<div @click="eve"></div>
methods: {
    eve() {
        Hub.$emit('change','hehe'); //Hub触发事件
    }
}123456123456
```

组件2接收:

```
<div></div>
created() {
    Hub.$on('change', () => { //Hub接收事件
        this.msg = 'hehe';
    });
}123456123456
```

这样就实现了非父子组件之间的通信了.原理就是把Hub当作一个中转站！

##### vue的生命周期

beforecreate    created

beforemounted	mounted

beforeupdate	updated

activated	deactivated

beforedestory	destroyed

##### vue的生命周期各阶段都做了什么？

`beforeCreate` 实例创建前：这个阶段实例的data、methods是读不到的
`created` 实例创建后：这个阶段已经完成了数据观测(data observer)，属性和方法的运算， watch/event 事件回调。mount挂载阶段还没开始，$el 属性目前不可见，数据并没有在DOM元素上进行渲染
`beforeMount`：在挂载开始之前被调用：相关的 render 函数首次被调用。
`mounted`：el选项的DOM节点 被新创建的 vm.$el 替换，并挂载到实例上去之后调用此生命周期函数。此时实例的数据在DOM节点上进行渲染
`beforeUpdate`：数据更新时调用，但不进行DOM重新渲染，在数据更新时DOM没渲染前可以在这个生命函数里进行状态处理
`updated`：这个状态下数据更新并且DOM重新渲染，当这个生命周期函数被调用时，组件 DOM 已经更新，所以你现在可以执行依赖于 DOM 的操作。当实例每次进行数据更新时updated都会执行
`beforeDestory`：实例销毁之前调用。
`destroyed`：Vue 实例销毁后调用。调用后，Vue 实例指示的所有东西都会解绑定，所有的事件监听器会被移除，所有的子实例也会被销毁。	

##### vue生命周期在真实场景下的业务应用

`created`：进行ajax请求异步数据的获取、初始化数据
`mounted`：挂载元素内dom节点的获取
`nextTick`：针对单一事件更新数据后立即操作dom
`updated`：任何数据的更新，如果要做统一的业务逻辑处理
`watch`：监听具体数据变化，并做相应的处理



#### vue和react的区别

##### 共同点

都是组件化

提供合理的钩子函数

ajax，route等功能都不在核心包里，而是以插件的方式加载

使用 Virtual DOM 

##### 区别

React依赖Virtual DOM,而Vue.js使用的是DOM模板

当组件状态发生变化的时候，react树会自上而下发生变化。需要通过shouldComponentUpdate方法避免某些组件渲染，然而这个时候也要保证props是这个子组件根。vue的渲染是自动追踪的。

vue有scope 自己的css作用域，相互之间不影响

ReactNative  vs  Vue+Veex 阿里，跨平台框架

状态变化跟踪，界面同步

- react 可以随时添加新的 state 成员；vue 不行，必须定义时准备好顶级成员，而且非顶级成员也必须通过api设置才能是响应式的；这点，react 比较方便 
- vue 可以跟踪任何 scope 的状态，包括各级父甚至不相关的，因为vue采用 getter/setter机制；react 默认只能检测本组件的状态变化，比较受限制 


以 javascript 为核心，和以 html 为核心

* react 是状态到 html 的映射 
* vue 是现有 HTML 模板，然后绑定到对应的 Vue javascript 对象上 


组件化比较

* react 倾向于细粒度的组件划分，确实也容易做到 
* vue 相对不太，但是如果 Vue 也采用 jsx 语法，那么还是比较容易做到的 


directive

vue 由于提供的 direct 特别是预置的 directive 因为场景场景开发更容易；react 没有 directive 
- v-if, v-show, v-else 
- v-text, v-html, 
- v-model 对于表单处理 vue 明显更方便 
- @event.prevent @keyxxx.enter 


watch

- vue 可以 watch 一个数据项；而 react 不行 


计算属性

- vue 有，提供方便；而 react 不行 






react的写法

```javascript
class Brother2 extends React.Component{
  constructor(props){
    super(props);
    this.state = {}
  }
  
  render(){
    return (
      <div>
         {this.props.text || "兄弟组件未更新"}
      </div>
    )
  }
}
class Parent extends React.Component{
  constructor(props){
    super(props);
    this.state = {}
  }
  refresh(){
    return (e)=>{
      this.setState({
        text: "兄弟组件沟通成功",
      })
    }
  }
  render(){
    return (
      <div>
        <h2>兄弟组件沟通</h2>
        <Brother1 refresh={this.refresh()}/>
        <Brother2 text={this.state.text}/>
      </div>
    )
```

##### react组件之间交流方式

父子：父组件更新组件状态／子组件触发更新父组件状态—也在props里调用父亲组件的方法改变state

兄弟：借助父组件更新，层次比较深就很不方便／React提供了一种上下文方式（挺方便的），可以让子组件直接访问祖先的数据或函数，无需从祖先组件一层层地传递数据到子组件中。










##### vue中socpe css怎么实现的

(源码https://github.com/AlloyTeam/AlloyTouch/blob/2b9f8ca35ab954c3a9a3ebb747e88c09503a16fa/example/scoped_css/index.html)

每个域中的dom都会自己添加自定义的data-id的属性，然后css中添加[data-id]，会匹配这些dom

动态的把css变成.example[_v-f3f3eg9].  

1. 在同一组件内，你能同时用有作用域和无作用域的样式：
2. 父组件的有作用域的CSS和子组件的有作用域的CSS将同时影响子组件。
3. 有作用域的样式对其他部分没有影响。
4. **有作用域限定的样式不排除类的需要**. 由于浏览器渲染方式支持多种不同的CSS选择器，加了作用域的 `p { color: red }` 会慢好多倍 （即，和属性选择器组合时候的时候）。如果你用类或者id选择器，比如 `.example { color: red }` ，你就能消除性能损失。[这里有个练习场](http://stevesouders.com/efws/css-selectors/csscreate.php) ，你可以比较测试下其中的差异。
5. **在递归组件中小心后代选择器！** 对于 CSS 规则的选择器 `.a .b`，如果匹配 `.a` 的元素内包含一个递归子组件，那么子组件中所有包含 `.b` 的元素都会被匹配到。





```javascript
        ;(function () {
            function scoper(css) {
                var id = generateID();
                var prefix = "#" + id;
                css = css.replace(/\/\*[\s\S]*?\*\//g, '');  //把样式放到一行
                var re = new RegExp("([^\r\n,{}]+)(,(?=[^}]*{)|\s*{)", "g"); //
                css = css.replace(re, function(g0, g1, g2) {
                    if (g1.match(/^\s*(@media|@keyframes|to|from|@font-face)/)) {
                        return g1 + g2;
                    }
                    if (g1.match(/:scope/)) {
                        g1 = g1.replace(/([^\s]*):scope/, function(h0, h1) {
                            if (h1 === "") {
                                return "> *";
                            } else {
                                return "> " + h1;
                            }
                        });
                    }
                    g1 = g1.replace(/^(\s*)/, "$1" + prefix + " ");
                    return g1 + g2;
                });
                addStyle(css,id+"-style");
                return id;
            }
            function generateID() {
                var id =  ("scoped"+ Math.random()).replace("0.","");
                if(document.getElementById(id)){
                    return generateID();
                }else {
                    return id;
                }
            }
            var isIE = (function () {
                var undef,
                    v = 3,
                    div = document.createElement('div'),
                    all = div.getElementsByTagName('i');
                while (
                    div.innerHTML = '<!--[if gt IE ' + (++v) + ']><i></i><![endif]-->',
                        all[0]
                    );
                return v > 4 ? v : undef;
            }());
            function addStyle(cssText, id) {
                var d = document,
                    someThingStyles = d.createElement('style');
                d.getElementsByTagName('head')[0].appendChild(someThingStyles);
                someThingStyles.setAttribute('type', 'text/css');
                someThingStyles.setAttribute('id', id);
                if (isIE) {
                    someThingStyles.styleSheet.cssText = cssText;
                } else {
                    someThingStyles.textContent = cssText;
                }
            }
            window.scoper = scoper;
        })();
        
        var id = scoper("h1 {\
                           color:red;\
                        /*color: #0079ff;*/\
                            }\
                    \
                            /*  h2 {\
                            color:green\
                            }*/");
        document.body.getElementsByTagName("div")[0].setAttribute("id",id);
```



##### Vuex-router 预备知识

###### 利用H5History API 无刷新更改地址栏

浏览器历史记录可以看作一个「栈」。

###### pushState(state, "My Profile", "/profile/") 方法

当调用他们修改浏览器历史记录栈后，虽然当前URL改变了，但浏览器不会立即发送请求该URL

执行`pushState`函数之后，会往浏览器的历史记录中添加一条新记录，同时改变地址栏的地址内容。它可以接收三个参数，按顺序分别为：

1. 一个对象或者字符串，用于描述新记录的一些特性。这个参数会被一并添加到历史记录中，以供以后使用。这个参数是开发者根据自己的需要自由给出的。
2. 一个字符串，代表新页面的标题。当前基本上所有浏览器都会忽略这个参数。
3. 一个字符串，代表新页面的相对地址。

###### popstate 事件

当用户点击浏览器的「前进」、「后退」按钮时，就会触发`popstate`事件。你可以监听这一事件，从而作出反应。

###### replaceState 方法

有时，你希望不添加一个新记录，而是替换当前的记录（比如对网站的 landing page），则可以使用`replaceState`方法。这个方法和`pushState`的参数完全一样。



###### 利用浏览器的hash特点

\#符号本身以及它后面的字符称之为hash，可通过window.location.hash属性读取。它具有如下特点：

- hash虽然出现在URL中，但不会被包括在HTTP请求中。它是用来指导浏览器动作的，对服务器端完全无用，因此，改变hash不会重新加载页面

- 可以为hash的改变添加监听事件：

  ```
  window.addEventListener("hashchange", funcRef, false)

  ```

- 每一次改变hash（window.location.hash），都会在浏览器的访问历史中增加一个记录

###### HashHistory.push()

```javascript
window.location.hash = route.fullPath
```



##### vue-router的具体实现的比较（https://zhuanlan.zhihu.com/p/27588422）

“更新视图但不重新请求页面”是前端路由原理的核心之一，目前在浏览器环境中这一功能的实现主要有两种方式：

- 利用URL中的hash（“#”）
- 利用History interface在 HTML5中新增的方法

###### 利用hash

从设置路由改变到视图更新的流程如下：

```javascript
$router.push() --> HashHistory.push() --> History.transitionTo() --> History.updateRoute() -！！！-> {app._route = route} --> vm.render()
```

在感叹号这一步过程中，updateRoute的回调函数触发了mixin（应该就是vue和router的mix）

```javascript
export function install (Vue) {
  Vue.mixin({
    beforeCreate () {
      if (isDef(this.$options.router)) {
        this._router = this.$options.router
        this._router.init(this)
        Vue.util.defineReactive(this, '_route', this._router.history.current)
      }
      registerInstance(this, this)
    },
  })
}
```

通过Vue.mixin()方法，全局注册一个混合，影响注册之后所有创建的每个 Vue 实例，该混合在beforeCreate钩子中通过Vue.util.defineReactive()定义了响应式的_route属性。所谓响应式属性，即当_route值改变时，会自动调用Vue实例的render()方法，更新视图。

repalce和push  同理，区别在于替换。另外：

```javascript
setupListeners ()  //用来监听手动替换的hash
```

###### 利用History

原理基本一样。不再赘述，方法替换就好。

###### 调用history.pushState()相比于直接修改hash主要有以下优势：

- pushState设置的新URL可以是与当前URL同源的任意URL；而hash只可修改#后面的部分，故只可设置与当前同文档的URL
- pushState设置的新URL可以与当前URL一模一样，这样也会把记录添加到栈中；而hash设置的新值必须与原来不一样才会触发记录添加到栈中
- pushState通过stateObject可以添加任意类型的数据到记录中；而hash只可添加短字符串
- pushState可额外设置title属性供后续使用



##### vuex是为什么出现的

- 管理多个组件共享状态。
- 全局状态管理。
- 状态变更跟踪。
- 让状态管理形成一种规范，使代码结构更清晰。

##### vue还是要更深层的了解，读源码

##### 双向绑定的的原理—源码   

##### 源码中的遍历对象

```javascript
     function touch(obj){
       if(obj === 'Object'){
         if(Array.isArray(obj)){
           obj.forEach(ele => {touch(ele)})
         }else{
           let keys=Object.keys(obj)
           for(let key in keys){
             touch(obj[key])
           }
         }
       }
       console.log(obj);
     }
```

#### 构建工具

##### webpack

```javascript
// webpack.config.js

var webpack = require("webpack");
var HtmlWebpackPlugin = require('html-webpack-plugin');
var ExtractTextPlugin = require('extract-text-webpack-plugin');

module.exports = {
    devtool: "source-map", 					//配置生成 Source Maps 的选项  将编译前后代码每行一												一对应起来，有四个不同轻重的选项。
    entry: __dirname + "/app/main.js", 		//入口文件路径
    output: {
        path: __dirname + "/build/", 		//存放打包后文件的地方路径
        filename: "[name]-[hash].js" 		//打包后的文件名
    },
    devServer: {							//构建本地服务器
        port: "9000",
        inline: true,						//改变文件自动刷新
        historyApiFallback: true,
        hot: true
    },
    module: {								//loader进行文件预处理，允许js之外所有静态自由
        loaders: [{							//匹配不同文件进行解析
            test: /\.json$/,
            loader: "json-loader"
        }, {
            test: /\.js$/,
            exclude: /node_modules/, 		//编译打包时需要排除 node_modules 文件夹
            loader: "babel-loader"			//.babelrc将babel配置写到这个文件中
        }, {
            test: /\.css$/,
            use: ExtractTextPlugin.extract({
                fallback: "style-loader",						
                use: "css-loader?modules!postcss-loader"		
              								//cssmodules运用模块
           									//postcss 解析scss,less之类的 
              								//css-loader 使你能够使用类似 @import 和 													url(...) 的方法实现 require() 的功能
              								//style-loader 将所有的计算后的样式加入页面中
            })
        }]
    },
    plugins: [								//插件
        new webpack.BannerPlugin("Copyright Flying Unicorns inc."), 
      										//在这个数组中new一个实例就可以了
        new HtmlWebpackPlugin({
            template: __dirname + "/app/index.tmpl.html" //new一个插件的实例，并传入相关的参数
        }),
        new webpack.HotModuleReplacementPlugin(), //热加载插件
        new webpack.optimize.OccurrenceOrderPlugin(),
        new webpack.optimize.UglifyJsPlugin(),
        new ExtractTextPlugin("[name]-[hash].css")		
    ]
}
```



##### Webpack中 —save-dev 和 —save 的区别

前者是开发时候用的，后者是发布之后也要用的

##### 模块系统

###### CommonJS

Nodejs

优点：

- 服务器端模块便于重用
- [NPM](https://www.npmjs.com/) 中已经有将近20万个可以使用模块包

缺点：

- 同步的模块加载方式不适合在浏览器环境中，同步意味着阻塞加载，浏览器资源是异步加载的

###### AMD

[Asynchronous Module Definition](https://github.com/amdjs/amdjs-api) 规范其实只有一个主要接口 `define(id?, dependencies?, factory)`，它要在声明模块的时候指定所有的依赖 `dependencies`，并且还要当做形参传到 `factory` 中，对于依赖的模块提前执行，依赖前置。

```
define("module", ["dep1", "dep2"], function(d1, d2) {
  return someExportedValue;
});
require(["module", "../file"], function(module, file) { /* ... */ });

```

优点：

- 适合在浏览器环境中异步加载模块

缺点：

- 提高了开发成本，代码的阅读和书写比较困难，模块定义方式的语义不顺畅
- 不符合通用的模块化思维方式，是一种妥协的实现

实现：

- [RequireJS](http://requirejs.org/)

###### CMD

[Common Module Definition](https://github.com/cmdjs/specification/blob/master/draft/module.md) 规范和 AMD 很相似，尽量保持简单，并与 CommonJS 和 Node.js 的 Modules 规范保持了很大的兼容性。

```
define(function(require, exports, module) {
  var $ = require('jquery');
  var Spinning = require('./spinning');
  exports.doSomething = ...
  module.exports = ...
})

```

优点：

- 依赖就近，延迟执行
- 可以很容易在 Node.js 中运行

缺点：

- 依赖 SPM 打包，模块的加载逻辑偏重

实现：

- [Sea.js](http://seajs.org/)

AMD | 速度快 | 会浪费资源 | 预先加载所有的依赖，直到使用的时候才执行

CMD | 只有真正需要才加载依赖 | 性能较差 | 直到使用的时候才定义依赖

AMD和CMD最大的区别是对依赖模块的执行时机处理不同，而不是加载的时机或者方式不同，二者皆为异步加载模块。
AMD依赖前置，js可以方便知道依赖模块是谁，立即加载；而CMD就近依赖，需要使用把模块变为字符串解析一遍才知道依赖了那些模块，这也是很多人诟病CMD的一点，牺牲性能来带来开发的便利性，实际上解析模块用的时间短到可以忽略。

```javascript
//CMD
define(function(require,exports,module){
    var a = require("./a");
    a.doSomethis();
    var b = require("./b")//依赖可以就近书写
    b.doSomething()
})
//AMD
define(['./a,./b'],function(a,b){//依赖必须一开始就写好
    a.dosomething()
    b.dosomething()
})
```



##### webpack vs gulp&grunt

前者的工作流程是，将整个项目作为一个主体，通过给定的主文件，根据整个文件开始找到项目的所有依赖。然后通过loaders处理，最后打包成一个浏览器可以识别的js文件。

后者就有点像小孩。在配置文件中给出需要对文件的各种操作命令，然后他会帮你操作完成。

##### Difference between transition and transform

###### transition

```css
img{
    height:15px;
    width:15px;
}

img:hover{
    height: 450px;
    width: 450px;
}
img{
    transition: 1s 1s height ease;
}
```

只选择效果时间。transition的优点在于简单易用，但是它有几个很大的局限。

（1）transition需要事件触发，所以没法在网页加载时自动发生。

（2）transition是一次性的，不能重复发生，除非一再触发。

（3）transition只能定义开始状态和结束状态，不能定义中间状态，也就是说只有两个状态。

（4）一条transition规则，只能定义一个属性的变化，不能涉及多个属性。

##### 清除浮动

清除浮动 还是 闭合浮动

外边距重合，父div撑不起来

清除浮动指的是运用clear属性去解决浮动父容器高度塌陷的问题，clear属性规定元素的哪一侧不允许其他浮动元素。
可选择的值有：left, right, both, none, inherit

**清除浮动方法1**：通过在浮动元素的末尾添加一个空元素，设置 clear：both属性，after伪元素其实也是通过 content 在元素的后面生成了内容为一个点的块级元素。

**清除浮动方法2**：BFC（Block Format Content）清理浮动，BFC可以**阻止垂直外边距折叠**、**不会重叠浮动元素**、可以**包含浮动**。因此清理浮动在BFC的语境下就是“包含浮动”，也即让父容器形成BFC就可以。



##### 会触发BFC的条件有：

- float的值不为none。

- overflow的值不为visible。

- display的值为table-cell, table-caption, inline-block 中的任何一个。

- position的值不为relative 和static。

  在CSS当中，相邻的两个盒子（可能是兄弟关系也可能是祖先关系）的外边距可以结合成一个单独的外边距。这种合并外边距的方式被称为折叠，并且因而所结合成的外边距称为折叠外边距.

  两个相邻的外边距都是正数时，折叠结果是它们两者之间较大的值。

  两个相邻的外边距都是负数时，折叠结果是两者绝对值的较大值。

  两个外边距一正一负时，折叠结果是两者的相加的和。

  ps：一个包另一个，被包的那个的margin就被吃掉了，还是贴在一起的！！！

  还有一种就是里边是float外边div，就撑不开，跑外边来了

##### css圆角

如果是长度，就是圆角的半径，0就是直角。

如果是百分比，超过50%，四个角就合成椭圆了

##### 浏览器渲染

兵分三路。HTML/SVG/XHTML负责DOM树，css负责css树，js通过相应的api操作这两个树，解析完成后通过两个树变成渲染树。

ps：Rendering Tree 渲染树并不等同于DOM树，因为一些像Header或display:none的东西就没必要放在渲染树中了。

##### 回流与重绘

1. 当render tree中的一部分(或全部)因为元素的规模尺寸，布局，隐藏等改变而需要重新构建。这就称为回流(reflow)。每个页面至少需要一次回流，就是在页面第一次加载的时候。在回流的时候，浏览器会使渲染树中受到影响的部分失效，并重新构造这部分渲染树，完成回流后，浏览器会重新绘制受影响的部分到屏幕中，该过程成为重绘。 Resize ,Add or delete element
2. 当render tree中的一些元素需要更新属性，而这些属性只是影响元素的外观，风格，而不会影响布局的，比如background-color。则就叫称为重绘。 Change color

注意：回流必将引起重绘，而重绘不一定会引起回流。

浏览器很聪明，回流重绘到一定数量才会发生。

###### 如何减少回流和重绘

一次性更改style，绝对定位复杂操作的动画。不要把DOM结点的属性值放在一个循环里当成循环里的变量。千万不要使用table布局

- 当你增加、删除、修改DOM结点时，会导致Reflow或Repaint
- 当你移动DOM的位置，或是搞个动画的时候。
- 当你修改CSS样式的时候。
- 当你Resize窗口的时候（移动端没有这个问题），或是滚动的时候。
- 当你修改网页的默认字体时。



## 第二部分 计算机网络

##### 在css/js代码上线之后开发人员经常会优化性能，从用户刷新网页开始，一次js请求一般情况下有哪些地方会有缓存处理？

dns缓存（地址），cdn缓存（文件），浏览器缓存，服务器缓存。



##### 浏览器缓存机制

Cache-Control：max-age=600。设置有效时长

Expires：一个时间节点，表示在这个时间节点之前都是有效的

Last-Modified（或 Etag）：最后一次更新时间节点

##### http报文格式
###### 请求报文           

请求行		//三个字段：方法字段，URL字段，HTTP版本

请求头部		//Host 请求主机名，Accept 客户端可识别的内容类型列表，User-Agent 产生请求的浏览器类型，					      Connection 是否持续保持连接

请求正文		//POST

###### 响应报文

状态行   	       //由HTTP协议版本号， 状态码， 状态消息

响应头部.        //Date：服务器生成响应报文并发送的日期和时间，该时间是服务器在它的文件系统中检索到该对象，插入到响应报文，并发送该响应报文的时间。Server:表示该报文是由一台Apache Web服务器产生的。Last-Modified： 对象创建或最后修改的时间。Content-Type：指定了实体中的对象是HTML(text/html),编码类型是UTF-8

空行告诉下一行是正文

响应正文

![sanciwoshou](/Users/deepglint/Downloads/sanciwoshou.png)

##### 为什么要三次握手，四次挥手

三次是为了server端一直等待。server 说好，但是服务器端没收到或者发的没收到。

四次是因为双方都有可能还有信息没有发，所以需要各自都说不发了。

##### 为什么要重定向

其中一个原因跟搜索引擎排名有关。如果一个页面有两个地址，就像http://www.yy.com/和http://yy.com/，搜索引擎会认为它们是两个网站，结果造成每个搜索链接都减少从而降低排名。

301 or 302

他们的不同在于。301表示旧地址A的资源已经被永久地移除了（这个资源不可访问了），**搜索引擎在抓取新内容的同时也将旧的网址交换为重定向之后的网址**；

302表示旧地址A的资源还在（仍然可以访问），这个重定向只是临时地从旧地址A跳转到地址B，**搜索引擎会抓取新的内容而保存旧的网址。 SEO302好于301**

反向代理

所有服务器前面加个代理来分配大量的请求响应



##### http状态码

1xx：信息性状态码，表示服务器接受请求正在处理

2xx：成功状态码，表示服务器正确处理完请求

3xx：重定向状态码，表示请求资源位置发生改变，需要重新重定向

4xx：服务器端错误状态码，服务器无法处理该请求

5xx：服务器错误状态码，服务器处理请求错误

> 面试官问了问题，是直接返回404好还是返回200在response的body中返回404比较好



|      | 1**：信息性状态码                               |
| ---- | ---------------------------------------- |
|      | 2**：成功状态码                                |
|      | 200：OK 请求正常处理                            |
|      | 204：No Content请求处理成功，但没有资源可返回            |
|      | 206：Partial Content对资源的某一部分的请求           |
|      | 3**：重定向状态码                               |
|      | 301：Moved Permanently 永久重定向              |
|      | 302：Found 临时性重定向                         |
|      | 304：Not Modified 缓存中读取                   |
|      | 4**：客户端错误状态码                             |
|      | 400：Bad Request 请求报文中存在语法错误              |
|      | 401：Unauthorized需要有通过Http认证的认证信息         |
|      | 403：Forbidden访问被拒绝                       |
|      | 404：Not Found无法找到请求资源                    |
|      | 5**：服务器错误状态码                             |
|      | 500：Internal Server Error 服务器端在执行时发生错误   |
|      | 503：Service Unavailable 服务器处于超负载或者正在进行停机维护 |

####  计算机网络---自顶向下总结(http://www.jianshu.com/p/48f2bebaeb40)

##### 应用层   进程与计算机网络间的接口

###### 协议

http协议（web） ：无状态，乱序是TCP考虑的事，拉协议

FTP协议（文件传输）

SMTP协议（电子邮件）：推协议

###### DNS

主机名—>IP地址转换的目录服务

通常从请求主机到本地DNS服务器的查询是递归的，其余的查询是迭代的

###### 攻击

DDos：向处理如.com域的域名服务器发送大量DNS请求，使得大部分合法请求无法获得响应

DNS反射：请求中冒充目标主机源地址，大量请求DNS服务器，DNS就大量向源地址主机发送回答，淹没目标主机

##### 传输层 为应用程序提供正确的应用级进程之间的交付服务

###### 协议

TCP：有连接的，需要握手包到底的。稳定但是大。HTTP FTP    head:20bit

TCP does error checking and error recovery. Erroneous packets are retransmitted from the source to the destination.

UDP：										DNS,VOIP.   	        8bit	

UDP does error checking but simply discards erroneous packets. Error recovery is not attempted.

##### 网络层

仅在网络层提供连接服务的计算机网络成为虚电路；仅在网络层提供无连接服务的计算机网络称为数据报网络

##### 链路层

##### 物理层



## 第三部分 数据结构和算法

### 数据结构

##### 栈的实现

```javascript
	function stack(){
      this.dataStore=[];
      this.top=0;
      this.push=push;
      this.pop=pop;
      this.peek=peek;
	}

	function push(element){
      this.dataStore[top++]=element;
	}
	function pop(element){
      return this.dataStore[--this.top];
	}
	function peek(){
      return this.dataStore[this.top-1];
	}
	function length(){
      return this.top;
	}
	function clear(){
      this.top=0;
	}
```

##### 队列的实现

```javascript
	function Queue(){
      this.dataStore=[];
      this.enqueue=enqueue;
      this.dequeue=dequeue;
      this.front=front;
      this.back=back;
      this.toString=toString;
      this.empty=empty;
    }
	function enqueue(element){
      this.dataStore.push(element);
	}
	function dequeue(element){
      this.dataStore.shift();
	}
	function front(){
      return this.dataStore[0];
	}
	function back(){
      return this.dataStore[this.dataStore.length-1];
	}
	function toString(){
      var resstr='';
      for(var i=0;i<this.dataStore.length;i++){
        resstr+=this.dataStore[i]+'/n';
      }
      return resstr;
	}
	function empty(){
      return this.dataStore.length==0?true:false;
	}
	
```

##### 链表的实现

```javascript
	function Node(element){
      this.element=element;
      this.next=null;
	}
	function llist(){
      this.head=new Node('head');
      this.find=find;
      this.insert=insert;
      this.remove=remove;
      this.display=display;
	}
	function find(item){
      var curNode=this.head;
      while(curNode!=item){
        curNode=curNode.next;
      }
      return curNode;
	}
	function insert(newELement,item){
      var newNode=new Node(newElment);
      var current=this.find(item);
      newNode.next=current.next;
      current.next=newNode;
	}
	function display(){
      var curNode=this.head;
      while(!(curNode.next==null)){
        print(curNode.next.element);
        curNode=curNode.next;
      }
	}
	function findprevious(item){
		var curNode=this.head;
      	while(!(curNode.next==null)&&(curNode.next.element!=item)){
          curNode=curNode.next;
      	}
      	return curNode;
    }
	function remove(item){
      var previous=findprevious(item);
      if(!(previous.next.next==null)){
        previous.next=previous.next.next;
      }
	}

	//双向链表
	function Node(element){
      this.element=element;
      this.next=null;
      this.previous=null;
	}
	function LList(){
      this.head=new Node('head');
      this.find=find;
      this.insert=insert;
      this.display=display;
      this.remove=remove;
      this.findlast=findlast;
      this.dispReverse=dispReverse;
	}
	funciton dispReverse(){
      var curNode=this.head;
      curNode=this.findLast();
      while(!(curNode==null)){
        print(curNode.element);
        curNode=curNode.previous;
      }
	}
	function findLast(){
      var curNode=this.head;
      while(!(curNode.next==null)){
        curNode=curNode.next;
      }
      return curNode;
	}
	

	
```

##### 深度优先和广度优先的遍历

```javascript
function wideTraversal(selectNode) {  
    var nodes = [];  
    if (selectNode != null) {  
        var queue = [];  
        queue.unshift(selectNode);  
        while (queue.length != 0) {  
            var item = queue.shift();  
            nodes.push(item);  
            var children = item.children;  
            for (var i = 0; i < children.length; i++)  
                queue.push(children[i]);  
        }  
    }  
    return nodes;   
}




var preOrder = function (node) {
  if (node) {
    console.log(node.value);
    preOrder(node.left);
    preOrder(node.right);
  }
}

var inOrder = function (node) {
  if(node){
    inOrder(node.left);
    console.log(node.value);
    inOrder(node.right);
  }
}

var postOrder = function (node) {
  if (node) {
    postOrder(node.left);
    postOrder(node.right);
    console.log(node.value);
  }
}
```



### 算法

##### 数组去重

```javascript
		var array = [1,3,4,4,5,6];
        function filt(array){
          var result=[];
          var hash = {};
          array.forEach(function(item){
            if(!hash[item]){
              result.push(item);
              hash[item]=true;
            }
          })
          console.log(result);
        }
		filt();
```

##### 判断两个json是否相同

```javascript
const x={a:1,b:2},y={b:2,a:1},z={a:2,b:3};
//我能想到的方法就是便利每个变量，然后对比
deter(x,y)  //true
deter(x,z)  //false
```

##### 快速排序时间复杂度 o(nlogn)

数组有n个元素，因为要递归运算，算出支点pivot的位置，然后递归调用左半部分和有半部分，这个时候理解上是若第一层的话就是n/2，n/2，若是第二层就是n/4,n/4,n/4,n/4这四部分，即n个元素理解上是一共有几层2^x=n，x=logn，然后每层都是n的复杂度，那么平均就是O(nlogn)的时间复杂度。但这种肯定是平均情况，如果你是标准排序的情况下，如果已经是ascending的顺序，那么递归只存在右半部分了，左半部分都被淘汰了。(n-1)*(n-2)*....*1，这个复杂度肯定就是O(n^2)，这种情况还不如用插入排序作者：

主定理严格推倒



















## 第四部分 简历和面试技巧总结



##### 反问面试官的最后一个问题

###### 这次面试我还有哪些需要提高的地方

###### 在公司里的部门，做什么

##### 平常关注的前端消息

知乎，前端周刊 https://zhuanlan.zhihu.com/p/27966492

​	   前端外看评论 https://zhuanlan.zhihu.com/FrontendMagazine

​	   前端学习指南 https://zhuanlan.zhihu.com/study-fe

​	   前端大哈 https://zhuanlan.zhihu.com/qianduandaha

##### 看过的前端书籍

权威指南，你不知道的js，understunding es6，阮es6，DOM编程艺术，bad things about JavaScript，图解http，css secrets ,css 权威指南，算法导论，jquery实战，黑客与画家,编写高质量JavaScript代码的68个有效方法,Head First HTML5 Programming,数据结构与算法JavaScript描述，编写高质量代码--Web前端开发修炼之道

#### 简历内容具体分析

##### 弹幕

功能实现：

颜色随机 span.style.color = colors[index];

高度：算出一共能多少行，随机行数

一开始在屏幕的最右侧：

var screenW = window.innerWidth;
span.style.left = screenW +'px';

动态往左：arr[i] -= 2。oSpan[i].style.left = arr[i]+'px';

判断是否超出屏幕：if (arr[i] < -oSpan[i].offsetWidth)

细节处理：

观看量35万的视5000条弹幕。可以设置屏幕的弹幕数，vip有优先权。

bilibili也会出现弹幕太多覆盖屏幕，只能关了再看。会有遮盖，不过颜色不同，行高固定。

使用的是transform translate

##### chorme插件

调试安卓手机

所以也不用再在chrome上安装ADB插件了 但需要下载最新的chrome

chrome://inspect

手机上看到的内容pc端可以同步审查元素

##### mock后端数据

json-server

##### react生命周期

http://www.jianshu.com/p/4784216b8194

##### git版本控制

Master		正式发版用

Develop		开发重大版本

Feature		开发某种特定功能

Release		发布正式版本之前（即合并到Master分支之前），我们可能需要有一个预发布的版本进行测试

Fixbug		修补bug分支

后三种为临时分支，用完就删



Dev

git checkout -b develop master		创建分枝

git checkout master				切换到Master分支

git merge --no-ff develop			对Develop分支进行合并

这次合并是“快进模式”，也就是直接把`master`指向`dev`的当前提交，所以合并速度非常快。



Feature

git checkout -b feature-x develop

git checkout develop

git merge --no-ff feature-x		

git branch -d feature-x			删除分支



Release

git checkout -b release-1.2 develop

git checkout master

git merge --no-ff release-1.2

git tag -a 1.2			 			对合并生成的新节点，做一个标签



Fixbug

git checkout -b fixbug-0.1 master

git checkout master

git merge --no-ff fixbug-0.1

git tag -a 0.1.1

git checkout develop

git merge --no-ff fixbug-0.1

git checkout develop

git merge --no-ff fixbug-0.1



##### 滚屏加载

```javascript
$(function(){ 
    var winH = $(window).height(); //页面可视区域高度 
    var i = 1; //设置当前页数 
    $(window).scroll(function () { 
        var pageH = $(document.body).height(); 
        var scrollT = $(window).scrollTop(); //滚动条top 
        var aa = (pageH-winH-scrollT)/winH; 
        if(aa<0.02){ 
            $.getJSON("result.php",{page:i},function(json){ 
                if(json){ 
                    var str = ""; 
                    $.each(json,function(index,array){ 
                        var str = "<div class=\"single_item\"><div class=\"element_head\">"; 
                        var str += "<div class=\"date\">"+array['date']+"</div>"; 
                        var str += "<div class=\"author\">"+array['author']+"</div>"; 
                        var str += "</div><div class=\"content\">"+array['content']+"</div></div>"; 
                        $("#container").append(str); 
                    }); 
                    i++; 
                }else{ 
                    $(".nodata").show().html("别滚动了，已经到底了。。。"); 
                    return false; 
                } 
            }); 
        } 
    }); 
}); 
```

Js：

网页可见区域宽： document.body.clientWidth
网页可见区域高： document.body.clientHeight
网页可见区域宽： document.body.offsetWidth (包括边线的宽)
网页可见区域高： document.body.offsetHeight (包括边线的高)
网页正文全文宽： document.body.scrollWidth
网页正文全文高： document.body.scrollHeight
网页被卷去的高： document.body.scrollTop
网页被卷去的左： document.body.scrollLeft
网页正文部分上： window.screenTop
网页正文部分左： window.screenLeft
屏幕分辨率的高： window.screen.height
屏幕分辨率的宽： window.screen.width
屏幕可用工作区高度： window.screen.availHeight
屏幕可用工作区宽度： window.screen.availWidth

JQuery:

$(document).ready(function(){
alert($(window).height()); //浏览器当前窗口可视区域高度
alert($(document).height()); //浏览器当前窗口文档的高度
alert($(document.body).height());//浏览器当前窗口文档body的高度
alert($(document.body).outerHeight(true));//浏览器当前窗口文档body的总高度 包括border padding margin
alert($(window).width()); //浏览器当前窗口可视区域宽度
alert($(document).width());//浏览器当前窗口文档对象宽度
alert($(document.body).width());//浏览器当前窗口文档body的宽度
alert($(document.body).outerWidth(true));//浏览器当前窗口文档body的总宽度



##### 
