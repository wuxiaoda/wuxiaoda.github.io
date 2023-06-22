---
layout:     post
title:      "Python常用语法"
subtitle:   "Python_Common_Syntax"
date:       2020-09-24
author:     "邬小达"
header-img: "img/post-bg-js-version.jpg"
tags:
    - 人工智能
---

## 基础

```
import numpy as np

improt pandas as pd

def function(a, b, c):

for i in range(0,10):

os.path.join(learn_dir, "Learn.txt")

",".join(['a', 'b', 'c'])

var_same = [var for var in var_names if var not in ['code', 'calendarDate']]

d1 = dict()
d2 = dict()
d2['x1'] = [1,5,10]
d1.update(d2)

pow((1+0.25), 250/T) - 1 # 年化收益率：(1+实际收益率）^(全年天数/投资天数)-1

print("输出数字:{}".format(5))
```

## 排序

```
data.sort_values(by='x1', ascending=True) # 按照变量x1升序

x.sort() # 升序

np.sort(x) # 升序

np.argsort(x) # 返回排好序的索引值

data.reindex(columns=['x3', 'x2', 'x1'])
```

## 切片

```
x[:, 0] # x的第一列

x[0, :] # x的第一行

data.loc[1:3] # index的值相对应

data.iloc[1:3] # index顺序相对应

data.loc[data['x1'] == 9, 'Y'] # 不能用data.loc[data['x1'] == 9, 3]
```

## pandas操作

```
data = pd.read_csv(filename, usecols=['x1', 'x2', 'x3'],
				   na_values=["NA", "N/A", "null", "NULL", "?"],
				   encoding="UTF-8")
				   
data.to_csv(fielname, index=None, encoding='gbk')

data = pd.DataFrame()

data[((data['x1'] >3) & (data['x1'] <5))]

ind0 = Y[Y == 0].index

data = data.reset_index(drop=True)

k = data["x1"].quantile(q=[0, 0.5, 0.9])
k.loc[0.5]

pd.DataFrame(dict(x1=[50, 100, 200], x2=[5, 10, 15])) # 构建数据框的两种方法
pd.DataFrame({'x1':[50,100,200], 'x2':[5, 10, 15]})

dataprocess = dict()
dataprocess['x1'] = [50, 100, 200]
dataprocess['x2'] = [5, 10, 15]

data.columns = ['a', 'b']

data.shape[0] # 行数
data.shape[1] # 列数
nrows, ncols = data.shape
len(data['x1'])

data['x1'].idxmin()
data['x1'].idxmax() # 最大和最小元素的索引
data['x1'].index(max(data['x1']))

pd.date_range('2014-01-01', '2020-08-18', freq='M')

data['x1'] = [0]

data.index = pd.to_datetime(data['date'])
data.loc[pd.to_datetime('2020-08-18'), 'x1']

data[data['x1'] = 15]['x2'].values[0]
```

## 合并

```
data_all = pd.merge(data1, data2, on='x1', how='right') # 根据共有的变量X1进行合并

data = pd.concat([data1, data2], axis=0, ignore_index=True) # 对行进行叠加，增加行
data = pd.concat([data1, data2], axis=1)  # 对列进行叠加，增加列。其中两个数据集的行索引相同、列名不同
```

## 数据处理

```
data.isna().any() #判断是否存在缺失值
np.isnan(data['x1']) # 判断是否有缺失值

y_nan = data['Y'].isnull() # 处理缺失值
data = data[~y_nan]

data = data.fillna(method='ffill') # 原始特征的缺失值使用上一期填补
data['x1'].fillna(imputation_value, inplace=True)

data.drop_duplicates(['x1'], inplace=True, keep='first')
data.drop_duplicates(subset=['x1', 'x2'], keep='first', inplace=True)
data.drop(columns=['x1'], inplace=True)
```

## 分割、应用和组合

```
data_sum = data.groupby('x1')['x2'].sum()

data_sum = data.groupby('x1')['x2'].agg(['sum','count']) #基于x1分组，并计算x2的累加值和总数

data_sum = data.groupby('x1').agg({'x2':'min', 'x3':'max'})

data_sum = data.groupby('x1').apply(func)
```

## 统计分析

```

data['x1'].astype(int).sum()

data['x1'].unique()

f = lambda x: x.max()
data_max = data.apply(f, axis=1) # 比较两个元素的大小，并返回最大的那个元素

data['x1'].cumsum()
data['x1'].cumprod()
n1 = data['x1'][data['x1'] == 1].count()

data['x1'].rolling(20).sum()
data['x1'].rolling(20).std() * np.sqrt(250)
data['x1'].shift(20)
abs(data['x1'])

# 按照日期分组，然后将pred进行离散化分成10组
param_dict = {'q':num,'labels':range(10, 0, -1)}
df.groupby('calendarDate')['pred'].apply(pd.qcut, **param_dict)
```


## 数据重塑

宽表

