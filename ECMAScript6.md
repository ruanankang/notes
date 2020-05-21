#  ECMAScript6 基础语法

## var let const

- var 声明的变量可以重复声明，而 let const 声明的变量不可以同名
- const 声明变量时必须赋值，且只能赋值一次
- var 的作用域是 **function scope (函数作用域)**  ，要么属于某个函数的内部变量，要么属于全局变量
- let const 的作用域是 **block scope (块级作用域)**，即如果变量声明在{ }中，则该变量只能在这个里面被访问，比如在 if 或 for 中声明的变量，在外部访问不到，但是 var 声明的就可以
- const 声明的变量如果是引用类型，比如**对象类型/数组类型**，则可以通过改变对象属性的方式或数组赋值，修改const声明的变量
- 如果不希望 const 的声明的对象变量能修改属性值，可以通过 **Object.freeze()** 方法声明
- var 声明的变量会在预编译的时候提升，而 let const 提升变量声明在一个临时性死区中，相当于没有提升变量声明；**Temporal Dead Zone (TDZ) 临时性死区**

```javascript
var price = 5;
var price = 1;
console.log(price) //1

let price = 10; //报错
-------------------------------------
const a; //报错
---------------------------------
if (true) {
    let b = 3;
    const c = 4;
    var d = 5;
}
console.log(d) //5
console.log(b) //报错
console.log(c) //报错
-----------------------------------
const person = {
    name: 'Job',
    age: 17
}
person.name == 'Bob'
person = 17 //报错

const Bob = Object.freeze(person); //Bob无法修改属性值

const arr = [1, 2, 3];
arr[0] = 5;
console.log(arr); //[5, 2, 3]
-------------------------------------------
console.log(a); //undefined
var a = 13;

console.log(b);//reference error
let b = 14;
-------------------------------------------
```

#### 用 let 和 const 代替`立即执行函数`

立即执行函数常用于 **变量私有化** 

```javascript
(function () {
    var name = "Bob"
}())

{
    const name = "Kelly"
}
```

关于for循环中的let和var的区别 https://segmentfault.com/q/1010000007541743



## 箭头函数 Arrow Function

```javascript
const numbers = [1, 2, 5, 1, 6];

const double = numbers.map((item) => {
        return item * 2;
    })

const double2 = numbers.map(item => {
        return item * 2;
    })

const double3 = numbers.map((item, index) => {
        return item * 2;
    })

//隐式返回
const double4 = numbers.map(item => item * 2)

const double5 = numbers.map(() => {
        return 1;
    })

//匿名函数
const greet = name => {
	alert(`hello ${name}`)
}
```

es6内置方法（如map、filter等）中的回调函数的 this 指向window、global（严格模式下为undefined）；

箭头函数的this值是词法作用域，指向父级作用域

```javascript
const Jelly = {
    	name: 'Jelly',
    	hobbies: ['coding', 'Sleeping', 'Reading'],
    	printHobbies: function() {
    		this.hobbies.map(function(elem, index) {
    			console.log(`${this.name} loves ${elem}`) //报错，this指向为window
    		})
    	}
    }

    const Jelly = {
    	name: 'Jelly',
    	hobbies: ['coding', 'Sleeping', 'Reading'],
    	printHobbies: function() {
    		this.hobbies.map(elem => {
    			console.log(`${this.name} loves ${elem}`) //正常打印，箭头函数不改变this指向
    		})
    	}
    }

    Jelly.printHobbies()
```



#### 函数默认参数值 Default Arguments

当可选参数不是第一个参数时，前面的参数需要传入undefined，此时才会使前面的参数为默认参数值

底层会在函数第一行执行  a = typeof a === undefined  ? 3 : undefined

```javascript
function multiply(a = 3, b = 4) {
    	return a * b
}

multiply(); //12
multiply(1); //4
multiply(undefined, 2); //6
```

**箭头函数不适用的场景**

箭头函数没有arguments参数伪数组对象

```javascript
// 1、作为构造函数，一个方法需要绑定到对象
    // const Person = (name, age) => {
    //     this.name = name;
    //     this.age = age;
    // } //报错
    const Person = function(name, age) {
        this.name = name;
        this.age = age;
    } //正常
    const Bob = new Person('Bob', 5);
    // Person.prototype.updateAge = () => {
    //     this.age++;
    //     console.log(this.age)
    // } //此时this是指向window，而不是Person
    Person.prototype.updateAge = function() {
        this.age++;
        console.log(this.age)
    }
    Bob.updateAge();

    // 2、当你真的需要this的时候
    const button = document.querySelector('.zoom');
    button.addEventListener('click', () => {
        console.log(this); //指向父级作用域window
        this.classList.add('in');
        setTimeout(() => {
            this.classList.remove('in');
        }, 2000)
    }) //此时的箭头函数的this指向父级作用域window
    button.addEventListener('click', function() {
        console.log(this); //指向button对象
        this.classList.add('in');
        setTimeout(() => {
            console.log(this); //指向父级作用域button对象
            this.classList.remove('in');
        }, 2000)
    }) //此时的箭头函数的this指向父级作用域window

    // 3、需要使用arguments对象
    const sum = function() {
        return Array.from(arguments).reduce((prevSum, value) => prevSum + value, 0)
    }
```



