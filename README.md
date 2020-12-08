# 开始使用 (测试版,更新中...)
## Python3版本Bug较多,功能不全. 推荐使用 [Written in GoLang](https://github.com/MoeClub/OneList/tree/master/Rewrite) 版本

## 所需依赖
```
# 自行安装 python3
pip3 install tornado
```

## 通过下面URL登录 (右键新标签打开)
[https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=78d4dc35-7e46-42c6-9023-2d39314433a5&response_type=code&redirect_uri=http://localhost/onedrive-login&response_mode=query&scope=offline_access%20User.Read%20Files.ReadWrite.All](https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=78d4dc35-7e46-42c6-9023-2d39314433a5&response_type=code&redirect_uri=http://localhost/onedrive-login&response_mode=query&scope=offline_access%20User.Read%20Files.ReadWrite.All)

## 初始化配置文件
```
# 运行
python3 OneList.py

# 在浏览器地址栏中获取 code 字段内容
# 粘贴并按回车, 每个 code 只能用一次
# 此操作将会自动初始化的配置文件
```

## 自定义配置文件
```
# config.json

{
    // OneDrive 中的某个需要列出的目录
    "RootPath": "/Document",
    // 网址中的子路径
    "SubPath": "/onedrive",
    // 目录刷新时间
    "FolderRefresh": 900,
    // 下载链接刷新时间
    "FileRefresh": 1200,
    // 认证令牌, 将会自动更新, 保持默认
    "RefreshToken": "",
    // 这个不用管, 保持默认
    "RedirectUri": "http://localhost/onedrive-login"
}
```

## 运行
```
python3 app.py

# 默认监听 127.0.0.1:5288 , 可在 app.py 中自行更改.
```

## 展示
[https://moeclub.org/onedrive/](https://moeclub.org/onedrive/)



1、授权认证
点击右侧URL登录并授权，授权地址→【国际版、个人版(家庭版)】、【中国版(世纪互联)】。

国际版、个人版(家庭版)
https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=78d4dc35-7e46-42c6-9023-2d39314433a5&response_type=code&redirect_uri=http://localhost/onedrive-login&response_mode=query&scope=offline_access%20User.Read%20Files.ReadWrite.All

中国版(世纪互联)
https://login.chinacloudapi.cn/common/oauth2/v2.0/authorize?client_id=dfe36e60-6133-48cf-869f-4d15b8354769&response_type=code&redirect_uri=http://localhost/onedrive-login&response_mode=query&scope=offline_access%20User.Read%20Files.ReadWrite.All

授权后会获取一个localhost开头打不开的链接，这里复制好整个链接地址，包括localhost。

2、安装OneDriveUploader

#64位系统下载
wget https://raw.githubusercontent.com/MoeClub/OneList/master/OneDriveUploader/amd64/linux/OneDriveUploader -P /usr/local/bin/
#32位系统下载
wget https://raw.githubusercontent.com/MoeClub/OneList/master/OneDriveUploader/i386/linux/OneDriveUploader -P /usr/local/bin/
#arm架构下载
wget https://raw.githubusercontent.com/MoeClub/OneList/master/OneDriveUploader/arm/linux/OneDriveUploader -P /usr/local/bin/

#给予权限
chmod +x /usr/local/bin/OneDriveUploader
3、初始化配置

#国际版，将url换成你上面复制的授权地址，包括http://loaclhost。
OneDriveUploader -a "url"

#个人版(家庭版)，将url换成你上面复制的授权地址，包括http://loaclhost。
OneDriveUploader -ms -a "url"

#中国版(世纪互联)，将url换成你上面复制的授权地址，包括http://loaclhost。
OneDriveUploader -cn -a "url"
如果提示Init config file: /path/to/file/auth.json类似信息，则初始化成功。

4、使用命令

Usage of OneDriveUploader:
  -a string
        // 初始化授权
        Setup and Init auth.json.
  -b string
        // 自定义上传分块大小, 可以提高网络吞吐量, 受限于磁盘性能和网络速度.
        Set block size. [Unit: M; 5<=b<=60;] (default "10")
  -c string
        // 配置文件路径
        Config file. (default "auth.json")
  -n string
        // 上传单个文件时,在网盘中重命名
        Rename file on upload to remote.
  -r string
        // 上传到网盘中的某个目录, 默认: 根目录
        Upload to reomte path.
  -s string
        // *必要参数, 要上传的文件或文件夹
        Upload item.
  -t string
        // 线程数, 同时上传文件的个数. 默认: 2
        Set thread num. (default "2")
  -f
        // 开关(推荐)
        // 加上 -f 参数，强制读取 auth.json 中的块大小配置和多线程配置.
        // 不加 -f 参数, 每次覆盖保存当前使用参数到 auth.json 配置文件中.
        Force Read config form config file. [BlockSize, ThreadNum]
  -skip
        // 开关
        // 跳过上传网盘中已存在的同名文件. (默认不跳过)
        Skip exist file on remote.
  -cn
        // 开关
        // 授权中国版(世纪互联), 需要此参数.
        OneDrive by 21Vianet.
  -ms
        // 开关
        // 授权个人版(家庭版), 需要此参数.
        OneDrive by Microsoft.
5、命令示例

#将当前目录下的mm00.jpg文件上传到OneDrive网盘根目录
OneDriveUploader -c /path/to/file/auth.json -s "mm00.jpg"

#将当前目录下的mm00.jpg文件上传到OneDrive网盘根目录，并改名为mm01.jpg
OneDriveUploader -c /path/to/file/auth.json -s "mm00.jpg" -n "mm01.jpg"

#将当前目录下的Download文件夹上传到OneDrive网盘根目录
OneDriveUploader -c /path/to/file/auth.json -s "Download" 

#将当前目录下的Download文件夹上传到OneDrive网盘Test目录中
OneDriveUploader -c /path/to/file/auth.json -s "Download" -r "Test"

#将同目录下的Download文件夹上传到OneDriv网盘Test目录中，使用10线程
OneDriveUploader -c /path/to/file/auth.json -t 10 -s "Download" -r "Test"

#将同目录下的Download文件夹上传到OneDrive网盘Test目录中，使用15线程，并设置分块大小为20M
OneDriveUploader -c /path/to/file/auth.json -t 15 -b 20 -s "Download" -r "Test"
/path/to/file/auth.json为初始化时，生成的auth.json绝对路径地址，本文默认/root/auth.json，自行调整。

注意：如果你之前上传手动中断过，再上传的时候，请使用-skip参数，默认会跳过你已经上传过的文件/文件夹。

Aria2自动上传
同样的这里也会提供个配套的Aria2自动上传脚本，上传配置方法参考→传送门。

上传脚本代码如下：

#!/bin/bash

GID="$1";
FileNum="$2";
File="$3";
MaxSize="15728640";
Thread="3";  #默认3线程，自行修改，服务器配置不好的话，不建议太多
Block="20";  #默认分块20m，自行修改
RemoteDIR="";  #上传到Onedrive的路径，默认为根目录，如果要上传到MOERATS目录，""里面请填成MOERATS
LocalDIR="/www/download/";  #Aria2下载目录，记得最后面加上/
Uploader="/usr/local/bin/OneDriveUploader";  #上传的程序完整路径，默认为本文安装的目录
Config="/root/auth.json";  #初始化生成的配置auth.json绝对路径，参考第3步骤生成的路径


if [[ -z $(echo "$FileNum" |grep -o '[0-9]*' |head -n1) ]]; then FileNum='0'; fi
if [[ "$FileNum" -le '0' ]]; then exit 0; fi
if [[ "$#" != '3' ]]; then exit 0; fi

function LoadFile(){
  if [[ ! -e "${Uploader}" ]]; then return; fi
  IFS_BAK=$IFS
  IFS=$'\n'
  tmpFile="$(echo "${File/#$LocalDIR}" |cut -f1 -d'/')"
  FileLoad="${LocalDIR}${tmpFile}"
  if [[ ! -e "${FileLoad}" ]]; then return; fi
  ItemSize=$(du -s "${FileLoad}" |cut -f1 |grep -o '[0-9]*' |head -n1)
  if [[ -z "$ItemSize" ]]; then return; fi
  if [[ "$ItemSize" -ge "$MaxSize" ]]; then
    echo -ne "\033[33m${FileLoad} \033[0mtoo large to spik.\n";
    return;
  fi
  ${Uploader} -c "${Config}" -t "${Thread}" -b "${Block}" -s "${FileLoad}" -r "${RemoteDIR}" -skip
  if [[ $? == '0' ]]; then
    rm -rf "${FileLoad}";
  fi
  IFS=$IFS_BAK
}
LoadFile;
编辑好上传脚本后，可以检测下脚本编码是否正确，比如我脚本路径为/root/upload.sh，使用命令：

bash /root/upload.sh
如果无任何输出，则正确，反之输出类似$'r': command not found错误，则需要转换下编码格式，具体步骤如下。

先安装dos2unix：

#CentOS系统
yum install dos2unix -y

#Debian/Ubuntu系统
apt install dos2unix -y
再转换编码：

#后面为脚本路径
dos2unix /root/upload.sh
Windows使用
这里就随便补充下Windows使用，先下载程序文件，下载地址→传送门。

比如我将exe文件放到D盘，然后使用Win+R，输入CMD运行，调出窗口后，使用命令：

#进入D盘
cd /d D:\

#国际版初始化，将url换成你上面复制的授权地址，包括http://loaclhost。
OneDriveUploader.exe -a "url"

#个人版(家庭版)初始化，将url换成你上面复制的授权地址，包括http://loaclhost。
OneDriveUploader.exe -ms -a "url"

#中国版(世纪互联)初始化，将url换成你上面复制的授权地址，包括http://loaclhost。
OneDriveUploader.exe -cn -a "url"
然后上传命令和上面一样，只需要把OneDriveUploader改成OneDriveUploader.exe即可。

最后经测试，该版本的上传已经完全能应对各种稀奇古怪的字符问题，如果有问题可以回复下，贴上报错代码，方便修复。
