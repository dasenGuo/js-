## 关于《js权威指南》笔记整理  

### 1. 多行文本注释
```javascript
/*
* xx
* xxx
*/
```

### 2. 数据类型比较
```javascript
var o = {x:1},p = {x:1};
o === p //false 两个单独的对象永不相等

var a = [],b=[];
a===b //false 两个单独的数组永不相等
```

### 3. toFixed()
```javascript
var a = 123456.111;
a.toFixed(0);  //"123456"
a.toFixed(2);  //"123456.11"
```

### 4. toString()
```javascript
[1,3,4].toString();  //"1,3,4"
```

### 5. instanceof
```javascript
var obj ={aa:22};
obj instanceof Object;  // true;
```

### 6. in 与 instanceof 的使用
`in`希望左操作数是一个字符串或者是可以转化为字符串的字节，右边是一个对象，如果右侧的对象拥有一个名为左操作数值的属性名，那么返回`true`；否则返回`false`；
```javascript
var point ={x:1,y:2};
"x" in point  //true;
var data =[7,8,9]
"0" in data  //true 下标
"1" in data  //true  转换为1的字符串，然后下标
"3" in data  //false 
```

### 7. delete
```javascript
var o ={x:1,y:2};
delete o.x;
"x" in o   //false;

var x = 1;
delete this.x;  //false

function f(){}
delete this.f;  //false
```
注：`var` 声明的变量是无法通过`delete`删除，继承的属性也无法删除，全局函数也不能删除。但是`this`创建一个可配置的全局属性（没有用`var`）
```javascript
this.x = 1;
delete x; //true
delete this.x  //true 严格模式下需要加this
```

### 8. for 和 for/in
`for`只会遍历私有的属性和方法（更多的是索引），自己在原型上扩展的方法不会遍历出来。
`for in`不仅可以遍历当前对象（或者当前实例）所有的私有属性和方法，也会遍历对象的所有可枚举属性，还可以把原型上自己创建的公共属性和方法进行遍历。（注意一点的是：原型上的公有属性是不可枚举的）
```javascript
let o = {one:1,two:2,three:3};
	for(p in o){
	console.log(p); //one two three
}
```

### 9. break 和 continue
`break` 直接终止循环
`continue`跳过这一次继续下一次循环

### 10. 抛错 throw
```javascript
throw new Error()
```

### 11. try，catch，finally
```javascript
try{
}catch(e){
}finally{
}     // finally 不管语句是否抛错都会运行
```

### 12. 数组
#### 1> join()
将数组中的所有元素转化成字符串并连接在一起，默认使用逗号隔开
```javascript
var a = [1,2,3];
a.join();  // "1,2,3"
a.join(""); // "123"
a.join("-"); // "1-2-3"
```

#### 2> sort()
在原数组上进行排序，不生成副本；参数可选，但必须是函数。
不传参数，按照字符编码顺序进行排序
```javascript
var arr = new Array(6)
arr[0] = "George"
arr[1] = "John"
arr[2] = "Thomas"
arr[3] = "James"
arr[4] = "Adrew"
arr[5] = "Martin"
document.write(arr.sort()) //Adrew,George,James,John,Martin,Thomas
```
传入参数
* 若 a 小于 b，在排序后的数组中 a 应该出现在 b 之前，则返回一个小于 0 的值。
* 若 a 等于 b，则返回 0。
* 若 a 大于 b，则返回一个大于 0 的值。
```javascript
function sortNumber(a,b){
    return a - b
}
var arr = new Array(6)
arr[0] = "10"
arr[1] = "5"
arr[2] = "40"
arr[3] = "25"
arr[4] = "1000"
arr[5] = "1"
document.write(arr.sort(sortNumber))  //1,5,10,25,40,1000
```

#### 3> reduce() 和 reduceRight()
```javascript
array.reduce(xx(
  total, //必须。初始值，或者计算结束后的返回值
  currentValue, //必须。当前元素
  currentIndex, //可选。当前元素的索引
  arr //可选。当前元素所属的数组对象
),initialValue //可选。传递给函数的初始值
)
```
执行时一个从左到右，一个从右到左
```javascript
var number = [65, 44, 12, 4];
  function getSum(total, num) {
    return total + num;
  }

  function myFunction() {
    alert(number.reduce(getSum));
  }
  myFunction(); //125
  
 function getSums(total, num) {
    return total - num;
 }
function myFunctions() {
    alert(numbers.reduceRight(getSums));
 }
 myFunctions(); //-117
```
#### 4> every() 和 some()
所有（&&）和一些（||），返回的是布尔类型
```javascript
var arr = [ 1, 2, 3, 4, 5, 6 ]; 
 arr.some( function(item){ 
    return item > 3;   // true
})
 
 arr.every( function(item){ 
    return item > 3;   // false
})  
```

#### 5> 解构赋值
```javascript
[x,y] = [x+1,y+1]  //等价于 x=x+1,y=y+1
```

### 13. 对象
#### 1> 创建对象：Object.create()
```javascript
var obj = Object.create({}, {
      "a": {
        value: 1,
        congigurable: false,
        enumerable: true,
        writable: true
      },
      "b": {
        value: 2,
        congigurable: false,
        enumerable: true,
        writable: true
      },
      "c": {
        value: 3,
        congigurable: false,
        enumerable: true,
        writable: true
      }
    });
    console.log(obj.a) //输出 1 
    console.log(obj.b) //输出 2 
    console.log(obj.c) //输出 3
```
数据四个特性：本身的值（`value`），可写性（`writable`），可枚举型（`enumerable`）和可配置性（`configurable`）；
访问器属性：读取（`get`），写入（`set`），可枚举性和可配置性。

#### 2> hasOwnProperty()
用来检测给定的名字是否是对象的自有属性（继承属性不算）

#### 3> propertyIsEnumerable()
用来检测是自有属性且这个属性是可枚举的（继承属性不算）

#### 4> getter 和 setter
一个读/写属性，属性值的两个方法就是 `getter` 和 `setter`，由`getter` 和 `setter`定义的属性称作 '存取器属性'

#### 5> 对象的三大属性
原型(`prototype`)，类（`class`），可扩展性（`extensibleattribute`）;
#####  (1) 原型
a.  isPrototypeOf()
检测一个对象是否是另外一个对象的原型（或在原型链中）；
```javascript
var p = {x:1};
var o = Object.create(p);
p.isPrototypeOf(o);  //true
```
b.   `prototype` 中的 `constructor` 不可枚举属性
```javascript
var F = function(){};
var p = F.prototype;
var c = p.constructor;
c === F  //true
```

##### (2) 类属性
是一个字符串，用以表示对象的类型信息。可以使用`toString()`;可以调用`classof()`函数，此函数可以传入任何类型的参数。

##### (3) 可扩展性（防止核心对象被篡改）
可以使用`Object.esExtensible()`来判断该对象是否是可扩展的。使用`Object.preventExtensions`将待转换的对象作为参数传进去，将该对象永久转换为不可扩展。
`Object.seal()`除了设置成不可扩展，而且将对象的所有自有属性都设置成为不可配置的，将对象封闭起来，可以使用`isSealed`来检测对象是否封闭。
`Object.freeze()`将更严格的冻结对象，除了有上述的能力，还将具有的所有数据属性设置为只读，使用`Object.isFrozen()`来检测对象是否冻结。

