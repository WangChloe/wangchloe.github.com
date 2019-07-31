---
title: 每天10个前端知识点：代码管理及常见命令
date: 2017-03-14 03:27:35
categories: 前端札记
tags: [工具]

---

以下内容若有问题烦请即时告知我予以修改，以免误导更多人。



---

- svn代码版本管理工具
- git分布式版本控制系统
	- 工作区
	- 缓存区
	- 本地仓库
	- 服务器仓库
	- 其他
	- reset的两种用法
	- reset命令的3种方式
- git的使用姿势
	- 本地代码放到github上
	- 服务器仓库已有项目
	- 多人合作
	- 关于SSH配置
- git与svn的区别
- 常用命令
	- DOS
	- Linux
	- 编辑文件
	- Node.js
- 常用快捷键
	- Windows
	- Firebug
	- 调试
	- Sublime
	- Emmet

<!-- more -->

## 1. svn代码版本管理工具
Subversion

1. 更新 `update`
2. 修改
3. 增加(已存在文件跳过这步) `add`
4. 提交(注释) `commit`

## 2. git分布式版本控制系统

![](http://www.ruanyifeng.com/blogimg/asset/2015/bg2015120901.png))

> 工作区 -> 缓存区 -> 本地仓库 -> 服务器仓库

### 工作区
- `git init `          当前目录改为git目录，变为工作区

### 缓存区
- `git add xx.txt`     添加一个文件至缓存区
- `git add .`          添加所有文件至缓存区
- `git rm --cache xx.txt`  从缓存区删除一个文件

### 本地仓库
- `git commit -m '注释'`   添加文件至本地仓库

### 服务器仓库

- `git remote add origin <github上的链接>`
- `git push -u origin master`

### 其他
- `git status`         查看git此时状态
- `git log `  			日志

- `git reset` 跟一个commit(key)回到某个状态下

### reset的两种用法
- `git reset [-q] [commit] [--] <paths>`

- `git reset [--soft | --mixed | --hard | --merge | --keep] [-q] [<commit>]`

### reset命令的3种方式

- `git reset –mixed`  此为默认方式，不带任何参数的git reset，即时这种方式，它回退到某个版本，只保留源码，回退commit和index信息

- `git reset –soft`  回退到某个版本，只回退了commit的信息，不会恢复到index file一级。如果还要提交，直接commit即可

- `git reset –hard`  彻底回退到某个版本，本地的源码也会变为上一个版本的内容
   eg: `git reset --hard HEAD^` 最新一次提交的父提交

## 3. git的使用姿势

### (1) 本地代码放到github上
**服务器上没有**

1. 跟github建立一个关系 `git remote add origin <github上的链接>`

2. 推送到github上  `git push -u origin master`

### (2) 服务器仓库已有项目
`git clone <github上的链接>`

### 多人合作
[ git merge 和 git rebase 小结](http://blog.csdn.net/wh_19910525/article/details/7554489)

### 关于SSH配置
[http://jingyan.baidu.com/article/5bbb5a1b17107e13eba179d1.html](github如何创建密钥)
**注意：id_rsa.pub才是公用秘钥，把这个放进github**

配置

`git config -l`     查看此时git的配置文件

`git config --global user.name '名字'`

`git config --global user.email '邮箱地址'`

## 4. git与svn的区别
> 来源博客

1. git是分布式的，svn不是。
git跟svn一样有自己的集中式版本库或服务器。但git更倾向于被使用于分布式模式，克隆版本库后即使没有网络也能够commit文件，查看历史版本记录，创建项目分支等，等网络再次连接上Push到服务器端。

2. git把内容按元数据方式存储，而svn是按文件。
所有的资源控制系统都是把文件的元信息隐藏在一个类似.svn,.cvs等的文件夹里。
git目录是处于你的机器上的一个克隆版的版本库，它拥有中心版本库上所有的东西，例如标签，分支，版本记录等。

3. git没有一个全局的版本号，svn有。

4. git的内容完整性优于svn。
因为git的内容存储使用的是SHA-1哈希算法。

5. git可以有无限个版本库，svn只能有一个指定中央版本库。
当svn中央版本库有问题时，所有工作成员都一起瘫痪直到版本库维修完毕或者新的版本库设立完成。
每一个git都是一个版本库，区别是它们是否拥有活跃目录（Git Working Tree）。如果主要版本库（例如：置於GitHub的版本库）有问题，工作成员仍然可以在自己的本地版本库（local repository）提交，等待主要版本库恢复即可。工作成员也可以提交到其他的版本库！

## 5. 常用命令
这些命令碰见了再慢慢补全吧

### DOS
- `d:`	 切换盘符
- `dir`      查看当前目录(文件夹)下所有文件夹及文件
- `cls`      清屏
- `cd xx`    进入目录(文件夹)

### Linux
- `ls`       查看当前目录(文件夹)下所有文件夹及文件
- `cd xx `   进入xx文件夹
- `cd ..`    返回上级目录(文件夹)
- `cd /`     返回根目录
- `touch xx` 创建文件并命名为xx
- `rm xx`    删除文件xx
- `clear`    清屏
- `cat xx`   查看文件xx的内容
- `mkdir xx` 创建目录(文件夹)并命名为xx
- `rmdir xx` 删除目录(文件夹)xx，当文件夹内容不为空时不能删除
- `rmdir -rf` 删除目录(文件夹)xx并删除其内容

#### 编辑文件
- 进入文件 `vi 文件名`
- 启用编辑 `按insert键`
- 开始编辑
- 保存  `esc -> :wq+ -> 回车`

快捷命令

- 创建文件并且输入内容 `echo 内容 > xxx.txt`

[http://www.cnblogs.com/roucheng/p/linuxdos.html](DOS 和 Linux 常用命令的对比)

### Node.js
这是一次看慕课视频的时候记下的

- `npm ls -g`  显示全局所有安装包
- `--depth=1`  最多展示一层
- `>	`	   重定向
- `2>/dev/null`  1 stand out   2 stand error 输出错误重定向到空设备文件
- `| `     上一个输出内容转为下一个输入内容
- `grep xxx-`  检索xxx开头的安装包

## 6. 常用快捷键

### Windows
- 显示桌面    `Win + D`
- 打开资源管理器    `Win + E`
- 相当于鼠标右键（等同于Shift + F10））    ![键盘右侧ctrl和alt中间的键](http://img.blog.csdn.net/20160511172120745)
- 新建文件夹    `Ctrl + Shift + N`
- 焦点移至任务栏托盘区    `Win + B`
- 打开所有活动窗口的切换栏    `Ctrl + Shift + Alt +Tab`
- 与上条想必按键松开切换栏会消失    `Alt + Tab`
- 打开Windows放大镜    `Win + "+"`
- 关闭当前标签页（浏览器中应用较多）    `Ctrl + W`
- 关闭窗口/关机    `Alt + F4`
- 定位到地址栏并选中文本（资源管理器/浏览器）   `Ctrl + L`
- 快速定位到地址栏（资源管理器浏览器）    `Alt + D`
- 刷新页面（浏览器）`Ctrl + R`
- 显示主窗口的“系统”菜单    `Alt + Space`
- 切换到多文档界面的下一个子窗口    `Ctrl + Tab`
- 在同一个程序多个窗口之间切换   `Alt + F6`
- 打开选定对象的属性   `Alt + Enter`
- 重命名对象   `F2`
- 查找所有文件   `F3`
- 在资源管理器中的窗格之间移动   `F6`

### Firebug

- 打开Firebug   `F12`
- 在新窗口中打开Firebug   `Ctrl + F12`
- 快速定位元素   `Ctrl + Shift +C`

### 调试

设置断点后

- 继续   `F8`
- 单步跳过   `F10`
- 单步进入   `F11`
- 单步退出   `Shift + F11`

### Sublime
- 选中一个元素   `Ctrl + D`
- 多选中同一个相同的元素     `按住Ctrl + D不放`
- 多行光标定位

>eg:`选中前五行` -> `Ctrl + Shift +L`  (光标定位在前五行末尾)
   —> `Home`  (光标定位在前五行行首)
   —> `Shift（不动）+ End（7次）` (选中前五行的前7个字符)
   或 —> `Ctrl + Shift +End(3次)` (选中前五行的前三列)

- 全屏   `F11`
- 快速打开文件  `Ctrl + P`
- 打开命令面板   `Ctrl + Shift + P`
- 打开/关闭SideBar   `Ctrl + K/B`
- 放大/缩小字号   `Ctrl + "+"/"-"`
- 打开一个新页面   `Ctrl + N`
- 在多个页面中跳转   `Ctrl + Tab`
- 合并两行（光标定位在第一行末尾处）   `Ctrl + J`
- 缩进/退回一个级别   `Ctrl + "]"/"["`
- 选中当前行   `Ctrl + L`
- 当前行下方新建行（允许光标不在当前行末尾时使用）   `Ctrl + Enter`
- 当前行上方新建行（允许光标不在当前行末尾时使用）  `Ctrl + Shift + N`
- 调整页面缩进   `Ctrl + Shift + P —>reindent lines`
- 打开控制台   `Ctrl + （Tab上面那个键） ->输入Sublime.log_command(True)查看命令名及参数`
- 本文件查找字符串   `Ctrl + F -> Enter 查找下一处
                                或->Shift + Enter查找上一处`
- 关闭当前文档   `Ctrl + W`
- 护肤已关闭的标签 `Ctrl + Shift + T`
-  删除当前行   `Ctrl + X`
- 当前位置插入注释  `Ctrl + Shift + /`
- 从光标处开始删除代码至行尾   `Ctrl + K + K`
- 闭合标签   `Alt + "."`
- 默认1屏 `Alt + Shift + "1"`
- 左右分屏2列 `Alt + Shift + "2"`
- 左右分屏3列 `Alt + Shift + "3"`
- 左右分屏4列 `Alt + Shift + "4"`
- 上下左右等分4屏（“田”字） `Alt + Shift + "5"`
- 垂直等分2屏 `Alt + Shift + "8"`
- 垂直等分3屏 `Alt + Shift + "9"`
- 选中当前最小区域  `Ctrl + Shift +Space -> Ctrl +Shift + Space 向外扩展选中区域`
- 局部矩形选中字符   `鼠标滚轮按住拖动`
- 去除最内层标签嵌套（光标定位在最内层）   `Ctrl + Shift + ";"`

- `ctrl+shift+d`  复制粘贴当前行
- `ctrl+shift+[`  折叠代码
- `ctrl+shift+]`  展开代码
- `ctrl+shift+v`  粘贴并格式化
- `ctrl+g `       跳转到第几行
- `ctrl+m `       跳转到对应括号
- `ctrl+w  `      关闭当前文件
- `ctrl+r `		  前往method
- `alt+数字 `     切换至当前窗口的第N个文件
- `alt+.   `  	  闭合标签
- `alt+F3   `     选中所有相同的词
- `ctrl+鼠标左键点击` 标记多个光标

###Emmet
- p20  ->  `Tab` -> padding: 20px;
- m0-auto -> `Tab` -> margin: 0 auto;
- .nav -> `Tab` -> `<div calss= "nav"></div>`
- #nav -> `Tab` -> `<div id= "nav"></div>`
- .siderbar>.nav -> `Tab` ->

```
<div calss= "siderbar">
	<div class="nav"></div>
</div>
```

- ul.nav>.li*3 -> `Tab` ->

```
<ul calss= "nav">
	<li></li>
	<li></li>
	<li></li>
</ul>
```

- 选中需嵌套的字符串 `Ctrl + Shift +G ->``<div>xxx</div>`
`.sidebar->``<div class= "sidebar"></div>`
`.sidebar>nav``<div class= "sidebar"><nav>xxx</nav></div>`
`.sidebar>#nav``<div class= "sidebar"><div id="nav">xxx</div></div>`


---
更多内容可以订阅本人微信公众号，一起开启前端小白进阶的世界！
![微信公众号：无媛无故](http://ww1.sinaimg.cn/large/006tNc79gy1g59sd1aky1j325s0m80xf.jpg)