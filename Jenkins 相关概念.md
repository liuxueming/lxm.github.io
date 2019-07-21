### Jenkins 踩坑指南
* **ocunit2junit**：一个脚本，用于将xcodebuild中的OCUnit输出转换为JUnit使用的XML格式。 允许在像Jenkins这样的连续集成服务器上构建XCode，并提供测试报告！<https://github.com/ciryon/OCUnit2JUnit>
* **slather**:为Xcode项目生成测试覆盖率报告, 并且可以挂进CI。<https://github.com/SlatherOrg/slather>
*  **2>&1**:将标准错误也输出到标准输出当中 .<https://blog.csdn.net/zhaominpro/article/details/82630528>
* 证书：Credentials中添加新的你所需的授权，例如远程仓库的账号密码或ssh私钥信息等，上传完成后，Jenkins会为每一个授权赋予一个加密并提供对应此授权的ID，之后需要用到的地方，选择或填写对应的ID即可。<br><https://jenkins.io/zh/doc/book/using/using-credentials/><br><https://www.jianshu.com/p/b197f3fc1789>
* 触发器
	* 	Trigger builds remotely :通过生成token，可以远程构建。即生成token后可以按照指定的URL去远程触发这个任务，而不用登录系统。<https://www.cnblogs.com/Rocky_/p/8297260.html>
	*  Build after other projects are built:在构建其他项目后构建，此时会设置一个观察对象
	*  Build periodically：周期性的构建，**不管源码有没有变化，都会构建**。
	*  Poll SCM:周期性构建，**源码发生变化才构建**。
	*  GitHub hook trigger for GITScm polling：这是GitHub的一个插件。可以通过配置GitHub相关功能，配合这个插件实现，向GitHub提交代码时触发Jenkins自动构建的功能。详细的可以参考：<https://blog.csdn.net/boling_cavalry/article/details/78943061>
		* 注意1：Hook URL的配置方式，请参考：<https://blog.csdn.net/qq_21768483/article/details/80177920>	  	
		* 注意2：localhost无法直接配置，需要内网转发外网，可以借助ngrok。参考链接：</br><https://stackoverflow.com/questions/42037370/jenkinsgithub-we-couldn-t-deliver-this-payload-couldnt-connect-to-server></br>原理：<https://blog.csdn.net/sunansheng/article/details/48372149>
		* 注意3：GitHub Webhooks Secret 和 Jenkins Secret  要配置一样（都是通过 GitHub Developer settings生成的）。

* 常见问题
	* jenkins-doesnt-have-label-xxxx ：需要先在Jenkins配置**Jenkins->Manage Nodes**, <https://stackoverflow.com/questions/52644447/jenkins-doesnt-have-label-linux>
	* fastlane-failed-to-run-pod-install <https://discuss.circleci.com/t/fastlane-failed-to-run-pod-install/3873/6>
	* 如果Jenkins是通过brew安装的。
		* 启动命令：**brew services start jenkins-lts**
		* brew安装关闭命令：**brew services stop jenkins-lts**
		* 修改port方法 <https://stackoverflow.com/questions/7139338/change-jenkins-port-on-macos>
		* 生成pipeline语法：通过在线语法生成器<http://localhost:8888/job/iOS%20AutoTest-Pipeline/pipeline-syntax/>