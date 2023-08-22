---
description: 一个项目管理工具
---
# Maven

## 使用Maven的好处

- 无需手动引入jar包到工程目录
- 解决jar包版本冲突
- 各种配置文件、资源整合到一起进行打包

## 仓库

- 本地仓库  先从本地仓库找对应jar包，如果找不到会从远程仓库去下载 jar 包，保存在本地仓库，第二次不需要从远程仓库去下载
- 远程仓库 
	- 中央仓库 由maven团队 统一维护 http://repo1.maven.org/maven2/ 
	- 私服 架设在公司局域网内，提供给内部的人员使用
	- 第三方仓库 阿里云镜像

## 常用命令

- `clean` 会删除target目录及内容
- `compile` 将 `src/main/java` 下的文件编译为class文件输出到target目录下
- `test` 是执行 `src/test/java` 下单元测试类，并编译为class文件
- `package` 对于java工程打成jar包，对于web工程打成war包
- `install` 打成jar包或war包，并发布到本地仓库
- `deploy` 将jar或war包部署（上传）到私服中


