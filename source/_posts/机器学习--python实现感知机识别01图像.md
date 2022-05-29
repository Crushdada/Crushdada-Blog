---
title: 机器学习--感知机识别0/1图像
tags:
  - python
  - 机器学习
  - 数字图像处理
categories: Code
abbrlink: 39787
date: 2021-02-11 00:00:00
---

首先，先说好即使你完全不了解机器学习、神经网络和数字图像处理的相关知识，也不用怕，因为总得有个从0到1的过程。

"这个一点也不难，其实你们自己完全能够做一个感知机"...

<!--more-->

## 几个概念

### **机器学习**

**Machine Learning：**通过给机器(计算机)预设一些算法，来让它模拟人的智能，从而相对独立地完成一些任务。

### **神经网络**

**Neural Network：**是实现机器学习任务的一种方法，其结构模拟生物神经系统，与外界交互。

### **感知机**

**Perceptron****：**是一种人工神经网络，是生物神经细胞的简单抽象，它的作用是区分两个具有不同特征的事物。因为感知机是这样一个二元分类模型(区分2个东西)，所以我们使用它来进行0、1图像的区分。

在本篇中，感知机就是一个数组w和一个数b而已。我们要做的就是训练计算机来寻找这样的w和b使其代入感知机模型函数中能够区分0、1图像。

下面给出两幅图给出神经网络与感知机的区别，直观感受一下--

神经网络：

