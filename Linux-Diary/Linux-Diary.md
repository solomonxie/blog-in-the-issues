# Linux 学习日记  
# 第一日 安装Ubuntu  
为了满足学习需要和日常需要，选择了双系统共存。  
Windows和Ubuntu双系统安装有三四种方法。  
最简单、合理的当然是在windows环境下安装ubuntu。  
硬盘分区  
在进行所有安装操作之前，必须要进行硬盘分区。因为之前我的win7是32位的，顺便重装个64位系统。  
-	分区前准备工作  
1.	先给windows所有重要文件备份，同时下载一个64位的win7的ghost。  
2.	将win7的ghost文件装入U盘，重启电脑，按F 2进入BIOS,将第一启动设为U盘，保存退出。  
3.	插上U盘，重启电脑后进入U盘的老毛桃系统，在winPE系统里进行分区操作。  
-	正式进行分区  
1.	删除所有分区，重新设计。  
2.	windows需要的就设位2个：保留C盘位windows主盘，ntfs格式，50G。D盘（必须）格式化为Fat32格式，20G。  
3.	剩下的“自由空间”有90G ，不格式化，都是留给ubuntu用的。ubuntu内部分区在之后安装的时候再设计。  
-	安装win7  
在老毛桃中安装ghost到c盘，正常安装。完毕后进入windows，安装必备软件。  
这样一来，windows环境就具备了，下一步是在windows环境下安装ubuntu。  
  
## 安装Ubuntu  
第一步实验了最简单的：利用Ubuntu自带的wubi程序。下载Ubuntu镜像.iso文件后，解压缩，有一个wubi.exe。  
安装好后，重启后就容易找不到系统了，很麻烦。所以放弃这种方法，选择用第三方的easyBCD软件引导安装ubuntu。  
-	下载ubuntu  
到Ubuntu官网下载最新版本的“稳定版”64位的，下好后是.iso格式。  
-	利用easyBCD软件进行引导安装  
1.	下载easyBCD软件，安装。  
2.	打开后按照http://www.linuxidc.com/Linux/2014-04/100369.htm 进行操作。  
然后就会出现一个menu.lst文件  
   
我们要编辑这个文件 因为系统就是这个文件找到我们的Ubuntu的。  
把下面的 英文 复制进去，把原来的全覆盖掉  
```
title Install Ubuntu
root (hd0,0)
kernel (hd0,0)/vmlinuz boot=casper iso-scan/filename=/ubuntu-14.04-desktop-i386.iso ro quiet splash locale=zh_CN.UTF-8
initrd (hd0,0)/initrd.lz  
```  
需要改的地方:  
1.	`ubuntu-14.04-desktop-i386.iso`是下载ubuntu的iso的名字改一下。  
2.	其中`(hd0,0)`指的是之后要用到的存储引导文件的磁盘位置（经过测试，必须是Fat32格式！）。因为C盘是ntfs不方便，就用格式化为fat32的D盘(注意：一旦确定了D盘，安装ubuntu完毕后就不能变了，如果那几个下面所说的几个文件不在D盘，ubuntu就加载不了，怎么改都没用)。位置是`(hd0,4)`。（hd0指的是第1块硬盘，hd1的话就是第2块硬盘。`hd0,0`是第1块硬盘的第1个分区，一般也就是C盘。`hd0,0`一直到hd0,3都是属于主分区的，而D盘是逻辑分区，要从`hd0,4`开始。所以D盘是`hd0,4`;E盘是`hd0,5`）  
3.	保存文件，关闭。  
  
把ubuntu镜像文件随便找个地方解压缩，把文件夹中的.disk文件夹和casper文件夹中的`initrd.lz`和`vmlinuz`文件复制到D盘根目录，把ubuntu镜像文件也放在D盘根目录，一共4个。  
  
重启 你就会看到有2个 启动菜单给你选择（win7和neogrub） 我们选择 NeoGrub 引导加载器 这个选项。  
   
然后稍等待一段时间 就会见到我们想要安装的 Ubuntu了。  
   
