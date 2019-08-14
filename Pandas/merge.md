
# 连接

    merge，join用于连接
    Pandas具有全功能的，高性能内存中连接操作，与SQL等关系数据库非常相似

语法
    
    pd.merge(left, right, how='inner', on=None, left_on=None, right_on=None,
             left_index=False, right_index=False, sort=True,
             suffixes=('_x', '_y'), copy=True, indicator=False)
 
 
#      

    on设置连接列
    参数how → 合并方式
    
    df1 = pd.DataFrame({'key': ['K0', 'K1', 'K2', 'K3'],
                         'A': ['A0', 'A1', 'A2', 'A3'],
                         'B': ['B0', 'B1', 'B2', 'B3']})
    df2 = pd.DataFrame({'key': ['K0', 'K1', 'K2', 'K3'],
                          'C': ['C0', 'C1', 'C2', 'C3'],
                          'D': ['D0', 'D1', 'D2', 'D3']})
    df3 = pd.DataFrame({'key1': ['K0', 'K0', 'K1', 'K2'],
                        'key2': ['K0', 'K1', 'K0', 'K1'],
                        'A': ['A0', 'A1', 'A2', 'A3'],
                        'B': ['B0', 'B1', 'B2', 'B3']})
    df4 = pd.DataFrame({'key1': ['K0', 'K1', 'K1', 'K2'],
                        'key2': ['K0', 'K0', 'K0', 'K0'],
                        'C': ['C0', 'C1', 'C2', 'C3'],
                        'D': ['D0', 'D1', 'D2', 'D3']})
    

    print(pd.merge(df1, df2, on='key'))
    print(pd.merge(df3, df4, on=['key1','key2']))
    

    print(pd.merge(df3, df4,on=['key1','key2'], how = 'inner'))  
    # inner：默认，取交集
    
    print(pd.merge(df3, df4, on=['key1','key2'], how = 'outer'))  
    # outer：取并集，数据缺失范围NaN
    
    print(pd.merge(df3, df4, on=['key1','key2'], how = 'left'))  
    # left：按照df3为参考合并，数据缺失范围NaN
    
    print(pd.merge(df3, df4, on=['key1','key2'], how = 'right'))  
    # right：按照df4为参考合并，数据缺失范围NaN
    


# 参数 left_on, right_on, left_index, right_index → 当键不为一个列时，可以单独设置左键与右键
    
    df1 = pd.DataFrame({'lkey':list('bbacaab'),
                       'data1':range(7)})
    df2 = pd.DataFrame({'rkey':list('abd'),
                       'date2':range(3)})
    print(pd.merge(df1, df2, left_on='lkey', right_on='rkey'))
    print('------')
    # df1以‘lkey’为键，df2以‘rkey’为键
    
    df1 = pd.DataFrame({'key':list('abcdfeg'),
                       'data1':range(7)})
    df2 = pd.DataFrame({'date2':range(100,105)},
                      index = list('abcde'))
    print(pd.merge(df1, df2, left_on='key', right_index=True))
    # df1以‘key’为键，df2以index为键
    # left_index：为True时，第一个df以index为键，默认False
    # right_index：为True时，第二个df以index为键，默认False
    
    # 所以left_on, right_on, left_index, right_index可以相互组合：
    # left_on + right_on, left_on + right_index, left_index + right_on, left_index + right_index
    

# pd.join() → 直接通过索引链接
    
    left = pd.DataFrame({'A': ['A0', 'A1', 'A2'],
                         'B': ['B0', 'B1', 'B2']},
                        index=['K0', 'K1', 'K2'])
    right = pd.DataFrame({'C': ['C0', 'C2', 'C3'],
                          'D': ['D0', 'D2', 'D3']},
                         index=['K0', 'K2', 'K3'])
    print(left)
    print(right)
    print(left.join(right))
    print(left.join(right, how='outer'))  
    print('-----')
    # 等价于：pd.merge(left, right, left_index=True, right_index=True, how='outer')
    
    df1 = pd.DataFrame({'key':list('bbacaab'),
                       'data1':[1,3,2,4,5,9,7]})
    df2 = pd.DataFrame({'key':list('abd'),
                       'date2':[11,2,33]})
    print(df1)
    print(df2)
    print(pd.merge(df1, df2, left_index=True, right_index=True, suffixes=('_1', '_2')))  
    print(df1.join(df2['date2']))
    print('-----')
    # suffixes=('_x', '_y')默认
    
    left = pd.DataFrame({'A': ['A0', 'A1', 'A2', 'A3'],
                         'B': ['B0', 'B1', 'B2', 'B3'],
                         'key': ['K0', 'K1', 'K0', 'K1']})
    right = pd.DataFrame({'C': ['C0', 'C1'],
                          'D': ['D0', 'D1']},
                         index=['K0', 'K1'])
    print(left)
    print(right)
    print(left.join(right, on = 'key'))
    # 等价于pd.merge(left, right, left_on='key', right_index=True, how='left', sort=False);
    # left的‘key’和right的index
    


