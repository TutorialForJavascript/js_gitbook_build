
# log工具

由于js在设计之初是作为浏览器脚本的,因此还是预留了调试用的log模块`console`的.但它是不完备的,它无法为log设定显示等级,也无法将log输出至文件或者其他媒介.个人认为输出至其他媒介并不是log模块本身最关心的,但没法设置log的显示等级是非常不现代的.因此这边推荐全平台的log工具[pino](https://www.npmjs.com/package/pino).不过本文还是只讲标准的log生成对象`console`

还是得吐槽下js的简陋,连log工具功能都不齐.

## console对象

`console`是指的控制台,js是单线程的,因此它的log其实和python中的`print`差不多,就是将文本信息输出到特定位置而已,在浏览器中就是控制台,在node.js中就是terminal的标准输入输出.

### log分级

实际上标准的log是有分级的,标准是4个等级:


```javascript
console.log('文字信息')
```

    文字信息



```javascript
console.info('提示信息')
```

    提示信息



```javascript
console.warn('警告信息')
```

    警告信息



```javascript
console.error('错误信息')
```

    错误信息


#### log的格式化输出

占位符	|含义
---|---
`%s`|符串输出
`%d or %i`|整数输出
`%f`|浮点数输出
`%o`|打印javascript对象，可以是整数、字符串以及JSON数据


### 分组输出

所谓是分组其实就是缩进.分组可以多层嵌套

使用`console.group()`和`console.groupEnd()`包裹分组内容.

还可以使用`console.groupCollapsed()`来代替`console.group()`生成折叠的分组.

这种用法有点像python中上下文的感觉.



```javascript
console.group('第一个组')
console.log("1-1")
console.log("1-2")
console.log("1-3")
console.groupEnd()

console.groupCollapsed('第二个组')
console.log("2-1")
console.log("2-2")
console.log("2-3")
console.groupEnd()
```

    第一个组
      1-1
      1-2
      1-3
    第二个组
      2-1
      2-2
      2-3


### 表格输出

使用console.table()可以将传入的对象,Map或数组以表格形式输出,这个函数适合输出格式化数据


```javascript
console.table({
    a:1,
    b:2
})
```

    ┌─────────┬────────┐
    │ (index) │ Values │
    ├─────────┼────────┤
    │    a    │   1    │
    │    b    │   2    │
    └─────────┴────────┘



```javascript
console.table([1,2,3,4])
```

    ┌─────────┬────────┐
    │ (index) │ Values │
    ├─────────┼────────┤
    │    0    │   1    │
    │    1    │   2    │
    │    2    │   3    │
    │    3    │   4    │
    └─────────┴────────┘



```javascript
console.table(new Map([['one',1], ['two', 2], ['three', 3]]))
```

    ┌───────────────────┬─────────┬────────┐
    │ (iteration index) │   Key   │ Values │
    ├───────────────────┼─────────┼────────┤
    │         0         │  'one'  │   1    │
    │         1         │  'two'  │   2    │
    │         2         │ 'three' │   3    │
    └───────────────────┴─────────┴────────┘



```javascript
console.table(new Set(["a","b","c"]))
```

    ┌───────────────────┬────────┐
    │ (iteration index) │ Values │
    ├───────────────────┼────────┤
    │         0         │  'a'   │
    │         1         │  'b'   │
    │         2         │  'c'   │
    └───────────────────┴────────┘


### 查看对象

使用Console.dir()显示一个对象的所有属性和方法,这个就有点像python中的dir(obj)


```javascript
console.dir({
    a:1,
    b:2,
    c:()=>1
})
```

    { a: 1, b: 2, c: [Function: c] }


### *查看dom节点

`console.dirxml()`这个接口算是浏览器脚本时代遗留的特有接口,用于查看`html/xml`生成的dom节点


```javascript
console.dirxml(`<ul id="box">
  <li>蚂蚁部落一</li>
  <li>蚂蚁部落二</li>
  <li>蚂蚁部落三</li>
  <li>蚂蚁部落四</li>
</ul>`)
```

    <ul id="box">
      <li>蚂蚁部落一</li>
      <li>蚂蚁部落二</li>
      <li>蚂蚁部落三</li>
      <li>蚂蚁部落四</li>
    </ul>


### 条件输出

`console.assert(exp,msg)`这个接口在一定程度上式assert的替代


```javascript
console.assert(true, "你永远看不见我")
```


```javascript
console.assert(false, "你永远看不见我")
```

    Assertion failed: 你永远看不见我


### 计次输出

使用`console.count(tag)`输出内容和被调用的次数可以使用`console.countReset(tag)`重置被调用的次数


```javascript
(()=> {
    for(let i = 0; i < 3; i++){
        console.count("运行次数：")
    }
    console.count("运行次数1：")
})()
```

    运行次数：: 1
    运行次数：: 2
    运行次数：: 3
    运行次数1：: 1


### 追踪调用堆栈

使用Console.trace()来追踪函数被调用的过程,在复杂项目时调用过程非常多,用这个命令可以查看到栈上的信息


```javascript
const add=(a, b)=> {
    console.trace("Add function")
    return a + b
}

const add3=(a, b)=> {
    return add2(a, b)
}

const add2=(a, b)=> {
    return add1(a, b)
}

const add1=(a, b) =>{
    return add(a, b)
}

let x = add3(1, 1)
```

    [object Object]
        at add (evalmachine.<anonymous>:2:13)
        at add1 (evalmachine.<anonymous>:15:12)
        at add2 (evalmachine.<anonymous>:11:12)
        at add3 (evalmachine.<anonymous>:7:12)
        at evalmachine.<anonymous>:18:9
        at Script.runInThisContext (vm.js:91:20)
        at Object.runInThisContext (vm.js:298:38)
        at run ([eval]:1002:15)
        at onRunRequest ([eval]:829:18)
        at onMessage ([eval]:789:13)


### 计时功能

使用`console.time(tag)`和`console.timeEnd(tag)`包裹需要计时的代码片段,输出运行这段代码的运行时间(毫秒记).一组time上下文使用tag参数做标识,一个tag就是一个计时器,最多同时运行10000个计时器.在其中可以插入`console.timeLog(tag, value)`



```javascript
console.time("Chrome中循环1000次的时间")
for(var i = 0; i < 10; i++)
{
console.timeLog("Chrome中循环1000次的时间", i)
}
console.timeEnd("Chrome中循环1000次的时间")
```

    Chrome中循环1000次的时间: 0.016ms 0
    Chrome中循环1000次的时间: 0.159ms 1
    Chrome中循环1000次的时间: 0.223ms 2
    Chrome中循环1000次的时间: 0.278ms 3
    Chrome中循环1000次的时间: 0.338ms 4
    Chrome中循环1000次的时间: 0.389ms 5
    Chrome中循环1000次的时间: 0.505ms 6
    Chrome中循环1000次的时间: 0.579ms 7
    Chrome中循环1000次的时间: 0.638ms 8
    Chrome中循环1000次的时间: 0.694ms 9
    Chrome中循环1000次的时间: 0.750ms


### 性能分析

使用`console.profile()`和`console.profileEnd()`进行性能分析,查看代码各部分运行消耗的时间.这个方法在node中需要使用`--inspect`标签打开调试模式才可以使用
