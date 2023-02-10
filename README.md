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

执行如下命令使文件生效：
```shell
source ~/.bash_profile
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
./dminit path=/home/dmdba/dmdbms/data PAGE_SIZE=32
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
<img width="364" alt="image" src="https://user-images.githubusercontent.com/73985884/217992716-ceb31bbd-eb77-4a74-b4eb-8c3369b6a28d.png"> <br>
3、点击【下一步】，选择数据库实例安装目录为/home/dmdba/dmdbms/data <br>
<img width="367" alt="image" src="https://user-images.githubusercontent.com/73985884/217992755-a2bbc690-379e-45a2-858c-11819a3dd6b0.png"> <br>
4、确定好数据库安装目录后，点击【下一步】，输入数据库名、实例名和端口号；<br>
<img width="364" alt="image" src="https://user-images.githubusercontent.com/73985884/217992872-4f1d62eb-e7c8-4d93-99b4-25aa3f64f001.png"> <br>
5、配置数据库文件路径，选择【默认路径】；<br>
<img width="384" alt="image" src="https://user-images.githubusercontent.com/73985884/217992956-b27f9b3d-51fd-4451-825b-dd4dbe0a603f.png"> <br>
6、点击【下一步】，配置初始化参数，注意簇大小、页大小、字符集以及大小写敏感确定后不可修改，将页大小设为32；<br>
<img width="386" alt="image" src="https://user-images.githubusercontent.com/73985884/217994068-c4b019b3-1790-4301-a152-c0a339218019.png"> <br>
7、点击【下一步】，配置数据库口令：可设置登陆口令，默认口令与登录名一致；<br>
<img width="501" alt="image" src="https://user-images.githubusercontent.com/73985884/217994136-7861a556-69fe-4797-b68f-8c9fefa8768b.png"> <br>
8、点击【下一步】，配置示例库：可以选择是否勾选创建示例库 BOOKSHOP 或 DMHR；<br>
<img width="501" alt="image" src="https://user-images.githubusercontent.com/73985884/217994175-8b908978-c504-424b-9a32-3181e63fdac2.png"> <br>
9、点击【下一步】，用户可检查创建参数，若有需要修改之处可点击【上一步】回到需要修改的位置进行修改；<br>
<img width="472" alt="image" src="https://user-images.githubusercontent.com/73985884/217994256-33c415ae-fbe3-4cbb-87d2-230cd9a4fe7a.png"> <br>
10、点击【完成】，创建完成数据库实例后，重新打开一个终端，切换到 root 用户执行下图的脚本即可完成实例配置；<br>
<img width="255" alt="image" src="https://user-images.githubusercontent.com/73985884/217994306-2b182cd8-519f-497c-abf1-0f923311ae5c.png"> <br>
<img width="254" alt="image" src="https://user-images.githubusercontent.com/73985884/217994497-16c76e89-1823-4ba2-93fe-35afb20aa410.png"> <br>
11、弹出提示框表示数据库创建成功。<br>
<img width="300" alt="image" src="https://user-images.githubusercontent.com/73985884/217994847-df532d46-7d73-42df-bb8e-490326c35359.png"> <br>

#### 注册服务
命令行注册服务:注册服务需使用 root 用户进行注册。使用 root 用户进入数据库安装目录的 /script/root 下进行注册：
```shell
cd /home/dmdba/dmdbms/script/root
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
<img width="264" alt="image" src="https://user-images.githubusercontent.com/73985884/217995081-4ecde79f-b00e-4041-a5ea-9aa3572c6fdc.png"> <br>
2、单击【开始】，弹出注册数据库服务页面；<br>
<img width="584" alt="image" src="https://user-images.githubusercontent.com/73985884/217995799-036c3dcb-fdaf-481e-aea5-4b1696882ac7.png"> <br>
3、点击【完成】后，弹出执行配置脚本页面，重新打开一个终端，切换到 root 用户执行执行脚本。脚本执行成功后，该实例已启动。<br>
<img width="256" alt="image" src="https://user-images.githubusercontent.com/73985884/217996400-5b77d085-4155-4ee2-a2ed-f0e89c14d5b5.png"> <br>
4、注册完成，点击【完成】完成服务注册。<br>
<img width="300" alt="image" src="https://user-images.githubusercontent.com/73985884/217996551-74608ba5-4910-485f-8298-5c1e0dbf6fe9.png"> <br>

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