## 模板字符串 Template StringsLiterals

```javascript
`${变量 表达式 函数 等等}`
```

反引号``;     可嵌套

```javascript
//自动删除数组的中括号，将数组元素以逗号连接成字符串
console.log(`${[1, 2]}`) // 1,2
```

#### 标签模板字符串 Tags Template StringsLiterals

```javascript
<script type="text/javascript">
  function highlight(string, ...values) {
    console.log(arguments);
    console.log(string, values);
    debugger;
    return 'laravist';
  }

  const user = 'Mary';
  const topic = 'Learn to use markdown';
  // （标签模板字符串）函数名+模板字符串 等价于 将模板字符串中的所有字符串整合成数组后，作为第一个参数传递给函数，其余变量作为其他参数，依次传入函数中
  // 以变量开头和结尾的标签模板字符串会多出空字符串
  const sentence = highlight `${user} has commented on your topic ${topic}`;

  console.log(sentence);
```

#### ES6新增字符串函数

##### startsWith



##### endsWith



##### includes



##### repeat

```javascript
const id = '51030019800820366x';
const fan = 'I love Bob';

console.log(id.startsWith('51')); //true
console.log(id.startsWith('1980', 6)); //true
console.log(id.endsWith('x')); //true
console.log(id.endsWith('1980', 10)); //true

console.log(fan.includes('Bob')); //true
console.log(fan.includes('Love', 7)); //false

console.log('='.repeat(5)); //=====
```



## 对象解构与数组解构

```javascript
const Tom = {
    name: 'Tom Jones',
    age: 25,
    family: {
      mother: 'Norah Jones',
      father: 'Richard Jones',
      brother: 'Howard Jones'
    }
  }

  //1、 对象解构
  const { name, age } = Tom;

  //2、 解构之前已有变量名被使用，那么在解构的时候需要用括号包住
  let name = 'Bob';
  ({ name, age } = Tom);
  console.log(name);

  //3、 对象解构可以嵌套
  const { mother, father } = Tom.family;

  //4、 对象解构重命名
  const { mother, father, brother: b } = Tom.family;

  //5、 如果对象没有该属性，打印结果为undefined，可以初始化赋值（回滚值），但是并不会给原来对象增加该属性
  const { mother, father, brother: b, sister = 'Alice' } = Tom.family;
  console.log(sister);

  /**
   * 对象解构应用
   */
  function appendChildDiv(options = {}) {
  	const {parent = 'body', width = '100px', height = '80px', backgroundColor = 'pink'} = options;
  	const div = document.createElement('div');
  	div.style.width = width;
  	div.style.height = height;
  	div.style.backgroundColor = backgroundColor;

  	document.querySelector(parent).appendChild(div);
  }

  appendChildDiv({
  	parent: '.container',
  	width: '200px',
  	height: '100px'
  })
```

```javascript
const numbers = [1, 2, 3];

  // 1、数组解构获取对应索引位置的值，声明关键字const let可以不加，如果不加默认为let
  const [num1, num2] = numbers;

  // 2、如果该索引位置的值不需要，就用逗号保留
  const [num1, , num3] = numbers;

  // 3、如果想获取后面所有元素，则使用剩余参数的方式获取，得到数组
  const [num1, ...nums] = numbers; //num1 = 1, nums = [2, 3]

  // 4、当某个索引值为undefined时，可以设置默认参数
  const [num1, , num3, num4 = 4] = numbers;

  /**
   * 数组解构的应用  交换a, b两个值
   */
  let a = 10;
  let b = 20;
  [a, b] = [b, a];
```



## 数组方法

#### 数组遍历 for-of

当变量类型有属性方法[Symbol.iterator]时，可以用for-of循环

```javascript
const fruits = ['apple', 'banana', 'pear', 'mango'];

  //四种遍历方式 for forEach for-in for-of
  for (let i = 0; i < fruits.length; i++) {
    console.log(fruits[i])
  }

  fruits.forEach(fruit => {
  	if (fruit === 'pear') {
  		return;
  	}
  	console.log(fruit);
  })

  for (let key in fruits) {
  	console.log(fruits[key])
  }

  for(let fruit of fruits) {
  	if (fruit === 'pear') {
  		break;
  	}
  	console.log(fruit)
  }

  /**
   * for-of的应用  可用于数组、字符串、map、set、arguments(类数组)、NodeList、Iterator等数据结构
   * Array.prototype.entries() 返回数组的迭代器 Array Iterator
   */
  for (let fruit of fruits.entries()) {
  	console.log(fruit)
  }
  for (let [index, fruit] of fruits.entries()) {
  	console.log(fruit)
  }

```

