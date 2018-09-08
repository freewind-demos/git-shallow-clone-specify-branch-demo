Git Shallow Clone Checkout Remote Branch Demo
=============================================

这个Demo用来测试当git使用"shallow clone"时，如何切从一个分支切换到另一个远程分支。

这个repo有两个远程分支：

- `master`: 默认。特点是`current-branch.txt`内容为`master`
- `admin`: 特点是`current-branch.txt`内容为`admin`

![demo](./images/demo.jpg)

（注：随着不断commit，上面会多出一些提交，但是最下面几行跟图中应该是一样的）

这里两个分支都已经push到了github上，所以下面做实验时，直接使用本repo即可。

如何测试
----

### 1. 首先shallow clone这个repo

```
git clone git@github.com:freewind-demos/git-shallow-clone-checkout-remote-branch-demo.git --depth 1
```

由于默认分支是`master`，所以在本地默认branch是`master`:

```
$ cd git-shallow-clone-checkout-remote-branch-demo
$ cat current-branch.txt
master
```

检查一下git commit，确定是shallow clone:

```
$ git log
commit ca37dc43a15dbf0d47ee186f3189270d39efe104 (grafted, HEAD -> master, origin/master, origin/HEAD)
Author: freewind <nowindlee@gmail.com>
Date:   Sat Sep 8 21:41:46 2018 +0800

    add git image
```

只有一条commit，是当前最新的。

查看一下branches:

```
$ git branch -a
* master
  remotes/origin/HEAD -> origin/master
  remotes/origin/master
```

### 2. 切换到`admin`分支

按照<https://stackoverflow.com/a/1783426/342235>推荐的新办法，我们试试：

```
$ git fetch
```

再看看是不是shallow（还是）:

```
$ git log
commit ca37dc43a15dbf0d47ee186f3189270d39efe104 (grafted, HEAD -> master, origin/master, origin/HEAD)
Author: freewind <nowindlee@gmail.com>
Date:   Sat Sep 8 21:41:46 2018 +0800

    add git image
```

再看看branches:

```
$ git branch -a
* master
  remotes/origin/HEAD -> origin/master
  remotes/origin/master
```

好像跟fetch之前一样，没什么变化？

试试checkout到admin:

```
$ git checkout admin
error: pathspec 'admin' did not match any file(s) known to git.
```

失败。

再试试"Old Answer":

```
$ git checkout -b admin origin/admin
fatal: 'origin/admin' is not a commit and a branch 'admin' cannot be created from it
```

也不行。

### 3. shallow clone时指定--no--single--branch

删掉旧的，重新shallow clone，加一个参数`--no--single--branch`

```
git clone --no-single-branch git@github.com:freewind-demos/git-shallow-clone-checkout-remote-branch-demo.git --depth 1
```

```
$ git branch -a
* master
  remotes/origin/HEAD -> origin/master
  remotes/origin/admin
  remotes/origin/master
```

```
$ git fetch
$ git branch -a
* master
  remotes/origin/HEAD -> origin/master
  remotes/origin/admin
  remotes/origin/master
```

好像没什么变化。

```
$ git checkout admin
Branch 'admin' set up to track remote branch 'admin' from 'origin'.
Switched to a new branch 'admin'
```

成功！

再验证一下：

```
$ cat current-branch.txt
admin
```

### 4. 直接shallow clone admin分支

换个思路，我们能否在shallow clone时指定某个分支，比如`admin`呢？

```
$ git clone git@github.com:freewind-demos/git-shallow-clone-checkout-remote-branch-demo.git --depth 1 --branch admin
```

```
cd git-shallow-clone-checkout-remote-branch-demo
```

验证一下：

```
$ cat current-branch.txt
admin
```

成功！