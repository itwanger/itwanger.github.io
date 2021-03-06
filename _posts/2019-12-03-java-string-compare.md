---
layout: post
category: java
title: Stack Overflow 上 370万浏览量的一个问题：如何比较 Java 的字符串？
tagline: by 沉默王二
tag: java
---

在逛 Stack Overflow 的时候，发现了一些访问量像喜马拉雅山一样高的问题，比如说这个：如何比较 Java 的字符串？访问量足足有 370万+，这不得了啊！说明有很多很多的程序员被这个问题困扰过。

<!--more-->

PS：系列文章回顾：《[Stack Overflow 上250万浏览量的一个问题：你对象丢了](https://mp.weixin.qq.com/s/PBqR_uj6dd4xKEX8SUWIYQ)》

我们来回顾一下提问者的问题：

>截止到目前为止，我一直使用“==”操作符来比较字符串，直到程序出现了一个 bug，需要使用 `.equals()` 方法来解决。这是为什么呢？“==”操作符和 `.equals()` 方法之间有什么区别呢？

和提问者相反，在我刚开始学习 Java 的时候，比较字符串一直使用的是 `.equals()` 方法，因为不管是书本还是老师，都告诫我不要直接使用“==”操作符来比较，会出 bug。至于为什么，书本和老师都没有帮我搞清楚。

那借此机会，我就来梳理一下 Stack Overflow 上的高赞答案，我们来一起学习进步，打怪升级。

- “==”操作符用于比较两个引用（内存中的存放地址）是否相等，它们是否是同一个对象。

- `.equals()` 用于比较两个对象的内容是否相等。

怎么理解这两句话呢？我来举个不恰当又很恰当的例子。

有一对双胞胎，姐姐叫阿丽塔，妹妹叫洛丽塔。我们普通人的眼睛完全无法分辨谁是姐姐谁是妹妹，可她们的妈妈却可以轻而易举地辨认出。


![](http://www.itwanger.com/assets/images/2019/12/java-string-compare-2.png)

`.equals()` 就好像我们普通人，看见阿丽塔以为是洛丽塔，看见洛丽塔以为是阿丽塔，看起来一样就觉得她们是同一个人；“==”操作符就好像她们的妈妈，要求更严格，观察更细致，一眼就能分辨出谁是姐姐谁是妹妹。

```java
String alita = new String("小萝莉");
String luolita = new String("小萝莉");

System.out.println(alita.equals(luolita)); // true
System.out.println(alita == luolita); // false
```

就上面这段代码来说，`.equals()` 输出的结果为 true，而“==”操作符输出的结果为 false——前者没后者要求那么严格。

大家都知道，Java 的所有类都默认地继承着 Object 这个超类，该类有一个名为 `.equals()` 的方法，源码如下所示。

```java
public boolean equals(Object obj) {
    return (this == obj);
}
```

可以看得出，Object 类的 `.equals()` 方法默认采用的是“==”操作符进行比较。假如子类没有重写该方法的话，那么“==”操作符和 `.equals()` 方法的功效就完全一样——比较两个对象的内存地址或者对象的引用是否相等。

但实际情况中，有不少类重写了 `.equals()` 方法，因为比较内存地址太重了，不太符合现实的场景需求。String 类就重写了 `.equals()` 方法，源码如下所示。

```java
public boolean equals(Object anObject) {
    if (this == anObject) {
        return true;
    }
    if (anObject instanceof String) {
        String anotherString = (String)anObject;
        int n = value.length;
        if (n == anotherString.value.length) {
            char v1[] = value;
            char v2[] = anotherString.value;
            int i = 0;
            while (n-- != 0) {
                if (v1[i] != v2[i])
                    return false;
                i++;
            }
            return true;
        }
    }
    return false;
}
```

可以看得出，如果两个字符串对象“==”，那么 `.equals()` 的结果就为 true；否则的话，就比较两个字符串的内容是否相等。

大家应该都知道了，创建字符串对象有两种写法，如下所示。

```java
String luolita = "小萝莉";
String alita = new String("小萝莉");
```

第一种是在字符串常量池（存储在方法区）中创建对象，并将对象的引用赋值给 luolita。第二种是通过 new 关键字在堆中创建对象，并将对象引用赋值给 alita。

PS：字符串作为最基础的数据类型，使用非常频繁，如果每次都通过 new 关键字进行创建，会耗费高昂的时间和空间代价。Java 虚拟机为了提高性能和减少内存开销，就设计了字符串常量池：相同字面量的对象只有一个。

PPS：Java 虚拟机在执行程序的过程中会把内存区域划分为若干个不同的数据区域，如下图所示。

![](http://www.itwanger.com/assets/images/2019/12/java-string-compare-3.png)






下面我们通过实际代码来看看字符串的比较。

第一种：

```java
new String("小萝莉").equals("小萝莉") // --> true 
```

`.equals()` 比较的是两个字符串对象的内容是否相等，所以结果为 true。

第二种：

```java
new String("小萝莉") == "小萝莉" // --> false 
```

“==”操作符左侧的对象存储在堆中，右侧的对象存储在方法区，所以返回 false。

第三种：

```java
new String("小萝莉") == new String("小萝莉") // --> false 
```

new 出来的两个对象肯定是不相等的，所以返回 false。

第四种：

```java
"小萝莉" == "小萝莉" // --> true 
```

字符串常量池中只会有一个对象，所以返回 true。

```java
"小萝莉" == "小" + "萝莉" // --> true
```

由于“小”和“萝莉”都在字符创常量池，所以编译器会将其自动优化为“小萝莉”，所以返回 true。

经过大量实例的分析，我们可以得出如下结论（也是对提问者的回答）：

- 当比较两个字符串对象的内容是否相等时，请使用 `.equals()` 方法。

- 当比较两个字符串对象是否相等时，请使用“==”操作符。


当然了，如果要进行两个字符串对象的内容比较，除了 `.equals()` 方法，还有其他可选的方法。

1）`Objects.equals()`


`Objects.equals()` 这个静态方法的优势在于不需要在调用之前判空。

```java
public static boolean equals(Object a, Object b) {
    return (a == b) || (a != null && a.equals(b));
}
```

如果直接使用 `a.equals(b)`，则需要在调用之前对 a 进行判空，否则可能会抛出空指针 `java.lang.NullPointerException`。

```java
Objects.equals("小萝莉", new String("小" + "萝莉")) // --> true
Objects.equals(null, new String("小" + "萝莉")); // --> false
Objects.equals(null, null) // --> true

String a = null;
a.equals(new String("小" + "萝莉")); // throw exception
```

2）String 类的 `.contentEquals()`

`.contentEquals()` 的优势在于可以将字符串与任何的字符序列（StringBuffer、StringBuilder、String、CharSequence）进行比较。

```java
public boolean contentEquals(CharSequence cs) {
    // Argument is a StringBuffer, StringBuilder
    if (cs instanceof AbstractStringBuilder) {
        if (cs instanceof StringBuffer) {
            synchronized(cs) {
               return nonSyncContentEquals((AbstractStringBuilder)cs);
            }
        } else {
            return nonSyncContentEquals((AbstractStringBuilder)cs);
        }
    }
    // Argument is a String
    if (cs instanceof String) {
        return equals(cs);
    }
    // Argument is a generic CharSequence
    char v1[] = value;
    int n = v1.length;
    if (n != cs.length()) {
        return false;
    }
    for (int i = 0; i < n; i++) {
        if (v1[i] != cs.charAt(i)) {
            return false;
        }
    }
    return true;
}
```

从源码上可以看得出，如果 cs 是 StringBuffer，该方法还会进行同步，非常的智能化。不过需要注意的是，使用该方法之前，需要确保比较的两个字符串都不为 null，否则将会抛出空指针。

再强调一点，`.equals()` 方法在比较的时候需要判 null，而“==”操作符则不需要。

```java
System.out.println( null == null); // --> true
```

好了各位读者朋友们，以上就是本文的全部内容了。**能看到这里的都是人才，二哥必须要为你点个赞**👍。如果觉得不过瘾，还想看到更多，可以查看我的[个人博客](http://www.itwanger.com/)。另外呢，给大家一个承诺，我每周都会更新一篇《程序人生》和一篇 Java 技术栈相关的文章，敬请期待。如果你有什么问题需要我的帮助，或者想喷我了，欢迎留言哟。

养成好习惯！如果觉得这篇文章有点用的话，**求点赞、求关注、求分享、求留言**，这将是我写下去的最强动力！如果大家想要第一时间看到二哥更新的文章，可以扫描下方的二维码，关注我的公众号。我们下篇文章见！

