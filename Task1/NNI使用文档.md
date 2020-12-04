# NNI体验文档
## 安装NNI
安装环境：Anaconda3
安装方法：
```
python -m pip install --upgrade nni
```
## 安装测试
通过git clone下载nni源代码:
```
git clone -b v1.9 https://github.com/Microsoft/nni.git
```
## 样例测试
使用mnist-pytorch样例测试。运行成功，代表nni安装成功。
 ```
nnictl create --config nni\examples\trials\mnist-pytorch\config_windows.yml
```
 
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201204152348186.PNG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzQwODU5OQ==,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201204165720151.PNG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzQwODU5OQ==,size_16,color_FFFFFF,t_70)


