### Swiftlint用法
#### 安装
* 全局安装
	1. 通过brew安装 <pre>brew install swiftlint
</pre>
	* 在xcode的Build Phases增加 **New Run script Phase** 代码如下： <pre>if which swiftlint >/dev/null; then
  swiftlint
else
  echo "warning: SwiftLint not installed, download from https://github.com/realm/SwiftLint"
fi</pre>
* 局部安装 
	1. 通过CocoaPods安装 ,在Podfile文件增加 <pre>pod 'SwiftLint'
	2. 在xcode的Build Phases增加 **New Run script Phase** 代码如下(⚠️与上面代码不同) <pr>"${PODS_ROOT}/SwiftLint/swiftlint"

#### 使用
* 安装完成后，重新编译就会弹出相关的提示。
* 自动纠正（按照默认配置）代码风格。<pre>swiftlint autocorrect

#### 自定义配置文件
* 创建**.swiftlint.yml** 文件 根据官方实例[官方Rules](https://github.com/realm/SwiftLint/blob/master/Rules.md)，自定义自己的规则

参考文档:</br>
 [SwiftLint，规范代码，成为完美的偏执患者](https://www.jianshu.com/p/40aa8695503f) </br>[realm/SwiftLint](https://github.com/realm/SwiftLint)</br>
