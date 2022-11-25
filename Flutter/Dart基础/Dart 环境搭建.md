# Dart 环境搭建
### SDK安装
> 不同平台的安装方式不一样

##### Windows
`choco install dart-sdk`
##### Linux
`sudo apt-get install dart`
##### Mac
`brew tap dart-lang/dart`
`brew install dart`

##### 更多详细的内容可以查看官网
[详细教程](https://dart.dev/get-dart)

##### 查看dart版本
安装成功可以通过下面命令查看具体版本，如果有输出
`Dart VM version: 2.5.0 (Fri Sep 6 20:10:36 2019 +0200) on "macos_x64"` 这样的文案，证明安装成功

* dart --version


### IDE安装
> Dart中的集成开发工具有很多种，比较流行的有：IntelliJ IDEA、Android Studio,VS Code、Sublime Text、Atom；我们课程会使用VS Code进行demo的展示

* VS Code 安装
https://code.visualstudio.com/download 

* ML Complete
> Dart2.5上推出了一个重要的功能:ML Complete，由机器学习 (ML) 驱动的代码补全功能

    ![](https://user-gold-cdn.xitu.io/2019/9/18/16d43f52809db525?imageslim)
    
**如何配置呢？**

* 修改 settings.json 文件
    * 添加 "dart.analyzerAdditionalArgs": ["--* enable-completion-model"]
    * 添加 "editor.suggestSelection": "first"

 [详细介绍请参考](https://juejin.im/post/5d82097ce51d4561ff6668d4)
 
 **注意 :  这种代码补全功能，要结合vscode快捷键才能生效**

 
 
### 最后
 > 所有配置都安装成功以后，展示一个hello world的程序
 
 
```dart
main(){
    print('hello world');
}
```



