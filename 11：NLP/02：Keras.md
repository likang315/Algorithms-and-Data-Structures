### Keras 

------

[TOC]

##### 01：keras 简介

- 基于 Python 的深度学习库

###### 特点

- 模块化：模型被理解为由独立的、完全可配置的模块构成的序列或图。这些模块可以以尽可能少的限制组装在一起。特别是神经网络层、损失函数、优化器、初始化方法、激活函数、正则化方法，它们都是可以结合起来构建新模型的模块。
- 易扩展性： 新的模块是很容易添加的（作为新的类和函数）。

##### 02：快速上手

```python
from keras.models import Sequential
from keras.layers import Dense

# 导入模型
model = Sequential()
# 使用add 来堆叠模型
model.add(Dense(units=64, activation='relu', input_dim=100))
model.add(Dense(units=10, activation='softmax'))
# 使用compile 来配置学习过程
model.compile(loss='categorical_crossentropy',
              optimizer='sgd',
              metrics=['accuracy'])
# x_train 和 y_train 是 Numpy 数组 -- 就像在 Scikit-Learn API 中一样。
# 批量的训练数据
model.fit(x_train, y_train, epochs=5, batch_size=32)
# 评估模型
loss_and_metrics = model.evaluate(x_test, y_test, batch_size=128)
# 对新的数据进行预测
classes = model.predict(x_test, batch_size=128)
```

##### 03：损失函数

- 训练模型时，给标注的标签，与预测输出相差多少；

- ```python
  model.compile(loss='mean_squared_error', optimizer='sgd')
  ```

##### 04：评价函数

- 评价函数用于评估当前训练模型的性能。当模型编译后（compile），评价函数应该作为 `metrics` 的参数来输入。

  - **参数**
    - **y_true**: 真实标签，Theano/Tensorflow 张量。
    - **y_pred**: 预测值。和 y_true 相同尺寸的 Theano/TensorFlow 张量。
  - **返回值**
    - 返回一个表示全部数据点平均值的张量。

- 

  ```python
  model.compile(loss='mean_squared_error',
                optimizer='sgd',
                metrics=['mae', 'acc'])
  ```













