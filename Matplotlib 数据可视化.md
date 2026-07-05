网站[https://matplotlib.org]

## 学习进度

- 2026-07-05：完成 matplotlib 数据可视化入门，已掌握基础绘图流程。
- 当前已会：`plt.subplots()` 建图、`plot()`/`scatter()`/`hist()` 基本用法、坐标轴标签与标题、`legend()`、线条样式、注释 `annotate()`、基础坐标范围设置。
- 下一步：补一个小练习，把同一份数据分别画成折线图、散点图和直方图，顺手练一遍样式与标注。

## 1.头文件
```python
import matplotlib.pyplot as plt

import numpy as np
```
## 2.两张图+贴label
```python

np.random.seed(0)

data = {'a': np.arange(50),                              # 0 到 49，作为 X 轴

        'c': np.random.randint(0, 50, 50),               # 50个 0-49 随机整数，作为颜色

        'd': np.random.randn(50)}                        # 50个标准正态分布数，暂存

data['b'] = data['a'] + 10 * np.random.randn(50)        # a + 噪声，作为 Y 轴

data['d'] = np.abs(data['d']) * 100                      # 取绝对值并放大100倍，作为点的大小

fig,(ax1,ax2) = plt.subplots(1,2,figsize = (10,4),layout = 'constrained')

# fig,ax = plt.subplots(figsize = (5,2.7),layout = 'constrained')

ax1.scatter('a','b',c='c',s='d',data=data)

ax1.set_xlabel('entry a')

ax1.set_ylabel('entry b')

# ax2画linear，quadratic，cubic

ax2.set_title('some common functions')

x = np.linspace(0,2,100)

ax2.plot(x,x,label = 'linear')

ax2.plot(x,x**2,label = 'quadratic')

ax2.plot(x,x**3,label = 'cubic')

ax2.set_xlabel('X')

ax2.set_ylabel('Y')

ax2.legend() # 图片加上方框，把所有label综合起来

  

plt.show()
```

## 3.Making a helpful functions

***创建一个plot函数，类似形式的输出可直接复用***

```python
def my_plotter(ax, data1, data2, param_dict) # param_dict用来传额外参数
	out = ax.plot(data1,data2,**param_dict) # **为解包操作
	return out
```

***示例***

```python
data1,data2,data3,data4 = np.random.randn(4,100)
fig, (ax1,ax2) = plt.subplots(1,2,figsize = (10,4), layout = 'constrained')
my_plotter(ax1,data1,data2,{'marker':'x'})
my_plotter(ax2,data3,data4,{'marker':'o'}) # marker标记，×号和圆圈
plt.show()
```

## 4.style

***设计线条样式***
*注意plot()里面设计颜色用color，在scatter散点图里用c*
*cumsum是累加函数*

```python
fig,ax = plt.subplots(figsize = (5,2.7))
x = np.arange(len(data1))
ax.plot(x,np.cumsum(data1),c = 'blue',linewidth = 3,linestyle = '--') # 最后的参数是说用虚线
l, = ax.plot(x,np.cumsum(data2),color = 'orange',linewidth = 2)
l.set_linestyle(':') # 改成点线
```

**ax.plot()返回一个列表  l,指的是把列表的第一个元素返回给l**

* ***scatter点颜色，边缘和里面不同***

```python
fig,ax = plt.subplots(figsize = (5,2.7))
ax.scatter(data1,data2,s=50,facecolor = 'C0', edgecolor = 'k')
```

## 5.画直方图

```python
n, bins, patches = ax.hist(x, 50, density=True, facecolor='C0', alpha=0.75)
```
- **`ax.hist()`**：画直方图，把数据按区间分组，统计每组有多少个。
- **`x`**：要分析的数据。
- **`50`**：分成 50 个柱子。
- **`density=True`**：不显示频数，而是**归一化为概率密度**，曲线下的总面积 = 1。这样就能和理论正态分布曲线对比。
- **`facecolor='C0'`**：用 Matplotlib 默认色环的第 0 种颜色（蓝色）。
- **`alpha=0.75`**：柱子半透明。
- `n`：每个柱子的高度值（概率密度）
- `bins`：每个柱子的边界值
- `patches`：柱子的图形对象列表
```python
mu, sigma = 115, 15 # 目标均值，标准差
x = mu + sigma * np.random.randn(10000) 正态分布
fig, ax = plt.subplots(figsize=(5, 2.7), layout='constrained')
# the histogram of the data
n, bins, patches = ax.hist(x, 50, density=True, facecolor='C0', alpha=0.75)

ax.set_xlabel('Length [cm]')
ax.set_ylabel('Probability')
ax.set_title('Aardvark lengths\n (not really)')
ax.text(75, .025, r'$\mu=115,\ \sigma=15$') # 前两个坐标，然后是插入文本内容
ax.axis([55, 175, 0, 0.03]) # 展示范围，一般是xmin,xmax,ymin,ymax
ax.grid(True) # 背景显示方格
```
![[Pasted image 20260704151511.png]]
```python
import matplotlib.pyplot as plt

import numpy as np

np.random.seed(0)
scores = np.random.normal(65,15,500)
mean_scores = np.mean(scores)
for i in range(len(scores)):
    if scores[i] < 0:
        scores[i] = 0
    elif scores[i] > 100:
        scores[i] = 100
fig, ax = plt.subplots(figsize = (5,2.7), layout = 'constrained')
n, bins, patches = ax.hist(scores,25, edgecolor = 'C2', alpha = 0.7)
ax.set_xlabel('Score')
ax.set_ylabel('Num of Students')
ax.set_title('Exam Score Distribution')
# 划线横坐标，颜色，虚线，宽度
ax.axvline(mean_scores, color='red', linestyle='--', linewidth=2)
ax.grid(True)
plt.show()
```

## 6.Annotations注释
```python
import matplotlib.pyplot as plt

import numpy as np

fig, ax = plt.subplots(figsize = (5,2.7), layout = 'constrained')
# 生成等差数列
t = np.arange(0.0,5.0,0.01)
s = np.cos(2 * np.pi * t)
# line是返回列表的第一个数据，包含了曲线的信息
line, = ax.plot(t,s,linewidth = 2)
# 注释的生成 shrink是回缩，即箭头和目标点的空袭
ax.annotate('local max', xy = (2,1),xytext = (3,1.5),

            arrowprops = dict(facecolor='red',shrink = 0.06))
# 设置显示范围，其他区域不看
ax.set_ylim(-1.5,1.5)

plt.show()
```

## 7.scale

* 坐标可以线性也可以log，后者的范围类似于10，100，1000
*一段代码示例*
```python
data1 = np.random.randn(100)
fig,axs = plt.subplots(1,2,figsize = (10,4), layout = 'constrained')
xdata = np.arange(len(data1))
data = 10**data1
axs[0].plot(xdata,data,color = 'red', lw = 2)
axs[1].set_yscale('log') # 第二张图的纵坐标刻度改为log，1 10 1000刻度均匀
axs[1].plot(xdata,data,color = 'orange', lw = 3)

```

* 类似代码实现刻度变换
```python
ax.set_xscale('linear/log/symlog/logit') # symlog是对称对数可处理0和负数，ologit处理概率数据
# Limits 显示范围
ax.set_xlim(0,10)
ax.set_xlim(auto=True) # 恢复自动调整
```

## 8.Tick locators and formatters

* 下面的代码实现了把axes的横坐标刻度范围和刻度形式修改

```python
ax.set_xticks(np.arange(1,100,30), ['zero','30','sixty','90'])
```

* plot dates
```python
# 引入头文件
from matplotlib.dates import ConciseDateFormatter

fig, ax = plt.subplots(figsize=(5, 2.7), layout='constrained')
# 它和普通的 `np.arange()` 类似
dates = np.arange(np.datetime64('2021-11-15'), np.datetime64('2021-12-25'),
                  np.timedelta64(1, 'h'))
data = np.cumsum(np.random.randn(len(dates)))
ax.plot(dates, data)
# matplotlib自带的locator
locator = ax.xaxis.get_major_locator()
# 定义新的ConciseDataFormatter,不用matplotlib的formatter
formatter = ConciseDateFormatter(locator)
ax.xaxis.set_major_formatter(formatter)
```

* plot strings
```python
import matplotlib.pyplt as plt
import numpy as np

np.random.seed(0)
fig, ax = plt.subplots(figsize = (5,2.7), layout = 'constrained')
catagories = ['FZJ','QYX','GHZ','GGL']
datas = np.random.rand(len(catagories))
# bar是柱状图
ax.bar(catagories,datas)
plt.show()
```

## 9. Additional axis

* 可以帮助实现两个x轴共用一个y轴，或相反

```python
import matplotlib.pyplot as plt
import numpy as np

  
fig,ax1 = plt.subplots(figsize = (10,4), layout = 'constrained')
t = np.linspace(0,5,100)
s1 = np.cos(2 * t * np.pi)
s2 = np.arange(len(t))
# 画到ax1
l1, = ax1.plot(t,s1)
ax2 = ax1.twinx()
# 画到共享x轴的ax2
l2, = ax2.plot(t,s2,'C2')
ax2.legend([l1,l2], ['Cosine(Left)','Straight(Right)'])
plt.show()
```

