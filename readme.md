# 总纲
## 1.1 目的
  通过制定统一的前端编码规范结合最佳实践，让前端开发人员编写出高效、严谨、健壮、统一、优雅的代码。
## 1.2 适用对象
  本规范适用于研发中心的所有前端开发人员。
## 1.3 适用范围
- 本规范适用于研发中心所有新的vue，react以及移动端项目（不在旧系统项目里面）。
- 具体实施过程中我们会根据业务方项目实际情况来执行。
## 1.4 约束
- 复杂业务逻辑必须要有注释信息
- 严格上不能存在回调地狱(超过两层的回调，用async await代替)
- 所有项目必须要通过hoolinks-cli生成
- 组件状态默认存放store里面。临时，不涉及传递，回调的状态除外，可以挂在组件自身
- api调用放在专门文件夹里面管理
- 前端必须要有数据模型层 model(数据模型必须有注释信息)
- 服务端数据在前端使用时，前端对于服务端结构必须统一使用observablePromise函数来接收
- 前端数据模型层必须要使用json-mapper-object来自动映射，具体使用可参考文档[json-mapper-object](https://github.com/duanguang/json-mapper-object)
-  vue默认三方基础组件[iview](https://www.iviewui.com/docs/guide/start)
# 2 JavaScript编码规范
## 2.1 总则
### 2.1.1 ES标准
-	全面使用ES6及以上语法。
-	全面使用Typescript开发项目。
### 2.1.2 Node 版本
-	Node版本 >= 9.8.0，始终安装LTS版本。
### 2.1.3 包管理器
- <font color="red">*</font>使用yarn作为包管理器。
### 2.1.4 包版本管理
- <font color="red">*</font>使用lock文件锁定包版本。
## 2.2 变量声明规范
- <font color="red">*</font>禁用var声明变量，应用let声明变量。
- <font color="red">*</font>常量或不会被重新赋值的变量需声明为const常量。
- <font color="red">*</font>坚持变量先声明后使用。
- <font color="red">*</font>坚持给变量赋默认值。
- <font color="red">*</font>坚持一个变量声明只声明一个变量。如：

```js
// good
let items = getItems();
let goSportsTeam = true;
let dragonball = 'z';

// bad
let items = getItems(),
    goSportsTeam = true,
    dragonball = 'z';
```
## 2.3 变量命名规范
- 文件中导出的常量命名需全部大写字母，中间用下划线隔开，如：API_ROOT。
- <font color="red">*</font> 常规变量命名坚持使用小驼峰式命名法，如： apiService。
- <font color="red">*</font> 类命名坚持使用大驼峰式命名法，如: HttpService。
- <font color="red">*</font> 变量或函数命名必须清晰完整，让人看到名称立刻知道它包含了什么，代表了什么，正确： HttpService，错误：HttpSer。

## 2.4 变量导入导出规范

### 2.4.1 变量导入
- 使用标准的ES6模块语法导入。
- 统一使用import命令接花括号导入某个模块中导出的变量，如：import { environment } from ‘@env/environment’;（注： 对于第三方模块可放开限制，使用import * as xxx from ‘xxxxx’; 进行导入）。
- import声明应始终位于文件顶部。
- 一个地方只在一个路径中导入，如：

```js
// good
import { foo, named1, named2 } from 'foo';

// bad
import { foo } from 'foo';
// … 其他一些 imports … //
import { named1, named2 } from 'foo';
```
### 2.4.2 变量导出
- 坚持为文件中每个需要导出的变量声明export，如：export const ENVIRONMENT = environment;
- export声明应始终位于文件结尾。
- 禁用导出默认语法，即：export default xxxx; 组件除外

## 2.5 函数规范

### 2.5.1 函数声明规范
- 坚持使用箭头函数代替function声明。（注： 如有特殊场景可不受限制）
- <font color="red">*</font> 坚持为必传入参需设定默认值。
- <font color="red">*</font> 函数和入参命名需语义化。
- <font color="red">*</font> 函数中未使用的参数需及时清除。
- 坚持使用rest参数代替arguments。
- 坚持将有默认值参数置于参数列表的最后面。
- <font color="red">*</font>坚持使用class代替构造函数声明。
- 坚持使用class继承语法代替直接操作prototype。
- <font color="red">*</font> 禁止在函数中声明全局变量。
- 函数传入参数超过3个以上时需要聚合，如

```js
 //bad
 function demo(name,age,user,date){}
 //good
 function demo(options) //options ={name,age,user,date}
```
- 禁止在函数中声明影子变量。如
```js
function demo(a, b) {
const a = 11;
}
```
- <font color="red">*</font>禁用使用new Function声明函数。
- <font color="red">*</font>禁止在块级作用域内使用function声明函数。
### 2.5.2 函数使用规范
- 坚持使用fun.bind(this)来绑定this来绑定this指向，避免写const self = this;
- <font color="red">*</font> 坚持职责单一原则，不宜在同一函数内操作过多的逻辑。
- 函数递归调用时，应尽量改写成尾递归方式避免栈溢出。
- <font color="red">*</font>禁止在函数使用过程中修改原型上的属性和方法。如确实有需要，应该合成一个新的对象。
### 2.5.3 类声明规范
- <font color="red">*</font>类名统一使用大驼峰式命名法。
- <font color="red">*</font>必须显式声明构造函数。
- <font color="red">*</font>继承类需在构造函数第一行显式调用super()。
- <font color="red">*</font> 先声明类属性，然后是构造函数，最后是方法。
- <font color="red">*</font> 私有属性和方法名需加下划线前缀，如_foo。
- <font color="red">*</font> 坚持在类属性声明时赋值默认值。

## 2.6 语言特性规范
### 2.6.1 字符串
- <font color="red">*</font> 坚持使用单引号代替双引号声明字符串。
- <font color="red">*</font> 坚持使用模板字符串即反引号创建含有变量的字符串代替 + 号连接。如
```js
 let str1='小明'
 let str=`我是${str1}`
```
- 坚持使用includes()、startsWith()、endsWith()来确定一个字符串是否包含在另一个字符串中，代替使用indexOf判断。
- 禁用 + 或者 \ 创建多行字符串，需使用反引号 ` 代替。
### 2.6.2 数值
- <font color="red">*</font> 长度较大的数值需声明为字符串，如ID。
- 运算时注意隐式转换，如’2’ * 2 = 4; ‘2’ + 2 = "22";
### 2.6.3 数组
- 坚持使用扩展运算符来复制数组，如const copyArr = […arr];
- 坚持使用for…of遍历数组或者map
- 坚持使用includes代替indexOf来检查是否包含某个值。
- 坚持使用扩展运算符来连接数组，代替使用concat，如

```js
// good
[...foo, ...bar]

// bad
foo.concat(bar)

```
- 禁用new Array()创建数组，应使用字面量来创建。
- 禁止在数组上定义非数值的key，length除外。
### 2.6.4 对象
- 坚持使用属性名简写，如

```js
const foo = 1;
const bar = 2;
const obj = {
  foo,
  bar,
  method() { return this.foo + this.bar; },
};

// bad
const obj = {
  foo: foo,
  bar: bar,
  method: function() { return this.foo + this.bar; },
};

```
- 坚持使用扩展运算符来合并对象或进行浅拷贝，代替使用Object.assign，如：

```js
// good
const obj = {
    ...obj1,
    ...obj2,
};

// bad
const obj = Object.assign({}, obj1, obj2);

```
- 调用window全局对象上的属性或方法需显式声明，document除外，如：

```js
// good
window.location

// bad
location

```
- 禁用for…in遍历对象，应用for (const [key, value] of Object.entries(obj))或 for (const key of Object.keys(obj))代替。
- 禁用new Object()创建对象，应使用字面量来创建。

## 2.7 控制体结构
### 2.7.1 for 循环
- 坚持使用for…of代替其他for循环。
- 坚持使用高阶函数代替for循环遍历数组，如forEach、map、filter、reduce、every、some等等。
### 2.7.2 switch 声明
- 坚持给每个switch声明包含default分支，就算default分支没有代码。
### 2.7.3 if判断
- <font color="red">*</font> 必须使用严格相等比较，即 === 或 !==，禁用 == 或 !=。
- <font color="red">*</font> 坚持利用短路判断优化代码风格，如

```js
if (arr && arr.length) {}
```
- 对于null ,'',undefined，布尔等判断使用方式。如

```js
let a=null;
//good
if(a)
//bad
if(a===null)

let b;
//good
if(b)
//bad
if(b===undefined)

let c=true;
//good
if(c)
//bad
if(c==true)

let d='';
//good
 if(d)
 //bad
 if(d==='')
```
## 2.8 异常处理
- AJAX请求需有统一的异常处理机制。
- AJAX回调处理数据必须使用try...catch...finally包裹代码块并在finally里做一些清理行为
- 其余主流程关键点或容易出现运行时错误的地方也需添加try...catch。
## 2.9 禁用的特性
- <font color="red">*</font> 禁止使用with。
- <font color="red">*</font> 禁止使用eval。
- <font color="red">*</font> 禁止添加、修改、删除ES标准内置对象的原型属性和方法。
## 3 代码风格规范
### 3.1 总则
- 字符串使用单引号包裹，需嵌套的除外。
- 使用大括号包含所有的多行代码块。
- 强制分号。if、switch、for和function声明大括号后无需加分号，其余语句必须加分号终止。
- 必须开启ESLint或TSLint进行代码风格检查。
- 使用空格缩进。（注：可选值2或4）
### 3.2 注释
- 双斜杆注释需在双斜杠前后各加一个空格，如：const a = 1; // comment
- 变量声明或类属性的注释采用双斜杆注释。
- <font color="red">*</font>所有具名函数和类方法都必须在头部加上注释，必须注明：函数作用，参数含义。可选注明：函数返回值类型，参数类型，是否为可选参数。如：

```js
/**
 * 设置样式
 * @param @requires {HTMLElement} element DOM元素
 * @param {object | string} styleName 样式名,单个传字符串,多个传对象
 * @param {string} value 样式值,如第二参数为字符串则该参数必传
 * @returns {void}
 */
function setStyle(element, styleName, value) {
    // ...
}

```
- 关键步骤必须写注释说明。
- 模块文明顶部要加上注释说明其作用
### 3.3 空白
- 在大括号前放置一个空格，如：

```js
function test() {
    console.log('test');
}

// bad
function test(){
    console.log('test');
}

```
- 在控制语句（if，while等）和循环语句（for）的小括号前放一个空格，如：

```js
// good
if (isJedi) {
    fight();
}

// bad
if(isJedi) {
    fight ();
}

```
- 使用空格把运算符隔开，如：

```js
// good
const x = y + 5;

// bad
const x=y+5;

```
- 在大括号内的前后添加空格，如：

```js
// good
const foo = { clark: 'kent' };

// bad
const foo = {clark: 'kent'};

```
- 对象中的冒号和value中间添加一个空格。
- 赋值等号前后各添加一个空格。
- 需及时清除无用的空白行。

### 3.4 逗号
- 函数的参数列表或数组中的逗号后添加一个空格，如：

```js
// good
const foo = [1, 2, 3];
function hero(parame1, parame2) {}

// bad
const foo = [1,2,3];
function hero(parame1,parame2) {}

```
- 多行字面量对象或数组需添加尾逗号，如：

```js
// good
const hero = {
    firstName: 'Dana',
    lastName: 'Scully',
};

const heroes = [
    'Batman',
    'Superman',
];

// bad
const hero = {
    firstName: 'Dana',
    lastName: 'Scully'
};

const heroes = [
    'Batman',
    'Superman'
];

```
### 3.5 其他
- 长方法链式调用时需换行并以 . 开头，如：

```js
// good
$('#items')
    .find('.selected')
    .highlight()
    .end()
    .find('.open')
    .updateCount();

// bad
$('#items').find('.selected').highlight().end().find('.open').updateCount();

```
- 字面量声明对象或数组时如果元素超过5个时需改成多行格式。
## 4 开发、打包、部署、持续集成
### 4.1 开发
- <font color="red">*</font>除UI组件库外，其余包版本号请勿随意改动或升级。如需升级必须拉一条分支出来测试稳定性。
- 任一组员改动包版本号后需通知其余组员yarn更新本地的包版本。
- 如需改动公共模块、核心模块、工具类等公有功能时需通知其余组员调研是否会影响到他人的模块或功能。
### 4.2 打包
- <font color="red">*</font>打包需去除所有的debugger，console语句
- 所有除第三方引入的js和css库外，需生成带有hash值的js和css文件。
- 如果引用的是公共库无法生成hash值则需要在库更新版本后在HTML的引用路径后面加上版本号参数，如https://css.banggood.com/js/jg.js?v=20180614
- 推送仓库进行自动化构建前必须在本地进行打包一次测试是否编译异常。
### 4.3 部署
- 开启gzip压缩。
- 拦截.html结尾的设置响应头为Cache-Control: no-cache，如

```js
location  ~ .*\.(html)$  {
    add_header Cache-Control no-cache;
}

```
- 拦截其余静态资源设置缓存，如

```js
location  ~ .*\.(js|css)$  {
    client_max_body_size 50m;   
    expires 3d;
}
#对字体进行缓存    
location ~* ^.+\.(eot|ttf|otf|woff|svg)$ {
    access_log off;
    expires max;
}

```
- 配置后台接口路径转发规则。
### 4.4 持续集成
- 利用Jenkins进行自动化构建。
- 已发布上线的版本需在master分支上打tag。并注明更新内容






