进入ubuntu的待装系统后，按`Ctrl+Alt+T `打开终端，输入代码:`sudo umount -l /isodevice`这一命令取消掉对光盘所在 驱动器的挂载（注意，这里的-l是L的小写，-l 与 /isodevice 有一个空格。），否则分区界面找不到分区。  
   
点击桌面上ubuntu开始安装  
  
下面就点击 安装Ubuntu 14.04开始安装，选语言不用说，  
    
   
   
   
选安装类型，我们用其他选项。 我也不卸载原来安装的Ubuntu 13.10了，等下覆盖安装。  
   
因为是第一次安装，这一步必须选“其他选项”，因为要在这里进行ubuntu内部的磁盘分区。  
  
## Ubuntu分区  
网上有很多种分区方案，先了解了下linux各分区的作用。  
大概就是：/home代表存储个人文件的地方，类似D盘，以后刷系统也不会把这个地方删掉，我设定到最大，80G；swap代表虚拟内存，一般跟物理内存一样就行，我设置了4G；挂载点/的是存系统用的，我设置了20G。  
具体什么概念，再查一下吧，不太懂。  
    
  
之后就一直下一步，默认操作，很简单完成安装。  
更改语言为英语  
因为学习需要，安装虽然默认中文，但是装好了后改为英文。  
1.	打开系统设置，里面有个“语言支持”。  
2.	添加或删除语言。因为我是从中文转英文，所以直接把英文选项拖拽到最上方就行了。  
3.	地区格式栏，可以保留中文，因为货币、日期之类的。然后应用到整个系统就行了。  
  
## 安装视频用的VLC和Chrome  
打开ubuntu应用中心，搜索下载VLC和chromium，安装就行了。  
   
然后更改默认视频播放器和默认浏览器，都在系统设置里：  
   
设置成功。  
  
# 第二日 安装中文输入法及WPS Office  
这个部分真的相当麻烦，尝试了好多种方法都不行。  
-	WPS office虽然说有64位的，但是官网下载64位的不能用，别人也一样，所以只能下载32位的。但是Ubuntu最新64位版本都把32位的支持库删掉了，需要手动添加32位库的支持。  
-	搜狗拼音也装不上，试了很多种方法，输入了n多行代码，解决了出现的好多错误，最后即使装上了也打不开。还试了google拼音，也不行，干脆用自带的sunpinyin吧，凑合了（只是像eu结尾的字打不出来，如月和学，只能用缩写找）。  
-	经验：不能同时开2个apt-get，这样的话terminal终端会无法继续。（但是目前还不知道怎么算开2个apt-get，反正不是开2个terminal这么简单，有可能是开着software center?。）  
## 丢失Language Support  
好像是安装输入法的过程中，装了好多东西，又删了好多东西，copy过来的代码我也不太懂。后来系统设置里的language support栏目没有了。  
然后搜索发现，是因为删除ibus语言、安装搜狗fcitx输入法把它删了。  
一行代码就回来了：  
`sudo apt-get install language-selector-gnome  `  
  
## 安装新立得软件包管理器 Synaptic  
在software center里搜索synaptic，安装，完成。  
新立得能管理系统里所有安装的包（程序？），卸载、更新已有的和搜索、安装没有的。  
  
## 添加32位库支持  
参考了这篇文章：http://linux.cn/article-2935-1.html 代表了大多数意见。不成功。  
然后发现这篇文章：http://forum.ubuntu.org.cn/viewtopic.php?p=3045611  
得知Ubuntu13 之后已经取消了`ia32-libs`这个包了，融入到了叫`multiarch`的包里。虽然我用的ubuntu14已经有了这个包，但是对于安装deb格式的软件来说，还是不支持它们的32位。还需要下载ubuntu12以前的ia32-libs的deb包来安装。  
测试，全部无效，各种不支持。  
  
## 安装解压缩软件  
参考：http://blog.csdn.net/chinaclock/article/details/42750501  
在software center安装了2个软件：7zip和ark。成功。  
  
  
## 学习Ubuntu快捷方式  
`HUD（Head-up Display）  `  
用来通过输入指令调用该程序的各种菜单项。如writer中，按下Alt键弹出hud，然后输入table insert，回车，就插入表格了。  
  
