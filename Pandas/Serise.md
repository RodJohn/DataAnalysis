

# Series 

数据结构

    Series 是带有索引的一维数组
    索引可以重复
    
# 查看    

    import numpy as np
    import pandas as pd  
    # 导入numpy、pandas模块
    
    s = pd.Series(np.random.rand(5))
    print(s)
    print(type(s))
    # 查看数据、数据类型
    
    print(s.index,type(s.index))
    print(s.values,type(s.values))
    # .index查看series索引，类型为rangeindex
    # .values查看series值，类型是ndarray
    

    
# 创建


字典创建

    字典的key就是index，values就是values
    
    dic = {'a':1 ,'b':2 , 'c':3, '4':4, '5':5}
    s = pd.Series(dic)
    print(s)

数组创建

    arr = np.random.randn(5)
    s = pd.Series(arr)
    print(arr)
    print(s)
    # 默认index是从0开始，步长为1的数字
    
    s = pd.Series(arr, index = ['a','b','c','d','e'],dtype = np.object)
    print(s)
    # index参数：设置index，长度保持一致
    # dtype参数：设置数值类型

由标量创建

    s = pd.Series(10, index = range(4))
    print(s)
    如果data是标量值，则必须提供索引。该值会重复，来匹配索引的长度

# name

# Series 名称属性：name

    s1 = pd.Series(np.random.randn(5))
    print(s1)
    print('-----')
    s2 = pd.Series(np.random.randn(5),name = 'test')
    print(s2)
    print(s1.name, s2.name,type(s2.name))
    # name为Series的一个参数，创建一个数组的 名称
    # .name方法：输出数组的名称，输出格式为str，如果没用定义输出名称，输出为None
    
    s3 = s2.rename('hehehe')
    print(s3)
    print(s3.name, s2.name)
    # .rename()重命名一个数组的名称，并且新指向一个数组，原数组不变
    
    
# 索引和切片

# 位置下标，类似序列

    s = pd.Series(np.random.rand(5))
    print(s)
    print(s[0],type(s[0]),s[0].dtype)
    print(float(s[0]),type(float(s[0])))
    #print(s[-1])
    # 位置下标从0开始


    s = pd.Series(np.random.rand(5), index = ['a','b','c','d','e'])
    print(s)
    print(s['a'],type(s['a']),s['a'].dtype)
    # 方法类似下标索引，用[]表示，内写上index，注意index是字符串
    
    sci = s[['a','b','e']]
    print(sci,type(sci))
    # 如果需要选择多个标签的值，用[[]]来表示（相当于[]中包含一个列表）
    # 多标签索引结果是新的数组
    
# 切片

    s1 = pd.Series(np.random.rand(5))
    s2 = pd.Series(np.random.rand(5), index = ['a','b','c','d','e'])
    print(s1[1:4],s1[4])
    print(s2['a':'c'],s2['c'])
    print(s2[0:3],s2[3])
    print('-----')
    # 注意：用index做切片是末端包含
    
    print(s2[:-1])
    print(s2[::2])
    # 下标索引做切片，和list写法一样


# 布尔索引

    
    s = pd.Series(np.random.rand(3)*100)
    s[4] = None  # 添加一个空值
    print(s)
    bs1 = s > 50
    bs2 = s.isnull()
    bs3 = s.notnull()
    print(bs1, type(bs1), bs1.dtype)
    print(bs2, type(bs2), bs2.dtype)
    print(bs3, type(bs3), bs3.dtype)
    print('-----')
    # 数组做判断之后，返回的是一个由布尔值组成的新的数组
    # .isnull() / .notnull() 判断是否为空值 (None代表空值，NaN代表有问题的数值，两个都会识别为空值)
    
    print(s[s > 50])
    print(s[bs3])
    # 布尔型索引方法：用[判断条件]表示，其中判断条件可以是 一个语句，或者是 一个布尔型数组！


# 查看

    s = pd.Series(np.random.rand(50))
    print(s.head(10))
    print(s.tail())
    # .head()查看头部数据
    # .tail()查看尾部数据
    # 默认查看5条

# 重新索引reindex
    
    # .reindex将会根据索引重新排序，如果当前索引不存在，则引入缺失值
    
    s = pd.Series(np.random.rand(3), index = ['a','b','c'])
    print(s)
    s1 = s.reindex(['c','b','a','d'])
    print(s1)
    # .reindex()中也是写列表
    # 这里'd'索引不存在，所以值为NaN
    
    s2 = s.reindex(['c','b','a','d'], fill_value = 0)
    print(s2)
    # fill_value参数：填充缺失值的值

# + 

# Series对齐

    s1 = pd.Series(np.random.rand(3), index = ['Jack','Marry','Tom'])
    s2 = pd.Series(np.random.rand(3), index = ['Wang','Jack','Marry'])
    print(s1)
    print(s2)
    print(s1+s2)
    # Series 和 ndarray 之间的主要区别是，Series 上的操作会根据标签自动对齐
    # index顺序不会影响数值计算，以标签来计算
    # 空值和任何值计算结果扔为空值
    
# drop
    
    # 删除：.drop
    
    s = pd.Series(np.random.rand(5), index = list('ngjur'))
    print(s)
    s1 = s.drop('n')
    s2 = s.drop(['g','j'])
    print(s1)
    print(s2)
    print(s)
    # drop 删除元素之后返回副本(inplace=False)
    
    
# 添加

    s1 = pd.Series(np.random.rand(5))
    s2 = pd.Series(np.random.rand(5), index = list('ngjur'))
    print(s1)
    print(s2)
    s1[5] = 100
    s2['a'] = 100
    print(s1)
    print(s2)
    print('-----')
    # 直接通过下标索引/标签index添加值
    
    s3 = s1.append(s2)
    print(s3)
    print(s1)
    # 通过.append方法，直接添加一个数组
    # .append方法生成一个新的数组，不改变之前的数组

# 修改
    
    s = pd.Series(np.random.rand(3), index = ['a','b','c'])
    print(s)
    s['a'] = 100
    s[['b','c']] = 200
    print(s)
    # 通过索引直接修改，类似序列    