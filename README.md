## 达梦安装

### Linux安装

#### 安装准备

##### 新建 dmdba 用户
创建用户所在的组，命令如下：
```shell
groupadd dinstall
```
创建用户，命令如下：
```shell
useradd -g dinstall -m -d /home/dmdba -s /bin/bash dmdba
```
修改用户密码，命令如下：
```shell
passwd dmdba
```

##### 修改文件打开最大数
可使用 dmdba 用户执行如下命令，使设置临时生效：
```shell
ulimit -n 65536
```

##### 挂载镜像
切换到 root 用户，将 DM 数据库的安全版的 iso 安装包保存在任意位置，例如 /opt 目录下，执行如下命令挂载镜像：
```shell
mount -o loop /opt/dm8_setup_rh7_64_sec_8.1.2.192_20230103.iso /mnt
```

#### 数据库安装
切换至 dmdba 用户下，在 /mnt 目录下使用命令行安装数据库程序，依次执行以下命令安装 DM 数据库。
```shell
su - dmdba
cd /mnt/
./DMInstall.bin
```

若出现“初始化图形界面失败，如果当前监视器窗口不支持图形界面，请进入安装文件所在文件夹并使用"./DMInstall.bin -i"进行命令行安装”错误提示，可按以下两种方式操作解决：<br>
方法一：注销当前用户，登陆 dmdba 用户，执行 ./DMInstall.bin 命令。<br>
方法二：用当前用户执行 xhost +，切换到 dmdba 用户，执行 export DISPLAY=:0，再执行 xhost +命令。<br>

1、弹出图形化安装界面后，请根据系统配置选择相应语言与时区，点击【确定】按钮继续安装；<br>
<img width="374" alt="image" src="https://user-images.githubusercontent.com/73985884/217770636-64b5d452-643a-41a4-9188-71a0c00a8ad0.png"> <br>
2、安装向导中点击【下一步】按钮继续安装，用户需阅读并接受许可证协议才能进入【下一步】；<br>
<img width="526" alt="image" src="https://user-images.githubusercontent.com/73985884/217770749-1a668e85-cd69-449b-99d6-d8fefb8c94cf.png"> <br>
3、选取安全版对应版本的 Key 文件，安装程序将自动验证 Key 文件合法性，点击【下一步】继续安装；<br>
<img width="523" alt="image" src="https://user-images.githubusercontent.com/73985884/217978881-adf15d51-3703-4d29-b324-8e0d18ff8381.png"> <br>
4、选择安装组件，选择【典型安装】默认组件服务器、客户端、驱动、用户手册和数据库服务全选；<br>
<img width="526" alt="image" src="https://user-images.githubusercontent.com/73985884/217979073-4d68b6bf-7569-4f2a-929b-0f36ffa20fe8.png"> <br>
5、选择安装目录：DM 默认安装在 /home/dmdba/dmdbms 目录下。<br>
<img width="450" alt="image" src="https://user-images.githubusercontent.com/73985884/217979212-45f0d69b-bade-4427-aecd-9579fe9d3718.png"> <br>
6、点击【下一步】后，弹出确认安装信息页面，检查安装信息是否准确，确认无误后点击【安装】。<br>
<img width="523" alt="image" src="https://user-images.githubusercontent.com/73985884/217979326-d6fabd18-59d7-42e0-b505-17ff6ce5843d.png"> <br>
7、点击【安装】后，等待 1~2 分钟即可安装完成，安装完成后弹出执行配置脚本页面，需要按照页面要求，重新打开一个终端，切换到 root 用户执行该脚本。 <br>
<img width="380" alt="image" src="https://user-images.githubusercontent.com/73985884/217979390-ee65e789-f363-4aaf-8a73-0c5b9d3dd4f6.png"> <br>
脚本执行完成后，点击【完成】，弹出提示框，提示是否关闭窗口，选择是，提示数据库安装完成，再点击【完成】按钮，完成数据库安装。

#### 环境配置
修改~/.bash_profile文件，添加下面的内容以便于测试软件的运行。
```shell
export LD_LIBRARY_PATH="$LD_LIBRARY_PATH:/home/dmdba/dmdbms/bin"
export DM_HOME="/home/dmdba/dmdbms"
```

#### 实例配置
dmdba用户进入DM 数据库安装目录下的 bin 目录中，使用 dminit 命令初始化实例。dminit 命令可设置多种参数，可执行如下命令查看可配置参数。
```shell
cd /home/dmdba/dmdbms/bin
./dminit help
```

**注意：页大小 (page_size)、簇大小 (extent_size)、大小写敏感 (case_sensitive)、字符集 (charset) 这四个参数，一旦确定无法修改。** <br>
**由于测试用例需要，数据库的页的大小需要设为32k。** <br>
```shell
./dminit path=/dm/data PAGE_SIZE=32
```

