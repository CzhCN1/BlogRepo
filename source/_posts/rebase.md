---
title: Git压缩历史
date: 2016-10-23 21:03:27
tags:
  - GIT
categories:
  - GIT
---
&emsp;&emsp;在使用github时，偶尔会遇到刚上传的代码有几个由于疏忽的小BUG。如果修改bug后再次推送，就会造成一个很尴尬的历史记录。在看了书之后，发现原来有压缩历史这么一个黑科技。  
&emsp;&emsp;压缩历史就是在修改提交后，将这个修改包含到前一个提交之中，压缩成一个历史记录。

## 具体过程：
&emsp;&emsp;首先我们已经上提交了一个文件，文件的内容是`我是hzc`。
![](http://7xk5u3.com1.z0.glb.clouddn.com/rebase2.png)

&emsp;&emsp;此时的查看git的提交日志如下图：
![](http://7xk5u3.com1.z0.glb.clouddn.com/rebase3.png)
1. 在本地修改文件中的BUG  

    &emsp;&emsp;随后我们在本地仓库中修改错误的内容为`我是czh`，确定修改正确后，再次添加后提交。 

    ![](http://7xk5u3.com1.z0.glb.clouddn.com/rebase4.png)

    此时的提交历史如下图所示，很显然fix name这次提交是没必要出现在历史中的，我们因此要对其进行压缩处理：
    ![](http://7xk5u3.com1.z0.glb.clouddn.com/rebase5.png)
2. 更改历史  

    &emsp;&emsp;我们需要将`fix name`与`add content`两次提交合并为一个完美的提交，在此需要使用`git rebase`命令。  

    执行下列指令,选定当前HEAD和上一个提交对象，并在编辑器中打开：
    ```
    git rebase -i HEAD~2
    ```
    命令执行后如下图所示：
    ![](http://7xk5u3.com1.z0.glb.clouddn.com/rebase6.png)

    将要压缩的提交前的 `pick`修改为`fixup`,修改好后保存退出。
    ![](http://7xk5u3.com1.z0.glb.clouddn.com/rebase7.png)

3. 修改完成

    &emsp;&emsp;短暂的等待后就可以压缩完成，再次用查看提交日志，可以发现`fix name`的提交已经不见了。两次提交记录合并为了一个新的提交记录，并且有一个新的commit编号。
    ![](http://7xk5u3.com1.z0.glb.clouddn.com/rebase8.png)
