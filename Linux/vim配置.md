##基本配置
终端使用vim进行编辑时,可以设置一些默认的配置,修改`~/.vimrc`修改配置.如果当前的用户目录下没有.vimrc文件,可以`vim ~/.vimrc`然后修改里面的内容.[参考](http://blog.itpub.net/8111049/viewspace-628456).

1. 显示行号: `set nu`
2. 设置匹配模式,类似输入左括号会匹配相应右括号

##插件
###[非常常用的插件](http://linux-wiki.cn/wiki/%E7%94%A8Vim%E7%BC%96%E7%A8%8B%E2%80%94%E2%80%94%E9%85%8D%E7%BD%AE%E4%B8%8E%E6%8A%80%E5%B7%A7) 含`git`地址
###使用[Vundle.vim](https://github.com/VundleVim/Vundle.vim#about)管理vim插件.
###使用[YouCompleteMe](https://github.com/Valloric/YouCompleteMe)自动完成.安装时如果出现错误,什么L9重复

	" Git plugin not hosted on GitHub
  	Plugin 'git://git.wincent.com/command-t.git'
  	" git repos on your local machine (i.e. when working on your own plugin)
  	Plugin 'file:///home/cmio/.vim/bundle/YouCompleteMe'

	上面这些只是范例插件,把错误的注释掉即可,自己安装的插件可以写在这 file:///home/cmio/.vim/bundle/YouCompleteMe

###[常用插件](http://harttle.com/2015/07/18/vim-cpp.html)
###使用[delimitMate](https://github.com/Raimondi/delimitMate), 在~/.vimrc添加 `Plugin 'Raimondi/delimitMate'`并`PluginInstall`重新打开vim就能自动补全

如果出错`ValueError: Still no compile flags, no completions yet.` 需要编辑 `~/.vimrc`,添加配置文件路径

		"Youcompleteme fix
		let g:ycm_global_ycm_extra_conf = '~/.vim/bundle/YouCompleteMe/third_party/ycmd/cpp/ycm/.ycm_extra_conf.py'

4. 常用设置

		#语法高亮度显示
		syntax on 
		#下面两行在进行编写代码时，在格式对齐上很有用；
		#vim使用自动对起，也就是把当前行的对起格式应用到下一行；
		set autoindent
		#依据上面的对起格式，智能的选择对起方式，对于类似C语言编写上很有用
		set smartindent
		#第一行设置tab键为4个空格，第二行设置当行之间交错时使用4个空格
		set tabstop=4
		set shiftwidth=4
		#设置匹配模式，类似当输入一个左括号时会匹配相应的那个右括号
		set showmatch

		#默认情况下，寻找匹配是高亮度显示的，该设置关闭高亮显示
		#set nohls
		#查询时非常方便，如要查找book单词，当输入到/b时，会自动找到第一
		#个b开头的单词，当输入到/bo时，会自动找到第一个bo开头的单词，依
		#次类推，进行查找时，使用此设置会快速找到答案，当你找要匹配的单词
		#时，别忘记回车
		set incsearch
        
        #为操作的一行添加下划线
        set cursorline
        #自动缩进 
        set autoindent 

		# vim  中文无法显示
		
		:set fileencodings=ucs-bom,utf-8,cp936                                                                                                               
		:set fileencoding=utf-8
		:set encoding=cp936
		:set cul

5. tab设置为4个空格

		在.vimrc中添加以下代码后，重启vim即可实现按TAB产生4个空格：
		set ts=4  (注：ts是tabstop的缩写，设TAB宽4个空格)
		set expandtab
		
		对于已保存的文件，可以使用下面的方法进行空格和TAB的替换：
		TAB替换为空格：
		:set ts=4
		:set expandtab
		:%retab!
		
		空格替换为TAB：
		:set ts=4
		:set noexpandtab
		:%retab!
		
		加!是用于处理非空白字符之后的TAB，即所有的TAB，若不加!，则只处理行首的TAB。

6. 忽略大小写
	- :set ic(ignorecase 的缩写) 忽略大小写
	- :set noic(noignorecase 的缩写) 不忽略大小写 

7. 众所周知，vim设置tab键为4个空格，对于编写代码非常有用，但是，makefile文件是需要识别tab键的，下面这个设置可以在代码中替换tab，在makefile中不替换。

		vim /etc/vimrc
		set tabstop=4 "设置tab键为4个空格
		set shiftwidth=4  “设置当行之间交错时使用4个空格
		set expandtab
		set autoindent
		autocmd BufNewFile, BufRead *.c, *.cpp, *.java :CFOLD

8. 重装vim, 让vim 支持lua扩展
    1. 在git 下载 vim 最新源代码 
    
        git clone https://github.com/vim/vim.git
    
    2. 参考 [安装vim方法](https://www.dwhd.org/20151004_133637.html)
    3. 注意, 要检查各种配置是否ok, 看 configure 输出, 发现lua配置有问题, 在 /usr/bin 中没找到lua, 实际上lua的安装目录可能是在 /usr/local/bin , 要修改 src/auto/configure 文件的 vi_cv_path_lua_pfx="/usr/"为 vi_cv_path_lua_pfx="/usr/local", 再按照其步骤设置
    
9. vim 退格键不能用

        vim 退格键（backspace）不能用 
        两个步骤：
        1.去掉讨厌的有关vi一致性模式，避免以前版本的一些bug和局限. set nocompatible 
        2.backspace有几种工作方式，默认是vi兼容的。对新手来说很不习惯。对老vi 不那么熟悉的人也都挺困扰的。可以用
        set backspace=indent,eol,start
        来解决。
        indent: 如果用了:set indent,:set ai 等自动缩进，想用退格键将字段缩进的删掉，必须设置这个选项。否则不响应。
        eol:如果插入模式下在行开头，想通过退格键合并两行，需要设置eol。
        start：要想删除此次插入前的输入，需设置这个。
        将以上两个命令加到vim的系统配置文件里就可以了。
        
10. [vim 编码理解](http://blog.csdn.net/cainiao_2010/article/details/8142051)

        set fileencodings=ucs-bom,utf-8,GB18030,gbk
    


   