# Javascript攻略

Javascript是当今最流行的编程语言之一,它诞生于网络技术,也几乎局限于网络技术.这是一门比较有争议的脚本语言.
一方面它相当简陋,在es6之前身为一个脚本语言其编程体验极差,
另一方面由于google的v8引擎和基于v8的node.js解释器的出现.js从只能做前端开发的浏览器脚本语言蜕变为全平台脚本语言.这让大量的前端开发者招到了新玩具从而迅速获得了一个极为庞大的社区.

在社区扩大后也涌现了一大批优秀的框架工具.事实上JS恐怕拥有着最打的编程社区,技术的更新换代恐怕也是所有语言中最快的.喜欢新鲜事物的人自然很喜欢这种状态,但这也让JS社区成了最碎片化的社区.
甚至于js也成了我见过唯一需要"编译"的脚本语言.

## Javascript的定位

JS的定位在历史上也是起起落落.最开始单纯作为浏览器脚本语言,到后来node.js出现一度有了JS成为前后端通用语言,拳打Java脚踢C,甚至因为其解释器很小资源消耗很低的特点还有了在嵌入式系统中应用的尝试,再到近年回归理性,基本确定了其应用范围就在前端和与前端最接近的一层后端--Web Gateway层以及一些脚本小工具上.

结合JS这门语言的特点这个定位相对还是比较理性客观的,首先浏览器前端脚本是js的自留地,其他语言基本完全没有插足的余地,其次,在虚拟机和语言特性层面,得力于google给力的v8以及天生没有多线程这一特点,js代码全部都是基于回调或者协程的,因此在io并发性能高的同时内存消耗很小,这一点也是它比python更适合开发服务端的原因.但和python一样,v8无法应用多核(永远单线程).因此在高负载情况下只能通过起多个进程的方式.因此js天生不适合做核心组件的开发语言.像什么Queue,数据库这种就基本没有js的事儿了.

如果要开发这种核心组件,还是要老老实实使用go,或者C靠谱.

## 本文针对人群

本文主要是为已经学过python的人而写的攻略文.一方面做数据科学的需要一定前端技术做可视化,也需要一定的网络技术用于项目落地.另一方面Javascript是一个很好的参照,让我们可以更好的理解编程技术.写这篇文章的一个很大的原因是JS是很好的函数式编程的学习语言.

+ 一方面它足够流行,这样不至于学了一点用没有,成为屠龙技(比如各种lisp方言)
+ 另一方面他有足够的部件实现和验证函数式编程.

因此本文也针对对函数式编程有兴趣的同学.

## 使用ES6标准

本文直接从ES6标准开始,绕开语法晦涩的低级标准,可以更好的服务于快速原型开发和结论验证.

+ 语法糖丰富

低版本的Js语法简单而且坑多,使用起来各种不方便,会给习惯python的数据科学研究人员造成学习困难, ES6新增了大量语法糖.在使用上可以给被python惯坏了的人一定程度上减少学习成本.

+ 接近Python的开发习惯

Python是模块化编程的语言,而Js在设计初期就没考虑模块化的问题,低版本的js在编写时模块化方面 会让被python惯坏了的人非常不喜欢,ES6使用类似python的import语句,相对容易接受.

## 文章结构

单说JavaScript的话是一个很简单的语言,本文大致分为几个部分

1. 执行环境

    这部分讲node的执行环境,工欲善其事必先利其器,js作为后起之秀有着相当优秀和完善的工具链,但前面也讲过了社区非常碎片化因此不少工具功能相同,这对选择困难症非常不友好
    这部分讲我常用的工具链,包括:
    + node环境和包管理工具
    + api自动生成工具
    + 测试工具
    + 代码风格规范
  
2. 基本语法

    这部分会参照python介绍语法和一些惯用法

3. 函数式编程

    之所以讲JS一个很大的原因是它既热门又比python更加适合使用函数式编程

4. 常见应用
   
   最后是应用环节,本文会介绍常见的js框架用于解决一些实际问题,比如前端编程,chrome插件编写等.这个部分会有示例代码,但不定期更新

本文使用`jupyter notebook`结合`jp-babel`编写,`gitbook`解析编译生成网页.

本文主要参考自阮一峰大大的书<ECMAScript 6 入门>和SICP,加上一些个人理解,本人才疏学浅如有错误望各位读者指正.
