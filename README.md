# 性能测试辅助平台-JAVA

  1.0版本：

    1、服务器nmon一键部署及结果整理；
  
    2、服务器、数据库、jvm实时监控；
  
    3、基本性能瓶颈自动定位；

## 搭建过程
### 环境准备
计划是先安装influxdb，然后安装telegraf，能把数据写入influxdb，最后用grafana展示。<br>
我来到influxdb官网，发现influx有云平台的，OSS对象存储的（因为数据库只适合存储数字文本，并不适合存储图片视频文件之类的。所以OSS对象存储就是用来存储图片视频文件之类的，并提供访问的url链接，这些链接就存在数据库中），本地版的，还有telegraf（这俩是一家公司的产品），我点开下载，它有linux、docker、windows的版本，但提示说windows处于试验状态。<br>
我下载windows版本，因为试验版所以官网没有安装指南，网友说下载完，解压后，先运行influxdb.exe，启动数据库，而且不能关，再运行influx.exe,就可以进行数据操作。同时，可以通过http://localhost:8086 这个默认端口进行增删改查，具体语句需要参考<a href="https://docs.influxdata.com/" target="_blank">官方文档</a>（这个地址在官网不明显，拉到下面，进influxdb的文档，里面有语法描述）。然后我安装telegraf，解压后直接运行那个.exe文件就行，然后influx里自动出现一个名为telegraf的表空间。我以为这就好了，于是安装grafana，配置数据源，可就是查不出数据。<br>
一番搜索，修改配置文件，还是没用。而且influx windows版本的配置文件里，默认地址是linux的，实际数据存储地址在c盘用户目录下。于是我删除了windows版本，打算用docker，之前是想到测试人员用windows的多，且我的破笔记本配置太低，不想装虚拟机+linux，所以想在windows上搭建，但天不助我，既然docker已经出了windows桌面版，而且docker打包也方便以后复制环境，所以决定改用docker搭建。<br>
我把之前弄得全部删干净，来到docker官网。之前在centos安装过docker，从未试过windows，所以不太明白官网这都是什么东西。一番搜索后，明白了官网的下载链接，只支持win10，win7/win8需要下载docker tool box，于是安装菜鸟教程的说法，来到的国内的镜像下载地址，发现版本名乱七八糟，有的带ce，有的不带，按照之前经验，ce是免费版，于是我下载了不带ce的，安装包是个全家桶，包括一个虚拟机软件virtualbox，docker，和git，结果安装后，启动报错something went worng...，也不说具体哪错，于是又是一番搜索，发现别人的报错都带提示，就我是...三个省略号，一个网友提示是git的问题，于是我卸载，重装，勾除git，结果启动不起来，奥，原来docker启动要用git取拉取部分安装包，得，再卸载，我这次选择了ce版本，最新的一个，安装成功后，启动报错，这次好歹有提示，解决方法<a href="https://blog.csdn.net/G____G/article/details/95484458" target="_blank">点我</a>,但重要的是，ce版本的配置文件都不一样，旧版本坑啊。<br>
安装完成后，终于能用了，但是下载镜像还是会超时，于是要配置国内镜像，之前linux是用一个.json的配置文件，找半天windows版的docker没有，于是又一番搜索，意外发现一篇讲<a href="https://blog.csdn.net/galen2016/article/details/89219199" target="_blank">win7安装docker并配置加速镜像</a>的文章，相见恨晚。<br>
接下来我按照上面的文章，用xshell操作docker，安装了influxdb，这时要运行镜像了，我又忘记了docker run那一堆参数，很烦。docker的windows版的三个软件，虚拟机只是个辅助，不影响docker实际使用，Docker Quickstart Terminal是命令行，用xshell代替了，还有个Kitematic，才是windows版本的主角，官网说只要点一点，就能运行镜像，于是我打开Kitematic，发现能检测到命令行模式下载的镜像，直接点create，一键运行，设置里还能改映射的端口，真方便。我立刻注册了docker账号，想着也许我可以在别的终端登录我的docker账号，能共享所有镜像及设置，其实原理不难，一个compose文件而已。<br>
至此，windows版docker+influx安装完成，明天我再弄telegraf。

