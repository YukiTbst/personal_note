# shell



## 简单知识

### 输出重定向、管道



### 变量相关

在bash中为变量赋值的语法是`foo=bar`，访问变量中存储的数值，其语法为 `$foo`。 需要注意的是，`foo = bar` （使用空格隔开）是不能正确工作的，因为解释器会调用程序`foo` 并将 `=` 和 `bar`作为参数。 **总的来说，在shell脚本中使用空格会起到分割参数的作用**，有时候可能会造成混淆，请务必多加检查。

Bash中的字符串通过`'` 和 `"`分隔符来定义，但是它们的含义并不相同。以`'`定义的字符串为原义字符串，其中的变量不会被转义，而 `"`定义的字符串会将变量值进行替换。

```bash
foo=bar
echo "$foo"
# 打印 bar
echo '$foo'
# 打印 $foo
```

**大括号扩展**

这种写法能创建一个连续的数字或字母的序列，比如{2..6}会被扩展为2 3 4 5 6。除此之外，也可以直接在大括号里进行枚举，比如`{a,d,e}`相当于`a d e`.值得注意的是，给变量赋这种值并不会将其扩展为序列，比如

```bash
mangwu@raspberrypi:~/missing/test $ a={a,b,c}
mangwu@raspberrypi:~/missing/test $ echo {a,b,c}
a b c
mangwu@raspberrypi:~/missing/test $ echo $a
{a,b,c}
```

大括号扩展还能增加前缀与后缀，比如：

```bash
mangwu@raspberrypi:~/missing/test $ echo a{1..3}
a1 a2 a3
mangwu@raspberrypi:~/missing/test $ echo {a..b}{1..3}
a1 a2 a3 b1 b2 b3
mangwu@raspberrypi:~/missing/test $ echo {a..b}{1..3}b
a1b a2b a3b b1b b2b b3b
mangwu@raspberrypi:~/missing/test $ echo {a..b}{1..3}b
```

**命令替换**

命令替换可以以变量的形式获取一个命令的输出。

当通过 `$( CMD )` 这样的方式来执行`CMD` 这个命令时，它的输出结果会替换掉 `$( CMD )` 。例如，如果执行 `for file in $(ls)` ，shell首先将调用`ls` ，然后遍历得到的这些返回值。还有一个冷门的类似特性是 *进程替换*（*process substitution*）， `<( CMD )` 会执行 `CMD` 并将结果输出到一个临时文件中，并将 `<( CMD )` 替换成临时文件名。这在我们希望返回值通过文件而不是STDIN传递时很有用。例如， `diff <(ls foo) <(ls bar)` 会显示文件夹 `foo` 和 `bar` 中文件的区别。

**脚本的变量**

bash使用了很多特殊的变量来表示参数、错误代码和相关变量。下面是列举来其中一些变量，更完整的列表可以参考 [这里](https://www.tldp.org/LDP/abs/html/special-chars.html)。

- `$0` - 脚本名
- `$1` 到 `$9` - 脚本的参数。 `$1` 是第一个参数，依此类推。
- `$@` - 所有参数
- `$#` - 参数个数
- `$?` - 前一个命令的返回值
- `$$` - 当前脚本的进程识别码
- `!!` - 完整的上一条命令，包括参数。常见应用：当你因为权限不足执行命令失败时，可以使用 `sudo !!`再尝试一次。
- `$_` - 上一条命令的最后一个参数。如果你正在使用的是交互式 shell，你可以通过按下 `Esc` 之后键入 . 来获取这个值。



## 选择、循环

**if..else 语句** `if..else` 语句在 bash 中的用法是这样的：

```
if [ condition ]
then
   command1
   command2
   ...
else
   command3
   command4
   ...
fi
```

**while 语句** `while` 语句的用法是：

```
while [ condition ]
do
   command1
   command2
   ...
done
```

**for 语句** `for` 语句的一种常见用法是：

```
for i in [ list ]
do
   echo "Welcome $i"
done
```

其中list是一个空格分隔的值序列。每次循环迭代都会从这个列表中取出一个值赋给变量。比如可以写`for i in 1 2 3 4 5`

对于1 2 3 4 5这种情况，可以使用Bash shell 中的“大括号扩展”（brace expansion）功能，将其简写为`{1..5}`。**case 语句** `case` 语句，用于多选择的情况：

```bash
case expression in
    pattern1 )
        commands ;;
    pattern2 )
        commands ;;
    ...
esac
```



- `expression` 是你要检查的变量或表达式。Bash 脚本会根据这个 `expression` 的值，匹配每个 `pattern`。
- 每个 `pattern` 可以是一个字符串，可以包含特殊的通配符来匹配多种可能的值。
- 符号 `;;` 表示结束当前的具体情况（case）。当匹配到对应的 `pattern` 后，它下面的 `commands` 将被执行，然后 bash 会跳过剩下的所有 `patterns`，直接到 `esac` 语句结束 `case` 语句。

下面是一个简单的例子：

```
#!/bin/bash
echo -n "Enter a number: "
read num
case $num in
  1 ) echo "You entered one." ;;
  2 ) echo "You entered two." ;;
  * ) echo "You did not enter one or two." ;;
esac
```

在这个例子中，当你输入 1，它会输出 "You entered one."，输入 2 会输出 "You entered two.". 如果输入其他的数字，它会输出 "You did not enter one or two.". 这个 `*` 是一个特殊的模式，匹配任何不符合其他模式的值。你可以把它看作是 `case` 语句的默认情况。

## 函数

函数的例子如下：

```
mcd () {
    mkdir -p "$1"
    cd "$1"
}
```