![图片](https://uploader.shimo.im/f/SZxjzS1FluyxqHfZ.png!thumbnail?fileGuid=htJ9wk6kP3G6Gcp8)

感知机：

![图片](https://uploader.shimo.im/f/544p0OVp2eO8ujsl.png!thumbnail?fileGuid=htJ9wk6kP3G6Gcp8)

### 参考链接：

[https://www.cnblogs.com/jiaxin359/p/9011457.html](https://www.cnblogs.com/jiaxin359/p/9011457.html)

[https://www.cnblogs.com/xym4869/p/11282469.html](https://www.cnblogs.com/xym4869/p/11282469.html)

[https://www.cnblogs.com/huangyc/p/9706575.html](https://www.cnblogs.com/huangyc/p/9706575.html)


---


## 实现步骤概述

### **感知机模型**：

f(x)=sign(wx+b)

1. **图像采集**
1. 数据集划分
2. 训练集
3. 测试集
2. **图像预处理**
1. 读取所有图像
2. 灰度化
3. 二值化
4. 打标签：为每个图像设置图像真值y：1或-1
3. **取特征x**
1. 将每幅图像划分为n×n个小格
2. 计算每个小格中黑色像素点个数
3. 该图像的特征x即为一个n*n的矩阵，其中每个元素对应一个小格的黑色像素点个数
4. **模型训练**
1. 读取所有训练集图像及对应特征
2. 初始化模型参数w0,b0，步长η
3. 计算每幅图像的预测值
    1. 计算：f(x)=sign(wx+b)
    2. w0*x+b0＞0，图片预测值f(x)=1
    3. w0*x+b0＜0，图片预测值f(x)=-1
4. 比较每幅图像的预测值f(x)和真值y
    1. 如果都相同，则证明模型参数拟合完好，训练完毕
    2. 当二者不同时，该图像识别错误，根据公式更新w0,b0

w0=w0+η*xi*yi

b0=b0+η*yi

ps：上述公式中xi和yi为该图像的特征值和真值

5. **模型测试(识别测试集图像)**
        1. 读取测试集图像及对应特征
        2. 计算每幅图像的**预测值**
        3. 比较预测值和真值
            1. 二者相同，则证明拟合，记录拟合的图像个数
        4. 得到感知机的**识别正确率**
            1. 以**识别正确率**作为量化感知机好坏的标准

---


## 实际实现：

### 一、图像采集

自己用画图，画两组样本图像，即0和1各10张

我的样本图像存储方式：

![图片](https://uploader.shimo.im/f/JwzQPJrq7Fb28CMp.png!thumbnail?fileGuid=htJ9wk6kP3G6Gcp8)

### 二、取特征

1.传入图片存储路径

2.根据图片名依次遍历10张图片

3.逻辑意义上，将每张图片划分为49维

4.两层循环遍历每个维度，累计黑色像素个数

5.返回二维列表[10][49]

![图片](https://uploader.shimo.im/f/COm3Df2ixwE5Zn3m.png!thumbnail?fileGuid=htJ9wk6kP3G6Gcp8)

### 三、一些初始化

全局变量W：weight0（49维列表）

全局变量b：bias0

学习效率η：learning_rate

打上标签y：向二维列表追加第50个数作为标签

![图片](https://uploader.shimo.im/f/4Tf1tUxzmscQSiCP.png!thumbnail?fileGuid=htJ9wk6kP3G6Gcp8)

### 四、感知机训练函数

![图片](https://uploader.shimo.im/f/2NwCoi8N7jwZL8HK.png!thumbnail?fileGuid=htJ9wk6kP3G6Gcp8)

![图片](https://uploader.shimo.im/f/rSTwUP7utNrQkKu3.png!thumbnail?fileGuid=htJ9wk6kP3G6Gcp8)

### 五、训练函数中用到的cal()和sign()函数

cal(train_weight)--用于计算**wx+b**![图片](https://uploader.shimo.im/f/EgMk5hnrhFyzfdn7.png!thumbnail?fileGuid=htJ9wk6kP3G6Gcp8)

sign(v)—用于确定最终分类(1 or -1)

![图片](https://uploader.shimo.im/f/fHIWTYAvWHUSby9O.png!thumbnail?fileGuid=htJ9wk6kP3G6Gcp8)

### 六、测试感知机

狠简单的一个测试函数**test(test_data)**

1.传入测试数据(10张图片的二维列表，初始化时已经得到

2. 依次遍历10张图片的数据

3.调用cal()函数返回感知器计算得出的结果(1 /-1)

4.比较返回值与标签，累计判断正确的次数

5.输出感知器识别的正确率

![图片](https://uploader.shimo.im/f/4xEOkC09GGoYotGQ.png!thumbnail?fileGuid=htJ9wk6kP3G6Gcp8)

### **End、我的测试结果**

![图片](https://uploader.shimo.im/f/wxt3EanMfvksbLy4.png!thumbnail?fileGuid=htJ9wk6kP3G6Gcp8)


---


## 踩坑

在实现过程中不可避免地遇到了一些问题，

### 其一

计算w*x，由于两者都是较长的列表，因此采用np.multiply()+np.array()进行矩阵相乘，考虑到特征x是多了一个标签[真值y]缀在列表最后面的，而该方法要求两列表等长，因此又预先用list.pop()方法丢掉了最后一位才行...

### 其二

这是一个大坑吧，我觉得：在我测试的过程中，发现无论怎样调参(更改w、b和步长)，感知机运行过程中我预设的一些输出都相同，包括参数的修正次数以及最后的识别正确率。但是没用报错，这意味着要么我的逻辑错了，要么逻辑实现不完善，因此我重新犁了整个思路，最终发现我在循环完一张图像后，识别失败的情况下，**没有重新回到第一幅图**。由于[循环]是按顺序对图片进行一系列操作的，这就导致--可能第三幅图刚刚修正完毕，能够识别成功后，之前的两幅图根据新的w、b却不能识别成功了。

错误之处如下

![图片](https://uploader.shimo.im/f/xufuxk80S3WQzksN.png!thumbnail?fileGuid=htJ9wk6kP3G6Gcp8)

**我原来的想法是直接令m=0，从而控制循环的顺序，但这是错误的。由于某些原因，为m的赋值没有影响到遍历的顺序。**

### 更有意思的事情

运行一个简单的循环进行测试，我发现了更有意思的事情--

![图片](https://uploader.shimo.im/f/wFt59zmk17LmgFkO.png!thumbnail?fileGuid=htJ9wk6kP3G6Gcp8)

你会发现结果为0，1，2，3，4，**4**

而在内部令m=2：

![图片](https://uploader.shimo.im/f/D2bwMwLZ7iUcDdX0.png!thumbnail?fileGuid=htJ9wk6kP3G6Gcp8)

你会发现结果为0，1，2，3，4，**2**

这意味着--for循环中定义的临时变量作用不局限于for循环内，for循环中最后的一次赋值泄露到了循环外。

而最后一次赋值为2后，range()方法返回的列表遍历完毕，没有新的赋值，for循环结束，此时再print，m的值当然为2了。

我错在没有深入理解for循环的原理--

for循环调用了range()方法返回了一个类似列表的可迭代对象，会先按既定的顺序给临时变量m赋值，之后再执行循环体，因此每次赋值m=0都没有起作用。

我现在是Python3，而Python2中range()方法会返回一个列表，详情见--

[https://blog.csdn.net/marsjhao/article/details/56853316](https://blog.csdn.net/marsjhao/article/details/56853316)

既定的顺序由传入的参数确定，详情见--

[https://www.runoob.com/python/python-func-range.html](https://www.runoob.com/python/python-func-range.html)

### 再者

最后就是读取图像的方法太过复杂，我简单粗暴地用循环进行读取，直到全部工作完成，才后知后觉想到能直接用python os模块...

---


## 后记

还有许许多多的小问题和bug，但最终总算是完成了吧。从0到1的一个过程，还是蛮有意思的，也得到了教训--除了自身逻辑思维是有限的，语言工具也是有限的，不同的语言会有不同的局限性，只有熟练地掌握一种工具，才能更好地使用它。

PS：由于程序中使用到绝对路径，无法保证该程序的移植性，因此需要在copy后做一些简单的修改。


---


## 附上源码：

```python
import cv2
import numpy as np
root_path="C:/Users/Administrator/Desktop/test/"
train_path=root_path+"train_imges/"
test_path=root_path+"test_imges/"
def get_data(path):
    data=[]
    for w in range(10): 
        img=cv2.imread(path+str(w+1)+".jpg",0)
        offsetRow=0
        offsetCol=0
        b=100
        m=img.shape[0]//b
        n=img.shape[1]//b
        
        # print(m,n) 
        count_all=[]         #一张图片中49个的小格的各个黑色像素数目
        for i in range(m):  
            offsetRow=i*b
            for j in range(n):
                offsetCol=j*b
                im=img[offsetRow:offsetRow+b-1,offsetCol:offsetCol+b-1]
                # print(offsetRow,offsetRow+b-1,offsetCol,offsetCol+b-1)
                count=0       #一个100*100小格中的黑色像素数目
                for x in range (99):
                    for y in range (99):
                        if(im[x][y]==0):
                            count=count+1
                count_all.append(count)
        data.append(count_all) 
    return data        
#sign函数
def sign(v):
      if v > 0:
          return 1
      else:
        return -1
    
    
#校验W、b
def cal(train_weight):
   
    x=train_weight[49]
    train_weight.pop()
    result = np.multiply(np.array(weight0),np.array(train_weight))
    sum=0
    for k in range(len(result)):
        sum=sum+result[k]
        sum=sum+bias0
    train_weight.append(x)
    judge=sign(sum)
    return judge
    
def training(train_data):
      global weight0
      global bias0
      count=0
      while True:  
            for m in range(10):  #第几幅图像
                if(train_data[m][49]!=cal(train_data[m])): #如果校验错误，则更新w、b
                    for n in range(49): #40列中第几列
                        # 更新权重
                        weight0[n] = weight0[n] + \
                                      learning_rate * \
                                      train_data[m][n] * \
                                      train_data[m][49]  
                        # 更新偏置量
                        bias0 = bias0 + learning_rate * \
                                train_data[m][49]*1.00  
            #10幅图片每循环一轮(最多更新10次W、b)后，检查是否已经全部拟合                   
            for j in range(10):  
                if train_data[j][49]==cal(train_data[j]):
                    count += 1  #拟合图片个数
                else:
                    count = 0
                    break
            if count==10:  #全部拟合，则退出while循环
                break
      print("训练出的W为: ")
      print(weight0)
      print("训练出的b为: ")
      print(bias0)
      return weight0,bias0
def test(test_data):
      add=0.00
      for q in range(10):  #第几幅图像
          # print(type(cal(test_data[q]))    
          if (cal(test_data[q])==test_data[q][49]):
              add=add+1
              
      right_rate=add/10.00
      right_rate=right_rate*100
      print('感知机判断正确率为:')
      print("%d" %right_rate+'%')
#获取训练和测试感知机的两组数据
train_data=get_data(train_path) #训练数据
test_data=get_data(test_path)  #测试数据
#初始化W0、b0、学习效率η
weight0 = [0, 0, 0, 0, 0, 0, 0, 0, 0, 2, 1300, 1300, 1300, 100, 0, 0, 1000, 1300, 1300, 50, 1300, 0,0, 1300, 1000, 0, 1300, 1300, 0, 0, 400, 1300, 6, 400, 0, 0, 0, 1300, 2000, 1300, 0, 0, 0, 0, 0, 0, 0, 0,0]  # 权重
bias0 = -100.00  
learning_rate = 0.10  
#人工打上标签(默认图像1为正样本，图像0为负样本)
for i in range(10):
    if(i%2==0):
        train_data[i].append(-1)
        test_data[i].append(-1)
    else:
        train_data[i].append(1)
        test_data[i].append(1)
#训练感知机
training(train_data)
#测试感知机
test(test_data)           
        
        
        
        
        
```





