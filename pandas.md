---
title: 'pandas'
date: 2022-09-29 20:52:06
tags: []
published: true
hideInList: false
feature: 
isTop: false
---
# pandas

## 1. 修改某一列的值

### 1.1 `pd.apply()`

```python
def f(x,filename):
    return(filename+'_'+x)
file_df["model"] = file_df["model"].apply(f,args=(file,))
```

> 如上，pd.apply(函数名，（参数1,))

```
DataFrame.apply(self, func, axis=0, raw=False, result_type=None, args=(), **kwargs)
```

> **参数：**
>
> - func：函数或 lambda 表达式,应用于每行或者每列
>
> - axis：{0 or ‘index’, 1 or ‘columns’}, 默认为0
>
>   - 0 or ‘index’: 表示函数处理的是每一列
>   - 1 or ‘columns’: 表示函数处理的是每一行
>
> - raw：bool 类型，默认为 False;
>
>   - False ，表示把每一行或列作为 Series 传入函数中；
>   - True，表示接受的是 ndarray 数据类型；
>   - result_type：{‘expand’, ‘reduce’, ‘broadcast’, None}, default None, These only act when axis=1 (columns):
>     - ‘expand’ : 列表式的结果将被转化为列。
>     - ‘reduce’ : 如果可能的话，返回一个 Series，而不是展开类似列表的结果。这与 expand 相反
>     - ‘broadcast’ : 结果将被广播到 DataFrame 的原始形状，原始索引和列将被保留。
>
> - **args: func 的位置参数**
>
>   > `args = (参数1，) ` 注意那个逗号
>
> - **kwargs：要作为关键字参数传递给 func 的其他关键字参数，1.3.0 开始支持
>
> - 返回值：Series 或者 DataFrame：沿数据的给定轴应用 func 的结果

### 1.2 `pd.map()`

```python
#①使用字典进行映射
data["gender"] = data["gender"].map({"男":1, "女":0})
​
#②使用函数
def gender_map(x):
    gender = 1 if x == "男" else 0
    return gender
#注意这里传入的是函数名，不带括号
data["gender"] = data["gender"].map(gender_map)
```

- 不论是利用字典还是函数进行映射，`map`方法都是把对应的数据**逐个当作参数**传入到字典或函数中，得到映射后的值。传入`map`的函数只接受一个参数，即遍历的元素，而`apply`可以接受多个

## 2. 修改`DataFrame` 

### 2.1.apply

对`DataFrame`而言，`apply`是非常重要的数据处理方法，它可以接收各种各样的函数（Python内置的或自定义的），处理方式很灵活，下面通过几个例子来看看`apply`的具体使用及其原理。

在进行具体介绍之前，首先需要介绍一下`DataFrame`中`axis`的概念，在`DataFrame`对象的大多数方法中，都会有`axis`这个参数，它控制了你指定的操作是沿着0轴还是1轴进行。`axis=0`代表操作对`列columns`进行，`axis=1`代表操作对`行row`进行，如下图所示。

```
# 沿着0轴求和
data[["height","weight","age"]].apply(np.sum, axis=0)
​
# 沿着0轴取对数
data[["height","weight","age"]].apply(np.log, axis=0)
```

### 2.2 applymap

`applymap`的用法比较简单，会对`DataFrame`中的每个单元格执行指定函数的操作

```python
df.applymap(lambda x:"%.2f" % x)
```

## 3. 合并多个dataframe

> http://t.csdn.cn/th5Ur 参考博客

```python
pd.concat(objs, axis=0, join='outer', join_axes=None, ignore_index=False,
          keys=None, levels=None, names=None, verify_integrity=False,
          copy=True)
```

```python
frames = [df1, df2, df3]
result = pd.concat(frames)
```

![img](https://img-blog.csdn.net/20170113165333543?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbWlsdG9uMjAxNw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)