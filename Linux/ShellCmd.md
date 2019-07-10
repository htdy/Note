
* awk 去除filed字段开头和末尾的空格

```
cat source.txt | awk -F\| '{sub(/^[[:blank:]]*/,"|",$1);sub(/[[:blank:]]*$/,"",$1);sub(/^[[:blank:]]*/,"",$2); sub(/[[:blank:]]*$/,"|",$2); print $1"|"$2}' > output.txt
```
* awk + grep + xargs

```
./cluster_list.sh | grep \"id\"\: | awk -F\" '{print $4}' | xargs -n1  -I {} ./$1 {}
```

* shell if 逻辑运算 数值运算 read (if中==两边空格个数相等，if[[  ]]紧邻， expr中变量和运算符空格分割，反引号子命令中$引用变量前要用\转义)*
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
