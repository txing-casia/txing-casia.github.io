---
layout:     post
title:      "ACM数据格式处理python方法总结"
subtitle:   "OJ在线编程常见输入输出练习场"
date:       2021-06-28
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Code Tips
---



# ACM数据格式处理python方法总结

## OJ在线编程常见输入输出练习场

在线测试网址：https://ac.nowcoder.com/acm/contest/5652#question

华为机考模拟试题：https://www.nowcoder.com/ta/huawei

### 总结：

- 数据行数具体是多少不重要，可通过while来解决，每行数据长度是否已知也无所谓。
- 熟悉sys，map，try-except的具体使用框架和方式
- 熟悉split()，strip()函数的使用方式
- 一些字符相关的函数：

  - 字符转化为asc-ii码函数（ord('a')）可以用来判断是否大小写/数字
- 字符串.isalnum() 所有字符都是数字或者字母，为真返回 Ture，否则返回 False。
  - 字符串.isalpha() 所有字符都是字母，为真返回 Ture，否则返回 False。
  - 字符串.isdigit() 所有字符都是数字，为真返回 Ture，否则返回 False。
  - 字符串.islower() 所有字符都是小写，为真返回 Ture，否则返回 False。
  - 字符串.isupper() 所有字符都是大写，为真返回 Ture，否则返回 False。
  - 字符串.istitle() 所有单词都是首字母大写，为真返回 Ture，否则返回 False。
  - 字符串.isspace() 所有字符都是空白字符，为真返回 Ture，否则返回 False。
- 进制转换：
  - 16进制转为10进制：int(num,16)
  - 2进制转为10进制：int(num,2)
  - 8进制转为10进制：int(num,8)
  - 10进制转为16进制：hex(num)
  - 10进制转2进制：bin(num)
  - 10进制转8进制：oct(num)
- 输出分割方式：
  - 按照空格分割：print(a, end=' ')
  - 按照行分割：print(a) or print(a, end='\n') 

```python
import sys

# 输入多行字符串转换为二维矩阵
num=int(input())
# 创建已知大小的二维矩阵
T = [[0]*num for i in range(num)]
i=0
for line in sys.stdin:
	# 字符串分割！！！
    T[i][:]=list(''.join(line.strip().split()))
    i+=1
```



- **已知数据行数，已知每行长度**：外加循环或者判断条件

```python
a,b = map(int,input().split(' '))
```

- **未知数据行数，未知每行长度**：使用sys库提取每行数据，结合strip()（去除\t, \n \r等无意义字符）和split()（分割函数，方便提取元素）函数实现数组建立

```python
import sys
for line in sys.stdin:
    print(' '.join(sorted(line.strip().split())))
```

- **已知行数，未知每行长度**：行数可以不储存不使用，每行未知长度数据，可以用下面语句直接变成一个数组

```python
a = list(map(int, input().split())) # a直接就是整数数组
```



下面给出了几个输入输出案例（包括**数字**和**字符串**部分）：

### 1. A+B(1)

输入包括两个正整数a,b(1 <= a, b <= 10^9),输入数据包括多组。

```python
import sys

try:
    while True:
        a,b = map(int,input().split(' '))
        # a,b = map(int,input().split())
        print(a + b)
except:
    pass 
```



### 2. A+B(2)

输入第一行包括一个数据组数t(1 <= t <= 100)
接下来每行包括两个正整数a,b(1 <= a, b <= 10^9)

```python
import sys

try:
    num = int(input())
    # num = int(sys.stdin.readline().strip()) # 读取一整行输入，strip()去掉末尾空行或空格，int()转换为整数形式
    for i in range(num):
        a , b = map(int,input().split(' '))
        print(a + b)
except:
    pass
```

```python
import sys
input()
for line in sys.stdin:
    print(sum(map(int, line.split())))
```



### 3. A+B(3)

输入包括两个正整数a,b(1 <= a, b <= 10^9),输入数据有多组, 如果输入为0 0则结束输入

```python
try:
    while True:
        a , b = map(int,input().split(' '))
        if a != 0 and b != 0:
            print(a + b)
        else:
            break
except:
    pass
```



