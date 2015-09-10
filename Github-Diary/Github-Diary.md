# Git 学习日记  

## 第一日 安装配置Git  
### 注册Github账号  
打开Github.com，然后注册。  
下载安装Git客户端 windows 64位  
下载安装Github客户端  
官网安装失败，所以下载的离线包安装  

### 配置Github客户端
Github登录失败  
打开Github后，尝试登录，然后提示登录失败：
```Logain Failed, “Unable to retrieve your user info from the server a proxy server might be interfering with the request."  ```  
上网搜索后，发现可能是windows的.NET Framework需要升级。  
打开本地目录：C:\Windows\Microsoft.NET\Framework，看到已有.net framework的1.0/2.0/3.0/3.5/4.0版本。所以可能不是.net问题。  
然后，打开Git Shell的命令行工具（安装Git客户端后，打开桌面Git Bash就是）。  
按照网上提示，输入如下指令：  
```
git config --global user.name "solomonxie"
git config --global user.email "solomonxie@foxmail.com"
```
重启Github客户端，再登录，成功！  

### 往Github的Repository仓库添加文件    

### 创建个人博客  
Hello World  
按照https://pages.github.com/ 中的指示，一步一步操作。  
大概如下：  
1. 添加新repository  
项目名称为solomonxie.github.io，这个是不能变的，因为我的用户名是solomonxie所以这个名称必须这样。
2. 然后clone项目  
在Git Shell命令行中将网上创建的新repo复制到本地，输入：
git clone https://github.com/solomonxie/solomonxie.github.io
3. 然后再创建html首页  
在shell中输入：
cd username.github.io
echo "Hello World" > index.html
4. 然后push  
再将本地内容push到在线托管repo中：
```
git add --all
git commit -m "Initial commit"
git push -u origin master
```
5. 最后查看  
可以打开http:// solomonxie.github.io 查看网页效果了

按照模板创建首页
参考：http://blog.csdn.net/renfufei/article/details/37725057/
这篇文章最浅显易懂。

### 利用Jekyll模板引擎维护博客  
参考：http://www.pchou.info/web-build/2013/01/05/build-github-blog-page-02.html

## 第二日 使用SublimeText2操作md和rst  
发现Editplus不支持交互，eclipse对配置要求太高一打开就卡，然后听说有sublime，就安装了sublime text2试试。  
### 添加Markdown写作支持  
参考：http://www.jianshu.com/p/378338f10263  
只需在package control中添加Markdown Preview插件即可。  
使用方法：打开package control，输入preview in browser或export html in sublinme   text。这样就可以看到网页效果。或者可以直接菜单tools – build，在当前文件夹生成html。  
Markdown Preview较常用的功能是preview in browser和Export HTML in Sublime Text，前者可以在浏览器看到预览效果，后者可将markdown保存为html文件。  
preview in browser据称是实时的，但是实践上还是需要在st保存，然后浏览器刷新才能看到新的效果，好在markdown写得多的话也不需要每敲一行看一次效果。  
### 快捷键  
st支持自定义快捷键，markdown preview默认没有快捷键，我们可以自己为preview in browser设置快捷键。  
方法是在Preferences -> Key Bindings User打开的文件的中括号中添加以下代码(可在Key Bindings Default找到格式)：  
 ```{ "keys": ["alt+m"], "command": "markdown_preview", "args": { "target": "browser"} }```
"alt+m"可设置为自己喜欢的按键。  
配置Sublime 的Markdown语法高亮  
安装Monokai Extended插件。然后在设置里更改颜色主题：  
Preferences -> Color Scheme -> User -> Monokai Extended  
完成。  

### 添加rst文件即Restructured Text语法插件
参考Git官网readme：https://github.com/mgaitan/sublime-rst-completion  
添加Restructured Text (RST) Snippets插件  
说明文件中还有关于：语法、快捷键等方面说明。  

## reStructured Text 使用
参考：https://www.youtube.com/watch?v=otM_tjIi_vY  
输入代号后，按下Tab，完成自动填写。  
如输入：h1，然后按tab。再输入内容，就变成1号字体了。  
其他代号和用法参考以上网址。  

## Markdown和reStructuredText比较/语法/安装/综述  
参考：https://github.com/windfire-cd/note/blob/master/rst_mkd/rst_mkd.rst#restructuredtext  








