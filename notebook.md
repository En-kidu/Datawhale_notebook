Datawhale机器学习笔记
================

解题思路
---------------

这里通过大量数据，自己提取特征、选择模型去训练，一般流程为：

问题分析->数据探索->数据清洗->特征工程->训练验证优化->输出结果

模型选择：二分类
----------  

机器学习而非深度学习：手动设计特征很重要

决策树而非逻辑回归：非线性关系的处理

[决策树的理解](https://zhuanlan.zhihu.com/p/133838427)：用于分类的二叉树，每个节点表示判断语句，信息增益最大的应当放根处。什么是信息增益：信息熵H指信息中排除了冗余后的平均信息量，信息增益即  H(D)-H(D|new information)

数据清洗
-----------------

* 原始数据：看看类型，画出**热力图**，深度越深越相关（[示例](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/preview/NhVhbDHboo5lzqxcuyGczvhGnkh/?preview_type=16)）
* 把一些字符或其他转换为数值（编码方式：[onehot编码](https://zhuanlan.zhihu.com/p/134495345)），编码的选择参考下条👇
* 注意观察数据是**类型**还是连续的变量，对不同情况有不同编码/筛选方式
* 对NaN和Inf进行填补处理
  
  ```python
  #采用平均值或0或其他东西填充NaN
  x['age'].fillna(x['age'].mean(), inplace=True) 

  pandas.DataFrame.fillna(value=None, method=None, axis=None, inplace=False, limit=None, downcast=None, **kwargs)
  
  ```

* 用转换好的数据替代之前的数据（drop ＆ add）（一般是最后训练时丢掉不要的）

特征工程
-------------

其实就是把数据处理好，然后加进训练数据中就完成了，训练的时候drop掉不用的数据，重要的还是怎么把数据处理成比较相关的样子呈现出来。

训练模型
-------------

训练模型一般采用抽出一部分样本作为测试集迭代改进，最后交差时采用验证集评估的方式。

常见方法：

* `sklearn.model_selection.train_test_split(X, y, random_state)` [官方文档](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.train_test_split.html)
* [K-fold cross-validation](https://zhuanlan.zhihu.com/p/38121870)
  
  `kf = sklearn.cross_validation.KFold(sizeofSample, n_folds)` [官方文档](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.KFold.html)

  ```python
  from sklearn.cross_validation import cross_val_score

  scores = cross_val_score(clf, X, y, kwargs)
  #X, y are raw data, cross_val_score() will split them automatically
  print(scores.mean())
  ```

* 一些评估方法:

  ```python
  precision_score(train_data['target'], pred)
  f1_score(train_data['target'], pred)
  recall_score(train_data['target'], pred)
  ```
