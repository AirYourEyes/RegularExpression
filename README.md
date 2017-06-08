# 《正则表达式必知必会》修订版学习笔记

### 一、什么是正则表达式
- 定义：简单的来说，正则表达式就是一些用来匹配和处理文本的字符串，它是通过正则表达语言构建
-  作用：
	- 搜索
	- 替换
	 
### 二、正则表达式语法
- 匹配任一字符
	- 使用`.` 
	- 使用`\.`匹配本身
- 匹配字符集
	- 使用`[]`，如`[0123]`将匹配0、1、2或者3
	- 使用`^`进行取非操作，如`[^0123]`表示不匹配0、1、2、3
	- 匹配中括号须转义
- 元字符
	- 具有特殊意义的字符
	- 使用本身需要进行转义
	- 匹配空白字符
		- `[\b]`匹配回退
		- `\f`匹配换页符
		- `\n`匹配换行符
		- `\r`匹配回车符
		- `\t`匹配制表符
		- `\v`匹配垂直制表符
	- 匹配特定字符类别
		- 数字类
			- 匹配数字
				- `\d`
				- 相当于`[0123456789]`
			- 匹配非数字
				- `\D`
				- 相当于`[^0123456789]`
		- 字母数字下划线类
			- 匹配字母数字下划线
				- `\w`
				- 相当于`[a-zA-Z0-9_]`
			- 匹配非字母数字下划线
				- `\W`
				- 相当于`[^a-zA-Z0-9_]`
		- 空白字符类
			- 匹配空白字符
				- `\s`
				- 相当于`[\f\n\r\t\v]`
			- 匹配非空白字符
				- `\S`
				- 相当于`[^\f\n\r\t\v]`  
		- 十六进制数
			- 以`\x`作为前缀
			- 如`\x0A`对应ASCII字符10（换行符）
		- 八进制数
			- 以`\0`作为前缀
			- 如`\011`表示的是ASCII字符9（制表符）   
		- POSIX字符类
			- 不支持JavaScript
			- `[[:alnum:]]`匹配字母数字，相当于`[a-zA-Z0-9] `
			- `[[:alpha:]]`匹配字母，相当于`[a-zA-Z]`
			- `[[:blank:]]`匹配空格或制表符，相当于`[\t ]`(这里注意有一空格)
			- `[[:cntrl:]]`匹配ASCII控制字符(ASCII 0到31，再加上ASCII 127)
			- `[[:digit:]]`匹配任一数字，相当于`[0-9]`
			- `[[:print:]]`匹配一个可打印字符
			- `[[:graph:]]`与`[[:print:]]`相同，但不包含空格
			- `[[:lower:]]`任一小写字母，相当于`[a-z]`
			- `[[:upper:]]`任一大写字母，相当于`[A-Z]`
			- `[[:punct:]]`既不属于`[[:alnum:]]`，也不属于`[[:cntrl:]]`的任一字符
			- `[[:space:]]`匹配任一空白字符，相当于`[\f\n\r\t\v ]`（注意有一空格）
			- `[[:xdigit:]]`任一十六进制数，相当于`[a-fA-F0-9]`    
- 重复匹配
	- `?`
		- 匹配最多一个
	- `*`
		- 匹配0个或多个
	- `+`
		- 匹配1个或多个 
	- `{m,n}`
		- 最少匹配m个，最多匹配n个
	- `{m,}`
		- 最少匹配m个
	- `{m}`
		- 刚好匹配m个 
- 贪婪型元字符与懒惰型元字符
	- 贪婪型元字符
		- 匹配尽可能多的字符，而不是适合而止
		- `*`、`+`、`{n,}`均是贪婪型元字符
	- 懒惰型元字符
		- 最小匹配
		- 在贪婪型元字符后面加上?可以使其转变成懒惰型元字符
		- 如`*?`，`+?`，`{n,}?`
- 位置匹配
	- 单词边界
		- 使用`\b`
		- `\b`匹配的是一个位置，在`\w`与`\W`之间
	- 非单词边界
		- 使用`\B`
		- `\B`匹配的是`\w`与`\w`或者`\W`与`\W`之间的位置 
	- 字符串边界
		- 字符串的开头 
			- 使用`^`
		- 字符串的结尾
			- 使用`$`
	- 分行匹配模式
		- 使用(?m)开启分行匹配模式 
		- 在分行匹配模式下，`^`不仅匹配字符串的开始位置，还匹配分隔符(换行符)后面的开始位置
		- 在分行匹配模式下，`$`不仅匹配字符串的结束位置，还匹配分隔符后面的结束位置
		- 有很多正则表达式不支持`(?m)` 
- 使用子表达式
	- 使用小括号`()`
	- 小括号内的字符是一个整体
	- 如`(hello)`将匹配`hello`这个单词而不是`hello`的任一字符
	- 字表达式支持嵌套
- 回溯引用
	- 类似于程序设计中的变量
	- `\1`表示引用正则表达式中的第一个子表达式，`\2`表示引用正则表达式中第一个子表达式，依次类推
	- 使用回溯引用可以用来保证**前后一致匹配**，如匹配不同等级的标题标签，必须使结束标签中的数字与开始标签中的数字相同，也就是使`<h1>`匹配`</h1>`而不会匹配到`</h2>`等其他的结束标签，这时可以将开始标签中的数字当做一个子表达式，然后在结束标签中引用已经在开始标签中匹配到的数字，从而保证前后一致性匹配，具体例子可以看《正则表达式必知必会》修订版中的第八章
	- 回溯引用在替换方面的作用也很大，具体可以看《正则表达式必知必会》修订版的第八章
- 前后查找
	- 用于确定正确的匹配位置，被匹配的文本不包含在最终返回的结果中
	- 正向前查找：
		- 是一个以`?=`开头的子表达式，`=`后跟的是被匹配的字符
		- 如：使用正则表达式`.+(?=:)`提取链接中的协议名，如http，ftp等（注意：返回结果不包括冒号）
	- 负向前查找：
		- 是一个以`?!`开头的子表达式，`!`后跟不被匹配的字符 
	- 正向后查找：
		- 是一个以`?<=`开头的子表达式
		- 如：使用正则表达式`(?<=\$)[0-9.]+`匹配出以`$`作为前缀的价格的数字，但是匹配结果不返回`$`字符    
	- 负向后查找：
		- 是一个以`?<!`开头的子表达式，`!`后跟不被匹配的字符
       	- 如：使用正则表达式`(?<!\$)[0-9.]+`匹配不是以`$`开头的数字
- 嵌入条件
	- 回溯引用条件
		- 方式一： 
			- 语法：`(?(backreference)true-regex)`
			- 解释：当且仅当backreference（回溯引用）存在时才会执行true-regex子表达式
			- 类似于程序设计中的if语句
		- 方式二：
			- 语法：`(?(backreference)true-regex|false-regex)`
			- 解释：当回溯引用存在时，执行true-regex子表达式，否则执行false-regex子表达式
			- 类似于程序设计中的if-else语句
	- 前后查找条件
		- 类似于回溯引用条件，只要将上面的backreference换成代表前后查找的正则表达式即可    
     