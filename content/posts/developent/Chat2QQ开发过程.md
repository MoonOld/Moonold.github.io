---
title: "Chat2QQ开发过程"
subtitle: "记录自己开发这个项目遇到的一些问题"
date: 2023-03-06T23:55:17+08:00
draft: true
description: 一篇自我介绍
tags: 
- "思考"
- "人生"
- "总结"
- "白学家"

---


{{<typeit>}}
To be continued...
{{</typeit>}}

## Mirai学习
### Kotlin & Java
因为没有学习Kotlin的打算，所以只准备简单了解一点语法方便开发即可。
有不懂的地方可以多用New Bing或者Chatgpt写一点示范代码，这样配合代码看解释就非常容易理解。
[参考链接](https://github.com/mamoe/mirai/blob/dev/docs/KotlinAndJava.md)
#### 通用
* Kotlin定义默认public final
* 用换行而不是分号作为语句结尾
* 不需要new获得对象
* 可以指明返回类型之后直接返回值
* 编译器给var自动创建getter和setter
* 可以在顶层定义var和函数，被编译在类里做静态变量
* 用object定义单例
  > object Test
* 使用@JvmField和@JvmStatic进行静态编译
以下两种定义等价
```Kotlin
    Definition A:
    class A {
        private val value: String
        
        constructor(value: String) {
            this.value = value
        }
        
        constructor(integer: Int) {
            this.value = integer.toString()
        }
    }

    Definition B:
    class A(val value: String) { // 类定义后面的括号表示主构造器
        constructor(integer: Int) : this(integer.toString())
    }
```

  
### 创建和配置Bot

## Java获取环境变量问题
在.zshrc中export后无论在IDEA的终端还是普通终端都可以echo $param获取环境变量，但在程序中System.getenv("param")始终无法完成
两个方法源码如下，其中theUnmodifiableEnvironment是ProcessEnvironment的final类变量，也就是说启动之后是不会变化的。
后续多次尝试重启IDEA发现Token及时变化了，推测可能是IDEA JVM在持续运行，过程中变量变化是不会被立刻读到的，也就是内存数据的不透明性。
```Java
/* Only for use by System.getenv(String) */
    static String getenv(String name) {
        return theUnmodifiableEnvironment.get(name);
    }

    /* Only for use by System.getenv() */
    static Map<String,String> getenv() {
        return theUnmodifiableEnvironment;
    }
```
## SK使用问题
https://help.openai.com/en/articles/5112595-best-practices-for-api-key-safety