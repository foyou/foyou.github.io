---
layout:        post
title:         在 python 中，如果 x 是 list，为什么 x += ha 可以运行，而 x = x + ha 却抛出异常呢？
description:   '在 python 中，如果 x 是 list，为什么 x += "ha" 可以运行，而 x = x + "ha" 却抛出异常呢？'
tags: [python,异常]
image:
  background: body_bg.gif
comments: true
share: true
---

原文：<a href="http://stackoverflow.com/questions/3216706/if-x-is-list-why-does-x-ha-work-while-x-x-ha-throws-an-exception" target="_blank">python - If x is list, why does x += "ha" work, while x = x + "ha" throw an exception?</a>

译文：[在 python 中，如果 x 是 list，为什么 x += "ha" 可以运行，而 x = x + "ha" 却抛出异常呢？]

##问题

众所周知，在 python 中，+ 运算符可以使用在列表上，+ 运算符只需要第二个操作数是可迭代的(原文：iterable。，那么 + 显然可以运算在 "ha" 上。

    
<!--more-->

代码如下：
{% highlight JavaScript %}
>>> x = []
>>> x += "ha"
>>> x
['h', 'a']

>>> x = x + "ha"
Traceback (most recent call last):
File "<stdin>", line 1, in <module>
TypeError: can only concatenate list (not "str") to list
{% endhighlight %}

##解答

当我们在列表 list 上使用 += 的时候，其实相当于调用函数 extend()，而不是使用的 +。

--你可以在一个可迭代(iterable)对象上调用 extend()。
--但是，当您使用 + 时，另一个操作数必须是列表(list)。

为什么 python 会如此诡异，也许是出于性能方面的考虑。 调用 + 时，将会创建一个新的对象，并复制里面的所有内容。但是当调用 extend() 函数时，将可以使用现有的空间。

这样就会产生另一个副作用：如果你写 X += Y，在其他对列表的引用(reference)中，会看到变化；但如果你使用 X = X + Y，就不会。

下面的代码说明了这一点：
{% highlight JavaScript %}
>>> x = ['a','b']
>>> y = ['c', d']
>>> z = x
>>> x += y
>>> z
['a', 'b', 'c', 'd']    // z 也发生了变化

>>> x = ['a','b']
>>> y = ['c', d']
>>> z = x
>>> x = x + y
>>> z
['a', 'b']  // z 函数原始值
{% endhighlight %}

##参考文献

Python source code for list.

python：+= 的源代码:

{% highlight JavaScript %}
static PyObject *
list_inplace_concat(PyListObject *self, PyObject *other)
{
    PyObject *result;

    result = listextend(self, other);
    if (result == NULL)
        return result;
    Py_DECREF(result);
    Py_INCREF(self);
    return (PyObject *)self;
}
{% endhighlight %}

python：+ 的源代码:

{% highlight JavaScript %}
static PyObject *
list_concat(PyListObject *a, PyObject *bb)
{
    Py_ssize_t size;
    Py_ssize_t i;
    PyObject **src, **dest;
    PyListObject *np;
    if (!PyList_Check(bb)) {
        PyErr_Format(PyExc_TypeError,
                  "can only concatenate list (not \"%.200s\") to list",
                  bb->ob_type->tp_name);
        return NULL;
    }

    // etc ...
}
{% endhighlight %}

From:<a href="http://coolshell.info/python/reprints/2013/03/11/if-x-is-list-why-does-x-ha-work-while-x-x-ha-throw-an-exception.html" target="_blank">http://coolshell.info/python/reprints/2013/03/11/if-x-is-list-why-does-x-ha-work-while-x-x-ha-throw-an-exception.html</a>
