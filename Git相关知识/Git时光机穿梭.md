#### Git 时光机穿梭

#### 1. 版本回退

```git log``` 命令显示从最近到最远的提交日志。如果嫌输出的信息太多，看得眼花缭乱， 可以试试加上```--pretty=oneline```参数。

要想进行版本回退，首先，Git 必须知识当前版本是哪个版本，在Git中，用```HEAD```来表示当前版本，也就是最新提交的 ```commit_id```，上一个版本就是```HEAD^```，上上一个版本就是```HEAD^^```，如果要往上100个版本，可以写成```HEAD~100```.

```bash
git reset --hard HEAD^
```

以上命令即可回退到上一个版本。

Git 的版本回退速度非常快，因为Git 在内部有个指向当前版本的```HEAD```指针，当你回退版本的时候，Git仅仅把 ```HEAD```从指向当前版本改为指向上一个版本。然后顺便把工作区的文件更新了。所以你让```HEAD```指向哪个版本号，你就把当前版本定位在哪。

现在当你回退到了某个版本，关掉电脑。第二天早上后悔了，想恢复到新版本怎么办？找不到新版本的```commit_id```怎么办？

在Git中，总是有后悔药可以吃的。当你用```git reset --hard HEAD^``` 回退到上一个版本时，想再恢复到最新版本，就必须找到最新版本的```commit_id```。Git 提供了一个命令```git reflog```用来记录你的每一次命令。

#### 小结

* ```HEAD```指向的是版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭，使用命令```git reset --hard commit_id```。
* 穿梭前，用```git log```可以查看提交历史，以便确定要回退到哪个版本。
* 要重返未来，用```git reflog```查看命令历史，以便确定回到未来的哪个版本。

#### 2. 撤销修改

**工作区----> 暂存区---> 版本库**

**工作区撤销：**

![image-20200409194308096](I:\GreatGeek\CollectKnowledge\Git相关知识\Git时光机穿梭.assets\image-20200409194308096.png)

在检查工作区的状态时，可以看见可以使用```git restore <file> ```来放弃这个文件的修改。

**暂存区撤销：**

```bash
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	modified:   readme.txt
```



再次查看状态时，Git同样告诉我们，用命令```git reset HEAD <file>```可以把暂存区的修改撤销掉(unstage)，重新放回工作区。此时若再进行工作区的撤销，则彻底变回原来的样子。

```git reset ``` 命令即可以回退版本，也可以把暂存区的修改回退到工作区。当我们用```HEAD```时，表示最新的版本。

**删除文件：**

一般情况下，你通常直接在文件管理器中把没用的文件删了，或者用 ```rm```命令删了：

```bash
$ rm test.txt
```

这个时候，Git知道你删除了文件，因此，工作区与版本库就不一致了，```git status``` 命令会立刻告诉你哪些文件被删除了。

```bash
$ git status
On branch master
Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	deleted:    test.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

现在你有两个选择，一是确实要从版本库中删除该文件，那就用命令```git rm```删掉，并且```git commit```:

```bash
$ git rm test.txt
rm 'test.txt'

$ git commit -m "remove test.txt"
[master d46f35e] remove test.txt
 1 file changed, 1 deletion(-)
 delete mode 100644 test.txt
```

现在，文件就从版本库中被删除了。

---

**Note:** 先手动删除文件，然后使用 ```git rm <file> ``` 和 ``` git add <file> ```效果是一样的。

Git 记录的是增量。

---

另一种情况是删错了，因为版本库里还有，所以可以很轻松地把误删的文件恢复到最新版本：

```bash
git checkout -- test.txt 
```

```git checkout ``` 其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”

**注意：**

从来没有被添加到版本库就被删除的文件，是无法恢复的。

#### 3. git checkout 的变更

```git checkout``` 这个命令承担了太多职责，即被用来切换分支，又被用来恢复工作区文件，对用户造成了很大的认知负担。

Git 社区发布了Git 的新版本2.23 。在该版本中，有一个特性非常引人瞩目，就是新版本的Git引入了两个新命令 ```git switch``` 和 ```git restore```， 用以替换现在的 ```git checkout ```。换言之，``` git checkout ```将逐渐退出历史舞台。

Git 社区决定这样做，是因为目前 ```git checkout```命令承载了太多功能，这让新手们感到困惑。```git checkout``` 的核心功能包括两个方面，一个是分支的管理，一个是文件的恢复。这两个核心功能，未来将由 ```git switch ```和```git restore```分别负责。

相比之下，新命令旨在将职责明确分为两个较窄的类别：更改分支的操作和更改文件的操作。



#### 参考来源

[廖雪峰Git教程](https://www.liaoxuefeng.com/wiki/896043488029600/897013573512192)

[git checkout 可替换命令 git switch和 git restore](https://www.cnblogs.com/tinywan/p/12344267.html)