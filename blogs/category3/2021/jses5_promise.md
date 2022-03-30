---
title: JavaScript基础知识(Promise)
date: 2021-01-30
tags:
 - JavaScript
categories:
 -  JavaScript
---
# promise基本使用
```javascript
Promise是ES6中新增的一个对象，
通过Promise就可以实现用同步的流程来表示异步的操作通过Promise就可以避免回调函数层层嵌套(回调地狱)问题

2.如何创建Promise对象?
new Promise(function(resolve，reject){});
promise对象不是异步的，只要创建promise对象就会立即执行存放的代码

3.Promise是如何实现通过同步的流程来表示异步的操作的?
promise对象是通过状态的改变来实现的，只要状态发生改变就会自动触发对应的函数

4.Promise对象三种状态
pending:默认状态，只要没有告诉promise任务是成功还是失败就是pending状态
fulfilled(resolved):只要调用resolve函数，状态就会变为fulfilled,表示操作成功
rejected:只要调用rejected函数，状态就会变为rejected，表示操作失败
注意点:状态一旦改变既不可逆，既从pending变为fulfilled,那么永远都是fulfilled
                          既从pending变为rejected，那么永远都是rejected

5.监听Promise状态改变
我们还可以通过函数来监听状态的变化
resolved -->then()
rejected -->catch()
```
# promis-then
```javascript
<script type="text/javascript">
    /*
    0.then方法
    then方法接收两个参数,
    第一个参数是状态切换为成功时的回调
    第二个参数是状态切换为失败时的回调
    */
    // let  promise = new Promise(function(resolve, reject){
    //     // resolve("111");
    //     reject("222");
    // });
    //
    // promise.then(function (date) {
    //     console.log("成功",date);
    // },function (date) {
    //     console.log("失败",date);
    // });

    /*
    1.then方法
    在修改promise状态时，可以传递参数给then方法中的回到函数
    */

    /*
    2.
    同一个promise对象可以多次调用then方法,
    当该promise对象的状态时所有then方法都会被执行

    3.
    then方法每次执行完毕后会返回一个新的promise对象

    4.
    可以通过上一个promise对象的then方法给下一个promise对象的then方法传递
    注意点:
    无论是在上一个promise对象成功的回调还是失败的回调传递的参数，
    都会传递给下一个promise对象成功的回调
     */

    let  promise = new Promise(function(resolve, reject){
        resolve("111");
        // reject("222");
    });

    let  ppp = new Promise(function(resolve, reject){
        // resolve("2222222");
        reject("bbbbbb");
    });

    let p2 = promise.then(function (date) {
        console.log("成功1",date);
        return ppp;
    },function (date) {
        console.log("失败1",date);
        return ppp;
    });

    p2.then(function (date) {
        console.log("成功2",date);
    },function (date) {
        console.log("失败2",date);
    })
</script>
```

