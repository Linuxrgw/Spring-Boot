# Spring-Boot
部署应用概述
更新时间：2018-12-19 20:09:52

   
 我的收藏
 新手学堂
 学习路径
本页目录
控制台部署应用
工具部署应用
在 EDAS 中，创建的集群类型决定应用类型，不同应用可部署的方式也不一样。

集群类型	应用类型	支持部署方式
ECS 集群	ECS 集群应用	WAR 包、JAR 包
容器服务 Kubernetes 版集群	容器服务 Kubernetes 版集群应用	镜像
Swarm 集群	Swarm 集群应用	WAR 包、JAR 包、镜像
控制台部署应用
使用以下资源来查找部署应用的相关操作教程，以便在 EDAS 中开始开发应用。

注意：使用 JAR 包部署应用的过程跟使用 WAR 包部署应用的操作过程类似，相关操作以 WAR 包部署应用的教程来示意。


部署 ECS 集群应用（WAR）
将一个含欢迎页面的 Java Web 应用快速在 EDAS 上完成创建并用 WAR 包部署。
 

部署容器服务 Kubernetes 版集群应用（镜像）
将一个基于容器服务的 Kubernetes 应用在 EDAS 上完成创建并用自定义镜像部署。


部署 Swarm 集群应用（WAR 包）
将一个 Docker 应用快速在 EDAS 上完成创建并用 WAR 包部署。
 

部署 Swarm 集群应用（镜像）
将一个 Docker 应用快速在 EDAS 上完成创建并用镜像部署。
 
工具部署应用
除在控制台完成部署操作外，您还可通过以下工具来部署应用。

通过 Maven 插件自动化部署应用
通过 edas-maven-plugin 插件来自动化部署普通应用或 Docker 应用。
 

使用 Jenkins 创建持续集成
使用 Jenkins 可以构建 EDAS 应用的持续集成和部署。


使用 Eclipse 插件快速部署应用
使用 Eclipse 插件 Cloud Toolkit 可以快速部署一个 EDAS 应用。
 

使用 CLI 快速部署 EDAS 应用
您可以通过阿里云 CLI，快速在 EDAS 上创建并部署应用。


使用 Intellij IDEA 插件快速部署应用
您可以通过Intellij IDEA 插件，快速在 EDAS 上部署应用。
 

使用云效进行持续集成和发布
本文档介绍了如何使用云效持续集成将代码快速生成部署包，并部署到 EDAS。