#### 6> 序列化对象
将对象的状态信息转换为可以储存或传输的形式的过程。
`JSON.stringify()`与`JSON.parse()`

#### 7> callee常做递归
```javascript
 var aa = function (x) {
    if (x <= 1) {
      return 1;
    } else {
      return x * arguments.callee(x - 1);
    }
  }
```
注意：严格模式下无法使用

#### 8> eval()
只接收原始字符串作为参数，并执行其中的js代码
```javascript
 eval("x=10;y=20;document.write(x*y)")  //200
 document.write(eval("2+2"))  //4
 var x=10
 document.write(eval(x+15))  //25
```


#### 9> 类数组对象
是类似数组一样有length属性和索引属性的对象
* 数组对象的类型是Array，类数组对象的类型是Object；
* 类数组对象不能直接调用数组API；
* 数组遍历可以用for in和for循环，类数组只能用for循环遍历；
```javascript
var a = {"0"："a","1":"b","2":"c",length:3}
```

#### 10> 高阶函数
接收一个或多个函数作为参数，并返回一个新的函数
```javascript
function not(f) {
    return function () {
      var result = f.apply(this, arguments);
      return !result
    }
  }
  var even = function (x) {
    return x % 2 === 0; //判断x是否为偶数的函数
  };
  var odd = not(even); //传进一个函数作为参数，并返回一个新函数
  [1, 1, 3, 5, 5].every(odd); //true 传进一个函数作为判断条件
```

#### 11> call 和 apply
```javascript
  function compose(f, g) {
    return function () {
      return f.call(this, g.apply(this, arguments));
    }
  }
  var square = function (x) {
    return x * x;
  }
  var sum = function (x, y) {
    return x + y;
  }
  var squareofsum = compose(square, sum);
  squareofsum(2, 3) //25
```

### 14. 正则
```javascript
/[^...]/   // 不在方括号内的任意字符
/[a-z]/    // 匹配的是26位小写字母
/[a-zA-Z0-9]/ // 匹配的是任意的字母和数字
   .   // 除换行符和其他Unicode行终止符之外的任意字符
  \w   // 任何ASCII字符组成的单词，等价于[a-zA-Z0-9]
  \W   // 任何不是ASCII字符组成的单词，等价于[^a-zA-Z0-9]
  \s   // 任何Unicode空白符
  \S   // 任何非Unicode空白符的字符，注意\W和\S不同
  \d   // 等价于[0-9]
  \D   // 等价于[^0-9]
  [\b]  // 退格直接量。
  {n,m} // 匹配前一项至少n次，但不能超过m次
  {n,}  // 匹配前一项n次或者更多次
  {n}   // 匹配前一项n次
   ？  // 匹配前一项0次或者1次，也就是说前一项是可选的，等价于{0,1}
   +   // 匹配前一项1次或多次，等价于{1，}
   *   // 匹配前一项0次或多次，等价于{0，}
   /\d{2,4}/  // 匹配2-4个数字
   /\w{3}\d?/  // 精确匹配三个单词和一个可选数字
   /\s+java\s+/ // 匹配前后带有一个或多个空格的字符串"java"
   /[^(]*/  // 匹配一个或多个非左括号的字符
   /ab|cd|ef/ // 分隔字符，可以匹配字符ab，或者cd，或者ef
   new RegExp("\\d{5}","g")
RegExp最主要的执行模式匹配的方法是exec(),对一个指定的字符串执行一个正则表达式，如果在这个字符串中没有找到所匹配的的东西，返回null，找到匹配返回一个数组。
```

### 15. 迭代器
#### 1> 迭代器必须包含`next()`方法，每一次调用都会返回集合的下一个值
```javascript
function counter(start){
  let nextValue = Math.round(start);
  return {
    next:function(){
      return nextValue++;
    }
  }
}
let serial = counter(1000);
let sn1 = serial.next(); //1000
let sn2 = serial.next(); //1001
```

#### 2> 迭代器函数`Iterator()`结合解构赋值
```javascript
for(let [k,v] in Iterator({a:1,b:2})) //对属性和值作迭代
console.log(k + "=" + v); //输出"a=1"和"b=2"
```

#### 3> `Iterator`函数返回的迭代器有两种重要的特性
* 他只对自有属性进行遍历而忽略继承的属性
* 如果给`Iterator()`传入第二个参数`true`，返回的迭代器只对属性名进行遍历，而忽略属性值
```javascript
o = {x:1,y:2}; //定义一个对象，他有两个属性
Object.prototype.z = 3;
for(p in o) { console.log(p)} // "x","y","z"
for(p in Iterator(o,true)){console.log(p)}; // "x","y"
```

#### 4> 使用[`Symbol.iterator`]结合`for/of`自定义循环迭代
```javascript
  let obj = {
    start: [1, 3, 2],
    end: [7, 9, 8],
    [Symbol.iterator]() {
      let self = this;
      let index = 0;
      //结合数组
      let arr = self.start.concat(self.end);
      //数组大小
      let length = arr.length;
      //返回闭包
      return {
        next() {
          if (index < length) {
            return {
              value: arr[index++],
              done: false
            }
          } else {
            return {
              value: arr[index++],
              done: true
            }
          }
        }
      }
    }
  };
  for (let key of obj) {
    console.log(key)
  }
```

#### 5> 数组推导语法
```javascript
[expression for (variable in object) if (condition)]
```
例子：
```javascript
let range = [1,2,3,4];
let evensquares = [x*x for (x in range) if (x % 2 === 0)]
//等同于
let evensquares = [];
for(x in range){
    if(x % 2 === 0){
        evensquares.push(x*x);  //[0,4]
    }
}
```

### 16. BOM 与 DOM
####  window：`浏览器对象模型`
```javascript
//对象属性
window.self  //引用本窗口window==window.self
window.name  //为窗口名字
window.defaultStatus  //窗户状态栏信息
window.location URL  //设置该属性可打开新的页面

//对象方法
window.alert("text")  //提示信息会话框
window.confirm("text")  //确认会话框
window.prompt("text")  //键盘输入会话框
window.setIntervel(func, time)  //每隔指定时间(毫秒)执行一次操作
window.clearInterval()  //清除时间间隔
window.setTimeout(action,time)  //等待指定时间(毫秒)后再执行操作
window.open()  //打开新的窗口
window.close()  //关闭窗口

//成员对象
window.event
window.document

window.history
window.history.length  //浏览过的页面数
window.history.back()  //后退
window.history.forward()  //前进
window.history.go(i)  //前进或后退i个页面（i>0前进，i<0后退） 

window.screen
window.screen.width  //屏幕宽度
window.screen.height  //屏幕高度
window.screen.colorDepth  //屏幕色深
window.screen.availWidth  //屏幕可用宽度
window.screen.availHeight  //屏幕可用高度（除去任务栏的高度）  

window.navigator   
window.navigator.appCodeName  //浏览器代码名
window.navigator.appName  //浏览器名
window.navigator.platform  //运行浏览器的操作系统平台
window.navigator.appVersion  //浏览器的平台和版本
window.navigator.userAgent  //由客户机发送服务器的user-agent 头部的值
window.navigator.cookieEnabled  //浏览器是否启用cookie
window.navigator.appMinorVersion  //浏览器补丁版本
window.navigator.cpuClass  //cpu类型
window.navigator.plugins  //插件标识
window.navigator.userProfile  //用户的个人信息
window.navigator.systemLanguage  //客户体系语言
window.navigator.userLanguage  //用户语言
window.navigator.onLine  //用户是否在线
window.navigator.mimeTypes  //MIME类型（数组）
```

