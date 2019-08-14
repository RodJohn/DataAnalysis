
# 分组

    df = pd.DataFrame({'A' : ['foo', 'bar', 'foo', 'bar','foo', 'bar', 'foo', 'foo'],
                       'B' : ['one', 'one', 'two', 'three', 'two', 'two', 'one', 'three'],
                       'C' : np.random.randn(8),
                       'D' : np.random.randn(8)})
    print(df)
    print(df.groupby('A'))
    print('------')
    # 直接分组得到一个groupby对象，是一个中间数据，没有进行计算
    
    a = df.groupby('A').mean()
    b = df.groupby(['A','B']).mean()
    c = df.groupby(['A'])['D'].mean()  # 以A分组，算D的平均值
    # 可单个或多个（[]）列分组
    
         A      B         C         D
    0  foo    one  0.891765 -1.121156
    1  bar    one -1.272769  1.188977
    2  foo    two  0.198131  0.543673
    3  bar  three -0.827655 -1.608699
    4  foo    two -1.114089 -0.696145
    5  bar    two -0.345336  0.718507
    6  foo    one -0.207091 -0.922269
    7  foo  three -0.431760 -0.123696
         
                C         D
    A                      
    bar -0.815253  0.099595
    foo -0.132609 -0.463918 <class 'pandas.core.frame.DataFrame'> 
    
                      C         D
    A   B                        
    bar one   -1.272769  1.188977
        three -0.827655 -1.608699
        two   -0.345336  0.718507
    foo one    0.342337 -1.021713
        three -0.431760 -0.123696
        two   -0.457979 -0.076236 <class 'pandas.core.frame.DataFrame'> 
    
    A
    bar    0.099595
    foo   -0.463918

# 分组对象

    df = pd.DataFrame({'X' : ['A', 'B', 'A', 'B'], 'Y' : [1, 4, 3, 2]})
    print(df)

           X  Y
        0  A  1
        1  B  4
        2  A  3
        3  B  2
    
    print(list(df.groupby('X')), '→ 可迭代对象，直接生成list\n')
    # n是组名，g是分组后的Dataframe
    for n,g in df.groupby('X'):
        print(n)
        print(g)
        print('###')
        
        
        [('A',    X  Y
        0  A  1
        2  A  3), ('B',    X  Y
        1  B  4
        3  B  2)] → 可迭代对象，直接生成list
        
        A
           X  Y
        0  A  1
        2  A  3
        ###
        B
           X  Y
        1  B  4
        3  B  2
        ###
        
    grouped = df.groupby(['X'])
    print(grouped.groups)
        
        {'B': [1, 3], 'A': [0, 2]}
        
    print(grouped.groups['A'])  
    
        [0, 2]
    
    sz = grouped.size()
    
        X
        A    2
        B    2
        
    
# 按列分组

    
    df = pd.DataFrame({'data1':np.random.rand(2),
                      'data2':np.random.rand(2),
                      'key1':['a','b'],
                      'key2':['one','two']})
    print(df)
    print(df.dtypes)
    print('-----')
    for n,p in df.groupby(df.dtypes, axis=1):
        print(n)
        print(p)
        print('##')
    
          data1     data2 key1 key2
    0  0.454580  0.692637    a  one
    1  0.496928  0.214309    b  two
    data1    float64
    data2    float64
    key1      object
    key2      object
    dtype: object
    -----
    float64
          data1     data2
    0  0.454580  0.692637
    1  0.496928  0.214309
    ##
    object
      key1 key2
    0    a  one
    1    b  two
    ##
    
    
# 通过字典或者Series分组

    df = pd.DataFrame(np.arange(16).reshape(4,4),
                      columns = ['a','b','c','d'])
    print(df)
    print('-----')
    
    mapping = {'a':'one','b':'one','c':'two','d':'two','e':'three'}
    by_column = df.groupby(mapping, axis = 1)
    print(by_column.sum())
    print('-----')
    # mapping中，a、b列对应的为one，c、d列对应的为two，以字典来分组
    
    s = pd.Series(mapping)
    print(s,'\n')
    print(s.groupby(s).count())
    # s中，index中a、b对应的为one，c、d对应的为two，以Series来分组
    

        a   b   c   d
    0   0   1   2   3
    1   4   5   6   7
    2   8   9  10  11
    3  12  13  14  15
    -----
       one  two
    0    1    5
    1    9   13
    2   17   21
    3   25   29
    -----
    a      one
    b      one
    c      two
    d      two
    e    three
    dtype: object 
    
    one      2
    three    1
    two      2
    dtype: int64
    