#### 类数组Arguments、NodeList、字符串转换为数组  Array.from()    

不是原型上的方法，是静态方法 ，无法通过数组对象调用该方法，只能通过构造函数即Array调用

```javascript
// nodelist是类数组   Array.from()可以将类数组转换成数组
  const todos = document.querySelectorAll('li');
  // const names = Array.from(todos).map(todo => todo.textContent);
 // const names = [...todos].map(todo => todo.textContent);
  const names = Array.from(todos, todo => todo.textContent);
  console.log(names);
```

#### 创建数组 Array.of()

```javascript
const arr = Array.of(1, 2 , 3); //arr = [1, 2, 3]
```

#### .find()

#### .findIndex()

#### .every()

#### .some()

```javascript
/**
   * .find((element, index, array) => {}) 返回element
   */
  const inventory = [
  	{name: 'apples', quantity: 2},
  	{name: 'bananas', quantity: 0},
  	{name: 'cherries', quantity: 5}
  ]
  // const bananas = inventory.find((fruit, index, inventory) => {
  // 	console.log(fruit)
  // 	if (fruit.name === 'bananas') {
  // 		return true;
  // 	}
  // 	return false;
  // })
  const bananas = inventory.find(fruit => fruit.name === 'bananas');
  console.log(bananas)

  /**
   * .findIndex((element, index, array) => {}) 返回index
   */
  
  /**
   * .some((element, index, array) => {}) 当遇到第一个return true时，结束循环，返回true，否则返回false
   */
  
  /**
   * .every((element, index, array) => {}) 当遇到第一个return false时，结束循环，返回false，否则返回true
   */
```



## 剩余参数 & 扩展运算符

- 剩余参数  **...**变量  可以在**函数形参**、**数组解构**场合出现；

- 剩余参数是将多个参数整合成一个数组；

  

- 扩展运算符  **[...可遍历对象]**  **{...可遍历对象}** 是把一个**可遍历对象**的每个元素扩展为新的参数序列;
  
- 可遍历对象是指实现了Iterator接口，可以用for-of循环的数据类型（**字符串、数组、类数组arguments、nodelist、set、map**）;
  
- 扩展运算符是浅拷贝；

- 扩展运算符可以在**函数传实参**、**数组拼接**场合出现

```javascript
console.log([...'afd']) //['a', 'f', 'd']

console.log([...[1, 2], 'sdf', ...[3]]) //[1, 2, 'sdf', '3']

const obj = {a: 1, b: 2};
const obj2 = {...obj}; //浅拷贝
console.log(obj2) //{a: 1, b: 2}

const fruit = ['apple', 'pear'];
const newFruit = ['orange', 'mongo'];
// fruit.push.apply(fruit, newFruit)
fruit.push(...newFruit);
```

##  计算属性

对象字面量的属性名称可以通过   **[表达式]** 的方式来定义

```javascript
let id = 0;
const obj = {
    [`user-&{++id}`]: id,
    [`user-&{++id}`]: id,
    [`user-&{++id}`]: id
}
console.log(obj) //{user-1: 1, user-2: 2; user-3: 3}

const keys = ['name', 'age', 'birthday'];
const values = ['Bob', 12, '2015-09'];
const Bob = {
    [keys.shift()]: values.shift(),
    [keys.shift()]: values.shift(),
    [keys.shift()]: values.shift()
}
const.log(Bob) //{name: 'Bob', age: 12, birthday: '2015-09'}
```

## Promise

- 解决异步请求的顺序问题；