### 4. A+B(4)

输入数据包括多组。
每组数据一行,每行的第一个整数为整数的个数n(1 <= n <= 100), n为0的时候结束输入。
接下来n个正整数,即需要求和的每个正整数。

```python
try:
    while True:
        a = input().split(' ') # a是字符数组
        # a = sys.stdin.readline().split(' ')
        sum = 0
        if int(a[0]) != 0:
            for i in range(1,len(a)):
                sum += int(a[i])
            print(sum)
        else:
            break
except:
    pass
```

```python
try:
    while True:
        a = list(map(int, input().split())) # a直接就是整数数组
        if a[0] != 0:
            print(sum(a[1:]))
        else: 
            break
except:
    pass
```

使用sys库

```python
import sys
 
for line in sys.stdin:
    if list(map(int, line.strip().split()))[0]!=0:
        print(sum(map(int, line.strip().split()[1:])))
```



### 5. A+B(5)

输入的第一行包括一个正整数t(1 <= t <= 100), 表示数据组数。
接下来t行, 每行一组数据。
每行的第一个整数为整数的个数n(1 <= n <= 100)。
接下来n个正整数, 即需要求和的每个正整数。

```python
try:
    nums = int(input())
    for i in range(nums):
        a = list(map(int,input().split()))
        print(sum(a[1:]))
except:
    pass
```

一个更简洁的代码：

```python
while True:
    try:
        numlist = list(map(int, input().strip().split()))
        if len(numlist) == 1:
            pass
        else:
            print(sum(numlist[1:]))
    except:
        break
```



### 6. A+B(6)

输入数据有多组, 每行表示一组输入数据。
每行的第一个整数为整数的个数n(1 <= n <= 100)。
接下来n个正整数, 即需要求和的每个正整数。
每组数据输出求和的结果

```python
import sys
 
for line in sys.stdin:
    print(sum(map(int, line.strip().split()[1:])))
```



### 7. A+B(7)

输入数据有多组, 每行表示一组输入数据。
每行不定有n个整数，空格隔开。(1 <= n <= 100)。
每组数据输出求和的结果

```python
import sys
 
for line in sys.stdin:
    print(sum(map(int, line.strip().split())))
```

```python
try:
    while True:
        print(sum(list(map(int, input().strip().split()))))
except:
    pass
```



### 8. 字符串排序(1)

输入有两行，第一行n
第二行是n个空格隔开的字符串
输出一行排序后的字符串，空格隔开，无结尾空格

```python
num = input()
print(' '.join(sorted(input().split())))
# sorted(num,reverse=Ture) # 逆序排序
```



### 9. 字符串排序(2)

多个测试用例，每个测试用例一行。
每行通过空格隔开，有n个字符，n＜100
对于每组测试用例，输出一行排序过的字符串，每个字符串通过空格隔开

```python
import sys

for line in sys.stdin:
    print(' '.join(sorted(line.strip().split())))
```



### 10. 字符串排序(3)

多个测试用例，每个测试用例一行。
每行通过,隔开，有n个字符，n＜100
对于每组用例输出一行排序后的字符串，用','隔开，无结尾空格

```python
import sys

for line in sys.stdin:
    print(','.join(sorted(line.strip().split(','))))
```



### 11. 自测本地通过，但是提交为0

每年前几场在线笔试编程题的时候，总有同学询问为什么我本地测试通过，自测也通过，提交代码系统却返回通过率0。
打开以下链接可以查看正确的代码
https://ac.nowcoder.com/acm/contest/5657#question
这不是系统的错，可能是因为

1. 你对题目理解错了，你的代码只过了样例或你自己的数据
2. 你的代码逻辑有问题，你的代码只过了样例或你自己的数据

总之就是你的代码只是过了样例和自测数据，后台的测试数据你根本不可见，要多自己思考。

这个题目如果你提交后通过率为0，又觉得自己代码是正确的，可以 点击查看 通过的代码

谨记：
当你笔试的时候怀疑系统或者题目数据有问题的时候请务必先怀疑自己的代码!

请帮忙把这个练习专题发给你的朋友同学吧，感谢感谢