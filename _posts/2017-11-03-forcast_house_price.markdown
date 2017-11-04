---
layout: post
author: 徐生梅
title: "使用 sklearn 进行房价预测（机器学习）"
catalog: true
tags: 机器学习
---

## 目标
通过相似房屋价格来预测新房屋价格。

## 实现方案
借助 sklearn 库构建回归模型预测房价。

## 开发环境
Jupyter Notebook，官网地址：http://ipython.org/notebook.html 具体安装使用教程参见：http://blog.csdn.net/red_stone1/article/details/72858962

## 步骤
- 使用 sklearn 库构建线性回归模型；
- 使用交叉验证方法（即计算预测集和验证集之间的残差值）进行评估；
- 构建多项式回归模型探测房屋数据集里的非线性关系。在此过程中分别画了线性曲线、二次曲线以及三次曲线，计算出这三种情况下的残差值；
- 最终评估出二次回归模型在此场景下性能以及拟合性最好。

## 代码实现
> 说明：项目中使用的数据集 house_data  是已经提供好的，具体下载地址见：http://pan.baidu.com/s/1o7Hb5v8

### 探索房屋数据集
```python
import pandas as pd 
df = pd.read_csv("house_data.csv")
# 为了确保数据存在，先读一下数据，通过 head 方法查看数据集的前几行数据
df.head()
```
结果如下图所示

![数据头](http://upload-images.jianshu.io/upload_images/2950962-7ee641de6fa9a411.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 查看数据集中两两维度的相关性

```python
import matplotlib.pyplot as plt
import seaborn as sns

# 设置内容，使得图在 jupyter notebook 中显示出来
sns.set(context = 'notebook')

#设置维度：人口百分比，房屋年限，与市中心的距离，犯罪率，税，平均房间数
cols = ['LSTAT','AGE','DIS','CRIM','MEDV','TAX','RM']

# 在前台展示图片：两两维度的相关性
plt.show()
```
维度相关性结果如下图所示：

![属性间相关性](http://upload-images.jianshu.io/upload_images/2950962-9e980333d3448250.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 使用 sklearn 构建回归模型
```python
from sklearn.linear_model import LinearRegression
#初始化模型
sk_model = LinearRegression()
sk_model.fit(X, y)
# 打印斜率
print('Slope: %.3f'% sk_model.coef_[0])
# 打印截距
print('Inercept:%.3f'% sk_model.intercept_)
# 画出回归图
Regression_plot(X, y, sk_model)
plt.xlabel('Percentage of the population')
plt.ylabel('House Price')
plt.show()
```
展示结果如下
![image.png](http://upload-images.jianshu.io/upload_images/2950962-a398d4becc697d52.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 评估线性回归模型
```python
from sklearn.cross_validation import train_test_split
# 一些重要的属性
cols = ['LSTAT','AGE','DIS','CRIM','TAX','RM']
X = df[cols].values
y = df['MEDV'].values
# 把数据分为训练集和测试集
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size = 0.25, random_state = 0)
sk_model = LinearRegression()
sk_model.fit(X_train, y_train)
y_train_predict = sk_model.predict(X_train)
y_test_predict = sk_model.predict(X_test)
# 画出训练集与测试集的误差
plt.scatter(y_train_predict, y_train_predict - y_train, c = 'red', marker = 'x', label = 'Trainning data')
plt.scatter(y_test_predict, y_test_predict - y_test, c = 'black', marker = 'o', label = 'Test data')
plt.xlabel('Predicted values')
plt.ylabel('Residuals')
# 增加一个图例在左上角
plt.legend(loc = 'upper left')
plt.hlines(y=0,xmin=0,xmax=50,lw=1,color='green')
plt.xlim([-10,50])
plt.show()
```
结果如下所示

![image.png](http://upload-images.jianshu.io/upload_images/2950962-aaa1c3aef1465b73.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 计算预测集和验证集的残差
 ```python
#第一种评估的标准
from sklearn.metrics import mean_squared_error
print('MSE train %.3f, test %.3f'%(mean_squared_error(y_train,y_train_predict),mean_squared_error(y_test,y_test_predict)))
#第二种评估标准
from sklearn.metrics import r2_score
print('R^2 train %.3f, test %.3f'%(r2_score(y_train,y_train_predict),r2_score(y_test,y_test_predict)))
```
> MSE train 25.106, test 36.671
>R^2 train 0.706, test 0.551


### 用平均房间数预测房价
```python
X= df[['RM']].values
y = df['MEDV'].values
#初始化模型
Regression_model = LinearRegression()
from sklearn.preprocessing import PolynomialFeatures
#二次变换
quadratic = PolynomialFeatures(degree = 2)
#三次线性变换
cubic = PolynomialFeatures(degree = 3)
X_squared = quadratic.fit_transform(X)
X_cubic = cubic.fit_transform(X)
X_fit = np.arange(X.min(), X.max(), 0.01)[:,np.newaxis]
# 画出线性曲线
Linear_model = Regression_model.fit(X, y)
y_line_fit = Linear_model.predict(X_fit)
linear_r2 = r2_score(y, Linear_model.predict(X))

#画出二次曲线
Squared_model = Regression_model.fit(X_squared, y)
y_quad_fit = Squared_model.predict(quadratic.fit_transform(X_fit))
quadratic_r2 = r2_score(y,Squared_model.predict(X_squared))

#画出三次曲线
Cubic_model = Regression_model.fit(X_cubic, y)
y_cubic_fit = Cubic_model.predict(cubic.fit_transform(X_fit))
cubic_r2 = r2_score(y,Cubic_model.predict(X_cubic))

#画出散点图
plt.scatter(X,y,label='Trainning point',color = 'lightgray')
plt.plot(X_fit, y_line_fit, label ='linear,$R^2=%.2f$' % linear_r2, color = 'blue',lw = 2, linestyle = ':')
plt.plot(X_fit, y_quad_fit, label ='quadratic,$R^2=%.2f$' % quadratic_r2, color = 'red',lw = 2, linestyle = '-')
plt.plot(X_fit, y_cubic_fit, label ='cubic,$R^2=%.2f$' % cubic_r2, color = 'green',lw = 2, linestyle = '--')
plt.xlabel('Room number')
plt.ylabel('House price')
plt.legend(loc = 'upper left')
plt.show()
```

结果如图所示

![image.png](http://upload-images.jianshu.io/upload_images/2950962-757791f5714420b9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)