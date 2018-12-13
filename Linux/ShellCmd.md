
* awk 去除filed字段开头和末尾的空格

```
cat source.txt | awk -F\| '{sub(/^[[:blank:]]*/,"|",$1);sub(/[[:blank:]]*$/,"",$1);sub(/^[[:blank:]]*/,"",$2); sub(/[[:blank:]]*$/,"|",$2); print $1"|"$2}' > output.txt
```
* awk + grep + xargs

```
./cluster_list.sh | grep \"id\"\: | awk -F\" '{print $4}' | xargs -n1  -I {} ./$1 {}
```
