# CocoaPods使用
### 安装
[CocoaPods官网](https://guides.cocoapods.org/using/getting-started.html)

- 使用Mac自带的Ruby环境就能安装，如果翻墙了直接在终端输入命令:

```
sudo gem install cocoapods
```

- 没有翻墙可以换镜像

```
gem sources -r https://rubygems.org/ // 移除旧版本的镜像
gem sources -a https://gems.ruby-china.org/ // 增加可用镜像
gem sources -l // 查看当前镜像
sudo gem install cocoapods // 安装
```

- CocoaPods版本更新

```
sudo gem install cocoapods --pre
```

- 安装缓慢可以查看文件下载进度，新开一个终端窗口，跳到cocoapods文件夹内，查看正在下载的文件夹的大小

```
cd ~/.cocoapods/
du -sh *
```
### 更新
更新本地仓库

```
pod repo update
```

### 集成第三方库
- 为当前项目新建podfile文件

```
// 在项目根目录下输入
pod init
pod install 
```

- 配置Podfile文件

```
# Uncomment this line to define a global platform for your project
platform :ios, '8.0' // 最低支持的版本
inhibit_all_warnings! // 忽略第三方库警告

target 'Emucoo' do // 当前依赖的项目名
  # Uncomment this line if you're using Swift or would like to use dynamic frameworks
  # use_frameworks!
  
  # Pods for Emucoo
  pod 'AFNetworking', '3.1.0'
  pod 'FMDB', '2.7.2'
  pod 'SDWebImage', '4.0.0'
  pod 'MBProgressHUD', '1.0.0'
end
```

- CocoaPods生成的文件

```
1. podfile文件为项目的每个target定义(在不同的iOS版本上运行时)所需要的依赖项目
2. podifle.lock文件用于记录当前每个依赖项目的版本，保证该项目的版本信息不被改变
2. xcworkspace文件为使用CocoaPods之后项目的启动文件
3. Pods文件夹是项目的依赖存放的地方
```

- 项目版本的指定

```
pod '项目名称', ‘版本号’
如
pod 'AFNetworking', '3.0.0'
给出版本范围 
符号>、>=、<、<=都能用， 
如： 
'> 0.1' 
符号~>用法见如下例子： 
'~> 0.1.2' 
表示范围为 >=0.1.2&&<0.2 
'~> 0.1' 
表示范围为>=0.1&&<1 
3、添加本地项目作为依赖 
如： 
pod 'Alamofire', :path => '~/Documents/Alamofire'
```

至此工程的CocoaPods配置完成。
### 其他命令

- 列出比podfile.lock文件记录的版本要新的项目

```
pod outdated
```
- 将某个依赖更新到最新版本 直接pod update就把所有依赖都更新到最新版本

```
pod update [依赖项目名]
```
- 获取不到最近的库

方法一. 将本地repo更新
```
pod repo update
```

方法二.解决方案：删除cocoapods重新安装下载

```
sudo rm -fr ~/.cocoapods/repos/master
pod setup
```

###CocoaPods工作流程
![2c5a3729f05e0ed26c16d83ae0ac13f0](media/14792883587567/2c5a3729f05e0ed26c16d83ae0ac13f0.png)

- 远程索引库：远程索引库里存放的是各种框架的描述信息，托管在 Github 上
- 本地索引库：在 install cocoapods 命令后，需要执行 pod setup 这个命令，pod setup 命令就是将远程索引库克隆到本地来，本地索引库的路径如下

```
~/.cocoapods/repos/master
```

- 本地索引文件 当执行 pod search 命令时，如果本地索引文件不存在，会创建这个文件。

```
pod search AFNetworking
Creating search index for spec repo 'master'..

索引路径：~/资源库/Caches/CocoaPods
```

###制作CocoaPos库
####基本流程
首先在桌面新建一个 MXFileManager 目录，在该目录下新建一个 Classes 目录，用来存放框架源码，然后将 MXFileManager 托管到 Git。

- 托管框架源码到 Git

```
创建github仓库，生成demo项目
```
- 创建框架描述信息，在MXFileManager目录下创建Pod文件

```
pod spec create MXFileManager
```

编辑MXFileManager.podspec文件

```
kurokydeMacBook-Pro:MXFileManager kuroky$ pod trunk me
  - Name:     kurokyfan
  - Email:    kuro2007cumt@126.com
  - Since:    November 15th, 2016 04:16
  - Pods:
    - KKWKWebView
  - Sessions:
    - November 15th, 2016 04:16 -   March 23rd, 04:16. IP: 101.81.238.31
    Description: create CocoaPods
    - November 15th, 2016 04:19 -    April 8th, 03:21. IP: 101.81.238.31
    Description: create CocoaPods
    - June 23rd, 02:42          - October 29th, 02:46. IP:
    180.169.8.242
```
- 上传.podspec文件到 https://github.com/CocoaPods/Specs

1.注册trunk

```
pod trunk register kuro2007cumt@126.com 'kuroky' --verbose
```
2.点击邮件中的确认链接
3.查看注册结果

```
pod lib lint
```
4.检查.podspec文件的合法性

```
pod trunk push MXFileManager.podspec
```

```
Updating spec repo `master`

--------------------------------------------------------------------------------
 🎉  Congrats

 🚀  testLib (1.0.0) successfully published
 📅  October 17th, 00:38
 🌎  https://cocoapods.org/pods/MXFileManager
 👍  Tell your friends!
--------------------------------------------------------------------------------
```

此时你的 MXFileManager.podspec 就会 pull request 到远程索引库，CocoaPods 官方审核通过后，就可以出现在远程索引库中，当远程索引库收录后：

pod setup
这时你的本地索引库，会新加入 MXFileManager.podspec 这条记录，但是本地索引文件还未更新，因此删除掉以下路径的本地索引文件：

~/资源库/Caches/CocoaPods/search_index.json
执行 pod search MXFileManager 命令，当 search_index.json 文件重建完毕后，就可以在使用这个远程框架库了。

- 命令行 pod setup ， 创建本地索引库
- 命令行 pod install ，将框架集成到项目中