- 解决多层回调嵌套问题；

  ```javascript
  /**
     * 创建 Promise
     * reslove 指成功后的回调函数，在then中返回
     * reject 指失败后的回调函数，在catch中返回
     */
    const p = new Promise((reslove, reject) => {
      // reslove('Bob is success');
      reject(Error('Alice is fatal'));
    })
  
    p.then(data => {
        console.log(data);
      })
      .catch(err => {
        console.error(err);
      });
  ```
  
  实例01
  
  ```javascript
    const repos = [{
        name: 'grit',
        owner: 'mojombo',
        description: 'Grit is no longer maintained',
        id: 1
      },
      {
        name: 'jsawesome',
        owner: 'vanpelt',
        description: 'Awesome JSON',
        id: 2
      }, {
        name: 'merb-core',
        owner: 'wycats',
        description: 'Merb Core: All you need. None you don\'t',
        id: 3
      }
    ];
  
    const owners = [{
        name: 'mojombo',
        location: 'San Francisco',
        followers: 123
      },
      {
        name: 'vanpelt',
        location: 'Bay Minette',
        followers: 1034
      },
      {
        name: 'wycats',
        location: 'Los Angeles, CA',
        followers: 388
      }
    ]
  
    // 根据id查找repo，返回一个 Promise 对象
    function getReposById(id) {
      return new Promise((resolve, reject) => {
        const repo = repos.find(item => item.id === id);
        if (repo) {
          resolve(repo);
        } else {
          reject(Error('No repo fond.'))
        }
      });
    }
  
    const ret = getReposById(1);
    console.log(ret);
  
  
    getReposById(1)
      .then(repo => {
        console.log(repo);
      })
      .catch(err => {
        console.error(err);
      })
  
    // 根据repo查找owner，返回一个 Promise 对象
    function comboundOwner(repo) {
      return new Promise((resolve, reject) => {
        const owner = owners.find(item => item.name === repo.owner);
        if (owner) {
          resolve(owner);
        } else {
          reject(Error('Can not find the owner.'))
        }
      })
    }
  
    getReposById(1)
      .then(repo => {
        return comboundOwner(repo);
      })
      .then(owner => {
        console.log(owner);
      })
      .catch(err => {
        console.error(err);
      })
  ```
  
  实例02：**处理多个 Promise 对象**
  
  **all**：只有当传入的**所有 Promise 对象**都是执行 resolve 方法时，才会执行 all 后面的 then，否则执行 catch；可以将多个Promise实例包装成一个新的Promise实例。同时，成功和失败的返回值是不同的，成功的时候返回的是一个结果数组，而失败的时候则返回最先被reject失败状态的值。
  
  **race**：Promse.race就是赛跑的意思，意思就是说，Promise.race([p1, p2, p3])里面哪个结果获得的快，就返回那个结果，不管结果本身是成功状态还是失败状态。
  
  
  
  Promise.race可以用来在有最大请求数量限制时，用来实现大量请求的队列发起。
  
  Promise.race用于和定时器绑定，可以测试一些接口的响应速度，分析用户的网络状况之类的，，比如将一个请求，和三秒后执行定时器 包装成promise 实例，然后加入 promise.race队列中， 当请求三秒还未响应时候，可以给用户一些提示， 或者是一些其他操作
  
  ```javascript
    const usersPromise = new Promise((resolve, reject) => {
      setTimeout(() => {
        resolve(['mojombo', 'vanpelt', 'wycats']);
      }, 2000);
    });
  
    const moviePromise = new Promise((resolve, reject) => {
      setTimeout(() => {
        resolve({ name: '摔跤吧！爸爸', rating: 9.2, year: 2016 });
      }, 500);
    });
  
    // all方法：只有当传入的所有 Promise 对象都是执行 resolve 方法时，才会执行 all 后面的 then，否则执行 catch
    Promise
      .all([usersPromise, moviePromise])
      .then(res => {
        // res是一个数组，每个元素是传入all里面的promise对象的返回值
        const [users, movie] = res;
        console.log(users);
        console.log(movie);
      })
      .catch(err => {
        console.error(err);
      })
  
    // race方法：
    Promise
      .race([usersPromise, moviePromise])
      .then(res => {
        // res是一个数组，每个元素是传入all里面的promise对象的返回值
        const [users, movie] = res;
        console.log(users);
        console.log(movie);
      })
      .catch(err => {
        console.error(err);
      })
  ```



## Symbol  第七个数据类型

**Symbol **是 es6 推出的继 **Number String Boolean Undefined Null Object** 六大数据类型之后推出的第七个数据类型；除了Object，其他数据类型是基本数据类型

![image-20200214163513799](C:\Users\15740\AppData\Roaming\Typora\typora-user-images\image-20200214163513799.png)

Symbol是生成唯一标识，可以解决对象属性相同时的冲突，可以用来定义**对象私有属性**；

Symbol类型的**属性名不能通过 for-in 遍历**；

Symbol类型的**属性名**可以通过 **Object.getOwnPropertySymbols()** **遍历**，返回值为一个所有属性名构成的数组；

Symbol类型的**属性值**可以通过 **Object.getOwnPropertySymbols()**.**map()** **遍历**；

```javascript
// 创建一个 Symbol 类型的值: Symbol()
  const peter = Symbol();

  const classRoom = {
    [Symbol('lily')]: { grade: 60, gender: 'female' },
    [Symbol('bobe')]: { grade: 80, gender: 'male' },
    [Symbol('nina')]: { grade: 90, gender: 'female' },
  }

  for (let key in classRoom) {
  	console.log(key); //无
  }

  // 遍历属性名，返回数组
  const syms = Object.getOwnPropertySymbols(classRoom);
  console.log(syms);

  // 通过属性名，获取 Symbol 类型的属性的属性值，只能通过 对象[属性名] 的方式访问，不能通过 对象.属性名 方式访问
  syms.map(item => {
  	console.log(classRoom[item]);
  })
```



## ESLint

**ESLint 是JavaScript语法检查工具；**

**Airbnb 是一种编程风格；**

awesome-eslint eslint插件；

```powershell
cnpm install -g eslint  //安装eslint
eslint --init  //初始化eslint，生成配置文件.eslintrc
eslint yourfile.js   //在任何文件或目录上运行ESLint
```

