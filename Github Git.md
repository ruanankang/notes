# Github  

## 1、基本概念

**Repository**（<u>仓库</u>）

用来存放项目代码，每个项目对应一个仓库，多个开源项目则有多个仓库

**Star**（<u>收藏</u>）

收藏项目，方便下次查看

**Fork**（<u>复制克隆项目</u>）

将别人的项目拿来，并新建一个一模一样的仓库

**Pull Request**（<u>发起合并请求</u>）

基于 Fork 的项目，向源项目发起合并请求

**Watch** （关注）

**Issue** （事务卡片）

发现代码bug 但是目前没有成型代码，需要讨论





# Git

通过 git 管理 github 托管项目代码





## 1、基础操作

### 01 设置用户名

```c
git config  --global user.name 'ruanankang'     // 用户名就是 ruanankang
```

### 02 设置用户名邮箱

```c
git config  --global user.name '1574030071@qq.com'  
```

### 03 初始化一个新的 Git 仓库

创建文件夹：  mkdir  #文件夹名#  ->  cd  #文件夹名#  ->  **git init**   初始化     （**pwd**  查看当前文件路径）

### 04 向仓库中添加文件

创建文件：**touch**  #文件名#  ->  添加到暂存区： **git add** #文件名#  -

->  提交到 Git 仓库：**git  commit  -m**  '描述内容'

### 05 修改文件后上传

查看本地仓库情况：**git status**  ->  添加到暂存区：git add #文件名#  -

-> 提交到 Git 仓库：**git commit -m** '描述内容'   ->  查看是否上传成功： **git status** 

### 06 删除文件

先本地删除文件，然后从 Git 中删除文件： **git rm** #文件名#  ->  提交操作： **git commit -m** '描述内容'





## 2、远程仓库

### 01 Git 克隆操作

将远程仓库（github 对应的项目）复制到本地

```c
git clone https://github.com/******    (url链接)
```

### 02 Git 提交远程

```c
git push
```

### 03 切换master到其他分支

```
git checkout 分支
```





常规拉取代码流程：

**stash**(贮藏本地代码)  -> **pull**(拉取代码) -> **merge**(合并代码) -> **apply stash**(应用贮藏) -> **解决冲突** -> **stage**(添加到暂存区) -> **commit**(提交代码至本地仓库区) -> **push**(提交代码至远程仓库)