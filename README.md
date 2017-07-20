# good_url

实用的连接 

http://www.shouce.ren/   手册网

http://git.oschina.net/  码云   可以下载开源项目

http://ourjs.com/userinfo/ourjs

http://ourjs.com/detail/572160b688feaf2d031d24e4 面试

https://www.zhihu.com/question/19568008 如何面试前端工程师?

https://github.com/oneuijs/You-Dont-Need-jQuery/blob/master/README.md You-Dont-Need-jQuery

------
# ubuntu中的命令
### 1. 全局安装的包目录在 /usr/local/nodejs/lib/node_modules/ 下边
### 2. /usr/local/bin 环境变量的配置
### 3. cp xxx.js xxx.js 目标目录  复制文件到目标目录
### 4. rm -rf xxx.js 删除文件
### 5. mv xxx.js 目标目录  移动文件到目标目录


## 全局安装的模块在linux中找不到的解决方案
eg:npm install gulp -g 
这时找到gulp的安装路劲　制作一个软链
sudo ln -s /usr/local/nodejs/lib/node_modules/gulp/bin/gulp.js /usr/sbin/gulp
如果gulp存在　sudo rm /usr/bin/gulp
------

# module.exports和exports的区别

## node 控制台直接打印console.log(module)

```javascript

{
  id: '.',
  exports: {},
  parent: null,
  filename: '/home/sa/Desktop/node_study/module/module.js',
  loaded: false,
  children: [],
  paths:[ '/home/sa/Desktop/node_study/module/node_modules',
     '/home/sa/Desktop/node_study/node_modules',
     '/home/sa/Desktop/node_modules',
     '/home/sa/node_modules',
     '/home/node_modules'] 
 }
 
```
## node 控制台直接打印console.log(exports)

```javascript

{}

```
## 在来看个例子

```javascript

var a = { name : 1 };
var b = a;
console.log(a) //{name:1}
console.log(b) //{name:1}

b.name = 2;
console.log(a) //{name:2}
console.log(b) //{name:2}

var b = {name:3};
console.log(a) //{name:2}
console.log(b) //{name:3}

```

#### 解释：a 是一个对象，b 是对 a 的引用，即 a 和 b 指向同一块内存，所以前两个输出一样。当对 b 作修改时，即 a 和 b 指向同一块内存地址的内容发生了改变，所以 a 也会体现出来，所以第三四个输出一样。当 b 被覆盖时，b 指向了一块新的内存，a 还是指向原来的内存，所以最后两个输出不一样。

#### 明白了上述例子后，我们只需知道三点就知道 exports 和 module.exports 的区别了：

> * module.exports 初始值为一个空对象 {}
> * exports 是指向的 module.exports 的引用
> * require() 返回的是 module.exports 而不是 export

### 结论

#### 1. exports = module.exports = { } 
#### 2. exports = { } 将会指向一块新的内存，不能用这种方式导出模块
#### 3. exports.xxx = { } 将会指向同一块内存，可以用这种方式导出模块
#### 4. 如果用第二种方式导出模块，最后加上 module.exports = exports 重新赋值，一样可以达到导出模块的效果

------

# apply call bind 用法
apply call bind 都是用来改变上下文的this指向的 
## 不同点
call和apply一样，直接引用在方法上，而bind绑定this后返回一个方法，但内部核心还是apply。
## 例子
```
    var obj = {
      a: 1,
      b: 2,
      getCount: function(c, d) {
        return this.a + this.b + c + d;
      }
    };
    window.a = window.b = 0;
    console.log(obj.getCount(3, 4));  // 10
    var func = obj.getCount;
    console.log(func(3, 4)); //7
``` 
上面为何会这样？因为func在上下文中的this是window，bind的存在正是为了改变this指向获取想要的值：
```
    var obj = {
      a: 1,
      b: 2,
      getCount: function(c, d) {
        return this.a + this.b + c + d;
      }
    };

    window.a = window.b = 0;
    var func = obj.getCount.bind(obj);
    console.log(func(3, 4));  // 10
```
- [x] 上面说明：bind是function的一个函数扩展方法，bind以后代码重新绑定了func内部的this指向 `（getCount中的this->bind的obj）`

## 兼容写法
```
    var obj = {
      a: 1,
      b: 2,
      getCount: function(c, d) {
        return this.a + this.b + c + d;
      }
    };

    Function.prototype.bind = Function.prototype.bind || function(context) {
      var that = this;
      return function() {
        // console.log(arguments); // console [3,4] if ie<6-8>
        return that.apply(context, arguments);

      }
    }
    window.a = window.b = 0;
    var func = obj.getCount.bind(obj);
    console.log(func(3, 4));  // 10
```
其实在我看来bind的核心是返回一个未执行的方法，如果直接使用apply或者call：
```
    var ans = obj.getCount.call(obj,3,4);
    console.log(ans); // 10
    或
    var ans = obj.getCount.apply(obj, [3, 4]);
    console.log(ans); // 10
```   
无法使用简写的func函数构造，所以用bind传递this指向，再返回一个未执行的方法，实现方式相当巧妙。


