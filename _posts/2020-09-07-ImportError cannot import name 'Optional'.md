---
layout:     post
title:      "ImportError: cannot import name 'Optional'"
subtitle:   ""
date:       2020-09-07
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Python
    - Pytorch
    - ImportError
---

# ImportError: cannot import name 'Optional'



遇到报错：ImportError: cannot import name 'Optional'

已安装所需的库，经查是由于torchvision和pytorch版本不对应导致的。具体对应版本号见[下图](https://github.com/pytorch/vision)

|       `torch`        |    `torchvision`     |         `python`          |
| :------------------: | :------------------: | :-----------------------: |
| `master` / `nightly` | `master` / `nightly` |          `>=3.6`          |
|       `1.6.0`        |       `0.7.0`        |          `>=3.6`          |
|       `1.5.1`        |       `0.6.1`        |          `>=3.5`          |
|       `1.5.0`        |       `0.6.0`        |          `>=3.5`          |
|       `1.4.0`        |       `0.5.0`        | `==2.7`, `>=3.5`, `<=3.8` |
|       `1.3.1`        |       `0.4.2`        | `==2.7`, `>=3.5`, `<=3.7` |
|       `1.3.0`        |       `0.4.1`        | `==2.7`, `>=3.5`, `<=3.7` |
|       `1.2.0`        |       `0.4.0`        | `==2.7`, `>=3.5`, `<=3.7` |
|       `1.1.0`        |       `0.3.0`        | `==2.7`, `>=3.5`, `<=3.7` |
|      `<=1.0.1`       |       `0.2.2`        | `==2.7`, `>=3.5`, `<=3.7` |

更改anaconda中的库的版本也很简单：

```python
conda install pytorch = version
```

将version改为对应的版本号即可（例如1.4）



Txing

2020-09-07