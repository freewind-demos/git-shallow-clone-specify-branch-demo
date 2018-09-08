Git Shallow Clone Specify Branch Demo
=====================================

这个Demo用来测试当git使用"shallow clone"时下载指定的分支。

这个repo有两个远程分支：

- `master`: 默认。特点是`current-branch.txt`内容为`master`
- `admin`: 特点是`current-branch.txt`内容为`admin`

如果不指定，下载的就是默认分支: `master`:

```
git clone git@github.com:freewind-demos/git-shallow-clone-specify-branch-demo.git --depth=1
```

如果指定`admin`分支，可以使用`--branch admin`：

```
git clone git@github.com:freewind-demos/git-shallow-clone-specify-branch-demo.git --depth=1 --branch=admin
```

Clone之后，可以使用`cat current-branch.txt`确认一下。
