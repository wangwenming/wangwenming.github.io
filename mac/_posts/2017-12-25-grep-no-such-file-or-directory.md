## 现象
```bash
$ tree
.
└── te\ st
    └── a.txt

1 directory, 1 file

$ find . -type f | xargs grep "koala"
grep: ./te: No such file or directory
grep: st/a.txt: No such file or directory
```

## 原因

就是因为文件或目录包含空白符

## 文档
```bash
$ man find
     -print0
             This primary always evaluates to true.  It prints the pathname of the current file to standard output, followed by an ASCII NUL
             character (character code 0).

     -X      Permit find to be safely used in conjunction with xargs(1).  If a file name contains any of the delimiting characters used by
             xargs(1), a diagnostic message is displayed on standard error, and the file is skipped.  The delimiting characters include single
             (`` ' '') and double (`` " '') quotes, backslash (``\''), space, tab and newline characters.

             However, you may wish to consider the -print0 primary in conjunction with ``xargs -0'' as an effective alternative.

$ man xargs 
     -0      Change xargs to expect NUL (``\0'') characters as separators, instead of spaces and newlines.  This is expected to be used in
             concert with the -print0 function in find(1).
```

## 解决方案
```
$ find . -type f -print0 | xargs -0 grep --color -In "koala"
```

## 备注
*Linux* 的 *find* 命令支持 *-printf* 参数，而 *MAC* 因为是继承自 *FreeBSD* ，所以没有此参数，如果只考虑 *Linux* ，可用命令：

```
find -path "*/.svn" -prune -o -printf "\"%p\"\n" | xargs grep --color -In "engine-ie-button"
```

## 小技巧
*find* 命令还支持正则表达式，例如
```bash
-regex ".*.\(css\|js\|html\)"
```

## References
[find lacks the option -printf, now what?](https://stackoverflow.com/questions/752818/find-lacks-the-option-printf-now-what)
[Why does grep print out “No such file or directory”?](https://stackoverflow.com/questions/44217298/why-does-grep-print-out-no-such-file-or-directory)
