
# api文档自动生成

python可以使用段注释自动的生成文档,但js不行,必须借助第三方工具实现,这个工具就是`esdoc`

安装依然是

```shell
npm install esdoc --save-dev
```

之后使用我们看个例子

>例1: 用esdoc生成api文档并输出html文档

1. js文件:

    ```javascript
    /**
     * this is MyClass.
     */
    export default class MyClass {
      /**
       * @param {number} param this is param.
       * @return {number} this is return.
       */
      method(param){}
    }

    ```

    看注释,使用

    ```javascript
    /**
    *
    */
    ```

    包裹的注释可以成为文档,其中使用@开头的行会被辨识为是有特殊意义的标签

    所有的标签说明可以在[官网](https://esdoc.org/tags.html)找到

    需要注意的是:

    ```
    @param {number} param this is param.
    ```

    代表输入的形参属性,要有几个参数写几个,按顺序写,`{number}`表示类型,`param`是形参名 后面的是说明

2. 写配置

    esdoc支持在package.json中设置,使用字段`"esdoc"`

    ```json
    {...,
        "esdoc": {
            "destination": "./docs",
            "source": "./es",
            "includes": [r"\\.js$"],
            "excludes": [r"\\.config\\.js$"],
            "coverage": true,//是否统计覆盖率
            "plugins": [
                {
                    "name": "esdoc-standard-plugin"
                }
            ]
        },
    ...
    }

    ```

3. 使用

    可以使用命令`esdoc`来生成一个接口的静态页面,而且默认的还会把项目下的README.md文件渲染进去作为主页
    
    但更常见的用法是在package.json中设置scripts
    ```json
    {...,
        "scripts":{
             ...
             "doc": "./node_modules/.bin/esdoc",
             ...
        },
    ...
    }
    ```
    要使用只需要执行命令`npm run doc`即可

如果要写项目文档,光API文档肯定不够,我们可以[使用sphinx来构建项目的文档](http://blog.hszofficial.site/blog/2016/11/29/%E4%BD%BF%E7%94%A8sphinx%E7%BB%93%E5%90%88markdown%E5%86%99%E9%A1%B9%E7%9B%AE%E6%96%87%E6%A1%A3/)配合sphinxcontrib-autojs插件来构建
