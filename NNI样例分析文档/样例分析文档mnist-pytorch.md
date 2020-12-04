# NNI样例分析文档——以mnist-pytorch为样例
## 1. 运行代码
- search_space.json
```
{
    "batch_size": {"_type":"choice", "_value": [16, 32, 64, 128]},
    "hidden_size":{"_type":"choice","_value":[128, 256, 512, 1024]},
    "lr":{"_type":"choice","_value":[0.0001, 0.001, 0.01, 0.1]},
    "momentum":{"_type":"uniform","_value":[0, 1]}
}
```
- config.yml
```
authorName: default
experimentName: example_mnist_pytorch
trialConcurrency: 1
maxExecDuration: 1h
maxTrialNum: 10
#choice: local, remote, pai
trainingServicePlatform: local
searchSpacePath: search_space.json
#choice: true, false
useAnnotation: false
tuner:
  #choice: TPE, Random, Anneal, Evolution, BatchTuner, MetisTuner, GPTuner
  #SMAC (SMAC should be installed through nnictl)
  builtinTunerName: TPE
  classArgs:
    #choice: maximize, minimize
    optimize_mode: maximize
trial:
  command: python mnist.py
  codeDir: .
  gpuNum: 0
```
设置训练次数：10；最长运行时间：60m；tuner:TPE。
## 2.运行结果
因为没有GPU，所以执行速度很慢，1h内成功运行了4次，运行结果如下：
- Overview:
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201204195727587.PNG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzQwODU5OQ==,size_16,color_FFFFFF,t_70)

- Trials Detail:
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201204195453142.PNG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzQwODU5OQ==,size_16,color_FFFFFF,t_70)
- Default Metric:
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201204195906535.PNG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzQwODU5OQ==,size_16,color_FFFFFF,t_70)
- Hyper Parameter:
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201204195946475.PNG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzQwODU5OQ==,size_16,color_FFFFFF,t_70)
- Trial Duration:
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201204200030211.PNG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzQwODU5OQ==,size_16,color_FFFFFF,t_70)
## 3.运行结果分析
- Default Metric
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201204200415228.PNG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzQwODU5OQ==,size_16,color_FFFFFF,t_70)
如上图， 横坐标Trial=2时，测试的精确度最小，此时参数为：
```{"batch-size":64,"hidden_size":256,"lr":0.01,"momentum":0.5929}```
- Hyper Parameter:
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201204200826497.PNG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzQwODU5OQ==,size_16,color_FFFFFF,t_70)
如图所示：测试精度最高的参数为：
```{"batch-size":64,"hidden_size":256,"lr":0.01,"momentum":0.5929}```
其次是：
```{"batch-size":64,"hidden_size":1024,"lr":0.001,"momentum":0.2473}```
- 一次实验的结果：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201204201122207.PNG)
- 最终参数的选择：
首先根据Hyper Parameter的结果，挑选出实验结果较好的几个参数组合，然后再看Trial Duration中的结果，挑选运行时间较短的，并且考虑Intermediate中loss收敛的速度。
