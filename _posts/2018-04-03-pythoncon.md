---
layout: post
title: A Noteworthy Fact in Python
tags: [Python, numpy]
image:
  feature: ori.jpg
---

This was found when working on the term project for CS269 last quarter.

Every Python programmer would probably have used the numpy package, which allows us to use the numpy array. Compared to the common array in Python, the numpy array supports various operations and is perfectly suitable for scientific usage. However, my teammates and I found a noteworthy fact about the numpy array when we were working on the project. There should be some reason for why this fact exists or say, why Python with numpy is implemented in this way, but it can be disastrous if not knowing about this in advance.

First, let us consider the following codes.

~~~ python
import numpy

a = [[]]
b = numpy.array([[]])

print(len(a))
print(len(b))
print(b.size)
print(a)
print(b)
~~~

You could guess what the results are, and try the codes on your own machine. The output should be as follows.

~~~ python
1
1
0
[[]]
[]
~~~

The first two results is reasonable, since both *a* and *b* acctually has one element in the array, which is *[]*. The result in the third line is a little surprising. Ths *size* attribute value for the numpy array *b* is *0*, which means its size is *0*. As we continue to see the last two outputs, when printing the content of the numpy array *b*, it does not show the *[]* in the array. That is to say, your array of *[[]]* becomes *[]* to some extent.

You would get the same results no matter which version of Python, 2 or 3, you are using. This fact became disastrous when my teammates and I needed to check whether the array has content in it, as well as whether it is *null*, when implementing the project. It was frustrating and took us a while to figure this out. Also, this can be a disaster when you use the *size* attribute to check the content of the numpy array, or print the temporary variable during debugging. You would probably ask yourself why it prints *[]*, which seems to be empty, but it has a non-zero length.

We think this problem should be fixed, or at least make an announcement somewhere. Maybe in Python 4, it will change.