# 分组计算函数方法
    
    s = pd.Series([1, 2, 3, 10, 20, 30], index = [1, 2, 3, 1, 2, 3])
    grouped = s.groupby(level=0)  # 唯一索引用.groupby(level=0)，将同一个index的分为一组
    print(grouped)
    print(grouped.first(),'→ first：非NaN的第一个值\n')
    print(grouped.last(),'→ last：非NaN的最后一个值\n')
    print(grouped.sum(),'→ sum：非NaN的和\n')
    print(grouped.mean(),'→ mean：非NaN的平均值\n')
    print(grouped.median(),'→ median：非NaN的算术中位数\n')
    print(grouped.count(),'→ count：非NaN的值\n')
    print(grouped.min(),'→ min、max：非NaN的最小值、最大值\n')
    print(grouped.std(),'→ std，var：非NaN的标准差和方差\n')
    print(grouped.prod(),'→ prod：非NaN的积\n')
    
   

# 多函数计算：agg()

    df = pd.DataFrame({'a':[1,1,2,2],
                      'b':np.random.rand(4),
                      'c':np.random.rand(4),
                      'd':np.random.rand(4),})
    print(df)
    print(df.groupby('a').agg(['mean',np.sum]))
    print(df.groupby('a')['b'].agg({'result1':np.mean,
                                   'result2':np.sum}))
    # 函数写法可以用str，或者np.方法
    # 可以通过list，dict传入，当用dict时，key名为columns
    
    
       a         b         c         d
    0  1  0.357911  0.318324  0.627797
    1  1  0.964829  0.500017  0.570063
    2  2  0.116608  0.194164  0.049509
    3  2  0.933123  0.542615  0.718640
              b                   c                   d         
           mean       sum      mean       sum      mean      sum
    a                                                           
    1  0.661370  1.322739  0.409171  0.818341  0.598930  1.19786
    2  0.524865  1.049730  0.368390  0.736780  0.384075  0.76815
        result2   result1
    a                    
    1  1.322739  0.661370
    2  1.049730  0.524865
    
        
# transform

    
    df = pd.DataFrame({'data1':np.random.rand(5),
                      'data2':np.random.rand(5),
                      'key1':list('aabba'),
                      'key2':['one','two','one','two','one']})
                      
              data1     data2 key1 key2
        0  0.003727  0.390301    a  one
        1  0.744777  0.130300    a  two
        2  0.887207  0.679309    b  one
        3  0.448585  0.169208    b  two
        4  0.448045  0.993775    a  one
                      
    k_mean = df.groupby('key1').mean()
    
                 data1     data2
        key1                    
        a     0.398850  0.504792
        b     0.667896  0.424258
    
    print(pd.merge(df,k_mean,left_on='key1',right_index=True).add_prefix('mean_'))  # .add_prefix('mean_')：添加前缀
    
           mean_data1_x  mean_data2_x mean_key1 mean_key2  mean_data1_y  mean_data2_y
        0      0.003727      0.390301         a       one      0.398850      0.504792
        1      0.744777      0.130300         a       two      0.398850      0.504792
        4      0.448045      0.993775         a       one      0.398850      0.504792
        2      0.887207      0.679309         b       one      0.667896      0.424258
        3      0.448585      0.169208         b       two      0.667896      0.424258
    
    
    # 通过分组、合并，得到一个包含均值的Dataframe
    
    print(df.groupby('key2').mean()) # 按照key2分组求均值
    print(df.groupby('key2').transform(np.mean))
    # data1、data2每个位置元素取对应分组列的均值
    # 字符串不能进行计算
    
                 data1     data2
        key2                    
        one   0.446326  0.687795
        two   0.596681  0.149754
        
              data1     data2
        0  0.446326  0.687795
        1  0.596681  0.149754
        2  0.446326  0.687795
        3  0.596681  0.149754
        4  0.446326  0.687795


