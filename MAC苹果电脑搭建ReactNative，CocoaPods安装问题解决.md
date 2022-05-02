# MAC苹果电脑搭建ReactNative，CocoaPods安装问题解决

为什么没法直接安装CocoaPods❓

解释看ReactNative中文网：

![image-20220428093406661](https://vichien-public.oss-cn-guangzhou.aliyuncs.com/typora/image-20220428093406661.png)

所以如果实在搞不定这部分全局Proxy的小伙伴可能真的无解，但如果已经配置了全局Proxy，也不一定能直接下载，因为终端的Proxy和系统的Proxy压根是不相通的。

### 解决办法如下：

在终端中敲入`curl cip.cc`，如果出现以下情况，基本就是安装不了CocoaPods的

![image-20220428093933565](https://vichien-public.oss-cn-guangzhou.aliyuncs.com/typora/image-20220428093933565.png)

这时候就需要用3⃣️条指令，来配置终端走Proxy：

⚠️ 该指令是临时性的，如果关闭重启终端，就要再走一次流程，再配置一次❗️

⚠️ 开始前必须确保Mac电脑已经开启全局Proxy

#### 在终端中依次输入以下内容：

`export http_proxy=http://服务器地址:端口号`

`export https_proxy=http://服务器地址:端口号`

`export ALL_PROXY=http://服务器地址:端口号`

#### 如何查看服务器地址和端口号呢？

打开「系统偏好设置」 -> 「网络」->「代理」

![image-20220428094730763](https://vichien-public.oss-cn-guangzhou.aliyuncs.com/typora/image-20220428094730763.png)

配置好后，在终端中再次敲入`curl cip.cc`，如果出现以下情况：

![image-20220428094933126](https://vichien-public.oss-cn-guangzhou.aliyuncs.com/typora/image-20220428094933126.png)

就代表已经可以了❗️

祝安装顺利：

![image-20220428095002704](https://vichien-public.oss-cn-guangzhou.aliyuncs.com/typora/image-20220428095002704.png)