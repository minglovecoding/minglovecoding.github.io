---
title: 机器学习笔记
date: 2025-07-23  11:38:00 
taxonomies:
  tags:
    - Machine learning
---
[ML入门](https://www.bilibili.com/video/BV1nt411r7tj?spm_id_from=333.788.videopod.episodes&vd_source=1161690079eded438f62622c2b6c537f&p=9)
## 机器学习概述
1. 机器学习、深度学习能做什么。
- 传统预测
- 图像识别
- 自然语言处理

2. 数据集构成：
特征值+目标

3. 机器学习算法分类
监督学习    
    目标值：类别 - 分类问题
    目标值：连续型数据 - 回归问题
目标值：无 - 无监督学习
    K-means

4. 机器学习开发流程
1）获取数据
2）数据处理
3）特征工程
4）算法训练 - 模型
5）模型评估
6）应用

5. 机器学习框架
sklearn 
Chainer->pytorch
caffe2
theano->tensorflow

6. 可用数据集
1）sklearn
2）kaggle
3）UCI

7. 使用scikit-learn
- 小规模数据用load，大规模数据用fetch 
- pip install numpy scipy matplotlib
- pip install -U scikit-learn
- python -c "import sklearn; sklearn.show_versions()"

## 特征工程
sklearn 特征工程
pandas 数据清洗
特征提取：将任意数据转换为可用于机器学习的数字特征
- 字典特征提取（特征离散化）one-hot处理
  sklearn.feature_extraction.DictVectorrizer()
- 文本特征提取
- 图像特征提取

```
from sklearn.datasets import load_iris;
from sklearn.model_selection import train_test_split
from sklearn.feature_extraction import DictVectorizer

#sklearn.show_versions()
def datasets_demo():
    iris=load_iris()
    print("iris databases is:\n",iris)
    print("description:\n",iris["DESCR"])
    print("name:\n",iris.feature_names)
    print("feature_value:\n",iris.data,iris.data.shape)
    #数据集的划分
    x_train,x_test,y_train,y_test=train_test_split(iris.data,iris.target,test_size=0.2,random_state=22)
    print("The Test feature_value is:\n",x_train,x_train.shape)
    return None

def dict_demo():
    data=[{'city':'beijing','temperature':100},{'city':'shanghai','temperature':60},{'city':'shenzhen','temperature':30}]
    #1、实例化一个转换器类
    transfer = DictVectorizer(sparse=False)
    #2、调用fit_transform()
    data_new = transfer.fit_transform(data)
    print("data_new:\n",data_new)
    print("特征名字:\n",transfer.get_feature_names_out())


if __name__=="__main__":
    #sklearn数据集使用
    datasets_demo()
    #字典特征抽取
    dict_demo()

```
## Tf-idf文本特征提取
在某一类别文章出现次数很多，而在其他类别文章出现很少。
TF - 词频（term frequency，tf）
IDF - 逆向文档频率