# apply

    df = pd.DataFrame({'data1':np.random.rand(5),
                      'data2':np.random.rand(5),
                      'key1':list('aabba'),
                      'key2':['one','two','one','two','one']})
    
    print(df.groupby('key1').apply(lambda x: x.describe()))
    # apply直接运行其中的函数
    # 这里为匿名函数，直接描述分组后的统计量
    
        
                       data1     data2
        key1                          
        a    count  3.000000  3.000000
             mean   0.561754  0.233470
             std    0.313439  0.337209
             min    0.325604  0.026906
             25%    0.383953  0.038906
             50%    0.442303  0.050906
             75%    0.679829  0.336753
             max    0.917355  0.622599
        b    count  2.000000  2.000000
             mean   0.881906  0.547206
             std    0.079357  0.254051
             min    0.825791  0.367564
             25%    0.853849  0.457385
             50%    0.881906  0.547206
             75%    0.909963  0.637026
             max    0.938020  0.726847
             
    
    def f_df1(d,n):
        return(d.sort_index()[:n])
    def f_df2(d,k1):
        return(d[k1])
    print(df.groupby('key1').apply(f_df1,2),'\n')
    print(df.groupby('key1').apply(f_df2,'data2'))
    print(type(df.groupby('key1').apply(f_df2,'data2')))
    # f_df1函数：返回排序后的前n行数据
    # f_df2函数：返回分组后表的k1列，结果为Series，层次化索引
    # 直接运行f_df函数
    # 参数直接写在后面，也可以为.apply(f_df,n = 2))
    
                   data1     data2 key1 key2
        key1                                
        a    0  0.325604  0.050906    a  one
             1  0.917355  0.622599    a  two
        b    2  0.825791  0.726847    b  one
             3  0.938020  0.367564    b  two 
        
        key1   
        a     0    0.050906
              1    0.622599
              4    0.026906
        b     2    0.726847
              3    0.367564


# pivot_table

    就是分组

    date = ['2017-5-1','2017-5-2','2017-5-3']*3
    rng = pd.to_datetime(date)
    df = pd.DataFrame({'date':rng,
                       'key':list('abcdabcda'),
                      'values':np.random.rand(9)*10})
    
    print(pd.pivot_table(df, values = 'values', index = 'date', columns = 'key', aggfunc=np.sum))  # 也可以写 aggfunc='sum'
    # data：DataFrame对象
    # values：要聚合的列或列的列表
    # index：数据透视表的index，从原数据的列中筛选
    # columns：数据透视表的columns，从原数据的列中筛选
    # aggfunc：用于聚合的函数，默认为numpy.mean，支持numpy计算方法
    
            date key    values
    0 2017-05-01   a  5.886424
    1 2017-05-02   b  9.906472
    2 2017-05-03   c  8.617297
    3 2017-05-01   d  8.972318
    4 2017-05-02   a  7.990905
    5 2017-05-03   b  8.131856
    6 2017-05-01   c  2.823731
    7 2017-05-02   d  2.394605
    8 2017-05-03   a  0.667917
    -----
    key                a         b         c         d
    date                                              
    2017-05-01  5.886424       NaN  2.823731  8.972318
    2017-05-02  7.990905  9.906472       NaN  2.394605
    2017-05-03  0.667917  8.131856  8.617297       NaN
    



    print(pd.pivot_table(df, values = 'values', index = ['date','key'], aggfunc=len))
    print('-----')
    # 这里就分别以date、key共同做数据透视，值为values：统计不同（date，key）情况下values的平均值
    # aggfunc=len(或者count)：计数
    
    date        key
    2017-05-01  a      1.0
                c      1.0
                d      1.0
    2017-05-02  a      1.0
                b      1.0
                d      1.0
    2017-05-03  a      1.0
                b      1.0
                c      1.0

# crosstab

    默认情况下，crosstab计算因子的频率表，比如用于str的数据透视分析
    
    df = pd.DataFrame({'A': [1, 2, 2, 2, 2],
                       'B': [3, 3, 4, 4, 4],
                       'C': [1, 1, np.nan, 1, 1]})
                       
           A  B    C
        0  1  3  1.0
        1  2  3  1.0
        2  2  4  NaN
        3  2  4  1.0
        4  2  4  1.0
    
    
    
    print(pd.crosstab(df['A'],df['B']))
    print('-----')
    # 如果crosstab只接收两个Series，它将提供一个频率表。
    # 用A的唯一值，统计B唯一值的出现次数
    
        B  3  4
        A      
        1  1  0
        2  1  3

    print(pd.crosstab(df['A'],df['B'],normalize=True))
    # normalize：默认False，将所有值除以值的总和进行归一化 → 为True时候显示百分比
    
        B    3    4
        A          
        1  0.2  0.0
        2  0.2  0.6
    

    print(pd.crosstab(df['A'],df['B'],values=df['C'],aggfunc=np.sum))
    # values：可选，根据因子聚合的值数组
    # aggfunc：可选，如果未传递values数组，则计算频率表，如果传递数组，则按照指定计算
    # 这里相当于以A和B界定分组，计算出每组中第三个系列C的值
    
        B    3    4
        A          
        1  1.0  NaN
        2  1.0  2.0


    print(pd.crosstab(df['A'],df['B'],values=df['C'],aggfunc=np.sum, margins=True))
    print('-----')
    # margins：布尔值，默认值False，添加行/列边距（小计）
    
        B      3    4  All
        A                 
        1    1.0  NaN  1.0
        2    1.0  2.0  3.0
        All  2.0  2.0  4.0
        
        