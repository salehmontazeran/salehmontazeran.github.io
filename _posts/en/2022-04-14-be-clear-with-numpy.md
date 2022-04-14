---
layout: post
title:  "Be clear with numpy"
date:   2022-04-14 22:00:00 +0430
categories: python numpy
comments: true
lang: en
---

Consider a list of 100 million items, which are float.\
we want to change this list into a numpy array. Normal way is using the array function or other functions mounted on this function:

{% highlight python %}
import numpy

n_list = [float(i) for i in range(100000000)]

n_array = np.array(n_list)
{% endhighlight %}

In recent versions of numpy, the default type for creating arrays has decimal numbers np.float64,so dtype of the final array will be `np.float64`.\
<br>
Now you may ask what is the problem?\
<br>
The operation of numpy is like that after reading the first element of  list, it realizes the type of output array must be `np.float64`. so after reading the second element, it tries to convert all elements to `np.float64`. Everything seems fine except a level, which we didn’t mentioned. Before numpy starts to convert, first it checks if it can convert the new element to `np.float64` or not ,which is an extra level for us. We are sure about  the type of list elements, so it doesn’t necessary numpy does that.\
To remove the extra step mentioned, in converting we should just say our considered `dtype` explicitly. This way numpy doesn’t do that extra check level and just tries to convert the numbers to np.float64 otherwise it throws an exception!

{% highlight python %}
n_array_c = np.array(n_list, dtype=np.float64)
{% endhighlight %}

On my personal computer, which has an i7-6700K processor, on average the second code runs at least a second faster and on a weaker computer it reaches ten seconds. Generally, in larger dimensions and size it will bother even powerful computers.

Reference: https://github.com/numpy/numpy/issues/21259