# concat

连接

    s1 = pd.Series([1,2,3])
    s2 = pd.Series([2,3,4])
    s3 = pd.Series([1,2,3],index = ['a','c','h'])
    s4 = pd.Series([2,3,4],index = ['b','e','d'])
    print(pd.concat([s1,s2]))
    print(pd.concat([s3,s4]).sort_index())
    print('-----')

    
    0    1
    1    2
    2    3
    0    2
    1    3
    2    4
    dtype: int64
    a    1
    b    2
    c    2
    d    4
    e    3
    h    3
    
axis
    
    # 默认axis=0，行+行
    print(pd.concat([s3,s4], axis=1))
    print('-----')
    # axis=1,列+列，成为一个Dataframe
    
    
    dtype: int64
    -----
         0    1
    a  1.0  NaN
    b  NaN  2.0
    c  2.0  NaN
    d  NaN  4.0
    e  NaN  3.0
    h  3.0  NaN


join

    s5 = pd.Series([1,2,3],index = ['a','b','c'])
    s6 = pd.Series([2,3,4],index = ['b','c','d'])
    print(pd.concat([s5,s6], axis= 1))
    print(pd.concat([s5,s6], axis= 1, join='inner'))
    print(pd.concat([s5,s6], axis= 1, join_axes=[['a','b','d']]))
    # join：{'inner'，'outer'}，默认为“outer”。如何处理其他轴上的索引。outer为联合和inner为交集。
    # join_axes：指定联合的index

    
         0    1
    a  1.0  NaN
    b  2.0  2.0
    c  3.0  3.0
    d  NaN  4.0
       0  1
    b  2  2
    c  3  3
         0    1
    a  1.0  NaN
    b  2.0  2.0
    d  NaN  4.0


覆盖列名

    sre = pd.concat([s5,s6], keys = ['one','two'])
    print(sre,type(sre))
    print(sre.index)
    print('-----')
    # keys：序列，默认值无。使用传递的键作为最外层构建层次索引
    
    sre = pd.concat([s5,s6], axis=1, keys = ['one','two'])
    print(sre,type(sre))
    # axis = 1, 覆盖列名
    
    
    one  a    1
         b    2
         c    3
    two  b    2
         c    3
         d    4
    dtype: int64 <class 'pandas.core.series.Series'>
    MultiIndex(levels=[['one', 'two'], ['a', 'b', 'c', 'd']],
               labels=[[0, 0, 0, 1, 1, 1], [0, 1, 2, 1, 2, 3]])
    -----
       one  two
    a  1.0  NaN
    b  2.0  2.0
    c  3.0  3.0
    d  NaN  4.0 <class 'pandas.core.frame.DataFrame'>
    

# combine_first
    
    # 修补 pd.combine_first()
    
    df1 = pd.DataFrame([[np.nan, 3., 5.], [-4.6, np.nan, np.nan],[np.nan, 7., np.nan]])
    df2 = pd.DataFrame([[-42.6, np.nan, -8.2], [-5., 1.6, 4]],index=[1, 2])
    print(df1)
    print(df2)
    print(df1.combine_first(df2))
    print('-----')
    # 根据index，df1的空值被df2替代
    # 如果df2的index多于df1，则更新到df1上，比如index=['a',1]
    
    df1.update(df2)
    print(df1)
    # update，直接df2覆盖df1，相同index位置
    
         0    1    2
    0  NaN  3.0  5.0
    1 -4.6  NaN  NaN
    2  NaN  7.0  NaN
          0    1    2
    1 -42.6  NaN -8.2
    2  -5.0  1.6  4.0
         0    1    2
    0  NaN  3.0  5.0
    1 -4.6  NaN -8.2
    2 -5.0  7.0  4.0
    -----
          0    1    2
    0   NaN  3.0  5.0
    1 -42.6  NaN -8.2
    2  -5.0  1.6  4.0
    
