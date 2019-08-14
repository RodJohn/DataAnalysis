
# timestamp


    import numpy as np
    import pandas as pd
    
    date1 = datetime.datetime(2016,12,1,12,45,30)  # 创建一个datetime.datetime
    date2 = '2017-12-21'  # 创建一个字符串
    t1 = pd.Timestamp(date1)
    t2 = pd.Timestamp(date2)
    print(t1,type(t1))
    print(t2)
    print(pd.Timestamp('2017-12-21 15:00:22'))
    # 直接生成pandas的时刻数据 → 时间戳
    # 数据类型为 pandas的Timestamp
    

# pd.to_datetime

    # pd.to_datetime → 多个时间数据转换时间戳索引
    
    date1 = [datetime(2015,6,1),datetime(2015,7,1),datetime(2015,8,1),datetime(2015,9,1),datetime(2015,10,1)]
    date2 = ['2017-2-1','2017-2-2','2017-2-3','2017-2-4','2017-2-5','2017-2-6']
    print(date1)
    print(date2)
    t1 = pd.to_datetime(date2)
    t2 = pd.to_datetime(date2)
    print(t1)
    print(t2)
    # 多个时间数据转换为 DatetimeIndex
    
    date3 = ['2017-2-1','2017-2-2','2017-2-3','hello world!','2017-2-5','2017-2-6']
    t3 = pd.to_datetime(date3, errors = 'ignore')
    print(t3,type(t3))
    # 当一组时间序列中夹杂其他格式数据，可用errors参数返回
    # errors = 'ignore':不可解析时返回原始输入，这里就是直接生成一般数组
    
    t4 = pd.to_datetime(date3, errors = 'coerce')
    print(t4,type(t4))
    # errors = 'coerce':不可扩展，缺失值返回NaT（Not a Time），结果认为DatetimeIndex
    

# pd.DatetimeIndex()与TimeSeries时间序列
    
    rng = pd.DatetimeIndex(['12/1/2017','12/2/2017','12/3/2017','12/4/2017','12/5/2017'])
    print(rng,type(rng))
    print(rng[0],type(rng[0]))
    # 直接生成时间戳索引，支持str、datetime.datetime
    # 单个时间戳为Timestamp，多个时间戳为DatetimeIndex
    
    st = pd.Series(np.random.rand(len(rng)), index = rng)
    print(st,type(st))
    print(st.index)
    # 以DatetimeIndex为index的Series，为TimeSries，时间序列
    

# date_range

    # pd.date_range()-日期范围：生成日期范围
    # 2种生成方式：①start + end； ②start/end + periods
    # 默认频率：day

    rng1 = pd.date_range('1/1/2017','1/10/2017', normalize=True)
    rng2 = pd.date_range(start = '1/1/2017', periods = 10)
    rng3 = pd.date_range(end = '1/30/2017 15:00:00', periods = 10)  # 增加了时、分、秒
    print(rng1,type(rng1))
    print(rng2)
    print(rng3)
    print('-------')
    # 直接生成DatetimeIndex
    # pd.date_range(start=None, end=None, periods=None, freq='D', tz=None, normalize=False, name=None, closed=None, **kwargs)
    # start：开始时间
    # end：结束时间
    # periods：偏移量
    # freq：频率，默认天，pd.date_range()默认频率为日历日，pd.bdate_range()默认频率为工作日
    # tz：时区

    rng4 = pd.date_range(start = '1/1/2017 15:30', periods = 10, name = 'hello world!', normalize = True)
    print(rng4)
    print('-------')
    # normalize：时间参数值正则化到午夜时间戳（这里最后就直接变成0:00:00，并不是15:30:00）
    # name：索引对象名称

    print(pd.date_range('20170101','20170104'))  # 20170101也可读取
    print(pd.date_range('20170101','20170104',closed = 'right'))
    print(pd.date_range('20170101','20170104',closed = 'left'))
    print('-------')
    # closed：默认为None的情况下，左闭右闭，left则左闭右开，right则左开右闭

    print(pd.bdate_range('20170101','20170107'))
    # pd.bdate_range()默认频率为工作日
    
    print(list(pd.date_range(start = '1/1/2017', periods = 10)))
    # 直接转化为list，元素为Timestamp    
    