## Dash  
按下win键(linux里这叫super键)，调出dash。可以搜索电脑里所有应用程序和在线资源。  
  
# 第三日 解决中文输入法／电子词典，学习《Linux就是这个范儿》  
## 安装谷歌拼音  
经历过昨天的折腾，今天几分钟就解决了。参考这篇文章：  
Ubuntu 14.04中文输入法的安装  
  
http://sixipiaoyang.blog.163.com/blog/static/623235882014450916276/  
  
1.	打开应用中心，安装IBUS-googlepinyin和ibus-sunpinyin（这个已经有了）  
2.	关键一步（之前在text-entry settings找不到就是因为这步）  
-	打开语言支持，然后将汉语拖拽到第一位（我的系统默认是英文，如果不做这一步，那么就找不到新装的输入法。）  
-	点击apply system-wide应用到全局  
   
3.	打开text entry settings设定，按+号添加语言，搜索google，添加googlepinyin。  
4.	点击右上角任务栏的语言栏，选择googlepinyin。  
5.	回到text entry settings，点-号删除sunpinyin。关闭窗口。  
6.	回到语言支持，把第一位语言改回英文，并应用，关闭。大功告成！  
  
## 中文输入法的细节设定  
有个地方很烦，就是每次要输入东西时候，旁边老出现输入法的面板。  
解决步骤如下：  
1.	打开computer文件夹，进入`computer/usr/share/applications`文件夹，打开`Keyboard Input Method`，这是专门设置ibus输入法的（googlepinyin是ibus的一个扩展）中文叫 键盘输入法，打开。  
2.	将`show property panel`面板显示设定为do not show，完事。  
   
  
## 安装电子词典  
先用应用中心安装了stardict和有道词典linux版。  
-	有道词典  
有道词典安装成功后，打开界面很友好，联网搜索效果也和windows一样。  
但是输入栏里面没法切换中英输入法，不管按什么都只能输入中文。而且鼠标指针有点怪，用起来很不爽，卸载。  
-	stardict  
stardict安装成功后在菜单里打不开。然后把菜单切换到教育菜单，再打开，成功了。  
词典是没有词库的，什么都查不了，所以需要下载词库。  
http://abloz.com/huzheng/stardict-dic/zh_CN/  
下载一个郎道的中英和一个英中的tar.bz包，然后在文件夹中右键解压缩。  
打开terminal终端，输入指令把两个词库文件夹移动到软件的词库文件夹里（手动剪切复制不成功）  
`sudo mv stardict-oxford-gb-2.4.2 /usr/share/stardict/dic/  `
前面是词库文件夹的名，后面是需要存入的软件目录。只需要改词库名就好了。  
移动成功，打开软件，就可以查词了。  
缺点是界面太次，不能联网搜索，显示结果很有效。  
解决方案，还不如用网页查寻。  
  
# 第四日 安装、使用Wine，virtualbox和下载工具  
## 安装wine  
1.	应用中心搜索wine  
2.	选中wine windows 程序加载器，点击more  
3.	全选附加项  
4.	点击安装  
  