```python
mydata=pd.DataFrame({
"Name":["苹果","脸书","腾讯"],
"Conpany":["Apple","Facebook","Tencent"],
"Sale2013":[5000,2300,3100],
"Sale2014":[5050,2900,3300],
"Sale2015":[5050,2900,3300],
"Sale2016":[5050,2900,3300]
       })
```

| Name | Company  | Sale2013 | Sale2014 | Sale2015 | Sale2016 |
| ---- | -------- | -------- | -------- | -------- | -------- |
| 苹果 | Apple    | 5000     | 5050     | 5050     | 5050     |
| 脸书 | Facebook | 2300     | 2900     | 2900     | 2900     |
| 腾讯 | Tencent  | 3100     | 3300     | 3300     | 3300     |

长表

| Name | Company  | YearN     | Sale |
| ---- | -------- | -------- | ---- |
| 苹果 | Apple    | Sale2013 | 5000 |
| 脸书 | Facebook | Sale2013 | 2300 |
| 腾讯 | Tencent  | Sale2013 | 3100 |
| 苹果 | Apple    | Sale2014 | 5050 |
| 脸书 | Facebook | Sale2014 | 2900 |
| 腾讯 | Tencent  | Sale2014 | 3300 |
| 苹果 | Apple    | Sale2015 | 5050 |
| 脸书 | Facebook | Sale2015 | 2900 |
| 腾讯 | Tencent  | Sale2015 | 3300 |
| 苹果 | Apple    | Sale2016 | 5050 |
| 脸书 | Facebook | Sale2016 | 2900 |
| 腾讯 | Tencent  | Sale2016 | 3300 |

* melt 宽表转长表

```python
mydata1=mydata.melt(
id_vars=["Name","Conpany"],   #要保留的主字段
var_name="YearN",                     #拉长的分类变量
value_name="Sale"                  #拉长的度量值名称
        )
```

* pivot_table：长表转宽表（数据透视的过程）

```python
mydata1.pivot_table(
index=["Name","Conpany"],    #行索引（可以使多个类别变量）
columns=["YearN"],                   #列索引（可以使多个类别变量）
values="Sale"                       #值（一般是度量指标）
     )
```

## 机器学习：scikit-learn

```
from sklearn import tree
from sklearn.ensemble import RandomForestClassifier
from sklearn.ensemble import GradientBoostingClassifier
from xgboost.sklearn import XGBClassifier

model.predict_proba(data)[:, 1]


RandomForestClassifier(n_estimators=hparms['n_estimators'],
                       max_depth=hparms['max_depth'],
                       random_state=123)
                       
XGBClassifier(max_depth=hparms['max_depth'], learning_rate=hparms['learning_rate'],
              n_estimators=500, objective='binary:logistic', eval_metric='auc',
              subsample=hparms['subsample'], random_state=123)
              
tree.DecisionTreeClassifier(max_depth=None, max_features=None, 
                            min_samples_split=2, random_state=123)
                            
GradientBoostingClassifier(n_estimators=1000, learning_rate=0.1, max_depth=3,
                           random_state=123)

round(metrics.roc_auc_score(Y, x_predict), 4)
```

## 画图

```
import matplotlib.pyplot as plt

# 变量分布图
fig = plt.figure()
ax = fig.add_subplot(111)
ax.hist(x, 100, color='blue', alpha=0.8, rwidth=0.9)
plt.title("the distribution of x" )
plt.xlabel("x1")
plt.ylabel('count')
plt.show()
fig.savefig(output, dpi=160, bbox_inches='tight')

# 条形图
fig = plt.figure()
plt.bar(range(len(data['x1']), data['x1'])
plt.title("variable importance")
plt.xlabel('variable')
plt.ylabel('variable importance')
plt.show()
fig.savefig(os.path.join(learn_dir, "VariableImportanceDistribution.png"), dpi=160, bbox_inches='tight')

# 净值曲线
plt.figure(figsize=(10, 5))
plt.rcParams['font.sans-serif'] = ['SimHei']  # 用来正常显示中文标签
plt.rcParams['axes.unicode_minus'] = False  # 用来正常显示负号
plt.title("模拟盘交易业绩与沪深300指数对比图")
plt.xticks(rotation=45)
plt.xlabel("交易日期")
plt.ylabel("折算单位净值")
plt.plot_date(data['date'], data['x1'], '-', label="模拟盘折算单位净值")
plt.plot_date(data['date'], data['x2'], '-', label="沪深300折算单位净值")
plt.xticks(rotation=45)

plt.legend()
plt.grid()
fig.savefig(os.path.join(output, 'ResultsOfComparedWithHS300.png'), dpi=160, bbox_inches='tight')
```

## Python中单下划线和双下划线

```
1、_name_：一种约定，Python内部的名字，用来与用户自定义的名字区分开，防止冲突。
```

```
2、_name：一种约定，用来指定变量私有。
```

```
3、_name：解释器用_classname__name来代替这个名字用以区别和其他类相同的命名。
```