#### **.eslintrc** 配置文件：

```json
{
    "rules": {
        "semi": ["error", "always"],
        "quotes": ["error", "double"]
    },
    "extends": "eslint:recommended"
}
```

`"semi"` 和 `"quotes"` 是 ESLint 中 [规则](http://eslint.cn/docs/rules) 的名称。第一个值是错误级别，可以使下面的值之一：

- `"off"` or `0` - 关闭规则
- `"warn"` or `1` - 将规则视为一个警告（不会影响退出码）
- `"error"` or `2` - 将规则视为一个错误 (退出码为1)

这三个错误级别可以允许你细粒度的控制 ESLint 是如何应用规则。[配置文件](http://eslint.cn/docs/user-guide/configuring)

"extends": "eslint:recommended"  表示遵循什么检测规则

http://eslint.cn/docs/user-guide/configuring



```javascript
行注释 禁用规则
/* eslint-disable no-new */
行注释 开启规则
/* eslint-enable no-new */
行注释 开启全局变量
/* global Vue */
```



**eslint插件**

https://github.com/dustinspecker/awesome-eslint

先安装插件，再在配置文件里面添加字段

 npm install --save-dev eslint-plugin-html`

```json
"plugins": [
   "vue","html"
]
```



## ES6模块

```shell
cnpm init  #生成package.json文件
```

```shell
cnpm install lodash --save  #安装依赖并写入package.json中的“dependencies”对象中, 会在根目录下生成一个node_modules文件夹，里面的子文件夹对应于每个依赖包
```

```shell
cnpm uninstall lodash --save  #卸载依赖
```

**--save**：将保存配置信息到pacjage.json的dependencies节点中。 这部分依赖是在运行项目代码前被运行

**--save-dev：**将保存配置信息到pacjage.json的devDependencies节点中。

**dependencies**：运行时的依赖，发布后，即生产环境下还需要用的模块

**devDependencies**：开发时的依赖。里面的模块是开发时用的，发布时用不到它。



在app.js中引入node_modules中的模块

直接引入模块，浏览器会报错，因为目前浏览器不支持es6模块，需要**第三方的打包工具**，如**webpack**，**SystemJS**

```shell
cnpm install webpack --save-dev  #安装webpack依赖
```

之后，还需要安装 Babel  依赖，将es6的语法编译为es5的语法，让浏览器认识

https://babeljs.io/en/setup/

```shell
cnpm install --save-dev babel-loader @babel/core  #安装Babel依赖
cnpm install --save-dev babel-preset-es2015  #支持es6语法的依赖
```

新建webpack.config.js文件

https://www.webpackjs.com/concepts/

```js
module.exports = {
  entry: './app.js', // 项目入口文件
  output: {
    filename: './dist/bundle.js'
  },
  module: {
    rules: [{
      test: /\.js$/,
      use: [{
        loader: 'babel-loader'
      }]
    }]
  },
  mode: 'development'
}
```

在package.json添加运行命令  定义 build 为 webpack --progress --watch  

--progress：监听运行进度

--watch：监听文件是否改动

```javascript
"scripts": {
    "build": "webpack --progress --watch"
  },
```

```shell
npm run build
```



#### *构建自己的es6模块

新建js文件，用export导出模块

在其他js文件中，用import进行模块导入

- 导出模块的方式有两种：

1. 默认导出/匿名导出：export default 
2. 命名导出：export

**一个文件默认导出的模块只能有一个**

```javascript
const code = 'code123';
export default code; // 默认导出(匿名导出)，一个文件只能有一个默认导出

export const apiKey = 'apiKey456'; // 命名导出，一个文件可以有多个命名导出
export const name = 'Tom';
export const age = 18;

// export {apiKey as Key, name, age} // 全部导出, as 可以重命名，导入模块时要有重命名之后的名称

export function getAge() {
  return age;
}
```

- 导入模块的方式有两种：

1. 默认导入：import   from
2. 命名导入：import { } from 

from 的文件后缀名可以省略

```javascript
import sscode from "./config.js"; //匿名导入，可以自定义模块名称
import { apiKey, name, getAge } from "./config.js"; //命名导入，必须用大括号且模块名称和模块导出时名称一致，多个命名模块导入时，用逗号隔开
```



#### **使用SystemJS打包es6模块**

直接在浏览器上运行，不需要像webpack那样结合npm进行依赖注入

jspm 是一个浏览器端包管理器，npm是本地包管理器



**browser-sync** 搭建本地服务器，利用npm安装，实现Apache这样的本地服务器

```shell
cnpm install browser-sync --save-dev
```

在package.json中添加运行命令

```json
"scripts": {
    "server": "browser-sync start --directory --server --files '*.html, *.js, *.css'"
  },
```

启动本地服务器

```shell
cnpm run server
```



**配置System JS**

1、配置systemjs的转义器  2、配置项目入口

使用SystemJS后不需要使用npm来下载包引入项目中，只需要在import的时候指明 from "npm:lodash" 即可



## Babel

https://babeljs.io/docs/en/

Babel是一种JS编译器，将es6的语法转换成es6之前的版本，让浏览器兼容es6语法

通过npm安装 Babel

```shell
npm install --save-dev @babel/core @babel/cli @babel/preset-env
npm install --save @babel/polyfill
```



https://segmentfault.com/a/1190000019718925

**@babel/core**：Babel的核心库，主要是进行代码转换的一些方法，可以将源代码根据配置转换成兼容目标环境的代码。

**@babel/cli** ：允许在终端使用Babel的脚手架工具， 用于命令行下**编译**源代码。

**@babel/plugin*** ：babel是通过插件来进行代码转换的，例如箭头函数使用`plugin-transform-arrow-functions`插件来进行转换。

**@babel/preset**：逐个导入插件很麻烦，presets是一组预设好的插件集合。官方为常见环境组装了一些 presets。

- **@babel/preset-env**
- @babel/preset-flow
- @babel/preset-react
- @babel/preset-typescript

Babel 把 Javascript 语法分为syntax 和 api， api 指那些我们可以通过 函数重新覆盖的语法 ，类似 from， map， includes， Promise， 凡是我们能想到重写的都可以归属到api。syntax 指像箭头函数，let，const，class， 依赖注入 Decorators等等这些，我们在 Javascript在运行是无法重写的，想象下，在不支持的浏览器里不管怎么样，你都用不了 let 这个关键字。

<u>@babel/presets默认只对syntax进行转换，我们需要使用 @babel/polyfill 来提供对api的的支持。</u>



**@babel/polyfill**：由 **core-js2** 和 **regenerator-runtime** 组成，前者是js标准库，包含不同版本javascipt语法的实现；后者是facebook开源库，用来实现对generator、async函数等的支持。

preset-env的配置项中的`useBuiltIns`属性可以方便@babel/polyfill的使用：

- `useBuiltIns:false(default)`:此时不对 `polyfill` 做操作。如果引入 `@babel/polyfill`，则无视配置的浏览器兼容，引入所有的 `polyfill`。
- `useBuiltIns:"entry"`:根据配置的浏览器兼容，引入浏览器不兼容的 `polyfill`。需要在入口文件手动添加 `import '@babel/polyfill'`，会自动根据 `browserslist` 替换成浏览器不兼容的所有 `polyfill`。
- `useBuiltIns:"usage"`:不需要在文件顶部手动引入@babel/polyfill，会根据代码中的使用进行按需添加。

polyfill 根据浏览器的 **user agent** 判断浏览器的版本



**@babel/runtime**：



在 package.json 文件中，添加命令

```json
"scripts": {
    "babel": "babel app.js --watch --out-file app-compiled.js"
  },
```

运行命令

```shell
cnpm run babel	
```

将app.js编译成es6之前的语法，输出到app-compiled.js文件



## 类class

类是一种特殊的函数 （typeof 可以验证）

定义类的两种方式：类的声明；类的表达式

```javascript
 // 类的声明
  class User {
    // 类的构造函数
    constructor(name, age) {
    	this.name = name;
    	this.age = age;
    }

    // 实例方法
    info() {
    	console.log('hello world');
    }

    // 静态方法
    static userDesc() {
    	console.log('userDesc hello');
    }

    // 定义属性的set方法
    set github(value) {
    	this.githubName = value;
    }

    // 定义属性的get方法
    get github() {
    	return this.githubName;
    }

  };

  const alice = new User('Alice', 18);

  // 类的表达式
  /*const Emplyee = class {
  	// 类的构造函数
    constructor(name, age) {
    	this.name = name;
    	this.age = age;
    }

    // 实例方法
    info() {
    	console.log('hello world');
    }
  };*/
```

**类的属性可以用计算属性进行定义**

```javascript
let methodName = 'info'

class User {
    // 类的构造函数
    constructor(name, age) {
    	this.name = name;
    	this.age = age;
    }

    // 实例方法
    [methodName]() {
    	console.log('hello world');
    }
 }
```

**类的继承**

```javascript

  // 类的继承
  class Animal {
    constructor(name) {
      this.name = name;
      this.belly = [];
    }

    eat(food) {
      this.belly.push(food);
    }
  }

  class Dog extends Animal {
    constructor(name, age) {
    	super(name);
    	this.age = age;
    }

    bark() {
    	console.log("wang wang")
    }
  }

  // ES5中实现继承
/*  function Animal(name) {
  	this.name = name;
    this.belly = [];
    this.eat = function(food) {
    	this.belly.push(food);
    }
  }
  function Dog(name, age) {
  	Animal.call(this, name, age);
  	this.name = name;
  	this.age = age;
  }
  Dog.prototype = new Animal();
  Dog.prototype.constructor = Dog();*/
```



## 迭代器 Iterator

可遍历对象是指有 [Symbol.iterator] 方法属性的一类对象。

 [Symbol.iterator] 方法返回一个 Iterator 迭代器对象， Iterator 迭代器对象有 next() 方法，该方法返回可遍历对象的元素和done组成的对象，done的值为布尔值，false表示当前不是最后一个元素，true表示遍历结束。

 **next() 方法返回的 value 是元素**

```javascript
const colors = ['red', 'blue'];
const iterator = colors[Symbol.iterator]();
iterator.next(); //{value: "red", done: false}
iterator.next(); //{value: "blue", done: false}
iterator.next(); //{value: undefined, done: true}
```

es6提供了新的方法返回迭代器对象，entries() 方法。该方法返回的迭代器与 [Symbol.iterator] () 方法返回的迭代器有差异。  差异在于 next() 方法返回的 value 不同。  **next() 方法返回的 value 是索引和元素组成的数组**

```javascript
const colors = ['red', 'blue'];
const iterator = colors.entries();
iterator.next(); //{value: [0, 'red'], done: false}
iterator.next(); //{value: [1, 'blue'], done: false}
iterator.next(); //{value: undefined, done: true}
```

es6还提供了新的方法返回迭代器对象，keys() 方法。 **next() 方法返回的 value 是索引**

```javascript
const colors = ['red', 'blue'];
const iterator = colors.keys();
iterator.next(); //{value: 0, done: false}
iterator.next(); //{value: 1, done: false}
iterator.next(); //{value: undefined, done: true}
```

**案例：实现next() 方法返回的 value 是元素**

```javascript
  Array.prototype.values = function() {
    let i = 0;
    let items = this;
    return {
      next() {
        const done = i >= items.length;
        const value = done ? undefined : items[i++];
        return {
          value,
          done
        }
      }
    }
  }

  const colors = ['red', 'blue'];

  const iterator = colors.values();
  iterator.next(); //{value: "red", done: false}
  iterator.next(); //{value: "blue", done: true}
  iterator.next(); //{value: undefined, done: true}
```

**for of 循环的底层就是从[Symbol.iterator] ()方法中获得遍历器，再取出value的值**



## 生成器 Generator 

生成器指的是某一类函数，这些函数返回迭代器对象

每执行一个 next() 方法，会将当前 yield 后面表达式的值传入 next() 返回对象的 value 属性中，等到下一次执行 next() 方法时，会将下一个的 yield 后面表达式的值传入 next() 返回对象的 value 属性中，依次下去。

执行当前 next() 方法时，会执行生成器函数里面上一个 yield 之后的代码，直到遇到下一个 yield 才停止。

如果给 next() 方法传入参数，则该参数将会作为当前  ‘yield + 表达式’  整体的值。

```javascript
function* listColors() {
	yield 'red';
	yield 'blue';
}

const colors = listColors();
colors.next(); // {value: "red", done: false}
colors.next(); // {value: "blue", done: false}
colors.next(); // {value: undefined, done: true}
```

**案例：生成器控制 ajax 工作流**

```javascript
  function myAjax(url) {
    axios
      .get(url)
      .then(res => userGen.next(res.data)) // 向next()方法传入参数
  }

  function* steps() {
    console.log("fetching users");
    const users = yield myAjax(`https://api.github.com/users`);
    console.log('users: ', users);

    console.log("fetching firstUser");
    const firstUser = yield myAjax(`https://api.github.com/users/${users[0].login}`);
    console.log('firstUser: ', firstUser);

    console.log("fetching followers");
    const followers = yield myAjax(firstUser.followers_url);
    console.log('followers', followers);
  }

  const userGen = steps(); // 返回生成器的迭代器
  userGen.next(); // 开启生成器
```

<img src="C:\Users\15740\AppData\Roaming\Typora\typora-user-images\image-20200225230239901.png" alt="image-20200225230239901" style="zoom:50%;" />

## 代理 Proxy

可以重写对象中默认的方法，检验对象中是否存在某个属性

new Proxy(target, handle)  target是需要重写的目标对象，handle是一个对象，包含我们需要重写的操作

```javascript
  const person = { name: 'Bob', age: 17 };

  const personProxy = new Proxy(person, {
    get(target, key) {
      return target[key].toUpperCase(); // 姓名大写
    },
    set(target, key, value) {
      if (typeof value === 'string') {
        target[key] = value.trim(); // 姓名去首尾空格
      }
    }
  })

  personProxy.name = '  alice'; // 前面有两个空格

  console.log(personProxy); // {name: "alice", age: 17}
  console.log(person); // {name: "alice", age: 17}
  console.log(person.name); // "alice"
  console.log(personProxy.name); // "ALICE"
```

**案例**

```javascript
  // 电话号码
  const phoneHandler = {
    set(target, key, value) {
      target[key] = value.match(/[0-9]/g)
        .join(''); // 设置phoneNumber的每个属性只能是数字组成
    },
    get(target, key) {
      return target[key].replace(/(\d{3})(\d{4})(\d{4})/, '$1-$2-$3');
    }
  };
  const phoneNumber = new Proxy({}, phoneHandler);
```



<img src="C:\Users\15740\AppData\Roaming\Typora\typora-user-images\image-20200226212450546.png" alt="image-20200226212450546" style="zoom:50%;" />

```javascript
// 处理不同用户输入的对象，属性不区分大小写
  const safetyHandler = {
    set(target, key, value) {
      const likeKey = Object.keys(target).find(item => item.toUpperCase() === key.toUpperCase());
      if (!(key in target) && likeKey) {
      	throw new Error(`${key} 属性不存在该对象中，但是有 ${key} 不区分大小写的属性 ${likeKey}`)
      }

      target[key] = value;
    }
  };
  const safetyProxy = new Proxy({ id: 2 }, safetyHandler);

  safetyProxy.ID = 5; // 报错
```

<img src="C:\Users\15740\AppData\Roaming\Typora\typora-user-images\image-20200226215421454.png" alt="image-20200226215421454" style="zoom:50%;" />

## Set 和 WeakSet

Set 集合：一种数据结构，元素无序且不能重复，无索引值。   数组是有序可重复的数据结构

forEach方法可以遍历set集合，并且传入的回调函数的形参依旧是三个，只不过item和index是一样的

**常用于数组去重**

```javascript
const colors = new Set();
  /*
  	add：添加元素，不可以重复；
  	delete：删除元素，返回布尔值；
  	size：set集合的大小；
  	has：判断集合中是否含某元素，返回布尔值；
  	clear：清空集合；
  	values：返回集合的迭代器；
  	for of 可以遍历set集合；
  	forEach 可以遍历set集合；
   */
  colors.add('red');
  colors.add('blue');
  colors.add('5');
  colors.add(5);
  colors.delete('blue');
  colors.has('5');
  colors.size;
  // colors.clear();
  const setIterator = colors.values();
  setIterator.next();

  for (let value of colors) {
    console.log(value);
  }

  colors.forEach((item, index, set) => {
    console.log(item, index, set)
  })

  const fruits = new Set(['apple', 'banana', 'mongo']);

  // 数组去重
  const numArr = [1, 2, 2, 3, 4, 5, 5, 5];
  const numSet = new Set(numArr); // {1,2,3,4,5}
  const numArr2 = [...numSet]; // [1,2,3,4,5]
```

**Weak Set**

与Set集合不同的点：

1、集合元素只能是对象；

2、不能通过 for of 和 foreach 循环迭代元素；

3、没有 clear 这个方法，但是它有自我清除的功能。

`WeakSet`持弱引用：集合中对象的引用为弱引用。 如果没有其他的对`WeakSet`中对象的引用，那么这些对象会被当成垃圾回收掉。 这也意味着WeakSet中没有存储当前对象的列表。 正因为这样，`WeakSet` 是不可枚举的。

```javascript
// Weak Set
 let jelly = {name: 'jelly', age: 17};
 let mary = {name: 'mary', age: 18};

 const weakPeople = new WeakSet([jelly, mary]);
 mary = null;
 console.log(weakPeople);
```

## Map 和 WeakMap

**`Map`** 对象保存键值对，并且能够记住键的原始插入顺序。**任何值(对象或者[原始值](https://developer.mozilla.org/en-US/docs/Glossary/Primitive)) 都可以作为一个键或一个值。**

Map集合的元素为键值对(两个元素的数组，例如: [[ 1, 'one' ],[ 2, 'two' ]])。 每个键值对都会添加到新的 Map。`null` 会被当做 `undefined。`    **元素的键不可重复，键所对应的值可以重复**

**Set集合和数组类似，Map集合和对象类似**

```javascript
  const people = new Map();

  /*
  	set：添加map元素
  	get：传入键名，返回对应的键值
  	size： 获取键值对的数量
  	has：判断某个键是否在map中
  	clear：清空
   */
  people.set('jelly', 23);
  people.set('mary', 24);
  people.set('jelly', 25); // 添加失败，元素的键不可以重复

  people.forEach((value, key, map) => {
    console.log(value, key, map)
  })

  for (let person of people) {
    console.log(person);
  }

  for (let [key, value] of people) {
    console.log([key, value]);
  }

  const fruits = new Map([['apple', 6], ['banana', 5]]);
```

<img src="C:\Users\15740\AppData\Roaming\Typora\typora-user-images\image-20200227195325840.png" alt="image-20200227195325840" style="zoom:67%;" />

**案例：记录button按钮点击的次数**

```javascript
const clickCounts = new Map();
  const buttons = document.querySelectorAll('button');

  buttons.forEach(button => {
    clickCounts.set(button, 0);
    button.addEventListener('click', function() {
      let val = clickCounts.get(this);
      clickCounts.set(this, ++val);
      console.log(clickCounts);
    })
  })
```

 **WeakMap要求键的类型必须是对象**

## async await

https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/async_function