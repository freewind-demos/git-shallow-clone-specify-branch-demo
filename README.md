Git Shallow Clone Checkout Remote Branch Demo
=============================================

这个Demo用来测试当git使用"shallow clone"时，如何切从一个分支切换到另一个远程分支。

这个repo有两个远程分支：

- `master`: 默认。特点是`current-branch.txt`内容为`master`
- `admin`: 特点是`current-branch.txt`内容为`admin`

图:TODO

如何测试
----

1. 首先shallow clone这个repo: `git clone`时使用`--depth 1`
