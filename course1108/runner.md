# 看懂runner
#### promise 和 generator 结合使用完成异步加载，以同步的方式呈现出来。以防止进入回调地狱。

#### runner 代码  
```
function runner(_gen){
  return new Promise((resolve, reject)=>{                     //主要用于导出数值，抛出错误
  var gen=_gen();                                             //参数对象重新声明
    _next();                                                  //函数调用
    function _next(_last_res){
    var res=gen.next(_last_res);                              //默认执行一次到第一个yeild结束，并把第一个执行函数的结果，赋值给obj。改处用于 执行 gengrator
                                                              // res{value：vvvv, done: false}
      if(!res.done){                                  //如果没执行完
        var obj=res.value;                                  // 上一次执行完res的value的结果，作为一个对像

        if(obj.then){                                       //如果OBJ有then方法。---------------promise 才会有then方法
          obj.then((res)=>{                                            //把上次执行完的，
            _next(res);
          }, (err)=>{
            reject(err);
          });
        }else if(typeof obj=='function'){                   //如果obj是个函数  如何处理
          if(obj.constructor.toString().startsWith('function GeneratorFunction()')){
            runner(obj).then(res=>_next(res), reject);      //如果是generator函数  继续调用runner主函数
          }else{
            _next(obj());
          }
        }else{                                              //既不是函数generator也不是promise对象 直接触发下一次
          _next(obj);
        }
      }else{                                           //执行完了，执行成功的值
        resolve(res.value);
      }
    }
  });
}
```
---
#### promise代码案例
```
Promise.all([
  $.ajax({url:'data/a.txt',dataType:'json'}) ,
  $.ajax({url:'data/b.txt',dataType:'json'}) ,
  $.ajax({url:'data/cd.txt',dataType:'json'}) ,
]).then(arr=>{
    let [data1,data2,data3] = arr;
    alert('成功了');
    alert(data1);
    alert(data2);
    alert(data3);
  },err=>{
    alert('出错了'+err);
    console.log(err);
})
```

#### GeneratorFunction
```
function *show() {
  alert('123');
  yield 12;
  alert('456');
  return 5;
}
let a = show();
let res1 = a.next();
console.log(res1);
let res2 = a.next();
console.log(res2);
```

#### 个人写的简单runner，通用性不强，方便理解runner
```
function getData(url) {
  return new Promise((resolve,reject)=>{
    $.ajax({
      url,
      dataType:'json',
      success(data){
        resolve(data)
      },
      err(error){
        reject(error)
      }
    })
  })
};
function run(fn) {
  let gen = fn();
  function next(predata) {
    gen.next(predata).value.then(nowdata=>{
      next(nowdata)
    })
  }
  next(null);
}

run(function *() {
  console.log('1');
  let data1 = yield getData('data/a.txt');
  console.log(data1);
  let data2 = yield getData('data/b.txt');
  console.log(data2);
})
```
