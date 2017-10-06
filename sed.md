# sed学习笔记

------

### s命令的替换
+ sed 's/XX/XXX' filename
+ sed 's/XX/XXX/g' filename
> 区别是带g的会把每一行匹配的都换成XXX，而第一种是会改变第一个

+ sed 's/XX/XXX/1' filename
+ sed 's/XX/XXX/2' filename
+ sed 's/XX/XXX/3g' filename
> 最后带上数字表示指替代的字母，数字加g的表示制定第几个以后的全部替换

+ sed '3s/XX/XXX/g' filename
+ sed '3,6s/XX/XXX/g' filename
> 最开始的数字，表示指定行，以及指定行之间进行操作

+ sed 's/XX/[&]/g' filename
> &来当做被匹配的变量，然后可以在基本左右加点东西

+ sed 's/This is my \([^,&]*\),.*is \(.*\)/\1:\2/g' my.txt
> 圆括号括起来的正则表达式所匹配的字符串会可以当成变量来使用，sed中使用的是\1,\2
```
eg:
This is my cat, my cat's name is betty
This is my dog, my dog's name is frank
This is my fish, my fish's name is george
This is my goat, my goat's name is adam
结果：
cat:betty
dog:frank
fish:george
goat:adam
```

+ sed 's/^/XXX/g' filename
+ sed 's/$/XXX/g' filename
> 开头和结尾添加XXX

```
正则的基本含义
^ 表示一行的开头。如：/^#/ 以#开头的匹配。
$ 表示一行的结尾。如：/}$/ 以}结尾的匹配。
\< 表示词首。 如：\<abc 表示以 abc 为首的詞。
\> 表示词尾。 如：abc\> 表示以 abc 結尾的詞。
. 表示任何单个字符。
* 表示某个字符出现了0次或多次。
[ ] 字符集合。 如：[abc] 表示匹配a或b或c，还有 [a-zA-Z] 表示匹配所有的26个字符。如果其中有^表示反，如 [^a] 表示非a的字符
```
### -i
带上-i的选项，会直接改变文本的内容


### -e
sed '1,3s/XX/XXX/g; 1,$s/XX/XXX/g' filename
实际上面的例子等价于
sed -e '' -e '' filename

### N 把下一行的内容纳入当成缓冲区做匹配
实际是把奇数行偶数行变成了一起了 中间以\n 连接

### a命令和i命令
这里a和i指的是在 sed '' 双引号之间的内容
sed '1,3 i XXXXX' filename
sed '1,3 a XXXXX' filename
在一到三行前添加一行XXXXX
在一到三行后添加一行XXXXX

sed "/fish/a XXX" filename
正则匹配有fish的行，后面添加一行XXX

### c命令 替换行
用法同上
可以添加行数范围以及正则的匹配，需要注意的
c命令的范围行数，表示的是范围的行的内容变成一行

### d命令 删除
用法和c相同

### p命令 打印
同样可以使用行数和正则匹配，对应符合的行会打印两次

### -n
加上 -n，可以只显示符合条件的那些行
### 总结
实际上'' 参数用起来是这样的：
先是范围， 再是命令 最后是命令的需要执行的语句
比如 
1,$ 
1,2 
/fish/,/dog/
1,/fish/ 
后面加上命令 a i c p

注意正则表达式必须是//否者会曝出错误

### 命令打包
一开始练习的时候，我想的是对指定行之间，进行匹配的操作怎么做：
我想把3-6行，/fish/ d 的行删掉，发现执行错误
sed '3,6 /fish/ d' 
我想的是用管道，那种多个sed。后面又看了下，发现可以用；以及｛｝来把多个cmd命令弄起来；
sed -n '3,6 p' | sed '/fish/d'
sed '3,6 {/fish/d}'

sed '3,6 {/This/{fish/d}}' 
这个表示对3到6行 中从This到fish执行删除
sed '1,$ {/This/d;s/^ *//g}'
这个是两个不同的命令，全部执行匹配This的删除，以及开头的空格删除
这样就解决了，我想在制定行之间，做后续的问题
我发现后面的命令如果在用数字加匹配的，不起作用



 


