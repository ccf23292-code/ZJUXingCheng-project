网站[https://matplotlib.org]

## 学习进度

- 2026-07-05：完成 matplotlib 数据可视化入门，已掌握基础绘图流程。
- 当前已会：`plt.subplots()` 建图、`plot()`/`scatter()`/`hist()` 基本用法、坐标轴标签与标题、`legend()`、线条样式、注释 `annotate()`、基础坐标范围设置。


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

# s是反映每个点大小的参数
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

## 10.imshow()显示二维图像

***imshow是 image show 的缩写，把二维数组中的每个数值显示成一个像素，CNN中常用来查看输入图片和特征图***

* 二维数组 `(height, width)` 会显示为灰度图或伪彩色图
* 三维数组 `(height, width, 3)` 会按照 RGB 彩色图显示
* `cmap` 是 color map 的缩写，只对二维数值图像起作用

```python
import matplotlib.pyplot as plt
	import numpy as np

# 创建一张8×8的模拟灰度图片，每个元素代表一个像素值
image = np.arange(64).reshape(8,8)

fig, ax = plt.subplots(figsize = (5,2.7), layout = 'constrained')

# imshow返回AxesImage对象，cmap='gray'表示使用灰度颜色
im = ax.imshow(image,cmap = 'gray',interpolation = 'nearest')

ax.set_title('8×8 Image')
ax.axis('off') # 关闭坐标轴刻度，观察图片时通常不需要显示坐标

# colorbar显示像素数值和颜色之间的对应关系
fig.colorbar(im,ax = ax)

plt.show()
```

### 常用参数

```python
ax.imshow(
	image,
	cmap = 'gray',          # 灰度颜色，数值越大通常越亮
	vmin = 0,               # 颜色映射的最小值
	vmax = 63,              # 颜色映射的最大值
	interpolation = 'nearest' # 不平滑像素，保留原始格子
)
```

* 比`vmin`小的都用最低颜色，比`vamx`大的都用最高颜色
* `interpolation`是插值方式，`nearest`表示使用最近邻，不对像素进行平滑
	* `origin`默认是`upper`，即数组第0行显示在图片最上方

### imshow返回的对象

```python
im = ax.imshow(image,cmap = 'gray')
im.set_cmap('plasma') # 修改整张图像的颜色映射
im.set_clim(0,63) # 修改颜色显示范围，相当于设置vmin和vmax
im.set_data(new_image) # 替换整张图像的数据
```

**`im`是一个AxesImage对象，代表整张图像，类似于`plot()`返回的Line2D对象代表整条曲线**

### CNN中同时显示多张样本

```python
from sklearn.datasets import load_digits
import matplotlib.pyplot as plt

digits = load_digits()

# 创建2行5列，共10个Axes
fig, axs = plt.subplots(2,5,figsize = (10,4),layout = 'constrained')

# axs.ravel()把2×5的Axes数组展开，方便和图片、标签一起遍历
for ax,image,label in zip(axs.ravel(),digits.images[:10],digits.target[:10]):
	ax.imshow(image,cmap = 'gray',interpolation = 'nearest')
	ax.set_title(f'Label: {label}')
	ax.axis('off')

fig.savefig('digits_samples.png',dpi = 150) # 在show之前保存图片
plt.show()
```

* `digits.images`的形状是`(样本数,8,8)`，每个样本是一张8×8灰度图片
* `digits.target`保存每张图片对应的数字标签
* `ravel`表示展平，这里把2×5的`axs`变成长度为10的一维序列
* `savefig`保存整个Figure，`dpi`是 dots per inch，表示每英寸像素点数量

## 11. 手写数字识别练习

```python
import matplotlib.pyplot as plt

from sklearn.datasets import load_digits

  

digits = load_digits()

images = digits.images[:6] # 8*8

labels = digits.target[:6] # 正确对应的数字

  

fig,axs = plt.subplots(2,3,figsize = (10,4), layout = 'constrained')

i = 0

for ax in axs.ravel(): # ravel 将二维数组展开为一维Numpy数组

        im = ax.imshow(images[i], cmap='gray', vmin=0, vmax=16, interpolation='nearest')

        ax.set_title(f"Label: {labels[i]}")

        i = i + 1

  

fig.colorbar(im,ax=axs.ravel().tolist()) # 转成一维列表

fig.suptitle('Handwritten Digits') # 给画布添加总标题

ax.axis('off')

fig.savefig("digits_preview.png", dpi=150) # 保存图片

plt.show()
```