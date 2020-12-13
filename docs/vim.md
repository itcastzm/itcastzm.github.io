# vim编辑器


## vim还没有启动的时候：

1.在终端里输入 vim file1 file2 ... filen便可以打开所有想要打开的文件

2.vim已经启动 输入 :e file 可以再打开一个文件，并且此时vim里会显示出file文件的内容。

3.同时显示多个文件： :sp //水平切分窗口 :vsplit //垂直切分窗口

二、在文件之间切换：

1.文件间切换 Ctrl+6 //两文件间的切换

:bn //下一个文件

:bp //上一个文件

:ls //列出打开的文件，带编号

:b1~n //切换至第n个文件 对于用(v)split在多个窗格中打开的文件，这种方法只会在当前窗格中切换不同的文件。

2.在窗格间切换的方法

Ctrl+w+方向键——切换到前／下／上／后一个窗格

Ctrl+w+h/j/k/l ——同上

Ctrl+ww——依次向后切换到下一个窗格中

## vim: vs sp 调整窗口高度和宽度
vim多窗口有时候需要调整默认的窗口宽度和高度，可以用如下命令配合使用

CTRL-W =        使得所有窗口 (几乎) 等宽、等高，但当前窗口使用 'winheight' 和 'winwidth'。

:res[ize] -N                               
CTRL-W -        使得当前窗口高度减 N (默认值是 1)。如果在 'vertical' 之后使用，则使得宽度减 N。

:res[ize] +N                                    
CTRL-W +        使得当前窗口高度加 N (默认值是 1)。如果在 'vertical' 之后使用，则使得宽度加 N。

:res[ize] [N]
CTRL-W CTRL-_                                  
CTRL-W _        设置当前窗口的高度为 N (默认值为最大可能高度)。

:vertical res[ize] [N]                  
CTRL-W |        设置当前窗口的宽度为 N (默认值为最大可能宽度)。

z{nr}<CR>       设置当前窗口的高度为 {nr}。
                                           
CTRL-W <        使得当前窗口宽度减 N (默认值是 1)。                                              
CTRL-W >        使得当前窗口宽度加 N (默认值是 1)。

<整个窗口的移动>
CTRL-W-H 将窗口移到最左边
CTRL-W-L 将窗口移到最右边
CTRL-W-J 将窗口移到底端
CTRL-W-K 将窗口移到顶端

注：
1、我比较倾向于命令:[vertical] res[ize] [N]，暴力直接，:-)
2、除此之外，也可以在sp或着vs的时候指定窗口的高度和宽度，具体在vim终查看帮助 help :sp



## vim-plug
https://github.com/junegunn/vim-plug
是一个vim插件管理器



## 多光标操作

https://github.com/mg979/vim-visual-multi

 ```shell
 
mkdir -p ~/.vim/bundle
git clone https://github.com/mg979/vim-visual-multi.git ~/.vim/bundle/vim-visual-multi

vim  ~/.vimrc

Plug 'mg979/vim-visual-multi', {'branch': 'master'}

 ```

e是vim的一个插件管理器， 同时它本身也是vim的一个插件。插件管理器用于方便、快速的安装、删除、Vim更新插件。

