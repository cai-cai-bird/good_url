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
    console.log(func(3, 4));






