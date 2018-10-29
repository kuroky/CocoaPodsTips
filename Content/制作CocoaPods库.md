# 制作CocoaPods库
### 1. CocoaPods的工作流程
![2c5a3729f05e0ed26c16d83ae0ac13f0](https://raw.githubusercontent.com/kuroky/CocoaPodsTips/master/Resource/2c5a3729f05e0ed26c16d83ae0ac13f0.png)

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

### 2. 制作CocoaPos库
#### 2.1 基本流程
首先在桌面新建一个 MXFileManager 目录，在该目录下新建一个 Classes 目录，用来存放框架源码，然后将 MXFileManager 托管到 Git。

- 托管框架源码到 Git

```
创建github仓库，生成demo项目
```
- 创建框架描述信息，在MXFileManager目录下创建Pod文件

```Ruby
pod spec create MXFileManager
```

```Ruby
Pod::Spec.new do |s|
  s.name         = "MXFileManager" // 项目名
  s.version      = "1.1.0" // 版本号
  s.summary      = "iOS 沙盒文件创建与管理" // 项目功能描述
  s.description  = <<-DESC // 项目功能详细描述
                    1.缓存文件创建与管理
                    2.临时文件创建与管理
                   DESC
  s.homepage     = "https://github.com/kuroky/MXFileManager.git" // 项目github地址
  s.license      = "MIT" // 证书
  s.author             = { "kuroky" => "kuro2007cumt@gmail.com" } // 作者
  s.platform     = :ios, "8.0" // iOS最低版本
  s.ios.deployment_target = "8.0" // iOS开发版本
  s.source       = { :git => "https://github.com/kuroky/MXFileManager.git", :tag => "#{s.version}" }
  s.source_files  = "MXFileManager/*.{h,m}" // cocoaPod依赖文件路径
  s.requires_arc = true // arc
end
```
- Github发布一个release版本，版本号与spec文件中填写的一致

```
完成后回到终端
```
#### 2.2 CocoaPods认证
- 注册trunk，在收到的确认邮件中点击链接

```
pod trunk register kuro2007cumt@gmail.com 'kuroky' --verbose
```
- 收到邮件后继续

```Ruby
pod trunk me
```
```Ruby
- Name:     kurokyfan
  - Email:    kuro2007cumt@gmail.com
  - Since:    November 15th, 2016 04:16
  - Pods:
    - KKWKWebView
    - MXFileManager
  - Sessions:
    - November 15th, 2016 04:16 -   March 23rd, 04:16. IP: 101.81.238.31
    Description: create CocoaPods
    - November 15th, 2016 04:19 -    April 8th, 03:21. IP: 101.81.238.31
    Description: create CocoaPods
    - June 23rd, 02:42          - October 31st, 20:38. IP:
    180.169.8.242
```
- 检查.podspec文件的合法性

```Ruby
pod lib lint

-> MXFileManager (1.0.0)
MXFileManager passed validation.
```
- 验证通过，上传spec文件

```Ruby
pod trunk push MXFileManager.podspec
```

```Ruby
Updating spec repo `master`

--------------------------------------------------------------------------------
 🎉  Congrats

 🚀  MXFileManager (1.0.0) successfully published
 📅  October 17th, 00:38
 🌎  https://cocoapods.org/pods/MXFileManager
 👍  Tell your friends!
--------------------------------------------------------------------------------
```
#### 2.3 后续
此时你的 MXFileManager.podspec 就会 pull request 到远程索引库，CocoaPods 官方审核通过后，就可以出现在远程索引库中，当远程索引库收录后：

pod setup
这时你的本地索引库，会新加入 MXFileManager.podspec 这条记录，但是本地索引文件还未更新，因此删除掉以下路径的本地索引文件：

~/资源库/Caches/CocoaPods/search_index.json
执行 pod search MXFileManager 命令，当 search_index.json 文件重建完毕后，就可以在使用这个远程框架库了。

- 命令行 pod setup ， 创建本地索引库
- 命令行 pod install ，将框架集成到项目中


