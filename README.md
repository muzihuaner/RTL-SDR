# RTL-SDR
使用RTL-SDR接收航空频率和ADS-B信号  
转载自我的CSDN博客
# 0. 航空管制频率和ADS-B
117.975-137Mhz为民航管制频率，包括空管语音通信和机场ATIS，AM调制。
ADS-B是飞行器广播自身位置，高度，速度等数据的技术，频率为1090Mhz。
ATIS机场通播，24小时自动播报机场天气风速等情况，离机场较近容易收到。

<font size = 5>由于这些数据是公开且非加密的，所以只要不发射不干扰不向境外传播飞行数据就不会被请喝茶。</font>
# 1. 设备准备
关键词：RTL2832U，R820T2，电视棒
原版就是一种电视棒，用于收看无线电视，但是因为接收范围极广所以被改装成一种SDR（软件定义无线电）设备。某多比某宝便宜，国内平台上基本都是山寨版，容易高温偏频，做工差，但是能用就行又便宜。
![硬件设备](https://img-blog.csdnimg.cn/77c61cba7d6a4431a81af21a7a1922d9.jpeg#pic_center)  
也可以选择蓝色塑料外壳的原版，但是自带天线接收效果可能不如拉杆天线好。
另外这种金属外壳一般都改了Q通道可以额外接收30Mhz以下的中短波。
当然还需要一台电脑！！！
## 1.1 设备搭建
把附赠的馈线和天线和设备连接好，接线很简单，拉杆天线完全拉出有一米左右，伸出窗外或在空旷地带，尽量不要有垂直遮挡或接触金属物体。
# 2. 软件平台
<font size = 5 color = "gray">VirtualRadar</font>
## 2.1 Zadig驱动
![Zadig](https://img-blog.csdnimg.cn/92ade68ecf37486f92955e319586a8ba.png)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/c33ddb6dd8cb44feba187f85fbff13a2.png)  

<code>插入设备</code>→<code>双击驱动安装程序</code>→<code>Option</code>→<code>勾选List All Devices</code>→<code>选择如图</code>→<code>Install Driver</code>
我已经安装过了，所以不一样。
## 2.2 SDRSharp
免安装，双击打开SDRSharp.exe。进入主界面后点击左上角播放按钮就可以听到无线电噪声了。

![设置](https://img-blog.csdnimg.cn/3577073be8804fe0889f25355ae9a018.png)  
<code>左上角设置按钮可以像我这样设置</code>

建议先测试收听当地的FM电台，左边<code>Radio</code>一栏选择<code>WFM</code>，调到对应频率（88MHz一108MHz）测试是否可以收听电台广播。

<font size = 5 color = "gray">下面开始收听航空广播</font>

以我的位置举例，不同机场不一样，下面会讲到查询方法，离我最近的是天府国际机场，管制频率大多在120Mhz左右。
![在这里插入图片描述](https://img-blog.csdnimg.cn/a8b860192ddf4e7e8eb15c1c4fb7e1bf.png)  
在这个频率附近可以发现许多尖峰频率，左边<code>Radio</code>一栏选择<code>AM</code>，<code>中心线</code>对准最最高的尖峰，拖动<code>灰色区域两边</code>可以调整带宽，右边的<code>Zoom</code>拖动条可以调整频谱缩放大小，其他几个可以自己试试。白天空中交通繁忙时，就会很热闹，飞行员会向塔台报告航班，高度等信息。由于有山阻挡，我这边机场ATIS收听不到。
### 2.2.1 空管频率查询
点击链接[航图查询](https://aip.chinaflier.com/#/)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/f670abe569214e869ca9c8cca62a8ea2.png)  
以天府国际机场为例，点击<code>ZUTF-1A:ADC</code>这张航图
![在这里插入图片描述](https://img-blog.csdnimg.cn/ddbdd85fcc9c47d893fbc3ed2a96b283.png)  
上面这几行就是对应频率。
## 2.3 RTL1090和VirtualRadar
<font size = 5 color = "gray">安装VirtualRadar及其插件</font>

首先安装<code>VirtualRadarSetup.exe</code>  
![在这里插入图片描述](https://img-blog.csdnimg.cn/755d41a34e8740898fc62da9247c7771.png)  
注意端口选择，选择一个不被占用的端口，其他默认就行

接着安装<code>DatabaseWriterPluginSetup.exe</code>插件，安装路径要与<code>VirtualRadarSetup.exe</code>安装路径在同一文件夹下

<font size = 5 color = "gray">设置RTL1090接收程序</font>

进入<code>RTL1090</code>文件夹，右击<code>rtl1090.beta3.exe</code>创建快捷方式，然后选中创建的快捷方式，打开属性

![在这里插入图片描述](https://img-blog.csdnimg.cn/d7634ec7ffb04e35a73739be18e8f4ba.png)  
在目标最后一栏添加<code> /30003</code>，注意有一个空格

接着插入设备，打开创建的快捷方式，点击右上角的<code>START</code>

![在这里插入图片描述](https://img-blog.csdnimg.cn/a699080670c040bf80e17ff70a5f66e0.png)  
下方选择<code>List</code>，如果滚动刷新十六进制数据就代表接收成功了
<code>Table</code>页面就有解码出的飞机信息

<font size = 5 color = "gray">设置VirtualRadar</font>

打开<code>VirtualRadar</code>软件→<code>Tools</code>→<code>Plugins</code>

![在这里插入图片描述](https://img-blog.csdnimg.cn/e3b40834147241fa9d7be573644ba8ec.png)  
选择<code>Options</code>

![在这里插入图片描述](https://img-blog.csdnimg.cn/3c020f0989214ebc8e3e121b92d2b9fc.png)  
如图勾选，如果要<code>Create Database</code>，要用管理员身份运行，但是不<code>Create Database</code>也没什么影响

接着返回主界面，<code>Tools</code>→<code>Options</code>→<code>Receivers</code>→<code>Receiver</code>

![在这里插入图片描述](https://img-blog.csdnimg.cn/c8b53b435b6343b4987a8d7db468a583.png)  

勾选<code>Enabled</code>，并点击<code>OK</code>

重启<code>VirtaulRadar</code>

保证<code>RTL1090</code>程序在后台运行

![在这里插入图片描述](https://img-blog.csdnimg.cn/99160291edd440d29f72a975eb7a1381.png)  
点击中间的网页链接

![在这里插入图片描述](https://img-blog.csdnimg.cn/d6759309a4b3455a84c5fffa93e58843.png)  

<font size = 5>不出意外，就能在浏览器看到这个画面了，大功告成！！！</font>

<font size = 5>飞常准和Flightradar24也是同样的原理，只不过它们的数据库是联网分享出来的</font>

<font size = 5>因为用的Google地图，所以加载不出来！还有不建议长时间挂在后台，软件可能会联网发送数据，有风险</font>

<font size = 5>RTL-SDR还有很多好玩的方法，比如接收卫星气象云图，SSTV。。。。。。</font>

<font size = 5>Linux和Android都有适配软件，例如安卓的SDRTouch</font>

<font size = 5>无线电这方面，地理位置 > 天线好坏 > 接收设备，所以接收效果各有不同</font>

<font size = 5>请遵守国家法律法规！</font>










