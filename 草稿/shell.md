1. 可以将 `#!/bin/sh` 放在文件第一行, 告诉系统执行该脚本的程序. 在脚本目录下输入 `./filename` 就能执行该脚本.
2. shell中以 `#` 开始的行表示注释.
3. 给变量赋值时, 应该是 `var=value`, 中间不能有空格, 取用一个变量的值, 要在变量名前加一个 `$`
        
        a="good job"
        echo $a
        echo "you are ${a}!" #可以将变量放到大括号里防止和后面的字符混淆 echo "you are $a!" 这个就打印不出来, 因为没有 `$a!` 变量
4. 常用命令: `read var` 提示用户输入, 将内容赋值给 `var`
            if [ -n "$1" ]
          `expr 2 "+" 3` 数学运算  
          其他: cut, cat, file, sort, uniq, find, tee, basename file(返回不包含路径的文件名 ), dirname file,head, tail, tailf, sed, awk
5. `hard quote ('',单引号) 与 soft quote("", 双引号)`, 单引号可以屏蔽任何变量扩展`$`. 双引号可以防止通配符扩展但允许变量扩展.

        A=9
        echo "$A"
        echo '$A'
6. 管道, 将一个命令的输出作为另外一个命令的输入 `grep "hello" file.log | wc -l` , 统计在file.log 中含有hello的行数.
    重定向, 将命令结果输出到文件, `>`(写入文件并覆盖旧文件 ), `>>`(追加到文件尾部, 保留旧文件)
    反短斜线, 可以将一个命令的输出作为其他命令的参数. find . -mtime -1(24小时, -2 48小时 ) -type f -print ,查找过去24小时内修改过的文件
7. 条件语句:

        if ...; then
        ... 
        elif ...; then
        ...
        else
        ...
        fi
        
    - 使用" [] " 来表示条件测试, 要保留方括号前后的空格.
    - 使用 " [[ ]]" 也能表示条件测试, 但是支持 逻辑运算符 `&&`, "||"
    - [ **"$a" == "$b"**] 判断 $a 和 $b 是否相等, **注意:括号条件判断时, 等号两边的空格不能少, 不然会直接返回true**
    - [ **-z "$name"** ]   长度为0则为真, **注意:其中变量的引用应该使用双引号括起来**,name="Jim"[ $name == "Jack" ]   #将会变成 [ Jim == "Jack" ], 也就出现错误了.
    - [ **-n "$var"** ] 判断 $var 变量是否有值
    - [ **-x"/bin/ls"** ] 判断/bin/ls 是否存在并有可执行权限
    - [ **-r file** ] 若文件存在且可读, 为真 
    - [ **-f "somefile"** ] 文件是否存在且是一个规则文件, 则为真
    - [ **-n string** ]  若string长度不为0, 则为真    
    - [ **-b file**] 若文件存在且是一个块特殊文件, 则为真  
    - [ -c file ] 若文件存在且是一个字符特殊文件, 则为真  
    - [ -e file ] 若文件存在, 则为真      
    - [ -g file ] 若文件存在且设置了SGID位的值, 则为真
    - [ -h file ] 若文件存在且为一个符合连接, 则为真
    - [ -k file ] 若文件存在且设置了 "sticky"位的值
    - [ -p file ] 若文件存在且为一已命名管道, 则为真    
    - [ -s file ] 若文件存在且其大小大于0, 则为真
    
    
    
    比较大小为 -eq, -ne, -lt, -le, -gt, -ge
    ! expr 非 
    expr1 -a expr2  若 expr1 和 expr2都为真则整式为真
    expr1 -o expr2  若 expr1 或 expr2 为真则为真 
    
    
    ### case 的使用 
    case ... in
    ...) do something here ;;
    csac
    
    ###select 的使用
    echo "what is you favourite OS?"
    select var in "Linux" "Gun Hurd" "Free BSD" "Other"; do
    break
    done
    echo "You have selected $var"
    
    
9. 循环语句
    while ...; do
    ...
    done
    break 跳出循环, continue 跳过一个循环的余下部分
    
    for var in ...; do
    ...
    done
8. $1 表示参数1, $2 表示参数2, $* 表示输入的所有参数
9. `echo ~- ` 打印当前工作目录 
10. 倒引号会执行其中的命令  如 a=\`expr 2 + 3 \``
11. `$#` 代表当前参数的数量. 如 `cat list.txt` 输出为1 , 命令不统计在内
12. if [ "${i:-0}" -ge 2 ]; then ...  如果没赋值,i的默认值为0,进行比较 
    
13. 函数, 返回值只能通过  `$?` 系统变量获得. 

		#函数内内容要以 `;` 结尾
		[ function ] functionname [()] #方括号的内容可以省略
		{
			# $1 表示第一个参数
			# $2 表示第一个参数
			# local var; 申请一个局部变量
			action;
			[return int;]   #return 不加将以最后一条命令运行结果作为返回值. return 后跟值 n(0-255)
		}
        
		#函数调用
		functionname arg1, arg2
        
14. `sh -x src.sh` 可以返回所有的语法错误 