## 解决Wine中文乱码问题  
安装QQ过程中，遇到乱码，所以解决这个问题。  
参考的文章这个最靠谱：  
ubuntu下wine中文乱码解决（彻底篇）  
http://blog.163.com/gavin168@126/blog/static/1390741142010499583562  
或http://blog.chinaunix.net/uid-26527046-id-3483227.html  
新装的wine中文全是乱码，需要修改一下几个配置文件，找到一篇比较详细的配置说明，分享一下：  
### wine下中文的配置方案  
步骤：   
1. 初始设置  
运行 winecfg，把模拟的 Windows 系统设置为 Windows XP 或者 Windows 2000。  
2. 准备字体  
为了让 Windows 应用程序看上去更美观，所以需要 Windows 下面的字体。  
由于我已经将 `simsun.ttc` 复制到` /usr/share/fonts/windows/` 目录中了。所以我只需要在 `~/.wine/drive_c/windows/fonts/` 目录中为 `simsun.ttc` 创建一个符号连接：  
```
cd ~/.wine/drive_c/windows/fonts  
ln -s /usr/share/fonts/windows/simsun.ttc simsun.ttc  
ln -s /usr/share/fonts/windows/simsun.ttc simfang.ttc  
```  
创建一个 `simfang.ttc` 是许多 Windows 应用默认使用 `simfang.ttc` 字体。  
3. 修改` ~/.wine/system.reg  `  
装好字体后，还要修改一下 Wine 的注册表设置，指定与字体相关的设置：  
`gedit ~/.wine/system.reg  `  
（一定要使用 gedit 或其他支持 gb2312/utf8 编码的编辑器修改这些文件，否则文件中的中文可能变乱码）  
搜索：` LogPixels  `  
找到的行应该是：`[System\\CurrentControlSet\\Hardware Profiles\\Current\\Software\\Fonts]  `  
将其中的：  
`"LogPixels"=dword:00000060  `  
改为：  
`"LogPixels"=dword:00000070  `   
搜索： `FontSubstitutes`  
找到的行应该是：  `[Software\\Microsoft\\Windows NT\\CurrentVersion\\FontSubstitutes]  `  
将其中的：  
```
"MS Shell Dlg"="Tahoma"  
"MS Shell Dlg 2″="Tahoma"  
```
改为：  
```
"MS Shell Dlg"="SimSun"  
"MS Shell Dlg 2″="SimSun"  
```  
4. 修改` ~/.wine/drive_c/windows/win.ini`    
`gedit ~/.wine/drive_c/windows/win.ini  `  
在文件末尾加入：  
```
[Desktop]  
menufontsize=13  
messagefontsize=13  
statusfontsize=13  
IconTitleSize=13  
```  
5. 最关键的一步，网上很多文章中没有提到的一步──把下面的代码保存为` zh.reg`，然后终端执行`regedit zh.reg`。从Windows目录下的Fonts里的`simsun.ttc`复制到`/home/user/.wine/drive_c/windows /fonts`里面。  
代码:    
```
      REGEDIT4  
      [HKEY_LOCAL_MACHINE\Software\Microsoft\Windows NT\CurrentVersion\FontSubstitutes]  
      "Arial"="simsun"  
      "Arial CE,238"="simsun"  
      "Arial CYR,204"="simsun"  
      "Arial Greek,161"="simsun"  
      "Arial TUR,162"="simsun"  
      "Courier New"="simsun"  
      "Courier New CE,238"="simsun"  
      "Courier New CYR,204"="simsun"  
      "Courier New Greek,161"="simsun"  
      "Courier New TUR,162"="simsun"  
      "FixedSys"="simsun"  
      "Helv"="simsun"  
      "Helvetica"="simsun"  
      "MS Sans Serif"="simsun"  
      "MS Shell Dlg"="simsun"  
      "MS Shell Dlg 2"="simsun"  
      "System"="simsun"  
      "Tahoma"="simsun"  
      "Times"="simsun"  
      "Times New Roman CE,238"="simsun"  
      "Times New Roman CYR,204"="simsun"  
      "Times New Roman Greek,161"="simsun"  
      "Times New Roman TUR,162"="simsun"  
      "Tms Rmn"="simsun"  
 ```  

## 安装2015最新版QQ7.5（Wine）  
先上官网下载qq的exe文件，下载后右键点击open with wine~~~，用wine打开。  
然后遇到提示“安装路径无效您没有权限”。  
然后知道，由于QQ2013以后版本运用了nprotct保护程序，与wine兼容性较差，因此会经常崩溃。所以放弃了wine QQ。  
  
## 安装IE （Wine）  
先下载ie8的exe文件，用wine打开，失败，提示“此操作系统不支持ie8”。   
下载ie7和ie6，打开，没用，彻底放弃wine。  
  
## 下载工具  
据说uget不错，在软件中心下载安装。  
http://baike.renwuyi.com/2014-11/2153.html  
  
  
## 安装virtualbox  
-	在 Ubuntu 12.10 中使用 Virtualbox 安装 Win7  
http://www.linuxidc.com/Linux/2012-11/74195.htm  
-	VirtualBox虚拟机：安装Ghost win7  
http://jingyan.baidu.com/album/6c67b1d6e977d12787bb1e22.html  
-	Ubuntu下使用VirtualBox  
http://blog.csdn.net/htttw/article/details/6776429  
- 下载virtualbox  
官网选择linux版本64位的deb包  
https://www.virtualbox.org/wiki/Linux_Downloads  
  
