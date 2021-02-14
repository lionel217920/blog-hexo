---
title: 面试中的一道算法题
date: 2020-04-12 21:30:02
---

今天下午视频面试，前面交谈之后发给了我一道算法题。

问题如下：

描述一下下面这个函数的作用，答题时间10分钟。

{% codeblock 函数代码如下 lang:java line_number:false %}
public int Unknown(int arr[], int i, int n) {
    if (i == n-1) {
        return arr[i];
    } else {
        int tmp = unknown(arr, i+1, n);

        if (tmp < arr[i]) {
            return arr[i];
        } else {
            return tmp;
        }
    }
}
{% endcodeblock %}

我看到这个题目之后，首先分析了这个函数。

首先{% label info@函数的入参 %}，一个`int`类型的数据，两个`int`类型的参数，{% label info@函数的出参 %}是一个数字。内部还有一个{% label danger@递归函数 %}。我的第一反应这个应该是一个排序的函数，八九不离十。PS：不过当时看到题目的时候还是比较紧张的，后来慢慢冷静下来了。

我们来看开始的两行代码，在结合下面的递归函数

```java
if(i == n-1) {
    return arr[i]
} else {
    unknown(arr, i+1, n);
}
```

可以看到，i就是数组中的下标，n也是和i有关系的。那么假设变量i就是数组的第一个位置，也就是0，变量n就是数组中元素的个数。

现在假设定义一个数组，那么i=0，n=8

```java
int arr[]= {1,5,20,8,10,15,3,16};
```

执行程序，tmp的第一个值是16，第二个值是20。所以这个函数的返回值是20，就是取这个数组中的最大值。