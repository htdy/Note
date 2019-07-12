
* awk 去除filed字段开头和末尾的空格

```
cat source.txt | awk -F\| '{sub(/^[[:blank:]]*/,"|",$1);sub(/[[:blank:]]*$/,"",$1);sub(/^[[:blank:]]*/,"",$2); sub(/[[:blank:]]*$/,"|",$2); print $1"|"$2}' > output.txt
```
* awk + grep + xargs

```
./cluster_list.sh | grep \"id\"\: | awk -F\" '{print $4}' | xargs -n1  -I {} ./$1 {}
```

* shell if 逻辑运算 数值运算 read (if中==两边空格个数相等，if[[  ]]紧邻， expr中变量和运算符空格分割，反引号子命令中$引用变量前要用\转义) *
```
fail=0
success=0
while read domain
do
   read ip
   oldresult=`dig @xxx.xxx.xxx.xxx \$domain +short`
   newresult=`dig @xxx.xxx.xxx \$domain +short`
   flag="FAILED"
   if [[ "$ip"=="$oldresult" && "$ip"=="$newresult" ]];then
      flag="OK"
      success=`expr $success + 1`
   else
      fail=`expr $fail + 1`
   fi
   echo "$flag $domain   $ip    $oldresult    $newresult"
done
total=`expr $fail + $success`
echo "total: $total" 
echo "fail: $fail" 
echo "success: $success"
```

* vim 文件编码（encoding fileencoding fileencoding） *
```
set encoding (显示当前编辑缓冲区中的编码)
set fileencoding (显示文件磁盘上的编码)
set fileencodings  (vim 显示时尝试使用的编码集)
set encoding = utf-8 (设置当前编辑缓冲区中的编码，并输送到操作系统显示模块，按操作系统设置的显示编码集进行译显)
set fileencoding = utf-8 (保存文件时使用的编码集)
:e ++enc=utf-8(将当前编辑缓冲区的内容转成utf-8编码，但依然使用encoding进行译显)
%！xdd 将vim 编码区内容，按4bit译16进制对应的utf-8编码（对应当前encoding编码），此时w,则将对应字符存储（按fileencoding）
%！xdd -r 则是将对应编码区内容按16进制转存，并以当前encoding输送到操作系统显示模块再译显
%！od  则是将对应编码区内容按8进制转存，并以当前encoding输送到操作系统显示模块再译显
:h digraph-table（查看十进制-特殊字符表）
(i编辑模式下)ctrl + v 出现字符^,再输入特殊字符对应的十进制码即可
(i编辑模式下)ctrl + v, and then ctrl + m;(vim 下输入^M)
(i编辑模式下)ctrl + v, and then ctrl + a;(vim 下输入^A)
:dig (查看二合-特殊字符表)
(i编辑模式下)ctrl + k 出现字符?,再输入特殊字符对应的二合字符即可
vim -b 以二进制打开文件(避免了对于文件开始的特定标识字符的处理，从而全部读取展示)
(命令模式下)q+:(展示历史命令)
(命令模式下)q+/(展示历史搜索)
iconv -f utf-8 -t utf-16 file -c out
```

* ctags
```
(在源码目录下执行)ctags -R .
ctrl + ] (跳转到定义)
ctrl + o (回退)
ctrl + i (前进)
ctrl + p (自动补全)

```
