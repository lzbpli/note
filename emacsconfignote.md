# emacs 配置笔记
## home 文件配置
在share/emacs/25.1/ 下建立home文件夹，再进入同路径下的site-lisp文件夹，建立“site-start.el”文件。
在“site-start.el”文件里添加一下代码：
```lisp
(defvar usb-drive-letter (substring data-directory 0 -4)) 
(defvar usb-home-dir (concat usb-drive-letter "home/"))
(setenv "HOME" usb-home-dir)
```
这样就能将emacs的home文件夹设置在安装包中。



## 整个配置文件目录的结构
首先，Emacs的初始化文件可以有两种设置方法：

    使用单个文件：~/.emacs。这种方法把所有初始化函数放在一个文件里，设置起来简单，但是一旦插件多了这个文件就会变得很长很乱。
    使用目录：~/.emacs.d/。所有配置文件都放在该目录下，并且Emacs启动时会自动执行该目录下名为init.el的文件。虽说只有一个文件会被自动执行，但可以在init.el里执行其它的函数，所以init.el可以变得很简洁；使用Emacs的Feature机制，可以很方便地把具体的初始化工作按类别分在其余文件中。这也是我选择的方法

我自己的配置放在这里，目录结构如下（注意其中elpa/目录没有被push到github上）：
```md
~/.emacs.d/
        README.md  #请无视该文件
        init.el    #Emacs会自动从init.el开始执行
        snippets/  #yasnippet的自定义模板保存的位置，不重要
        elpa/      #通过ELPA下载的插件所保存的位置
        lisp/      #就是加载各个插件的初始化文件的位置啦
            init-xxx.el       #某初始化文件
            editing-utils/    #文本编辑用的一些小工具
            custom-themes/    #自定义的主题，不重要
            custom-dicts/     #自定义的auto-complete词典，不重要
```

