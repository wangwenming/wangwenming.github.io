## IFS: Internal Field Separator

### 简介
```bash
$ echo ${#IFS}
3
$ echo -n "$IFS" | xxd
00000000: 2009 0a
```
上述代码说明 IFS 是由 3 个空白字符(空格 \t \n)组成，即：
```
<space><tab><newline>
```

### 什么时候需要修改这个值？
读取csv文件，特别是csv的值里包含空格时(这个时候 *,* 作为分隔符最合适)：
```bash
$cat data 
id,name
1,one
2,two
```

```bash
cat data | while read line
    do
        IFS=',' read -a a <<< "$line"
        echo "${a[0]} / ${a[1]}"
    done
```

## References
[$IFS](http://bash.cyberciti.biz/guide/$IFS)
