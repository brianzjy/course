# CMD和AMD规范区别
---
## AMD 与 Require.js
#### AMD https://github.com/amdjs/amdjs-api/wiki/AMD
AMD规范定义了一个全局变量或者说是全局变量define的函数。
`define(id?,dependencies?,factory)`

AMD规范   
di?: id为字符串类型，表示模块标示，为可选参数。如果不存在则模块标识应该默认定义在加载器中被请求脚本标示。如果存在，那么模块标示必须为顶层的或者一个绝对标识。
dependencies?: 是一个当前模块依赖的，已被模块定义的模块标识数组字面量。
factory: 是一个需要进行实例化的函数或者一个对象。

创建模块标识为 alpha 的模块，依赖于 require， export，和标识为 beta 的模块  
```
define("alpha", [ "require", "exports", "beta" ], function( require, exports, beta ){
    export.verb = function(){
        return beta.verb();
        // or:
        return require("beta").verb();
    }
});
```

一个返回对象字面量的异步模块
```
define(["alpha"], function( alpha ){
    return {
        verb : function(){
            return alpha.verb() + 1 ;
        }
    }
});
```

无依赖模块可以直接使用对象字面量来定义
```
define( {
    add : function( x, y ){
        return x + y ;
    }
} );
```


RequireJS   
RequireJS 是一个前端的模块化管理的工具库，遵循AMD规范，它的作者就是AMD规范的创始人 James Burke。所以说RequireJS是对AMD规范的阐述一点也不为过。

RequireJS 的基本思想为：通过一个函数来将所有所需要的或者说所依赖的模块实现装载进来，然后返回一个新的函数（模块），我们所有的关于新模块的业务代码都在这个函数内部操作，其内部也可无限制的使用已经加载进来的以来的模块。

引入
```
<script data-main='scripts/main' src='scripts/require.js'></script>
```

###### 注意：defined用于定义模块，RequireJS要求每个模块均放在独立的文件之中。按照是否有依赖其他模块的情况分为独立模块和非独立模块。


1、独立模块，不依赖其他模块。直接定义：
```
define({
    method1: function(){},
    method2: function(){}
});
```
等价与
```
define(function(){
    return{
        method1: function(){},
        method2: function(){}
    }
});
```
2、非独立模块，对其他模块有依赖。
```
define([ 'module1', 'module2' ], function(m1, m2){
    ...
});
```
或者
```
define( function( require ){
    var m1 = require( 'module1' ),
          m2 = require( 'module2' );
    ...
});
```

#####  require方法调用模块
在require进行调用模块时，其参数与define类似。
```
require( ['foo', 'bar'], function( foo, bar ){
    foo.func();
    bar.func();
} );
```
在加载 foo 与 bar 两个模块之后执行回调函数实现具体过程。

当然还可以如之前的例子中的，在define定义模块内部进行require调用模块
```
define( function( require ){
    var m1 = require( 'module1' ),
          m2 = require( 'module2' );
    ...
});
```

## CMD 与 seaJS.js
#### CMD     
全局函数define，用来定义模块。
参数 factory  可以是一个函数，也可以为对象或者字符串。
当 factory 为对象、字符串时，表示模块的接口就是该对象、字符串。


定义JSON数据模块：

`define({ "foo": "bar" });`

通过字符串定义模板模块：

`define('this is {{data}}.');`

factory 为函数的时候，表示模块的构造方法，执行构造方法便可以得到模块向外提供的接口。

```
define( function(require, exports, module) {
    // 模块代码
});
```

####` define( id?, deps?, factory );`
define也可以接受两个以上的参数，字符串id为模块标识，数组deps为模块依赖：
```
define( 'module', ['module1', 'module2'], function( require, exports, module ){
    // 模块代码
} );
```
require 是 factory 的第一个参数。
require( id );
接受模块标识作为唯一的参数，用来获取其他模块提供的接口：
```
define(function( require, exports ){
    var a = require('./a');
    a.doSomething();
});
```

exports 是 factory 的第二个参数，用来向外提供模块接口。
```
define(function( require, exports ){
    exports.foo = 'bar'; // 向外提供的属性
    exports.do = function(){}; // 向外提供的方法
});
```

当然也可以使用 return 直接向外提供接口。
```
define(function( require, exports ){
    return{
        foo : 'bar', // 向外提供的属性
        do : function(){} // 向外提供的方法
    }
});
```

#### seaJS
```
// 加载一个模块
seajs.use('./a');
// 加载模块，加载完成时执行回调
seajs.use('./a'，function(a){
    a.doSomething();
});
// 加载多个模块执行回调
seajs.use(['./a','./b']，function(a , b){
    a.doSomething();
    b.doSomething();
});
```



##区别
```
// CMD
define(function(require, exports, module) {
    var a = require('./a')
    a.doSomething()
    // 此处略去 100 行
    var b = require('./b') // 依赖可以就近书写
    b.doSomething()
    // ...
})

// AMD 默认推荐的是
define(['./a', './b'], function(a, b) { // 依赖必须一开始就写好
    a.doSomething()
    // 此处略去 100 行
    b.doSomething()
    // ...
})
```
