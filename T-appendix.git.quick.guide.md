
# Github快速指南

## Git简介

Git是一个分布式版本控制软件，关键字：分布式，版本控制。

分布式，意味着Git本身并不是集中存放内容的，内容被分布的存放在了各个地方。
版本控制，可以追踪文件变化，可回滚。

加在一起，就可以：
> * 备份文件
> * 跟踪文件变化
> * 协作共同操作文件

## Git仓库结构

对于一个Git仓库，有四个抽象的层：
> * upstream repository云端仓库
> * local repository本地仓库
> * staging area缓存区域
> * working directory工作区

![](images/git_command_relationships.png)

**将我们的操作对应到上面的结构：**  

> * 上行：  
> 我们在jupyter lab中编辑文档是在working directory中，当我们点击了保存，数据被提交到了staging area中，然后在Github Desktop中点击commit，数据就被提交到了local repository中，最后在Github Destop中点击push，数据就被上传到了你Github站点中的云端仓库里，假如你想把你的数据提交给的原始云端仓库，提交pull request。
> * 下行：  
> 在Github站点上，点击fork，可以把我的仓库fork到你的Github站点云端仓库里，通过fetch，会将云端仓库的内容clone到本地仓库，然后用jupyter lab打开编辑就可以了。
> * Branche
> 对于branches，可以理解为是fork之后的再fork，这样就不会影响到“主线”上的数据，除非你把branches跟主线上的数据合并。