也可以使用图形化工具配置实例。使用图形化界面安装数据库安装完成后，会弹出选择是否初始化数据库页面，选择【初始化】。<br>
<img width="394" alt="image" src="https://user-images.githubusercontent.com/73985884/217982235-fd733811-0c6c-4880-a9c7-c8057c2181cf.png"> <br>
1、点击初始化后会弹出数据库配置助手，通过数据库配置助手便可以配置数据库；<br>
<img width="396" alt="image" src="https://user-images.githubusercontent.com/73985884/217982296-89c09dce-432c-4afa-b11a-7c6228463053.png"> <br>
手动打开数据库配置助手需进入DM 数据库安装目录下的 tool 目录中，执行如下命令打开。
```shell
cd /home/dmdba/dmdbms/tool
./dbca.sh
```

2、选择创建数据库实例，点击【开始】，进入创建数据库页面的创建数据库模版页签，选择【一般用途】；<br>
<img width="544" alt="image" src="https://user-images.githubusercontent.com/73985884/217982636-24b25f00-3352-4182-8c19-f19347c5a057.png"> <br>
3、点击【下一步】，选择数据库实例安装目录为/home/dmdba/dmdbms/data <br>
4、确定好数据库安装目录后，点击【下一步】，输入数据库名、实例名和端口号；<br>
<img width="548" alt="image" src="https://user-images.githubusercontent.com/73985884/217982916-5c412bb0-8b62-478b-8e2b-7eae979275e0.png"> <br>
5、配置数据库文件路径，选择【默认路径】；<br>
6、点击【下一步】，配置初始化参数，注意簇大小、页大小、字符集以及大小写敏感确定后不可修改，将页大小设为32；<br>
<img width="544" alt="image" src="https://user-images.githubusercontent.com/73985884/217983665-c9f671f2-b0d5-40de-a1cc-11e1fb2ddf5f.png"> <br>
7、点击【下一步】，配置数据库口令：可设置登陆口令，默认口令与登录名一致；<br>
<img width="586" alt="image" src="https://user-images.githubusercontent.com/73985884/217984553-5afc727f-bf61-4730-9bf2-02c8fb9bfb6b.png"> <br>
8、点击【下一步】，配置示例库：可以选择是否勾选创建示例库 BOOKSHOP 或 DMHR；<br>
9、点击【下一步】，用户可检查创建参数，若有需要修改之处可点击【上一步】回到需要修改的位置进行修改；<br>
10、点击【完成】，创建完成数据库实例后，重新打开一个终端，切换到 root 用户执行下图的脚本即可完成实例配置。<br>
<img width="379" alt="image" src="https://user-images.githubusercontent.com/73985884/217983935-84c7e870-2f84-42a5-9878-5fbc916b1586.png"> <br>

#### 注册服务
命令行注册服务:注册服务需使用 root 用户进行注册。使用 root 用户进入数据库安装目录的 /script/root 下进行注册：
```shell
cd /dm8/script/root
./dm_service_installer.sh -t dmserver -dm_ini /home/dmdba/dmdbms/data/DAMENG/dm.ini -p DMSERVER
```

用户可根据自己的环境更改 dm.ini 文件的路径以及服务名。
```shell
./dm_service_installer.sh -h
```

如需为其他实例注册服务，需打开 dbca 工具，进行注册服务。
```shell
cd /home/dmdba/dmdbms/tool
./dbca.sh
```
1、打开运行 dbca 工具，选择【注册数据库服务】；<br>
<img width="395" alt="image" src="https://user-images.githubusercontent.com/73985884/217985005-d68723f5-ae2e-43eb-9085-da0a970801e8.png"> <br>
2、单击【开始】，弹出注册数据库服务页面；<br>
<img width="585" alt="image" src="https://user-images.githubusercontent.com/73985884/217985062-f2619f27-ef11-40c3-9af2-688d73d91592.png"> <br>
3、点击【完成】后，弹出执行配置脚本页面，重新打开一个终端，切换到 root 用户执行执行脚本。脚本执行成功后，该实例已启动。<br>
<img width="384" alt="image" src="https://user-images.githubusercontent.com/73985884/217985228-6f3dba2c-f533-444d-b3c3-9fff407ced2a.png"> <br>

#### 启停数据库
服务注册成功后，root用户启动数据库：
```shell
systemctl start DmServiceDMSERVER.service
```

停止数据库：
```shell
systemctl stop DmServiceDMSERVER.service
```

重启数据库：
```shell
systemctl restart DmServiceDMSERVER.service
```

查看数据库服务状态：
```shell
systemctl status DmServiceDMSERVER.service
```

可前台启动，dmdba用户进入 DM 安装目录下的 bin 目录下然后执行下面的命令启动数据库,该启动方式为前台启动，若想关闭数据库，则输入 exit 即可。
```shell
./dmserver /home/dmdba/dmdbms/data/DAMENG/dm.ini
```

图形化启动数据库，root用户进入 DM 安装目录下的 tool 目录下然后打开DM 服务查看器，选择对应的服务，右键可选择【启动】或【停止】服务。
```shell
cd /home/dmdba/dmdbms/tool
./dmserver.sh
```

## 使用
