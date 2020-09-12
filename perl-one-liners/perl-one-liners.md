# Perl单行命令: 提高数据处理效率


### 1. perl 命令行接口

`CentOS 8.2` 默认安装的版本是`v5.26.3`

    $ perl  -h
    
    Usage: perl [switches] [--] [programfile] [arguments]
      -0[octal]         specify record separator (\0, if no argument)
      -a                autosplit mode with -n or -p (splits $_ into @F)
      -C[number/list]   enables the listed Unicode features
      -c                check syntax only (runs BEGIN and CHECK blocks)
      -d[:debugger]     run program under debugger
      -D[number/list]   set debugging flags (argument is a bit mask or alphabets)
      -e program        one line of program (several -e's allowed, omit programfile)
      -E program        like -e, but enables all optional features
      -f                don't do $sitelib/sitecustomize.pl at startup
      -F/pattern/       split() pattern for -a switch (//'s are optional)
      -i[extension]     edit <> files in place (makes backup if extension supplied)
      -Idirectory       specify @INC/#include directory (several -I's allowed)
      -l[octal]         enable line ending processing, specifies line terminator
      -[mM][-]module    execute "use/no module..." before executing program
      -n                assume "while (<>) { ... }" loop around program
      -p                assume loop like -n but print line also, like sed
      -s                enable rudimentary parsing for switches after programfile
      -S                look for programfile using PATH environment variable
      -t                enable tainting warnings
      -T                enable tainting checks
      -u                dump core after parsing program
      -U                allow unsafe operations
      -v                print version, patchlevel and license
      -V[:variable]     print configuration summary (or a single Config.pm variable)
      -w                enable many useful warnings
      -W                enable all warnings
      -x[directory]     ignore text before #!perl line (optionally cd to directory)
      -X                disable all warnings
    
    Run 'perldoc perl' for more help with Perl.

常用的命令参数功能描述：

    -0<octal>     (8进制表示)指定记录分隔符($/变量)，默认为换行。
    -a            自动分隔模式，用空格分隔$_并保存到@F中。相当于@F = split ''。分隔符可以使用`-F`参数指定。
    -F            指定`-a`的分隔符，可以使用正则表达式 /pattern/。
    -e            执行指定代码块。
    -i<extension> 原地替换文件，并将旧文件用指定的扩展名备份。不指定扩展名则不备份。
    -l            对输入内容自动chomp，对输出内容自动添加换行。
    -n            自动按行循环，相当于 while(<>) { # your code goes here }
    -p            自动按行循环以及输出，相当于 while(<>) { # your code goes here; print; }


### 2. 常用的`one-line`命令行

- 替换一个文件行尾所有的空白

```perl
perl -lpe 's/\s*$//'
```

`注意事项`: `-l` 参数保留了每行的`\n` 分隔符。

- 原位编辑

```perl
perl -i.bak -pne 's/\t/,/g' zotu_table.txt
```

- 给每一行添加行号. `$.` 是行号符.

```perl
perl -ne 'print "$.\t$_"' zotu_table.txt
```

- 自动分割, 默认按照`\t` 制表符分割

```perl
perl -lane 'print "$F[0]\t$F[1]"'  zotu_table.txt
```

`注意事项`: 使用 `-F` 参数可以设定自动分隔符

```perl
perl -F: -lane 'print $F[0]'    /etc/passwd
perl -F/:/ -lane 'print $F[0]'  /etc/passwd
perl -F'\t' -lne  'print $F[0]' zotu_table.txt
```

- BEGIN 和 END 可以在循环前初始化参数，接受打印结果

```perl
grep -v "#" zotu_table.txt  | perl -lane 'BEGIN {$t = 0}; $t += $F[1]; END { print $t }'
```

- 在线计算器

```perl
perl -l -e 'print 2**8'
```

### 3.Reference

[The top 10 tricks of Perl one-liners](https://blogs.oracle.com/linux/the-top-10-tricks-of-perl-one-liners-v2)