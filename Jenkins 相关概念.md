### Jenkins 相关概念
* **ocunit2junit**：一个脚本，用于将xcodebuild中的OCUnit输出转换为JUnit使用的XML格式。 允许在像Jenkins这样的连续集成服务器上构建XCode，并提供测试报告！<https://github.com/ciryon/OCUnit2JUnit>
* **slather**:为Xcode项目生成测试覆盖率报告, 并且可以挂进CI。<https://github.com/SlatherOrg/slather>
*  **2>&1**:将标准错误也输出到标准输出当中 .<https://blog.csdn.net/zhaominpro/article/details/82630528>
* 证书：Credentials中添加新的你所需的授权，例如远程仓库的账号密码或ssh私钥信息等，上传完成后，Jenkins会为每一个授权赋予一个加密并提供对应此授权的ID，之后需要用到的地方，选择或填写对应的ID即可。<br><https://jenkins.io/zh/doc/book/using/using-credentials/><br><https://www.jianshu.com/p/b197f3fc1789>