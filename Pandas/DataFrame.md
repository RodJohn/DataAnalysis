

# Dataframe

    "二维数组"Dataframe：是一个表格型的数据结构，
    有index（行标签）和columns（列标签）    

示例

    data = {'name':['Jack','Tom','Mary'],
            'age':[18,19,20],
           'gender':['m','m','w']}
    frame = pd.DataFrame(data)
    print(frame)  
    print(type(frame))
    print(frame.index,'\n该数据类型为：',type(frame.index))
    print(frame.columns,'\n该数据类型为：',type(frame.columns))
    print(frame.values,'\n该数据类型为：',type(frame.values))
    # 查看数据，数据类型为dataframe
    # .index查看行标签
    # .columns查看列标签
    # .values查看值，数据类型为ndarray
    
       age gender  name
    0   18      m  Jack
    1   19      m   Tom
    2   20      w  Mary
    <class 'pandas.core.frame.DataFrame'>
    RangeIndex(start=0, stop=3, step=1) 
    该数据类型为： <class 'pandas.indexes.range.RangeIndex'>
    Index(['age', 'gender', 'name'], dtype='object') 
    该数据类型为： <class 'pandas.indexes.base.Index'>
    [[18 'm' 'Jack']
     [19 'm' 'Tom']
     [20 'w' 'Mary']] 
    该数据类型为： <class 'numpy.ndarray'>


# 创建


数组

    # Dataframe 创建方法一：由数组/list组成的字典
    # 创建方法:pandas.Dataframe()
    
    data1 = {'a':[1,2,3],
            'b':[3,4,5],
            'c':[5,6,7]}
    data2 = {'one':np.random.rand(3),
            'two':np.random.rand(3)}   # 这里如果尝试  'two':np.random.rand(4) 会怎么样？
    print(data1)
    print(data2)
    df1 = pd.DataFrame(data1)
    df2 = pd.DataFrame(data2)
    print(df1)
    print(df2)
    # 由数组/list组成的字典 创建Dataframe，columns为字典key，index为默认数字标签
    # 字典的值的长度必须保持一致！
    
    df1 = pd.DataFrame(data1, columns = ['b','c','a','d'])
    print(df1)
    df1 = pd.DataFrame(data1, columns = ['b','c'])
    print(df1)
    # columns参数：可以重新指定列的顺序，格式为list，如果现有数据中没有该列（比如'd'），则产生NaN值
    # 如果columns重新指定时候，列的数量可以少于原数据
    
    df2 = pd.DataFrame(data2, index = ['f1','f2','f3'])  # 这里如果尝试  index = ['f1','f2','f3','f4'] 会怎么样？
    print(df2)
    # index参数：重新定义index，格式为list，长度必须保持一致

Series

    # Dataframe 创建方法二：由Series组成的字典
    
    data1 = {'one':pd.Series(np.random.rand(2)),
            'two':pd.Series(np.random.rand(3))}  # 没有设置index的Series
    data2 = {'one':pd.Series(np.random.rand(2), index = ['a','b']),
            'two':pd.Series(np.random.rand(3),index = ['a','b','c'])}  # 设置了index的Series
    print(data1)
    print(data2)
    df1 = pd.DataFrame(data1)
    df2 = pd.DataFrame(data2)
    print(df1)
    print(df2)
    # 由Seris组成的字典 创建Dataframe，columns为字典key，index为Series的标签（如果Series没有指定标签，则是默认数字标签）
    # Series可以长度不一样，生成的Dataframe会出现NaN值


二维数组    
    
    ar = np.random.rand(9).reshape(3,3)
    print(ar)
    df1 = pd.DataFrame(ar)
    df2 = pd.DataFrame(ar, index = ['a', 'b', 'c'], columns = ['one','two','three'])  # 可以尝试一下index或columns长度不等于已有数组的情况
    print(df1)
    print(df2)
    # 通过二维数组直接创建Dataframe，得到一样形状的结果数据，如果不指定index和columns，两者均返回默认数字格式
    # index和colunms指定长度与原数组保持一致


# 选择

# 选择行与列
    
    df = pd.DataFrame(np.random.rand(12).reshape(3,4)*100,
                       index = ['one','two','three'],
                       columns = ['a','b','c','d'])
    print(df)
    
    data1 = df['a']
    data2 = df[['a','c']]
    print(data1,type(data1))
    print(data2,type(data2))
    print('-----')
    # 按照列名选择列，只选择一列输出Series，选择多列输出Dataframe
    
    data3 = df.loc['one']
    data4 = df.loc[['one','two']]
    print(data2,type(data3))
    print(data3,type(data4))
    # 按照index选择行，只选择一行输出Series，选择多行输出Dataframe


    # df[] - 选择列
    # 一般用于选择列，也可以选择行
    
    # df.loc[] - 按index选择行
    
# iloc

    # df.iloc[] - 按照整数位置（从轴的0到length-1）选择行
    # 类似list的索引，其顺序就是dataframe的整数位置，从0开始计
    
    df = pd.DataFrame(np.random.rand(16).reshape(4,4)*100,
                       index = ['one','two','three','four'],
                       columns = ['a','b','c','d'])
    print(df)
    print('------')
    
    print(df.iloc[0])
    print(df.iloc[-1])
    #print(df.iloc[4])
    print('单位置索引\n-----')
    # 单位置索引
    # 和loc索引不同，不能索引超出数据行数的整数位置
    
    print(df.iloc[[0,2]])
    print(df.iloc[[3,2,1]])
    print('多位置索引\n-----')
    # 多位置索引
    # 顺序可变
    
    print(df.iloc[1:3])
    print(df.iloc[::2])
    print('切片索引')
    # 切片索引
    # 末端不包含
    
    
