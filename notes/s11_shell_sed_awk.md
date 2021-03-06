##### sed 与 awk 初探

**sed 编辑器**

sed编辑器被称作流编辑器（stream editor），和普通的交互式文本编辑器恰好相反。在交互式
文本编辑器中（比如vim），你可以用键盘命令来交互式地插入、删除或替换数据中的文本。流编
辑器则会在编辑器处理数据之前基于预先提供的一组规则来编辑数据流

sed命令的格式如下。
`sed options script file`


* sed命令选项
   
| 选 项     | 描 述                                                |
| :-----:   | :-----:                                              |
| -e script | 在处理输入时，将script中指定的命令添加到已有的命令中 |
| -f file   | 在处理输入时，将file中指定的命令添加到已有的命令中   |
| -n        | 不产生命令输出，使用print命令来完成输出              |

script参数指定了应用于流数据上的单个命令。如果需要用多个命令，要么使用-e选项在
命令行中指定，要么使用-f选项在单独的文件中指定。有大量的命令可用来处理数据。

* sed substitute 命令(`s/old/new/`)

`echo "This is a test" | sed 's/test/big test/'`

`sed 's/dog/cat/' data1.txt`

要在sed命令行上执行多个命令时，只要用-e选项就可以了。
`sed -e 's/brown/green/; s/dog/cat/' data1.txt`


* sed 替换标记 (`s/old/new/flag`)

替换命令在替换多行中的文本时能正常工作，但默认情况下它只替换每行中出现的第一处。
要让替换命令能够替换一行中不同地方出现的文本必须使用替换标记（substitution flag）。替换标
记会在替换命令字符串之后设置。

有替换标记时的格式如下：
`s/pattern/replacement/flags`

有4种可用的替换标记：
 数字，表明新文本将替换第几处模式匹配的地方；
 g，表明新文本将会替换所有匹配的文本；
 p，表明原先行的内容要打印出来；
 w file，将替换的结果写到文件中。


`sed s/tree/apple/g data.txt`

`sed -f cmd.sed data.txt`

* 使用地址

默认情况下，在sed编辑器中使用的命令会作用于文本数据的所有行。如果只想将命令作用 于特定行或某些行，则必须用行寻址(line addressing)。
 
 在sed编辑器中有两种形式的行寻址: 
	
	 以数字形式表示行区间
	 用文本模式来过滤出行
	
 `$$` 当使用数字方式的行寻址时，可以用行在文本流中的行位置来引用。sed编辑器会将文本流中的第一行编号为1，然后继续按顺序为接下来的行分配行号。		
				
> **仅仅修改第三行**的`dog`为`cute dog`:
`sed '3s/dog/cute dog/' data1.txt`

> **仅仅修改第三行到第五行**的`dog`为`cute dog`:
`sed 3,5s/dog.cute dog/ data1.txt`

> **修改第三行带最后一行**的`dog`为`cute dog`:
`sed 3,$s/dog/cut dog/ data.txt`

`$$` sed编辑器允许指定文本模式来过 滤出命令要作用的行。格式如下:
`/pattern/command`

*必须用正斜线将要指定的pattern封起来*






**gawk 程序**

`gawk`程序是`Unix`中的原始`awk`程序的GNU版本。gawk程序让流编辑迈上了一个新的台阶，它提供了一种编程语言而不只是编辑器命令。在gawk编程语言中，你可以做下面的事情：

	 定义变量来保存数据；
	 使用算术和字符串操作符来处理数据；
	 使用结构化编程概念（比如if-then语句和循环）来为数据处理增加处理逻辑；
	 通过提取数据文件中的数据元素，将其重新排列或格式化，生成格式化报告。

gawk程序的基本格式如下：
gawk options program file

| 选 项        | 描 述                              |
| :-----:      | :-----:                            |
| -F fs        | 指定行中划分数据字段的字段分隔符   |
| -f file      | 从指定的文件中读取程序             |
| -v var=value | 定义gawk程序中的一个变量及其默认值 |
| -mf N        | 指定要处理的数据文件中的最大字段数 |
| -mr N        | 指定数据文件中的最大数据行数       |
| -W keyword   | 指定gawk的兼容模式或警告等级       |

`gawk -F : 'BEGIN {print "user\t\t\tHOME"; print "---\t\t\t---"} {print $1 "\t\t\t" $6} END {print "=========finish============"}' /etc/passwd`




