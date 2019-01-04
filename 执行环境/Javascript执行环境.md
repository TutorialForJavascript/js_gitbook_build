
# Javascript环境搭建


由于历史原因Javascript的运行环境有两种:

+ 浏览器

    浏览器环境复杂,一般通过babel转码的形式间接地使用ES6特性

+ node.js(v8虚拟机)

    v8引擎原生的支持一些ES6特性,但在有的时候也不得不通过使用Babel转码来实现一些特定的语法.使用node.js的原生ES6支持需要在使用node命令时后面带上`--harmony`参数,方便起见可以在环境变量中直接使用`alias node=node --harmony`语句,这样就可以在repl中使用ES6支持了,但如果要执行脚本,必须开启js的严格模式,也就是在每个脚本头上写上`"use strict"`

他们之间多数时候语法层面是通用的.

## npm

npm是node.js的官方包管理工具,它可以用于全局环境的搭建,也可以用于单独项目的环境搭建,全局环境可以理解为python中的pip工具的功能,而单独项目管理可以理解为python中的虚拟环境类似概念,只是一个虚拟环境对应一个项目

npm的工具的操作有:

### 通用操作


+ `npm search <package>` |查找包
+ `npm view <package> dependencies`|查看包的依赖关系
+ `npm view <package> repository.url`|查看包的源文件地址
+ `npm view <package> engines`|查看包所依赖的Node的版本
+ `npm root`| 查看项目路径,可以加`-g`表示全局的package存储位置
+ `npm install <package>`|安装包,可以加`-g`表示全局,`--save`表示计入配置文件的`Dependencies`,`--save-dev`表示计入配置文件的`devDependencies`
+ `npm uninstall <package>`|卸载包,可以加`-g`表示全局
+ `npm update <package>` |更新包,可以加`-g`表示全局
+ `npm outdated` |检查过时的包,可以加`-g`表示全局
+ `npm list`| 查看当前目录下已安装的node包,注意事项：Node模块搜索是从代码执行的当前目录开始的，搜索结果取决于当前使用的目录中,node_modules下的内容。`npm list parseable=true`可以目录的形式来展现当前安装的所有node包,可以加`-g`表示全局


### 全局环境设置专用操作

操作|说明
---|---
npm adduser|创建npm的用户
npm config ls|查看npm在机器上的设置


### 单独项目专用操作

操作|说明
---|---
npm init|初始化一个项目,会生成一个`package.json`作为配置文件来管理该项目


### package.json配置文件

package.json是一个node.js项目的配置文件,它大约是这样的:

```json
{
  "name": "ex3",    //项目名
  "version": "1.0.0", //版本
  "description": "", //描述
  "main": "index.js", //入口文件
  "scripts": {  //可运行的脚本
    "docs": "esdoc -c esdoc.json",
    "start": "gulp",
    "lint": "gulp lint",
    "test":"./node_modules/.bin/babel-node ./node_modules/.bin/isparta cover --report html ./node_modules/.bin/_mocha "
  },
  "author": "hsz",//作者
  "license": "ISC",//版权声明
  "devDependencies": {//开发用的依赖
    "babel-cli": "^6.6.5",
    "babel-core": "^6.7.4",
    "babel-eslint": "^6.0.2",
    "babel-loader": "^6.2.4",
    "babel-polyfill": "^6.7.4",
    "babel-preset-es2015": "^6.6.0",
    "babel-register": "^6.7.2",
    "chai": "^3.5.0",
    "esdoc": "^0.4.6",
    "eslint": "^2.7.0",
    "gulp": "^3.9.1",
    "gulp-clean": "^0.3.2",
    "gulp-eslint": "^2.0.0",
    "gulp-minify-html": "^1.0.6",
    "gulp-notify": "^2.2.0",
    "gulp-rename": "^1.2.2",
    "gulp-uglify": "^1.5.3",
    "gulp-util": "^3.0.7",
    "isparta": "^4.0.0",
    "mocha": "^2.4.5",
    "mochawesome": "^1.3.2",
    "webpack": "^1.12.14"
  }
}
```

有些工具他们可以使用`package.json`设置自己,有些不行.

### 依赖的版本声明

使用NPM下载和发布代码时都会接触到版本号。NPM使用语义版本号来管理代码，这里简单介绍一下。
语义版本号分为X.Y.Z三位，分别代表主版本号、次版本号和补丁版本号。当代码变更时，版本号按以下原则更新。
如果只是修复bug，需要更新Z位。
如果是新增了功能，但是向下兼容，需要更新Y位。
如果有大变动，向下不兼容，需要更新X位。
版本号有了这个保证后，在申明第三方包依赖时，除了可依赖于一个固定版本号外，还可依赖于某个范围的版本号。例如"argv": "0.0.x"表示依赖于0.0.x系列的最新版argv。

版本控制同样支持通配符

+ \*: 任意版本
+ 1.1.0: 指定版本
+ ~1.1.0: >=1.1.0 && < 1.2.0
+ ^1.1.0: >=1.1.0 && < 2.0.0
+ 其中 ~ 和 ^ 两个前缀让人比较迷惑，简单的来说：
+ ~ 前缀表示，安装大于指定的这个版本，并且匹配到 x.y.z 中 z 最新的版本。
+ ^ 前缀在 ^0.y.z 时的表现和 ~0.y.z 是一样的，然而 ^1.y.z 的时候，就会 匹配到 y 和 z 都是最新的版本。

特殊的是，当版本号为 ^0.0.z 或者 ~0.0.z 的时候，考虑到 0.0.z 是一个不稳定版本， 所以它们都相当于 =0.0.z。


## 墙内换源

npm虽好,但无奈天朝有墙,我们只能用国内镜像作为源了

在你的home目录下,编辑`~/.npmrc `,

```shell
registry =https://registry.npm.taobao.org
```

## index.js

习惯上一个项目的入口文件会被命名为`index.js`.这个没有强制但基本大家都这么干.

## babel环境

babel是js的语法编译器,它可以把高级语法的js代码编译为低级语法的js代码以适应各种环境.babel高度模块化,以至于让人觉得有点过度设计.其安装方法是

```shell
npm install --save-dev babel-cli
npm install --save-dev babel-preset-env
npm install --save-dev babel-register
npm install --save-dev babel-polyfill
```

通常babel环境不装在全局,这样减少依赖冲突的可能.

其中
+ `babel-cli`提供命令行工具以及一个高级语法的解释器
+ `babel-preset-env`提供最近最常见的语法包组合
+ `babel-register`提供一个代码层面的工具,让代码在es5环境下也可以使用es6语法
+ `babel-polyfill`提供新的运行时特性


安装好后我们还需要配置需要的babel语法包,可以在package.json中通过`babel`字段定义

```json
{...
    "babel": {
        "presets": [
                    ["env"]
        ]
    },
 ...
}
```

而我们通常将编译和执行操作写到package.json中方便调用:

```json
{...
    "scripts":{
        "run":"./node_modules/.bin/babel-node index.js",
        "build": f"./node_modules/.bin/babel es -d lib",
     },
 ...
}
```

只有使用命令`npm run run` 即可执行项目,`npm run build`即可编译项目