# 布尔型索引

    # 和Series原理相同
    
    df = pd.DataFrame(np.random.rand(16).reshape(4,4)*100,
                       index = ['one','two','three','four'],
                       columns = ['a','b','c','d'])
    print(df)
    print('------')
    
    b1 = df < 20
    print(b1,type(b1))
    print(df[b1])  # 也可以书写为 df[df < 20]
    print('------')
    # 不做索引则会对数据每个值进行判断
    # 索引结果保留 所有数据：True返回原数据，False返回值为NaN
    
    b2 = df['a'] > 50
    print(b2,type(b2))
    print(df[b2])  # 也可以书写为 df[df['a'] > 50]
    print('------')
    # 单列做判断
    # 索引结果保留 单列判断为True的行数据，包括其他列
    
    b3 = df[['a','b']] > 50
    print(b3,type(b3))
    print(df[b3])  # 也可以书写为 df[df[['a','b']] > 50]
    print('------')
    # 多列做判断
    # 索引结果保留 所有数据：True返回原数据，False返回值为NaN
    
    b4 = df.loc[['one','three']] < 50
    print(b4,type(b4))
    print(df[b4])  # 也可以书写为 df[df.loc[['one','three']] < 50]
    print('------')
    # 多行做判断
    # 索引结果保留 所有数据：True返回原数据，False返回值为NaN
    
#  

    # 多重索引：比如同时索引行和列
    # 先选择列再选择行 —— 相当于对于一个数据，先筛选字段，再选择数据量
    
    df = pd.DataFrame(np.random.rand(16).reshape(4,4)*100,
                       index = ['one','two','three','four'],
                       columns = ['a','b','c','d'])
    print(df)
    print('------')
    
    print(df['a'].loc[['one','three']])   # 选择a列的one，three行
    print(df[['b','c','d']].iloc[::2])   # 选择b，c，d列的one，three行
    print(df[df['a'] < 50].iloc[:2])   # 选择满足判断索引的前两行数据
    

# 数据查看、转置
    
    df = pd.DataFrame(np.random.rand(16).reshape(8,2)*100,
                       columns = ['a','b'])
    print(df.head(2))
    print(df.tail())
    # .head()查看头部数据
    # .tail()查看尾部数据
    # 默认查看5条
    
    print(df.T)
    # .T 转置
    、
# 添加与修改

    df = pd.DataFrame(np.random.rand(16).reshape(4,4)*100,
                       columns = ['a','b','c','d'])
    print(df)
    
    df['e'] = 10
    df.loc[4] = 20
    print(df)
    # 新增列/行并赋值
    
    df['e'] = 20
    df[['a','c']] = 100
    print(df)
    # 索引后直接修改值
    
# 删除  del / drop()

    df = pd.DataFrame(np.random.rand(16).reshape(4,4)*100,
                       columns = ['a','b','c','d'])
    print(df)
    
    del df['a']
    print(df)
    print('-----')
    # del语句 - 删除列
    
    print(df.drop(0))
    print(df.drop([1,2]))
    print(df)
    print('-----')
    # drop()删除行，inplace=False → 删除后生成新的数据，不改变原数据
    
    print(df.drop(['d'], axis = 1))
    print(df)
    # drop()删除列，需要加上axis = 1，inplace=False → 删除后生成新的数据，不改变原数据    
    
    
# 对齐

    df1 = pd.DataFrame(np.random.randn(10, 4), columns=['A', 'B', 'C', 'D'])
    df2 = pd.DataFrame(np.random.randn(7, 3), columns=['A', 'B', 'C'])
    print(df1 + df2)
    # DataFrame对象之间的数据自动按照列和索引（行标签）对齐
    

    
# 排序1 - 按值排序 .sort_values

    # 同样适用于Series
    
    df1 = pd.DataFrame(np.random.rand(16).reshape(4,4)*100,
                       columns = ['a','b','c','d'])
    print(df1)
    print(df1.sort_values(['a'], ascending = True))  # 升序
    print(df1.sort_values(['a'], ascending = False))  # 降序
    print('------')
    # ascending参数：设置升序降序，默认升序
    # 单列排序
    
    df2 = pd.DataFrame({'a':[1,1,1,1,2,2,2,2],
                      'b':list(range(8)),
                      'c':list(range(8,0,-1))})
    print(df2)
    print(df2.sort_values(['a','c']))
    # 多列排序，按列顺序排序

# 排序2 - 索引排序 .sort_index

    df1 = pd.DataFrame(np.random.rand(16).reshape(4,4)*100,
                      index = [5,4,3,2],
                       columns = ['a','b','c','d'])
    df2 = pd.DataFrame(np.random.rand(16).reshape(4,4)*100,
                      index = ['h','s','x','g'],
                       columns = ['a','b','c','d'])
    print(df1)
    print(df1.sort_index())
    print(df2)
    print(df2.sort_index())
    # 按照index排序
    # 默认 ascending=True, inplace=False
    
    
            