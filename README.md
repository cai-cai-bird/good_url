# good_url

实用的连接 

http://www.shouce.ren/   手册网

http://git.oschina.net/  码云   可以下载开源项目

http://ourjs.com/userinfo/ourjs

http://ourjs.com/detail/572160b688feaf2d031d24e4 面试

# apply call bind
apply call bind 都是用来改变上下文的this指向的 
## 不同点
call和apply一样，直接引用在方法上，而bind绑定this后返回一个方法，但内部核心还是apply。
## 例子

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
    
为何会这样？因为func在上下文中的this是window，bind的存在正是为了改变this指向获取想要的值：

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

bind是function的一个函数扩展方法，bind以后代码重新绑定了func内部的this指向（getCount中的this->bind的obj）

## 兼容写法

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
    
其实在我看来bind的核心是返回一个未执行的方法，如果直接使用apply或者call：

    var ans = obj.getCount.call(obj,3,4);
    console.log(ans); // 10
    或
    var ans = obj.getCount.apply(obj, [3, 4]);
    console.log(ans); // 10
    
无法使用简写的func函数构造，所以用bind传递this指向，再返回一个未执行的方法，实现方式相当巧妙。


