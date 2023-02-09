## 达梦安装

### window安装

#### 安装准备

* 在安装 DM 数据库前，需要检查当前操作系统的相关信息，确认 DM 数据库安装程序与当前操作系统匹配，以保证 DM 数据库能够正确安装和运行。
* 要尽量保证操作系统至少 1 GB 以上的可用内存 (RAM)。如果可用内存过少，可能导致 DM 数据库安装或启动失败。
* DM 完全安装需要至少 1 GB 以上的存储空间，用户需要提前规划好安装目录，预留足够的存储空间。

#### 数据库安装

1、双击运行【setup.exe】安装程序，请根据系统配置选择相应语言与时区，点击【确定】按钮继续安装；<br>
2、安装向导中点击【下一步】按钮继续安装，用户需阅读并接受许可证协议才能进入【下一步】；<br>
3、选取对应版本的 Key 文件，安装程序将自动验证 Key 文件合法性，点击【下一步】继续安装，如果没有key文件则直接进行【下一步】；<br>
4、选择安装组件：DM 安装程序提供四种安装方式：“典型安装”、“服务器安装”、“客户端安装”和“自定义安装”，此处建议选择【典型安装】；<br>
5、选择安装目录：DM 默认安装在 C:\dmdbms 目录下，可改为其他任意盘符，如D:dmdbms<br>
6、安装小结：显示用户即将进行的数据库安装信息，例如产品名称、版本信息、安装类型、安装目录、可用空间、可用内存等信息，用户检查无误后点击【安装】按钮进行 DM 数据库的安装。<br>

#### 实例配置

1、数据库安装后会弹出达梦数据库配置助手，选择【创建数据库实例】，点击【开始】进入下一步骤;<br>
2、创建数据库模板：有【一般用途】、【联机分析处理】、和【联机事务处理】三种模板供选择；<br>
3、选择数据库目录：如 D:\dmdbs\data<br>
4、输入数据库标识：输入数据库名称、实例名、端口号等参数；<br>
5、数据库文件所在位置：可通过选择或输入确定数据库控制、数据库日志等文件的所在位置；<br>
6、数据库初始化参数：可以配置数据库的初始化参数；<br>
7、口令管理：可设置登陆口令，默认口令与登录名一致；<br>
8、选择创建示例库：可以选择是否勾选创建示例库 BOOKSHOP 或 DMHR；<br>
9、创建数据库摘要：在安装数据库之前，将显示用户通过数据库配置工具设置的相关参数。点击【完成】进行数据库实例的初始化工作。<br>

#### 数据库启动和停止

* 数据库可以通过DM服务查看器进行启动和停止。在开始菜单中的【达梦数据库】是快捷方式，打开目录下的DM服务查看器，可以看到已创建的实例服务。选择对应的服务，右键可选择【启动】或【停止】服务。<br>
* 以命令行的方式启停服务。启动数据库服务后，敲入exit停止数据库服务。
```shell
cd D:\dmdbms\bin
dmserver.exe D:\dmdbms\data\DAMENG\dm.ini
```

### Linux安装


## 使用