#### 1> 异步载入并执行脚本`loadasync()`
代码通过动态创建`script`元素并把它插入到文档中，来实现脚本的异步载入和执行。
```javascript
function loadasync(url){
    var head = document.getElementsByTagName("head")[0];//找到<head>元素
    var s = document.createElement("script");//创建一个<script>元素
    s.src = url;
    head.appendChild(s);                    //将script元素插入到head标签中
}
loadasync(test.js)
```

#### 2> 兼容性内容
* IE不支持`canvas`，但他支持`VML`；
* IE不支持`addEventListener()`，但他支持`attactEvent()`；

#### 3> web渲染模式有“怪异模式”和“标准模式”
在标准模式页面按照`html`，`css`的定义渲染,而在怪异模式就是为了浏览器很早之前针对旧版本浏览器设计，并未严格遵循`W3C`标准而产生的一种页面渲染模式。浏览器基于页面中文件类型描述的存在决定采用哪种渲染模式，如果存在一个完整的`DOCTYPE`则浏览器将会采用标准模式，如果缺失就会采用怪异模式。
通过检测`document.compatMode`属性，如果其值为`CSS1Compat`则为标准模式；如果其值为`BackCompat`则是怪异模式。

![](https://github.com/Michelle111111/makedown-images/blob/master/img/js%E6%9D%83%E5%A8%81%E6%8C%87%E5%8D%97%E7%9A%84png/%E6%A0%87%E5%87%86%E7%9B%92%E5%AD%90%E6%A8%A1%E5%9E%8B.png)

![](https://github.com/Michelle111111/makedown-images/blob/master/img/js%E6%9D%83%E5%A8%81%E6%8C%87%E5%8D%97%E7%9A%84png/%E6%80%AA%E5%BC%82%E7%9B%92%E5%AD%90%E6%A8%A1%E5%9E%8B.png)

#### 4> 同源策略
1. 同源策略是对`js`代码能够操作哪些`web`内容的一条完整的安全限制。
2. 脚本本身的来源和同源策略并不相关，相关的是脚本所嵌入的文档的来源。
3. 不严格的同源策略
* 多子域的大站点，使用`document`对象的`domain`属性，默认情况下属性`domain`存放的是载入文档的服务器的主机名。将两个窗体的`domain`设置成相同的值，即可相互读取对方的属性。
* 跨域资源共享：使用新的`Origin`请求头和新的`Access-Control-Allow-Origin`响应头来扩展`HTTP`。
* 跨文档信息：调用`window`对象上的`postMessage()`方法，异步传递消息事件。

#### 5> 跨站脚本
英文名`XSS`，一类安全问题，就是攻击者向目标脚本`Web`站点注入`HTML`标签或者脚本。

#### 6> 解析URL
`window`对象的`location`属性引用的是`Location`，他表示该窗口中当前显示的文档的URL。其内的属性包括`protocol`，`host`，`hostname`，`port`，`pathname`和`search`，称之为“URL分解”属性。`Location`对象的`hash`和`search`属性返回URL中的“片段标识符”部分。`search`属性返回的是问号之后的URL，这部分通常是某种类型的查询字符串。

#### 7> 对话框的三个方法
* `alert()`
* `confirm()`返回一个布尔值
* `prompt()`显示一条信息，等待用户输入字符串，并返回这个字符串

#### 8> 错误处理
```javascript
window.onerror = function(message, source, lineno, colno, error) { ... }
```
* message    错误信息（字符串）
* source        发生错误的脚本URL（字符串）
* lineno        发生错误的行号（数字）
* colno         发生错误的列号（数字）
* error          error对象（对象）

#### 9> 关于嵌套属性iframe（[点击查看详情](https://www.cnblogs.com/lvhw/p/7107436.html)）
```html
<iframe src="http://pic15.nipic.com/20110628/1369025_192645024000_2.jpg"></iframe>  //基础用法
```

#### Document：`文档对象模型`
```javascript
document //代表整个HTML 文档，可用来访问页面中的所有元素
//对象属性
document.title   //文档标题，等价于HTML的<title>标签
document.bgColor  //页面背景色
document.fgColor  //前景色(文本颜色)
document.linkColor  //未点击过的链接颜色
document.alinkColor  //激活链接(焦点在此链接上)的颜色
document.vlinkColor   //已点击过的链接颜色
document.URL   //在同一窗口打开另一网页
document.fileCreatedDate  //文件建立日期，只读属性
document.fileModifiedDate  //文件修改日期，只读属性
document.fileSize  //大小，只读属性
document.cookie    //设置和读出cookie
document.charset   //字符集

//对象方法
document.write()  //动态向页面写入内容
document.createElement(tag) //创建指定标签的元素
document.getElementById(id)  //获得指定id值的元素
document.getElementsByName(name) //获得指定Name值的元素

//body对象
document.body  //文档主体开始和结束，等价于<body></body>
document.body.bgColor //背景颜色
document.body.link   //未点击过的链接颜色
document.body.alink   //激活链接(焦点在此链接上)的颜色
document.body.vlink  //已点击过的链接颜色
document.body.text   //文本色
document.body.innerText  //<body>...</body>之间的文本
document.body.innerHTML  //<body>...</body>之间的HTML代码
document.body.topMargin  //页面上边距
document.body.leftMargin  //页面左边距
document.body.rightMargin  //页面右边距
document.body.bottomMargin  //页面下边距
document.body.background  //背景
document.body.appendChild(oTag) //添加DOM对象
document.body.onclick="func()"  //鼠标指针单击对象是触发
document.body.onmouseover="func()"  //鼠标指针移到对象时触发
document.body.onmouseout="func()"  //鼠标指针移出对象时触发

//location
document.location.hash  //#号后的部分
document.location.host   //域名+端口号
document.location.hostname  //域名
document.location.href  //完整URL
document.location.pathname  //目录部分
document.location.port //端口号
document.location.protocol  //网络协议
document.location.search  //?号后的部分

//通过集合引用（以images集合为例，forms集合等类似）
document.images  //<img>标签
document.images.length  //<img>标签的个数
document.images[0]  //第1个<img>标签           
document.images[i]   //第i-1个<img>标签
document.body  //返回DOM中的body节点，即<body>
document.documentElement  //返回DOM中的html节点，即<html>
```
DOM以树结构表达HTML文档，如下图：
![](https://github.com/Michelle111111/makedown-images/blob/master/img/js%E6%9D%83%E5%A8%81%E6%8C%87%E5%8D%97%E7%9A%84png/%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_20190308093956.png)

关于节点树，如下图：

![](https://github.com/Michelle111111/makedown-images/blob/master/img/js%E6%9D%83%E5%A8%81%E6%8C%87%E5%8D%97%E7%9A%84png/%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_20190308094007.png)

#### 1> 获取节点
```javascript
var eg1 = document.getElementById("section") //通过id拿到节点
var eg2 = document.getElementsByName("content") //通过name拿到节点
var eg3 = document.getElementsByTagName("span") //通过标签名拿到节点
var eg4 = document.getElementsByClassName("box") //通过类名拿到节点（只兼容ie8+）
var eg5 = document.querySelector(".box") //通过类名拿到节点
```

#### 2> 节点介绍
关于详细节点的介绍及操作（见：[点击查看详情](https://wangdoc.com/javascript/dom/node.html)）
DOM节点之间的关系：
![](https://github.com/Michelle111111/makedown-images/blob/master/img/js%E6%9D%83%E5%A8%81%E6%8C%87%E5%8D%97%E7%9A%84png/%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_20190308094016.png)

#### 3> 操作节点方法
##### (1) 创建节点
```javascript
var btn = document.createElement("BUTTON") //button按钮创建
var textnode = document.createTextNode("Water") //text节点创建
```

##### (2) 插入节点
1. `appendChild()`：插入指定节点的最后一个子节点
2. `insertBefore()`：接收两个参数，第一个参数是待插入的节点，第二个参数是已存在的节点，新节点将插入该节点的前面。该方法应该是在新节点的父节点上调用，方法的第二个参数必须是该父节点的子节点。如果传递`null`作为第二个参数，`insertBefore()`的行为类似`appendChild()`，他将节点插入在最后。
```html
<ul id="myList">
<li>Coffee</li>
<li>Tea</li>
</ul>
<button onclick="myFunction()">试一下</button>
```
```javascript
function myFunction(){
    var list = document.getElementById("myList");
    var textnode = document.createTextNode("Water");
    list.insertBefore(textnode,null);
}
/*输出
Coffee
Tea
WaterWaterWater */
```

##### (3) 删除和替换节点
`removeChild()`：在父节点上调用，并将需要的子节点作为方法参数传递给他
`replaceChild()`：在父节点上调用，删除一个子节点并用一个新节点取而代之
```javascript
n.parentNode.removeChild(n);
n.parentNode.replaceChild(document.createTextNode("Water"),n);
```

##### (4) 克隆节点
`cloneNode`：接受一个布尔值作为参数，表示是否同时克隆子节点。他的返回值是一个克隆出来的新节点。
```javascript
var cloneUL = document.querySelector('ul').cloneNode(true);
```
1. 克隆一个节点，会拷贝该节点的所有属性，但会丧失`addEventListener`方法和`on`属性，添加在这个节点上的时间回调函数。
2. 该方法返回的节点不在文档之中，既没有任何父节点，必须使用诸如`Node.appendChild`这样的方法添加到文档之中。
3. 克隆一个节点之后，DOM有可能出现两个有相同`id`属性（即`id="xxx"`）的网页元素，这时应该修改其中一个元素的`id`属性。如果原节点有`name`属性，可能也需要修改。

##### (5) 特殊的 “缓冲” 节点
`DocumentFragment`对象
1. 一般动态创建`html`元素都是创建好了直接`appendChild()`上去，但是如果要添加大量的元素还用这个方法的话就会导致大量的重绘以及回流，所以需要一个 “缓冲区” 来保存创建的节点，然后再一次性添加到父节点中。这时候`DocumentFragment`对象就派上用场了。
2. `DocumentFragment`节点不属于文档树，继承的`parentNode`属性总是`null`。
3. 当请求把一个`DocumentFragment`节点插入文档树时，插入的不是`DocumentFragment`自身，而是他的所有子孙节点。这使得`DocumentFragment`成了有用的占位符，暂时存放那些一次插入文档的节点。他还有利于实现文档的剪切，复制和粘贴操作。
4. 重点就在于`DocumentFragment`节点不属于文档树。因此当把创建的节点添加到该对象时，并不会导致页面的回流，因此性能就自然上去了。
创建该对象：
```javascript
var fragment = document.createDocumentFragment();
```
实例：
```javascript
var pNode,fragment = document.createDocumentFragment();
  for(var i=0;i<20;i++){
    pNode = document.createElement('p');
    pNode.innerHTML = i;
    fragment.appendChild(pNode);
  }
document.body.appendChild(fragment);
```

> `延伸`：节点都是单个对象，有时需要一种数据结构，能够容纳多个节点，DOM提供两种节点集合，用于容纳多个节点：`NodeList`和`HTMLCollection`。而这差别是前者可以包含各种类型的节点，后者只能包含HTML元素节点。

##### (1) NodeList
`NodeList`实例是一个类似数组的对象，他的成员是节点对象。通过以下方法可以得到`NodeList`实例：
* Node.childNodes
* document.querySelectorAll()等节点搜索方法
`NodeList`实例很像数组，可以使用`length`属性和`forEach`方法，但是他不是数组，不能使用`pop`或`push`之类数组特有的方法。
```javascript
var children = document.body.childNodes;
Array.isArray(children); //false 用于确定传递的值是否是一个Array
child.length;
children.forEach(console.log);
var nodeArr = Array.prototype.slice.call(children); //将其转换为真正的数组
```
`item`方法接收一个整数值作为参数，表示成员的位置，返回该位置上的成员。
```javascript
document.body.childNodes.item(0);  //item(0)返回第一个成员
```
注意，`NodeList`实例可能是动态集合，也可能是静态集合。所谓动态集合就是一个活的集合，DOM删除或者新增一个相关节点，都会立刻反映在`NodeList`实例上。目前只有`Node.childNodes`返回的是一个动态集合，其他的`NodeList`都是静态集合。

##### (2) HTMLCollection
`HTMLCollection`是一个节点对象的集合，只能包含元素节点（`element`），不能包含其他类型的节点。他的返回值是一个类似数组的对象，但是与`NodeList`接口不同，`HTMLCollection`没有`forEach`方法，只能使用`for`循环遍历。返回`HTMLCollection`实例的，主要是一些`Document`对象的集合属性。比如`document.links`，`document.forms`，`document.images`等。`HTMLCollection`实例都是动态集合，节点的变化会实时反映在集合中。
```html
<img id="pic" src="http://pic1.nipic.com/2008-08-14/2008814183939909_2.jpg">
```
```javascript
var pic = document.getElementById('pic');
document.images.pic === pic //true
document.images.length; //length属性返回HTMLCollection实例包含的成员数量
document.images.item(0); //item(0)表示返回0号位置的成员
```
`namedItem`方法的参数是一个字符串，表示`id`属性或`name`属性的值，返回对应的元素节点。如果没有对应的节点，则返回`null`。
```javascript
<img name="pic" src="http://pic1.nipic.com/2008-08-14/2008814183939909_2.jpg">
var pic = document.images.namedItem('pic');
```

#### 4> 关于文档在DOM中的位置坐标（[点击查看原文](https://www.cnblogs.com/youziclub/p/4811069.html)）
![](https://github.com/Michelle111111/makedown-images/blob/master/img/js%E6%9D%83%E5%A8%81%E6%8C%87%E5%8D%97%E7%9A%84png/clipboard.png)

#### 5> DOM事件
##### (1) Event对象（[点击查看详情](http://www.w3school.com.cn/jsref/dom_obj_event.asp)）
`Event`对象代表事件的状态，比如事件在其中发生的元素，键盘按键的状态，鼠标的位置，鼠标按钮的状态。
注：事件通常与函数结合使用，函数不会在事件发生前被执行。

##### (2) 基本手势事件
`touchstart`：手指触摸屏幕时触发事件；
`touchmove`：手指移动时会触发事件；
`touchend`：手指离开屏幕时会触发事件。

##### (3) 手指触摸事件
关于`touches`，`targetTouches`，`changedTouches`解析
* `touches`：当前屏幕上所有触摸点的列表
* `targetTouches`：当前对象上所有触摸点的列表
* `changedTouches`：涉及当前（引发）事件的触摸点的列表

通过一个例子来区分一下触摸事件中的这三个属性：
* 用一个手指触摸屏幕，触发事件，此时这三个属性有相同的值。
* 用第二个手指接触屏幕，此时`touches`有两个元素，每个手指触摸点为一个值。当两个手指触摸相同元素时，`targetTouches`和`touches`的值相同，否则`targetTouches`只有一个值。`changedTouches`此时只有一个值，为第二个手指的触摸点，因为第二个手指是引发事件的原因。
* 用两个手指同时接触屏幕，此时`changedTouches`有两个值，每一个手指的触摸点都有一个值。
* 手指滑动时，三个值都会发生变化。
* 一个手指离开屏幕，`touches`和`targetTouches`中对应的元素会同时移除，而`changedTouches`仍然会存在元素。
* 手指都离开屏幕之后，`touches`和`targetTouches`中将不会再有值，`changedTouches`还会有一个值，此值为最后一个离开屏幕的手指的接触点。

注：关于两指操作，Apple中`Safari`自带有缩放和旋转手势。参数是`gesturestart`开始，`gestureend`结束，这两个事件之间是跟踪手势过程的`gesturechange`事件队列。这些事件传递的事件对象有数字属性`scale`和`rotation`。`scale`属性是两个手指之间间距的变化情况。“捏紧”手势的`scale`值小于1.0，而“撑开”手势的`scale`值大于1.0。`rotation`属性是指手指变化引起的旋转角度，他以度为单位，正值表示按照顺时针方向旋转（该值从0开始）。

##### (4) 注册事件处理程序
a.  addEventListener()
```javascript
element.addEventListener(event,function,boolean)
```
* 第一个参数传入事件类型字符串，不需要加前缀“`on`”；
* 第二个参数是事件发生时应该执行的函数；
* 第三个参数是一个布尔值，通常传入的是`false`，按照冒泡顺序执行，如果相反传递了`true`，那么函数将注册为捕获事件处理。
阻止冒泡：
```javascript
function onclickFn(event){
	event.stopPropagation();
}
```
b.  removeEventListener()
```javascript
document.removeEventListener("mousemove",handleMouseMove,true);
document.removeEventListener("mouseup",handleMouseUp,true);
```

##### (5) 拖放事件`drag()`
a.  在拖动目标上触发事件（源元素）：
* `draggable`：是否可以拖动
* `ondragstart`：用户开始拖动元素时触发
* `ondrag`：元素正在拖动时触发
* `ondragend`：用户完成元素拖动后触发
b.  释放目标时触发的事件：
* `ondragenter`：当被鼠标拖动的对象进入其容器范围内时触发此事件
* `ondragover`：当某被拖动的对象在另一对象容器范围内拖动时触发此事件
* `ondragleave`：当被鼠标拖动的对象离开其容器范围内时触发此事件
* `ondrop`：在一个拖动过程中，释放鼠标键时触发此事件
```html
<style>
  .droptarget {
    float: left;
    width: 100px;
    height: 35px;
    margin: 15px;
    padding: 10px;
    border: 1px solid #aaaaaa;
  }
</style>
<body>
  <p>在两个矩形框中来回拖动p元素</p>
  <div class="droptarget" ondrop="drop(event)" ondragover="allowDrop(event)">
    <p ondragstart="dragStart(event)" draggable="true" id="drag">拖动我！</p>
  </div>
  <div class="droptarget" ondrop="drop(event)" ondragover="allowDrop(event)">
  </div>
</body>
```
```javascript
function dragStart(event){
    event.dataTransfer.setData("Text",event.target.id);
  }

  function allowDrop(event){
    event.preventDefault();  //消除默认动作
  }

  function drop(event){
    event.preventDefault();
    var data = event.dataTransfer.getData("Text");
    event.target.appendChild(document.getElementById(data));
  }
```
注：
    `setData(format,data)`：将指定格式的数据赋值给`dataTransfer`对象，参数`format`定义数据的格式也就是数据的类型（传递的一个标志，须与`getData`一致），`data`为待赋值的数据。
    `setData(format)`：从`dataTransfer`对象中获取指定格式的数据，`format`代表数据格式（须与`setData`一致），`data`为数据。
    `dataTransfer`：可以为每一种`MIME`类型都保存一个值，不过保存在`dataTransfer`对象中的数据只能在`drop`事件程序中读取，如果没有读到，那么就是`dataTransfer`对象被销毁了，数据也丢失了。

##### (6) 文本事件与鼠标滚轮事件
拿到当前所输入的东西`textInput`与鼠标滚轮事件`wheel`
```html
<input id="myDIV" type="text" />
```
```javascript
var ipt = document.getElementById("myDIV");
function randomColor(){
  var r = Math.floor(Math.random() * 256);
  var g = Math.floor(Math.random() * 256);
  var b = Math.floor(Math.random() * 256);
  var colors = "rgb(" + r +","+ g +","+ b +")";
  return colors;
}
ipt.addEventListener("wheel",myFunction,false);
function myFunction(event){
  event.target.style.color = randomColor();
}
ipt.addEventListener("textInput",function(event){
  event.target.style.color = randomColor();
},false)
```

### 17. http（超文本传输）协议
在Web应用中，服务器把网页传给浏览器，实际上就是把网页的`HTML`代码发送给浏览器，让浏览器显示出来。而浏览器和服务器之间的传输协议是`HTTP`，所以：
* `HTML`是一种用来定义网页的文本，会`HTML`，就可以编写网页；
* `HTTP`是在网络上传输`HTML`的协议，用于浏览器和服务器的通信。
工作流程：客户端和服务器端没有任何联系——建立连接，客户端发送请求——沿着建立好的连接，服务器端返回响应信息——断开连接。
请求格式：请求信息分为三部分：请求行，请求头信息和请求主体信息。请求头信息和请求主体信息之间用一个空行分割，不管是否有请求主体信息，这个空行都必须存在。 请求行又分为三部分：请求方法，请求资源的路径，所用协议的版本（目前一般是http1.1,1.0已经基本不用了）。
响应格式：响应行，响应头信息，响应主体信息。响应行：协议版本，状态码，状态描述信息。

#### 1> 什么是跨域？
跨域，指的是浏览器不能执行其他网站的脚本。它是由浏览器的同源策略造成的，是浏览器对 `JavaScript`施加的安全限制。
所谓同源是指：域名，协议，端口均相同
* http://www.123.com/index.html 调用 http://www.123.com/server.php（非跨域）
* http://www.123.com/index.html 调用 http://www.456.com/server.php（主域名不同：123/456，跨域）
* http://abc.123.com/index.html 调用 http://def.123.com/server.php（子域名不同：abc/def，跨域）
* http://www.123.com:8080/index.html 调用 http://www.123.com:8081/server.php （端口不同：8080/8081，跨域）
* http://www.123.com/index.html 调用 https://www.123.com/server.php（协议不同：http/https，跨域）

请注意：localhost 和 127.0.0.1虽然都指向本机，但也属于跨域。
浏览器执行`javascript`脚本时，会检查这个脚本属于哪个页面，如果不是同源页面，就不会被执行。你可以理解为两个域名之间不能跨过域名来发送请求或者请求数据，否则就是不安全的，这种不安全也就是CSRF（Cross-site request forgery），中文名称：跨站请求伪造，也被称为：one click attack/session riding，缩写为：CSRF/XSRF。

![](https://github.com/Michelle111111/makedown-images/blob/master/img/js%E6%9D%83%E5%A8%81%E6%8C%87%E5%8D%97%E7%9A%84png/clip.png)

#### 2> 同源策略限制以下几种行为：
* `Cookie`，`LocalStorage`和`IndexDB`无法读取
* `DOM`和`Js`对象无法获得
* `AJAX`请求不能发送

#### 3> 跨域解决方案：（[摘自此处](https://segmentfault.com/a/1190000011145364?utm_source=tag-newest)）
* 通过`jsonp`跨域
* `document.domain + iframe`跨域
* `location.hash + iframe`
* `window.name + iframe`跨域
* `postMessage`跨域
* 跨域资源共享（`CORS`）
* `nginx`代理跨域
* `nodejs`中间件代理跨域
* `WebSocket`协议跨域

`src`属性规避同源，使用`<iframe>`，`<script>`都可以规避同源。

##### 方案一
`JSONP`：使用`script`的`Ajax`传输协议时，服务器响应采用`JSON`编码的数据格式，当执行脚本时，js解析器能自动将其“解码”。
```javascript
 <script>
    function callbackFunction(result, methodName) {
      var html = "<ul>";
      for (var i = 0; i < result.length; i++) {
        html += "<li>" + result[i] + "</li>";
      }
      html += "</ul>";
      document.getElementById("content").innerHTML = html;
    }
  </script>
  <script src="http://www.runoob.com/try/ajax/jsonp.php?jsoncallback=callbackFunction"></script>
```

##### 方案二
跨域资源共享（`CORS`）
普通跨域请求只服务端设置`Access-Control-Allow-Origin`即可，前端无需设置，若要带`cookie`请求，前后端都需要设置。
需注意的是：由于同源策略的限制，所读取的`cookie`为跨域请求接口所在域的`cookie`，而非当前页。如果想实现当前页`cookie`的写入可参考：`nginx`反向代理中设置`proxy_cookie_domain`和`NodeJs`中间件代理中`cookieDomainRewrite`参数的设置。
目前，所有浏览器都支持该功能（IE8/9需要使用`XDomainRequest`对象来支持`CORS`），`CORS`也已经成为主流的跨域解决方案。

关于`ajax`：
* `XMLHttpRequest`是`Ajax`的基础
* 当请求被发送到服务器时，我们需要执行一些基于响应的任务
* 每当`readyState`改变时，都会触发`onreadystatechange`事件
* `readyState`属性存在`XMLHttpRequest`的状态信息

下面是`XMLHttpRequest`对象的三个重要的属性：
##### (1) onreadystatechange：存储函数（或函数名），每当`readyState`属性改变时，就会调用该函数。
##### (2) readyState：存有`XMLHttpRequest`的状态。从0到4发生变化。
* 0：请求未初始化
* 1：服务器连接已建立
* 2：请求已接收
* 3：请求处理中
* 4：请求已完成，且响应已就绪

##### (3) `status`
* 200：“OK”
* 404：未找到页面

##### 前端设置
a. 原生 ajax
```javascript
var xhr = new XMLHttpRequest(); // IE8/9需用window.XDomainRequest兼容
// 前端设置是否带cookie
xhr.withCredentials = true;

xhr.open('post','http://www.domain2.com:8080/login',true);
xhr.setRequestHeader('Content-Type','application/x-www-form-urlencoded');
xhr.send('user=admin');

xhr.onreadystatechange = function(){
  if(xhr.readyState == 4 && xhr.status == 200){
    alert(xhr.responseText);
  }
}
```
b. jQuery ajax
```javascript
$.ajax({
  ...
  xhrFields:{
    withCredentials: true
  },
  crossDomain: true, //会让请求头中包含跨域的额外信息，但不会含cookie
  ...
})
```
c. vue框架
① axios设置：
```javascript
axios.defaults.withCredentials = true
```
② vue-resource设置：
```javascript
Vue.http.options.credentials = true
```
##### 服务端设置
若后端设置成功，前端浏览器控制台则不会出现跨域报错信息，反之，说明没设成功。	
a. Java后台：
```java
/*
* 导入包：import javax.servlet.http.HttpServletResponse;
* 接口参数中定义：HttpServletResponse response
*/

//允许跨域访问的域名：若有端口写全（协议+域名+端口），若没有端口末尾不用加'/'
response.setHeader("Access-Control-Allow-Origin","http://www.domain1.com");

//允许前端带认证cookie:启用此项后，上面的域名不能为'*'，必须指定具体的域名，否则浏览器会提示
response.setHeader("Access-Control-Allow-Creadentials","true");

//提示OPTIONS预检时，后端需要设置的两个常用自定义头
response.setHeader("Access-Control-Allow-Headers","Content-Type,X-Rrquested-With");
```
b. Node.js后台示例：
```javascript
var http = require('http');
var server = http.createServer();
var qs = require('querystring');

server.on('request', function(req, res) {
    var postData = '';

    // 数据块接收中
    req.addListener('data', function(chunk) {
        postData += chunk;
    });

    // 数据接收完毕
    req.addListener('end', function() {
        postData = qs.parse(postData);

        // 跨域后台设置
        res.writeHead(200, {
            'Access-Control-Allow-Credentials': 'true',     // 后端允许发送Cookie
            'Access-Control-Allow-Origin': 'http://www.domain1.com',    // 允许访问的域（协议+域名+端口）
            /* 
             * 此处设置的cookie还是domain2的而非domain1，因为后端也不能跨域写cookie(nginx反向代理可以实现)，
             * 但只要domain2中写入一次cookie认证，后面的跨域接口都能从domain2中获取cookie，从而实现所有的接口都能跨域访问
             */
            'Set-Cookie': 'l=a123456;Path=/;Domain=www.domain2.com;HttpOnly'  // HttpOnly的作用是让js无法读取cookie
        });

        res.write(JSON.stringify(postData));
        res.end();
    });
});

server.listen('8080');
console.log('Server is running at port 8080...');
```

##### 方案三
`nginx`代理跨域：
##### (1) `nginx`配置解决`iconfont`跨域
浏览器跨域访问`js`，`css`，`img`等常规静态资源被同源策略许可，但`iconfont`字体文件（`eot`|`otf`|`ttf`|`woff`|`svg`）例外，此时可在`nginx`的静态资源服务器中加入以下配置：
```javascript
location /{
   add_header Access-Control-Allow-Origin *;
}
```
##### (2) `nginx`反向代理接口跨域
跨域原理：同源策略是浏览器的安全策略，不是HTTP协议的一部分。服务器端调用HTTP接口知识使用HTTP协议，不会执行js脚本，不需要同源策略，也就不存在跨域问题。
实现思路：通过`nginx`配置一个代理服务器（域名与domain1相同，端口不同）做跳板机，反向代理访问`damain2`接口，并且可以顺便修改`cookie`中`domain`信息，方便当前域`cookie`写入，实现跨域登录。
`nginx`具体配置：
```javascript
//proxy服务器
server {
    listen       8081;
    server_name  www.domain1.com;

    location / {
        proxy_pass   http://www.domain2.com:8080;  //反向代理
        proxy_cookie_domain www.domain2.com www.domain1.com; //修改cookie里域名
        index  index.html index.htm;

        // 当用webpack-dev-server等中间件代理接口访问nginx时，此时无浏览器参与，故没有同源限制，下面的跨域配置可不启用
        add_header Access-Control-Allow-Origin http://www.domain1.com;  //当前端只跨域不带cookie时，可为*
        add_header Access-Control-Allow-Credentials true;
    }
}
```
a. 前端代码示例：
```javascript
var xhr = new XMLHttpRequest();
//浏览器是否读写cookie
xhr.withCredentials = true;
//访问nginx中的代理服务器
xhr.open('get','http://www.domain1.com:81/?user=admin',true);
xhr.send();
```
b. Node.js后台示例：
```javascript
var http = require('http');
var server = http.createServer();
var qs = require('querystring');

server.on('request',function(req,res){
   var params = qs.parse(req.url.substring(2));
   //向前台接收cookie
   res.writeHead(200,{
	'Set-Cookie':'l=a123456;Path=/;Domain=www.domain2.com;HttpOnly'   // HttpOnly:脚本无法读取
   });
   res.write(JSON.stringify(params));
   res.end();
});
server.listen('8080');
console.log('Server is running at port 8080...');
```

### 18. 客户端存储
#### 1> localStorage 和 sessionStorage
```javascript
localStorage.setItem("x",1);
localStorage.getItem("x");
localStorage.removeItem("x");
localStorage.clear();
```
* `event.key`：属性值为在`session`或`localStorage`中被修改的数据键值
* `event.oldValue`：属性值为在`sessionStorage`或`localStorage`中被修改的值
* `event.newValue`：属性值为在`sessionStorage`或`localStorage`中被修改后的值
* `event.url`：属性值为修改`sessionStorage`或`localStorage`中值的页面的URL地址
* `event.storageArea`：属性值为被变动的`sessionStorage`对象或`localStorage`对象

#### 2> cookie
检测在各种浏览器中`cookie`是否开启，可以用`navigator.cookieEnabled`属性为`true`或`false`来检测。
```javascript
  window.onload = function () {
    function setCookie(name, value, time) {
      var d = new Date();
      d.setTime(d.getTime() + (time * 24 * 60 * 60 * 1000)); //以毫秒设置Date对象
      var expires = "expires=" + d.toGMTString();
      document.cookie = name + "=" + value + ";" + expires;
    }

    function getCookie(name) {
      var cname = name + "=";
      var ca = document.cookie.split(';');
      for (var i = 0; i < ca.length; i++) {
        var c = ca[i].trim();
        if (c.indexOf(cname) == 0) {
          return c.substring(cname.length, c.length);
        } else {
          return "";
        }
      }
    }

    var btn = document.createElement("BUTTON");
    var t = document.createTextNode("点击我");
    btn.appendChild(t);
    document.body.appendChild(btn);
    btn.addEventListener("click", function () {
      var user = getCookie("username");
      if (user != '') {
        alert("欢迎" + user + "回来");
      } else {
        user = prompt("请输入你的姓名", "");
        if (user != '' && user != null) {
          setCookie("username", user, 30)
        }
      }
    })
  }
```

### 18. svg([点击查看原文](http://www.ruanyifeng.com/blog/2018/08/svg.html))
`svg`有一些预定义的形状元素：
* 矩形`<rect>`
* 圆形`<circle>`
* 椭圆`<ellipse>`
* 线`<line>`
* 折线`<polyline>`
* 多边形`<polygon>`
* 路径`<path>`
* 线性渐变`<linearGradient>`
* 放射渐变`<radialGradient>`

CSS的`fill`属性定义填充颜色（`rgb`值，颜色名或者十六进制值）
CSS的`stroke-width`属性定义边框的宽度
CSS的`stroke`属性定义边框的颜色
#### 1> 矩形`<rect>`
`rect`元素的`width`和`height`属性可定义矩形的宽度和高度

#### 2> 圆形`<circle>`
`circle`元素的`cx`和`cy`属性定义圆形的`x`和`y`坐标，如若不填，圆心为`(0,0)`

#### 3> 椭圆`<ellipse>`
`ellipse`元素的`cx`和`cy`属性定义椭圆形的`x`和`y`坐标，如若不填，圆心为`(0,0)`，`rx`和`ry`定义水平和垂直半径

#### 4> 线条`<line>`
`line`元素的`x1`和`y1`属性定义线条的开始，`x2`和`y2`定义线条的结束

#### 5> 折线`<polyline>`
`points`属性里定义了各个点的坐标，`x`和`y`坐标之间用逗号分别，多个坐标之间用空格分隔

#### 6> 多边形`<polygon>`：用来创建含有不少于三个边的图形
`points`属性定义多边形每个角的`x`和`y`坐标

#### 7> 路径`path`
M = moveto
L = lineto
H = horizontal lineto
V = vertical lineto
C = curveto
S = smooth curveto
Q = quadratic Bézier curve
T = smooth quadratic Bézier curveto
A = elliptical Arc
Z = closepath
注意：以上所有命令均允许小写字母。大写表示绝对定位，小写表示相对定位。

#### 8> 线性渐变`<linearGradient>`
`linearGradient`标签必须嵌套在`<defs>`的内部。`<defs>`标签是`definitions`的缩写，它可对诸如渐变之类的特殊元素进行定义。
线性渐变可被定义为水平，垂直或角形的渐变：
* 当 y1 和 y2 相等，而 x1 和 x2 不同时，可创建水平渐变
* 当 x1 和 x2 相等，而 y1 和 y2 不同时，可创建垂直渐变
* 当 x1 和 x2 不同，且 y1 和 y2 不同时，可创建角形渐变
```html
<svg width="100%" height="100%" version="1.1"
xmlns="http://www.w3.org/2000/svg">

<defs>
<linearGradient id="orange_red" x1="0%" y1="0%" x2="100%" y2="0%">
<stop offset="0%" style="stop-color:rgb(255,255,0);
stop-opacity:1"/>
<stop offset="100%" style="stop-color:rgb(255,0,0);
stop-opacity:1"/>
</linearGradient>
</defs>

<ellipse cx="200" cy="190" rx="85" ry="55"
style="fill:url(#orange_red)"/>

</svg>
```
##### (1) `<linearGradient>`标签的`id`属性可为渐变定义一个唯一的名称
##### (2) `fill:url(#orange_red)` 属性把`ellipse`元素连接到此渐变
##### (3) `<linearGradient>`标签的`x1`，`x2`，`y1`，`y2`属性可定义渐变的开始和结束位置
##### (4) 渐变的颜色范围可由两种或多种颜色组成。每种颜色通过一个`<stop>`标签来规定。`offset`属性用来定义渐变的开始和结束位置。

#### 9> 放射渐变
```html
<svg width="100%" height="100%" version="1.1" xmlns="http://www.w3.org/2000/svg">

  <defs>
    <radialGradient id="orange_red" cx="50%" cy="50%" r="50%" fx="30%" fy="50%">
      <stop offset="0%" style="stop-color:rgb(255,255,0);
stop-opacity:1" />
      <stop offset="100%" style="stop-color:rgb(255,0,0);
stop-opacity:1" />
    </radialGradient>
  </defs>

  <ellipse cx="200" cy="190" rx="85" ry="55" style="fill:url(#orange_red)" />

</svg>
```

#### 20. 获取地理位置（[点击查看原文](http://www.w3school.com.cn/html5/html_5_geolocation.asp)）
* navigator.geolocation.getCurrentPosition(successCallback,errorCallback,positionOptions)：获取用户当前位置
* navigator.geolocation.watchPosition()：获取当前位置，同时不断地监视当前位置，一旦用户位置发生更改，就会调用指定的回调函数
* navigator.geolocation.clearWatch()：停止监视用户位置，传递给此方法的参数应当是调用`watchPosition()`方法获得的返回值
```html
<p id="demo">点击这个按钮，获得您的坐标：</p>
<button onclick="getLocation()">试一下</button>
```
```javascript
    var x = document.getElementById("demo");

    function getLocation() {
      if (navigator.geolocation) {
        navigator.geolocation.getCurrentPosition(showPosition);
      } else {
        x.innerHTML = "Geolocation is not supported by this browser.";
      }
    }

    function showPosition(position) {
      x.innerHTML = "Latitude:" + position.coords.latitude + "<br />Longitude:" + position.coords.longitude;
    }
```
##### (1) 检测是否支持地理定位
##### (2) 如果支持，则运行`getCurrentPosition()`方法。如果不支持，则向用户显示一段消息。
##### (3) 如果`getCurrentPosition()`运行成功，则向参数`showPosition`中规定的函数返回一个`coordinates`对象
##### (4) `showPosition()`函数获得并显示经度和纬度

### 21. js中的堆和栈的简单解释（[点击查看原文](https://www.cnblogs.com/jiangk1214/p/6650957.html)）
堆和栈都是运行时内存中分配的一个数据区，因此也被称为堆区和栈区，但二者存储的数据类型和处理速度不同。堆（`heap`）用于复杂数据类型（`引用类型`）分配空间，例如数组对象，`object`对象；他是运行时动态分配内存的，因此存取速度较慢。栈（`stack`）中主要存放一些基本类型的变量和对象的引用，其优势是存取速度比堆要快，并且栈内的数据可以共享，但缺点是存在栈中的数据大小与生存期必须是确定的，缺乏灵活性。
#### 1> 栈的使用规则
栈有一个很重要的特殊性，就是存在栈中的数据可共享，例如下面的代码定义两个变量，变量的值都是数字类型。
```javascript
var a = 3;
var b = 4;
```
`JavaScript`引擎先处理`var a=3`；首先会在栈中创建一个变量为`a`引用，然后查找栈中是否有`3` 这个值，如果没有找到，就将`3`存放进来，然后将`a`指向`3`。接着处理`var b=3`；在创建为`b`的引用变量后，查找栈中是否有`3`这个值，因为此时栈中已经存在了`3`，便将`b`直接指向`3`。这样，就出现了`a`与`b`同时指向`3`的情况。此时，如果再令`a=4`，那么`JavaScript`引擎会重新搜查栈中是否有`4`这个值，如果没有，则将`4`存放进来，并令`a`指向`4`；如果已经有了，则直接将`a`指向这个地址。因此`a`值的改变不会影响到`b`的值。

#### 2> 堆的使用规则
下面通过`Array`来看一下堆的行为，例如存在下面的代码：
```javascript
var fruit_1 = "apple";
var fruit_2 = "orange";
var fruit_3 = "banana";
var oArray = [fruit_1,fruit_2,fruit_3];
var newArray = oArray;
```
当创建数组时，就会在堆内存创建一个数组对象，并且在栈内存中创建一个对数组的引用。变量`fruit_1`，`fruit_2`，`fruit_3`为基本数据类型，他们的值直接存放在栈中；`newArray`，`oArray`为复合数据类型（`引用类型`），他们的引用变量存放在栈中，指向于存放堆中的实际对象。
注意，`newArray`的值等于变量引用`oArray`，所以他也是复合数据类型（`引用类型`）。此时，如果更改变量`fruit_1`，`fruit_2`，`fruit_3`的值，那么其实是更改栈中的值；如果更改`newArray`或`oArray`的值，那么其实是更改堆中的实际对象，因此，对两个变量引用都会发生作用。例如，首先更改`newArray`的值，然后看`oArray`的值，代码如下：
```javascript
alert(oArray[1]); //返回 orange
newArray[1] = "berry";
alert(oArray[1]); //返回 berry
```
同样，首先更改`oArray`的值，然后看`newArray`的值，代码如下：
```javascript
alert(newArray[1]); //返回 orange
oArray[1] = 'tomato';
alert(newArray[1]); //返回 tomato
```
`JavaScript`堆不需要程序代码来显示地释放，因为堆是由自动的垃圾回收来负责的，每种浏览器中的`JavaScript`引擎有不同的自动回收方式，但一个最基本的原则是：如果栈中不存在对堆中某个对象的引用，那么就认为该对象已经不再需要，在垃圾回收时就会清除该对象占用的内存空间。因此，在不需要时应该将对象的引用释放掉，以利于垃圾回收，这样就可以提高程序的性能。释放对象的引用最常用的方法就是为其赋值为`null`，例如下面代码将`newArray`赋值为`null`：
```javascript
newArray = null;
```

#### 3> 易犯的错误
在堆和栈的使用问题上，最易犯的错误就是`String`的使用，例如下面的代码：
```javascript
var str = new String('abc');
var str = 'abc';
```
同样是创建两个字符串，第一种是用`new`关键字来新建`String`对象，对象会存放在堆中，每调用一次就会创建一个新的对象；而第二种是在栈中，栈中存放值`abc`和对值的引用。推荐使用第二种方式创建多个`abc`字符串，这种写法在内存中只存在一个值，有利于节省内存空间。同时他可以在一定程度行提高程序的运行速度，因为存储在栈或者那个，其值可以共享，并且由于栈访问更快，所以对于性能的提高大有裨益。`而第一种方式每次都在堆中创建一个新的 String 对象，而不管其字符串值是否相等是否有必要创建新对象，从而加重了程序的负担。`并且堆的访问速度慢，对程序性能的影响也大。另外，出于逻辑运算的考虑，当对两个变量进行比较时，使用堆和栈存储就会有差异。下面来看一下逻辑等于和逻辑全等运算，深入理解一下堆和栈：
(1) 例如下面的代码，实际只比较栈中的值：
```javascript
var str1 = "abc";
var str2 = "abc";
alert(str1 == str2); //true
alert(str1 === str2); //true
```
不管是逻辑等于和逻辑全等运算都返回`true`，可以看出`str1`和`str2`指向同一个值。
(2) 例如下面的代码，实际只比较堆中的值：
```javascript
var str1 = new String('abc');
var str2 = new String('abc');
alert(str1 == str2); //false
alert(str1 === str2); //false
```
不管是逻辑等于还是逻辑全等都返回`false`，可以看出`str1`和`str2`指向的不是同一个对象。
(3) 例如下面的代码，比较堆和栈中的值：
```javascript
var str1 = new String('abc');
var str2 = 'abc';
alert(str1 == str2); //true
alert(str1 === str2); //false
```
在进行逻辑等于和逻辑全等运算时，会首先将变量转成相同的数据类型，然后进行对比。变量`str1`和`str2`的数据类型虽然不同，但比较运算还是返回`true`。但逻辑全等运算与逻辑等于运算不同，他会对数据类型进行比较，看是否是引用的同一个数据。
