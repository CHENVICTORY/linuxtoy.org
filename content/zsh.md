Title: 终极 Shell——Zsh
Date: 2008-08-05 08:13
Author: Kardinal
Category: Apps
Tags: Shell, Zsh
Slug: zsh

[撰文/Kardinal]

> 子曾经曰过，zsh: The last shell you’ll ever need!  
> Z 是最后一个字母，所以它是终极 Shell。
>
> 我曾经搜索到一个比较各种 Shell 的文章，Zsh 交互性是 A+ 级别的，远高于其它 Shell。在编程方面，Zsh 是 A 级的吧，也是最高的。只是不知道出于什么原因，Zsh 被严重的低估了。

大多数的 Linux 用户比较偏爱 Bash，因为大多数的发行版默认的就是它。平心而论，Bash 确实比 Csh 之流的好用多了。不过 Bash 也有很多地方不尽人意，像自动补全的功能不够强大，定位较长路径不够方便等。

后来我使用 Zsh。如果不调整一些必要的配置的话，Zsh 甚至还不如 Bash 好用。这也是很多人尝试过并放弃过的原因。

[Zsh 配置文件试用](http://i.linuxtoy.org/i/2008/08/zshrctar.gz) (内附讲解)

不熟悉 Zsh 的人，对 Zsh 最深刻的印象应该就是它的命令提示符了。它支持右侧对齐的提示符，并且可以配置成这个样子的：

[![zsh1-thumb.gif](http://i.linuxtoy.org/i/2008/08/zsh1-thumb.gif)](http://i.linuxtoy.org/i/2008/08/zsh1.gif)

不过我还是喜欢比较简单的样式。

Zsh 的**自动补全功能**十分的强大，如图所示：

[![zsh2-thumb.gif](http://i.linuxtoy.org/i/2008/08/zsh2-thumb.gif)](http://i.linuxtoy.org/i/2008/08/zsh2.gif)

它可以自动补全命令、参数、文件名、进程、用户名、变量、权限符等。

Zsh 还有一个贴心的功能：**路径别名**。假设有一个很长的路径，例如 `/home/lighttpd/html`，可以把这个路径命名为 `~WWW`。

[![zsh3-thumb.gif](http://i.linuxtoy.org/i/2008/08/zsh3-thumb.gif)](http://i.linuxtoy.org/i/2008/08/zsh3.gif)

Zsh 可以使用 **Emacs 风格的键绑定**，习惯 Bash 键绑定的朋友无需重新适应。Zsh 兼容大多数主流 Shell，像 Bash、Csh 等。

**错误校正**

**![](http://i.linuxtoy.org/i/2008/08/crct11.jpg)**

*-- directory -- 是补全类型提示*

`/etc/x11 [tab]` 后被修正为 `/etc/X11`

![](http://i.linuxtoy.org/i/2008/08/crct21.jpg)  

*补全类型提示变成了 -- corrections --*

请注意，这个功能不是单纯的修正大小写，而是各种拼写错误 比如说上面的例子，如果输入的是 11 或者 s11，它一样会修正为 X11

有一个前提，就是每次修正，只允许有一处字符错误 两个以上的错误，除非可以匹配其它的选项，否则就不能修正 12 就不能修正为 X11 ，除非候选里有 X12、Y12、Z12……

在配置文件里找到这一行，修改容错字数

    zstyle ':completion:*:approximate:*' max-errors 1 numeric

当然可以把容错字数改大一些，不过太大了也没有意义了 随便输点什么，就可以匹配所有的，和没有一样

**强大的重定向功能**

同时重定向 stdout 和 stderr 到 file: `command |& >file` 同时重定向到多个文件: `command >file.1 >file.2`

比如装系统的时候，可以用这个命令

    blkid >> /boot/grub/menu.lst >> /etc/fstab

**补全类型控制**

例如：  

    compctl -g '*.tar.gz *.gz*.tgz' + -g '*(-/)' tar zxvf  

过滤候选项

`tar zxvf [tab]` 候选菜单中只出现扩展名为 .tar.gz .gz .tgz 的文件

不过这个功能比较复杂，容易引起混乱，通常需要脚本配合

    compctl -g '*.tar.bz2 *.tar.gz *.bz2 *.gz *.jar *.rar *.tar *.tbz2 *.tgz *.zip *.Z' + -g '*(-/)' extractextract() {  
       if [[ -z "$1" ]] ; then  
           print -P "usage: \\e[1;36mextract\\e[1;0m < filename >"  
           print -P "       Extract the file specified based on the extension"  
       elif [[ -f $1 ]] ; then  
           case ${(L)1} in  
               *.tar.bz2)  tar -jxvf $1    ;;  
               *.tar.gz)   tar -zxvf $1    ;;  
               *.bz2)      bunzip2 $1       ;;  
               *.gz)       gunzip $1       ;;  
               *.jar)      unzip $1       ;;  
               *.rar)      unrar x $1       ;;  
               *.tar)      tar -xvf $1       ;;  
               *.tbz2)     tar -jxvf $1    ;;  
               *.tgz)      tar -zxvf $1    ;;  
               *.zip)      unzip $1          ;;  
               *.Z)        uncompress $1    ;;  
               *)          echo "Unable to extract '$1' :: Unknown extension"  
           esac  
       else  
           echo "File ('$1') does not exist!"  
       fi  
    }

考虑到使用的不多，配置又麻烦，我没有配置这个功能，不过我想肯定有人愿意在这上面花点时间

**将 Zsh 设置为默认 Shell**(不建议更改 root 用户的默认 shell)

    usermod -s /usr/local/bin/zsh

[下载配置](http://i.linuxtoy.org/i/2008/08/zshrctar.gz)
