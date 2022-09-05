---
layout:     post
title:      "Python报错libGL error: MESA-LOADER: failed to open iris"
subtitle:   ""
date:       2022-09-05
author:     "Pcon"
header-img: "img/post-bg-py.jpg"
tags:
    - Python
---

## Python报错libGL error: MESA-LOADER: failed to open iris

### 1 问题背景

执行gym-like环境代码时，渲染动画输出时报错无法找到系统显卡驱动（笔记本，核显，Ubuntu），报错信息：

```
libGL error: MESA-LOADER: failed to open iris: /usr/lib/dri/iris_dri.so: cannot open shared object file: No such file or directory (search paths /usr/lib/x86_64-linux-gnu/dri:\$${ORIGIN}/dri:/usr/lib/dri, suffix _dri)
libGL error: failed to load driver: iris
libGL error: MESA-LOADER: failed to open iris: /usr/lib/dri/iris_dri.so: cannot open shared object file: No such file or directory (search paths /usr/lib/x86_64-linux-gnu/dri:\$${ORIGIN}/dri:/usr/lib/dri, suffix _dri)
libGL error: failed to load driver: iris
libGL error: MESA-LOADER: failed to open swrast: /usr/lib/dri/swrast_dri.so: cannot open shared object file: No such file or directory (search paths /usr/lib/x86_64-linux-gnu/dri:\$${ORIGIN}/dri:/usr/lib/dri, suffix _dri)
libGL error: failed to load driver: swrast
```

然而，根据提示，在`/usr/lib/x86_64-linux-gnu/dri`路径下是可以找`iris_dri.so`驱动的。



### 2 处理办法 

 [这篇博客](https://zhuanlan.zhihu.com/p/538877347)提供了一个有效解决方法：

Step 1: 建立一个 /usr/lib/dri/iris_dri.so 的软连接

```
(base) pcon@pcon-ThinkPad-L14-Gen-2:/usr/lib$ sudo mkdir dri
(base) pcon@pcon-ThinkPad-L14-Gen-2:/usr/lib$ cd dri 
(base) pcon@pcon-ThinkPad-L14-Gen-2:/usr/lib/dri$ sudo ln -s /usr/lib/x86_64-linux-gnu/dri/iris_dri.so iris_dri.so
```

Step 2: 再次python执行代码，报错变为

```
libGL error: MESA-LOADER: failed to open iris: /home/pcon/anaconda3/bin/../lib/libstdc++.so.6: version `GLIBCXX_3.4.29' not found (required by /usr/lib/dri/iris_dri.so) (search paths /usr/lib/x86_64-linux-gnu/dri:\$${ORIGIN}/dri:/usr/lib/dri, suffix _dri)
libGL error: failed to load driver: iris
libGL error: MESA-LOADER: failed to open iris: /home/pcon/anaconda3/bin/../lib/libstdc++.so.6: version `GLIBCXX_3.4.29' not found (required by /usr/lib/dri/iris_dri.so) (search paths /usr/lib/x86_64-linux-gnu/dri:\$${ORIGIN}/dri:/usr/lib/dri, suffix _dri)
libGL error: failed to load driver: iris
libGL error: MESA-LOADER: failed to open swrast: /usr/lib/dri/swrast_dri.so: cannot open shared object file: No such file or directory (search paths /usr/lib/x86_64-linux-gnu/dri:\$${ORIGIN}/dri:/usr/lib/dri, suffix _dri)
libGL error: failed to load driver: swrast
```

这是由于 conda 里的 libstdcxx-ng 版本不够高造成的。

Step 3：执行以下命令查看GLIBCXX版本信息

```
(base) pcon@pcon-ThinkPad-L14-Gen-2:~/anaconda3$ strings /home/pcon/anaconda3//lib/libstdc++.so.6 | grep GLIBCXX
```

Step 4：升级 conda 里的 libstdcxx-ng (根据[博客](https://zhuanlan.zhihu.com/p/538877347)) 。（截至2022.09该版本号有效）

```
(base) pcon@pcon-ThinkPad-L14-Gen-2:~/anaconda3$ conda install libstdcxx-ng=12.1.0
```

再次查看版本号

```
(base) pcon@pcon-ThinkPad-L14-Gen-2:~/anaconda3$ strings /home/zjj/anaconda3//lib/libstdc++.so.6 | grep GLIBCXX
```

执行Python代码，成功运行！



ref

https://zhuanlan.zhihu.com/p/538877347