- 下载win7镜像（不知道是安装版还是ghost版）  
  
  
  
# 第五日 在Windows环境下安装Linux虚拟机  
由于双系统来回切换各种不便（主要是VPN、BT下载和云同步之类的），最后决定还是在windows环境下用虚拟机来操作linux，这样对我这个新手还算是比较友好的。  
因为买的Linux参考书是以CentOS为例，所以就不用ubuntu了，改用centos入门。  
## 在VMware虚拟机中安装CentOS  
参考文章：http://jingyan.baidu.com/article/86112f135e584a273697876b.html  
先在VMware官网下载最新的VMware Workstation，并安装。  
安装步骤就是一路下一步，最后要求输入序列号。根据版本百度一下，我找到的是`1F04Z-6D111-7Z029-AV0Q4-3AEH8` 。  
然后再到CentOS官网下载镜像文件，我选择的是`CentOS-7-x86_64-LiveCD-1503.iso`。其中可以选择的各种版本介绍如下：  
bin就是直接程序安装  
livecd是CD光盘镜像，刻录在U盘或者CD光盘上安装。  
livedvd是DvD镜像，就是把CD镜像拆成多个分卷，用于刻录DVD光盘，每个分卷的大小为一张DVD光盘大小。有多个分卷，不用，不推荐。  
- netinstall是网络安装包  
- minimal是最简安装包  
我的当然就是livecd版本。然后打开vmware开始安装，参考：http://www.centoscn.com/image-text/setup/2014/0723/3341.html  
  
配置好虚拟机，运行centos后，出现错误，提示64位出错问题：  
```
This virtual machine is configured for 64-bit guest operating systems. However, 64-bit operation is not possible. This host is VT-capable, but VT is disabled.   
VT might be disabled if it has been disabled in the BIOS settings or the host has not been power-cycled since changing this setting. (1) Verify that the BIOS settings enable VT and disable 'trusted execution.'   
(2) Power-cycle the host if either of these BIOS settings have been changed.   
(3) Power-cycle the host if you have not done so since installing VMware Workstation.   
(4) Update the host's BIOS to the latest version.   
For more detailed information, see http://vmware.com/info?id=152. Continue without 64-bit support?   
  ```  
开始找解决方案，  
这段提示的关键就是“VT”， VT就是“Virtualization Technology（虚拟化）。 要在VM安装64位操作系统问题，必要满足以下三个条件，缺一不可：   
- 第一，	CPU要为64位。  
- 第二，	CPU要支持VT技术。   
- 第三，	主板Bios设置要打开VT。  
前两项可以用securable来检测。百度搜索，下载securable.exe，是绿色版的。打开后显示这个结果：  
   
说明满足要求。  
“YES Hardware virtualization”,是指该CPU支持硬件虚拟化，如果此项显示为“YES”的话，说明你的CPU支持VT技术，如果还不能在VM中安装64位系统的话，就说明BIOS中此CPU VT功能没有开启。（有的securable版本会显示“Locked ON、Locked Off”,意思类似。）   
这时就需要在“主板Bios当中要打开VT”功能。进入BIOS后，选择Security选项，然后选择Virtualization选项，进去后把Inter(R) Virtualization Technology选项设置为Enable后，退出重启电脑。  
   
  
BIOS设置完毕重启后，打开虚拟机还是不成功。再次重启进入BIOS设置保存重启后，就成功了（和网上的一样）。  
然后开始在虚拟机中进行CentOS的安装过程。  
  
## CentOS系统安装  
参考：http://jingyan.baidu.com/article/a3aad71aa180e7b1fa009676.html  
自动进入了centos桌面，点击install hardware开始安装。  
最好先选择中文，如果一开始是英文，那以后安装中文就会有点费劲了。  
安装中，需要给root密码，并建立用户。我建立的是管理员用户，用户名solomon。因为密码要求必须是强密码，所以root和solomon我用的都是g3  
  
  
  
