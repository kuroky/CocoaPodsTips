## 制作基于Git的CocoaPods私有库 
### 创建Git项目
![屏幕快照 2018-10-29 上午10.16.19](https://raw.githubusercontent.com/kuroky/CocoaPodsTips/master/Resource/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202018-10-29%20%E4%B8%8A%E5%8D%8810.16.19.png)

- **Classes** 私有库的类文件
- **EventTrack.podspec** 私有库的podspec
- **EventTrackDemo** 演示Demo
- **LICENSE** MIT证书
- **README.md** 说明

### 编辑podspec文件

```Ruby
Pod::Spec.new do |s|
  s.name             = 'EventTrack'
  s.version          = '1.1.0'
  s.summary          = '基于登录状态的用户追踪'

  s.description      = <<-DESC
追踪当前用户的状态，包括登录、App启动登
                       DESC

  s.homepage         = 'http://www.emucoo.com'
  # s.screenshots     = 'www.example.com/screenshots_1', 'www.example.com/screenshots_2'
  s.license          = { :type => 'MIT', :file => 'LICENSE' }
  s.author           = { 'kurokyfan' => 'kuroky@emucoo.com' }
  s.source           = { :git => 'ssh://git@192.168.16.172:7999/em/ios-eventtrack.git', :tag => s.version.to_s } # 项目地址
  s.ios.deployment_target = '9.0' # 依赖版本

  s.source_files = 'Classes/**/*' # 类文件路径
```
### 在项目中使用

```Swift
// 使用 Master 分支
pod 'EventTrack' , :git => 'ssh://git@192.168.16.172:7999/em/ios-eventtrack.git'

// 指定 Branch
pod 'EventTrack' , :git => 'ssh://git@192.168.16.172:7999/em/ios-eventtrack.git', :branch => 'EventTrack-1.1.0'

// 指定tag
pod 'EventTrack' , :git => 'ssh://git@192.168.16.172:7999/em/ios-eventtrack.git', :tag => '1.0.0'
```
