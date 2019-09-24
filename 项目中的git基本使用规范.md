### 项目中的git基本使用规范
* 前言：实际项目中的git流程

1. 最新代码全部在**master**分支
2. 稳定分支为**stable**分支
3. 三个主要的文件夹
	* **feature** :`feature/xxx-feature`业务功能相关
	* **bugfix**  :`bugfix/some-bug`修复bug相关
	* **tech**    :`tech/xxx`技术相关
	
4. 先在基于**master**在本地创建相关分支，开发完毕后push到远程
5. 小步commit
6. 若远程分支有更新 则先`rebase`,如有冲突，逐步解决。
7. merge到master分支至少有两人code review 并 approve。