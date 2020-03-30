# Sublime Text 3 一些常用插件安装说明

---

## 写在前面
* 如果你是第一次接触此工具，请花10分钟时间认真看下此文档。
* `Package Control`插件本身是一个为了方便管理插件的插件，以下所有插件99%都是基于这个插件基础上的。
* `Sublime Text 3`在执行操作时，在软件左下角会出现如图的样式，执行中的时候这个等号会左右移动。![image](http://olv6ixm8v.bkt.clouddn.com/sublime_text3_3143_loading.png)`Sublime Text 3`是英文软件，软件装好默认是英文的，所以这里在没汉化之前都是用英文来讲。

## 安装工具和插件
* 大致分为安装工具，安装插件，配置插件，激活汉化等杂项。
## 安装`Sublime Text 3`和激活
* 安装，选择自己喜欢的盘符即可，以下没有特殊说明均以`D盘`为示范。
* 打开软件，`Help`>` Enter License`菜单，输入以下代码（已更新支持3143），
```
—– BEGIN LICENSE —–
TwitterInc
200 User License
EA7E-890007
1D77F72E 390CDD93 4DCBA022 FAF60790
61AA12C0 A37081C5 D0316412 4584D136
94D7F7D4 95BC8C1C 527DA828 560BB037
D1EDDD8C AE7B379F 50C9D69D B35179EF
2FE898C4 8E4277A8 555CE714 E1FB0E43
D5D52613 C3D12E98 BC49967F 7652EED2
9D2D2E61 67610860 6D338B72 5CF95C69
E36B85CC 84991F19 7575D828 470A92AB
—— END LICENSE ——
```

## 安装`Package Control`
* 这个安装方法有2种，推荐第一种，第一种安装不了再来第二种方法。

### 方法1：
* 通过快捷键 `ctrl`+`` ` ``或者 `View`>`Show Console`菜单打开控制台，复制以下代码（_代码为1行的_）
```
import urllib.request,os,hashlib; h = 'df21e130d211cfc94d9b0905775a7c0f' + '1e3d39e33b79698005270310898eea76'; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); by = urllib.request.urlopen( 'http://packagecontrol.io/' + pf.replace(' ', '%20')).read(); dh = hashlib.sha256(by).hexdigest(); print('Error validating download (got %s instead of %s), please try manual install' % (dh, h)) if dh != h else open(os.path.join( ipp, pf), 'wb' ).write(by)
```
* 粘贴到输入框，回车。等它执行完，如果报错（一般科学上网就可以安装成功），可以用离线安装包安装方法。

### 方法2：
* `Preferences`>` Browse Packages…`菜单打开资源管理器，打开的目录会是`C:\Users\`**_用户名_**`\AppData\Roaming\Sublime Text 3\Packages`，切换到`C:\Users\`**_用户名_**`\AppData\Roaming\Sublime Text 3\Installed Packages`，
把`Package Control.sublime-package`文件放在里面，
重启`Sublime Text 3`，就会发现`Preferences`多了一个`Package Control`的选项。
该方法可以适用于各类离线包插件。
(这里的**_用户名_**是电脑上的登陆账号用户名,演示为Windows7系统。)
至此，`Package Control`安装完毕。

## 安装和删除插件插件

* 由于有了`Package Control`，他就是一个类似于商店管理的工具，所以插件有千千万万。

## 安装和删除方法

* `Preferences`>`Package Control`菜单，输入字母i，默认第一个回车，或者直接选择`Package Control:Install Package`选项。
* 第一次的时候会加载，稍等后就会出现一个列表，在列表里输入插件名字，回车，加载后如果没有报错就代表插件安装成功，部分插件可能需要重启`Sublime Text 3`生效。
* 删除插件是`Remove Package`选项，选择相应的插件回车即可删除。

## 插件配置

* 这里讲的是快捷键的配置。
* `Preferences`>`Package Settings`菜单，这里会有很多列表，选择你想配置的插件，选择`Key Bindings – User`，这是用户自定义的快捷键，不建议更改软件/插件自身的快捷键。按照如下的格式就好了：

```
[
	{ "keys":["ctrl+alt+d"], "command":"delete_trailing_spaces"},
	{ "keys":["ctrl+alt+o"], "command":"toggle_trailing_spaces"},
	{ "keys": ["ctrl+c"], "command": "clipboard_manager_copy" },
	{ "keys": ["ctrl+x"], "command": "clipboard_manager_cut" },
	{ "keys": ["ctrl+v"], "command": "paste_and_indent" },
	{ "keys": ["ctrl+shift+v"], "command": "clipboard_manager_choose_and_paste" }
]
```

* 值得注意的是，所有插件的自定义用户快捷键都是放在一起的，请按照实际情况修改。

## 插件推荐
- `Ctags`函数跳转，对着你想跳转的函数名右键“转到定义”就可以跳转到相应的function函数，重新添加项目生效。
- `Thinkphp`ThinkPHP语法补全，没啥感觉的插件。
- `Sublime Input`输入法框跟随，Sublime Text的诟病。
- `ConvertToUTF8`编码支持+转换。
- `Alignment`对齐等号对齐，默认快捷键`Ctrl`+`Alt`+`A`，与默认QQ截图快捷键冲突，下面会介绍如何配置。
- `SideBarEnhancements`侧边栏右键功能扩展增强，安装生效，可以参考这个文章，汉化有几率失败，[http://www.iwzxi.com/article/11.html](http://www.iwzxi.com/article/11.html)
- `SyncedSidebarBg` 侧边栏背景设置为编辑框的颜色（默认主题背景色）
- `ChineseLocalizations`汉化，喜闻乐见。
- `Clipboard Manager`剪切板历史，可以存留多个剪切记录，该插件需要配置快捷键，触发方法 `Ctrl`+`Shift`+`V` 就可以打开列表，选择即粘贴操作。配置代码：
```
[
	{ "keys": ["ctrl+c"], "command": "clipboard_manager_copy" },
	{ "keys": ["ctrl+x"], "command": "clipboard_manager_cut" },
	{ "keys": ["ctrl+v"], "command": "paste_and_indent" },
	{ "keys": ["ctrl+shift+v"], "command": "clipboard_manager_choose_and_paste" }
]
```
- `TrailingSpacer` 去掉代码末尾的空格，该插件需要配置快捷键。配置代码(上面为一键去空格操作，下面为该功能颜色高亮开关)：
```
[
	{ "keys":["ctrl+alt+d"], "command":"delete_trailing_spaces"},
	{ "keys":["ctrl+alt+o"], "command":"toggle_trailing_spaces"}
]
```

- `BracketHighlighter` 符号配对，高亮显示()、{}、””、’’、[]等符号所对应的上下个关系。需要配置：有兴趣的可以研究，以下为相关资料：
[http://www.darkpool.net/archives/95](http://www.darkpool.net/archives/95)
`D:\Program Files\Sublime Text 3\Packages\Color Scheme - Default.sublime-package`

## 插件离线包
* 部分插件已放入插件离线包内，插件不一定最新，在线安装的能够自动更新，离线安装的不清楚。

## 快捷键增强篇
* 快捷键源自网上的文章，部分快捷键能提高开发效率，请选择性学习。

#### 选择类
`Ctrl`+`D` 选中光标所占的文本，继续操作则会选中下一个相同的文本。

`Alt`+`F3` 选中文本按下快捷键，即可一次性选择全部的相同文本进行同时编辑。举个例子：快速选中并更改所有相同的变量名、函数名等。

`Ctrl`+`L` 选中整行，继续操作则继续选择下一行，效果和 `Shift`+`↓` 效果一样。

`Ctrl`+`Shift`+`L` 先选中多行，再按下快捷键，会在每行行尾插入光标，即可同时编辑这些行。

`Ctrl`+`Shift`+`M` 选择括号内的内容（继续选择父括号）。举个例子：快速选中删除函数中的代码，重写函数体代码或重写括号内里的内容。

`Ctrl`+`M` 光标移动至括号内结束或开始的位置。

`Ctrl`+`Enter` 在下一行插入新行。举个例子：即使光标不在行尾，也能快速向下插入一行。

`Ctrl`+`Shift`+`Enter` 在上一行插入新行。举个例子：即使光标不在行首，也能快速向上插入一行。

`Ctrl`+`Shift`+`[` 选中代码，按下快捷键，折叠代码。

`Ctrl`+`Shift`+`]` 选中代码，按下快捷键，展开代码。

`Ctrl`+`K`+`0` 展开所有折叠代码。

`Ctrl`+`←` 向左单位性地移动光标，快速移动光标。

`Ctrl`+`→` 向右单位性地移动光标，快速移动光标。

`Shift`+`↑` 向上选中多行。

`Shift`+`↓` 向下选中多行。

`Shift`+`←` 向左选中文本。

`Shift`+`→` 向右选中文本。

`Ctrl`+`Shift`+`←` 向左单位性地选中文本。

`Ctrl`+`Shift`+`→` 向右单位性地选中文本。

`Ctrl`+`Shift`+`↑` 将光标所在行和上一行代码互换（将光标所在行插入到上一行之前）。

`Ctrl`+`Shift`+`↓` 将光标所在行和下一行代码互换（将光标所在行插入到下一行之后）。

`Ctrl`+`Alt`+`↑` 向上添加多行光标，可同时编辑多行。

`Ctrl`+`Alt`+`↓` 向下添加多行光标，可同时编辑多行。


#### 编辑类
`Ctrl`+`J` 合并选中的多行代码为一行。举个例子：将多行格式的CSS属性合并为一行。

`Ctrl`+`Shift`+`D` 复制光标所在整行，插入到下一行。

`Tab` 向右缩进。

`Shift`+`Tab` 向左缩进。

`Ctrl`+`K`+`K` 从光标处开始删除代码至行尾。

`Ctrl`+`Shift`+`K` 删除整行。

`Ctrl`+`/` 注释单行。

`Ctrl`+`Shift`+`/` 注释多行。

`Ctrl`+`K`+`U` 转换大写。

`Ctrl`+`K`+`L` 转换小写。

`Ctrl`+`Z` 撤销。

`Ctrl`+`Y` 恢复撤销。

`Ctrl`+`U` 软撤销，感觉和 `Ctrl`+`Z` 一样。

`Ctrl`+`F2` 设置书签

`Ctrl`+`T` 左右字母互换。

`F6` 单词检测拼写


#### 搜索类
`Ctrl`+`F` 打开底部搜索框，查找关键字。

`Ctrl`+`Shift`+`F` 在文件夹内查找，与普通编辑器不同的地方是Sublime允许添加多个文件夹进行查找，略高端，未研究。

`Ctrl`+`P` 打开搜索框。举个例子：1、输入当前项目中的文件名，快速搜索文件，2、输入@和关键字，查找文件中函数名，3、输入：和数字，跳转到文件中该行代码，4、输入#和关键字，查找变量名。

`Ctrl`+`G` 打开搜索框，自动带：，输入数字跳转到该行代码。举个例子：在页面代码比较长的文件中快速定位。

`Ctrl`+`R` 打开搜索框，自动带@，输入关键字，查找文件中的函数名。举个例子：在函数较多的页面快速查找某个函数。

`Ctrl`+`:` 打开搜索框，自动带#，输入关键字，查找文件中的变量名、属性名等。

`Ctrl`+`Shift`+`P` 打开命令框。场景例子：打开命名框，输入关键字，调用`Sublime Text`或插件的功能，例如使用`Package`安装插件。

`Esc` 退出光标多行选择，退出搜索框，命令框等。


#### 显示类
`Ctrl`+`Tab` 按文件浏览过的顺序，切换当前窗口的标签页。

`Ctrl`+`PageDown` 向左切换当前窗口的标签页。

`Ctrl`+`PageUp` 向右切换当前窗口的标签页。

`Alt`+`Shift`+`1` 窗口分屏，恢复默认1屏**（非小键盘的数字）**

`Alt`+`Shift`+`2` 左右分屏-2列

`Alt`+`Shift`+`3` 左右分屏-3列

`Alt`+`Shift`+`4` 左右分屏-4列

`Alt`+`Shift`+`5` 等分4屏

`Alt`+`Shift`+`8` 垂直分屏-2屏

`Alt`+`Shift`+`9` 垂直分屏-3

`Ctrl`+`K`+`B` 开启/关闭侧边栏。

`F11` 全屏模式

`Shift`+`F11` 免打扰模式


## 写在最后

以上是关于个人的对`Sublime Text 3`的整理，如果大家有更好的插件，请补充更新此文。

---


> 本文最后更新时间：2017.09.19