# promis-catch
```javascript
/*
0.catch方法
catch 其实是 then(undefined,() => {} ) 的语法糖



1.如果promise的状态是失败，但是没有对应失败的监听就会报错
2.then方法会返回一个新的promise，新的promise会继承原有promise的状态
3.如果新的promise状态是失败，但是没有对应失败的监听也会报错

 */
let promise = new Promise(function (resolve, reject) {
    // resolve();
    reject();
});

promise.then(function () {
    console.log("成功");
}).catch(function () {
    console.log("失败");
});
/*
1.catch方法
和then一样，在修改promise状态时，可以传递参数给catch方法中的回到函数

2.
和then一样，同一个promise对象可以多次调用catch方法,
当该promise对象的状态时所有catch方法都会被执行

3.
和then一样, catch方法每次执行完毕后会返回一个新的promise对象

 4.
 和then方法一样，上一个promise对象也可以给下一个promise成功的传递参数注意点:
无论是在上一个promise对象成功的回调还是失败的回调传递的参数，都会传递给下一个promise对象成功的回调

5.
和then一样,catch方法如果返回的是一个Promise对象，那么会将返回的Promise对象的执行结果中的值传递给下一个catch方法

6.
和then方法第二个参数的区别在于, catch方法可以捕获上一个promise对象then方法中的异常
 */
let p3 = new Promise(function (resolve,reject) {
    resolve();
});

p3.then(function (){
    console.log("成功");
    XXXX
}).catch(function (e) {
    console.log("失败",e);
});
```
# 异常处理
```javascript
/*
1.JS中的异常
    简单粗暴就是有错误出现
    由于JS是单线程的,编写的代码都是串行的，
    所以一旦前面代码出现错误,程序就会被中断，后续代码就不会被执行

2.JS中的异常处理
    2.1自身编写代码问题,-->手动修复BUG
    2.2外界原因问题,-->try{}catch{}
    对于一些可预见的异常，我们可以使用try{}catch{}来处理,

3.JS中如何进行异常处理
利用try{}catch{}来处理异常可以保证程序不被中断，也可以记录错误原因以便于后续优化迭代更新
try {
    可能遇到的意外的代码
}catch(e){
    捕获错误代码块
}
 */

try {
    console.log("...");
    say();
    console.log("???");
}catch (e) {
    console.log(e);
}
```
# 手写Promise
```javascript
<script>
    //定义常量保存对象状态
    const PENDING = "pending";
    const FULFILLED = "fulfilled";
    const REJECTED = "rejected";

    class MyPromise{
        constructor(handle){
            //初始化默认的状态
            this.status = PENDING;
            //保持传入的参数
            this.value = undefined;
            this.reason = undefined;
            //保存监听的函数
            // this.onResolvedCallback = null;
            // this.onRejectedCallback = null;
            this.onResolvedCallbacks = [];
            this.onRejectedCallbacks = [];
            //1.判断是否传入了一个函数，否则抛出异常
            if (!this._isFunction(handle)){
                throw new Error("请传入一个函数");
            }
            //给传入的函数传递两个形参
            handle(this._resolve.bind(this),this._reject.bind(this));

        }

        then(onResolved,onRejected){
            // then() 每次执行都会返回一个新的Promise对象
            return new MyPromise((nextResolve,nextReject) => {
                //判断有没有传入成功的回调
                if(this._isFunction(onResolved)){
                    //判断当前的状态是否是成功的状态
                    if(this.status == FULFILLED){
                        try {
                            //拿到上一个promise成功的回调执行结果
                            let result = onResolved(this.value);
                            //判断执行的结果是否是一个promise对象
                            if (result instanceof MyPromise){
                                //执行完把下一个成功/失败的回调给他
                                result.then(nextResolve,nextReject);
                            }else {
                                //将上一个promise成功回调的结果传递给下一个promise成功的回调
                                nextResolve(result);
                            }
                        }catch (e) {
                            nextResolve(e);
                        }
                    }
                }
                //判断有没有传入失败的回调
                // if (this._isFunction(onRejected)){
                    try {
                        if (this.status == REJECTED){
                            let result = onRejected(this.reason);
                            if (result instanceof MyPromise){
                                result.then(nextResolve,nextReject);
                            }else if(result !== undefined){
                                nextReject(result);
                            }else {
                                nextReject();
                            }
                        }
                    }catch (e) {
                        nextReject(e);
                    }
                // }
                //判断当前的状态是否是默认状态
                if(this.status == PENDING){
                    if(this._isFunction(onResolved)){
                        // this.onResolvedCallback = onResolved;
                        this.onResolvedCallbacks.push( () => {
                            try {
                                let result = onResolved(this.value);
                                if (result instanceof MyPromise){
                                    result.then(nextResolve,nextReject);
                                }else if (result !== undefined){
                                    nextResolve(result);
                                }else {
                                    nextResolve();
                                }
                            }catch (e) {
                                nextResolve(e);
                            }

                        });
                    }
                    // if (this._isFunction(onRejected)){
                        // this.onRejectedCallback = onRejected;
                        this.onRejectedCallbacks.push( () => {
                            try {
                                let result = onRejected(this.reason);
                                if (result instanceof MyPromise){
                                    result.then(nextResolve,nextReject);
                                }else if(result != undefined){
                                    nextReject(result);
                                }else {
                                    nextReject();
                                }
                            }catch (e) {
                                nextReject(e);
                            }
                        });
                    // }
                }
            });
        }

        catch(onReject){
            return this.then(undefined,onReject);
        }

        _resolve(value){
            //防止重复修改
            if (this.status == PENDING){
                this.status = FULFILLED;
                this.value = value;
                // this.onResolvedCallback(this.value);
                this.onResolvedCallbacks.forEach( fn => fn(this.value));
            };

            // console.log("_resolve");
        }
        _reject(reason){
            if (this.status == PENDING){
                this.status = REJECTED;
                this.reason = reason;
                // this.onRejectedCallback(this.reason);
                this.onRejectedCallbacks.forEach( fn => fn(this.reason));
            };

        }
        _isFunction(fn){
            return typeof fn === "function";
        }
    }
</script>

<script type="text/javascript">
    /*
    1.Promise特点
    1.1创建时必须传入一个函数，否则会报错
    1.2会给传入的函数设置两个回调函数
    1.3刚创建的Promise对象状态是pending
    1.4状态一旦发生改变就不可再次改变1.5可以通过then来监听状态的改变
    1.5.1如果添加监听时状态已经改变，立即执行监听的回调
    1.5.2如果添加监听时状态还未改变，那么状态改变时候再执行监听回到
    1.5.3同一个Promise对象可以添加多个then监听，状态改变时所有的监听按照添加顺序执行
     */
    let promise = new MyPromise(function (resolve,reject) {
        // console.log(resolve);
        setTimeout(function (){
            resolve("传参aaa");
            // reject("bbb");
        },2000);
        // resolve("aaaa");
        // reject("bbb");
    });
    let ppp = new MyPromise(function (resolve,reject) {
        reject("ppp数据");
    })
    // console.log(promise);
    let p1 = promise.then(function (data){
        console.log("成功1",data);
        return ppp;
    },function (data){
        console.log("失败",data);
        return "BBBB";
    });
    p1.then(function (data){
        console.log("成功2",data);
    },function (data){
        console.log("失败",data);
    });
</script>
```
# promise-all()方法
```javascript
/*
1.Promise的all静态方法特点
1.1 all方法会返回一个新的Promise对象
1.2 会按照传入数组的顺序将所有Promise中成功返回的结果保存到一个新的数组返回
1.3 数组中有一个Promise失败就会失败，只有所有成功才会成功
 */
  static all(list) {
        return new MyPromise(function (resolve, reject) {
            let arr = [];
            let count = 0;
            for (let i = 0; i < list.length; i++) {
                let p = list[i];
                p.then(function (value) {
                    arr.push(value);
                    count++;
                    if (list.length == count){
                        //遍历了所有数组，返回arr
                        resolve(arr);
                    }
                }).catch(function (e) {
                    reject(e);
                });
            }
        });
    }
}
```
# promise-race()方法
```javascript
static race(list){
    return new MyPromise(function (resolve,reject) {
        for (let p of list){
            p.then(function (value) {
                reverse(value)
            }).catch(function (e) {
                reject(e)
            });
        }
    })
}
```
# fetch()
```javascript
/*
1.什么是fetcl
和Ajax一样都是用于请求网络数据的
fetch是ES6中新增的，基于Promise的网络请求方法
2.fetch基本使用
fetch(url,{options})
.then()
.catch();

 http://127.0.0.1/jQuery/Ajax/41.php
 */
fetch("http://127.0.0.1/Ajax/41.php?teacger=lj&age=18",{
    method:"get",
    // method:"post",
    // post传参
    body:JSON.stringify({teacher:lj,age:18})
}).then(function (res) {
    // console.log(res.text());
    // return res.text();
    return res.json();
}).then(function (value) {
    console.log(value);
}).catch(function (e) {
    console.log(e);
})
```
# axios
```javascript
/*
1.什么是axios?
Axias是一个基于 promise的 HTTP库网络请求插件
2.axios特点
2.1可以用在浏览器和 node.js中
2.2支持 Promise API
2.3自动转换JSON数据
2.4客户端支持防御XSRE
 */

// axios.get("http://127.0.0.1/Ajax/41.php")
axios.post("http://127.0.0.1/Ajax/41.php",{
    name:lj,age:18
})
    .thead(function (res) {
        res.data
    })
    .catch(function (e) {
        console.log(e);
    })
    
//请求超时时间
axios.defaults.timeout = 2000;
//全局URL根地址
axios.defaults.baseURL = "http://127.0.0.1"
ajax.post("/ajax/41.php",{})
```
# Generator函数
```javascript
/*
1.什么是Generator函数?
Generator 函数是 ES6提供的一种异步编程解决方案
Generator函数内部可以封装多个状态，因此又可以理解为是一个状态机

2.如何定义Generator函数
只需要在普通函数的function后面加上*即可

3.Generator函数和普通函数区别
    3.1调用Generator函数后，无论函数有没有返回值，都会返回一个迭代器对象
    3.2调用Generator函数后,函数中封装的代码不会立即被执行
    4.真正让Generator具有价值的是yield关键字
    4.1在Generator函数内部使用yield关键字定义状态
    4.2并且yield关键字可以让Generator内部的逻辑能够切割成多个部分。
    4.3通过调用迭代器对象的next方法执行一个部分代码,

5.在调用next方法的时候可以传递一个参数，这个参数会传递给上一个yield
 */
function* gen() {
    console.log("say");
    yield "say";

    console.log("true");
    let res = yield "ccc";
    console.log(res);
    yield true;

    console.log("1*3");
    yield 1*3;
}

let it = gen();
console.log(it.next());
console.log(it.next("n2"));
console.log(it.next());
```

