# gdb相关

## 常用指令

| 指令全称      | 指令缩写 | 说明                                             |
| ------------- | -------- | ------------------------------------------------ |
| file <文件名> |          | 加载文件                                         |
| run           | r        | 运行文件                                         |
| start         |          | 运行文件，停留在第一句                           |
| next          | n        | 单步运行（调用函数算一步完成）                   |
| step          | s        | 单步运行（调用函数会逐行运行函数）               |
| continue      | c        | 继续运行                                         |
| backtrace     | bt       | 查看函数调用的栈帧和层级关系                     |
| frame         | f        | 切换函数的栈帧                                   |
| info          | i        | 查看函数内部局部变量的值                         |
| finish        |          | 结束当前函数，返回调用点。（会把当前函数执行完） |
| list          | l        | 查看源代码                                       |
| set           |          | 设置变量的值                                     |
| print         | p        | 打印值及地址                                     |
| quit          | q        | 退出gdb                                          |

## 简易教程

使用gdb之前需要在gcc命令中加上-g参数。如果不确定一个文件能否用gdb调试，可以运行 `gdb <文件名>`查看输出结果判断。

[GDB调试指南(入门，看这篇够了)-CSDN博客](https://blog.csdn.net/chen1415886044/article/details/105094688)