# project 2 Gitlet

**Gitlet**, 2023.09.28开始

目标：实现git的基本功能，也就是gitlet

工具：Maps, hashing, file I/O, graphs 



参考：lab6, Capers

https://git-scm.com/book/en/v2/Git-Internals-Plumbing-and-Porcelain

## 0.Git

**the core parts of Git** 

The `objects` directory stores all the content for your database, the `refs` directory stores pointers into commit objects in that data (branches, tags, remotes and more), the `HEAD` file points to the branch you currently have checked out, and the `index` file is where Git stores your staging area information. 



## 1. Classes and Data Structures

> - Include here any class definitions. 
> - For each class list the instance variables and static variables (if any).
> - Include a ***brief description\*** of each variable and its purpose in the class. Your explanations in this section should be as concise as possible.

### Main

Your Main class should mostly be calling helper methods in the the `Repository` class 

### Commit

> **Commit Object**: A commit in Git contains metadata such as the commit message, author information, and a reference to a tree object. The tree object represents the state of the entire project's directory structure and file contents at the time of the commit. 

**Instance variables**

- message
- timestamp
- parent
- tree--->HashMap，保存文件名和对应的blob引用关系
- naming each commit using its SHA-1 id

==注意==：Incorporating trees into commits and not dealing with subdirectories。



### Blob

> Blobs are Git objects that store the actual file content. Each blob is identified by a unique SHA-1 hash, and the content of each file is stored as a blob object 

### Tree

用来表示当前的文件结构、对blob的引用关系等

> **Tree Object**: The tree object is a representation of a directory tree, similar to a file system directory. It contains entries for all the files and subdirectories in the project at the time of the commit. Each entry in the tree object includes the following information:
>
> - The type of the object (blob or subtree)
> - The file/directory name
> - The SHA-1 hash of the associated blob or subtree object
>
> **Each tree object corresponds to a directory in your project's file system** 

The blob objects are referenced by the tree object, which, in turn, is referenced by the commit object. 

==to create a tree object==, you first have to set up an index by staging some files. To create an index with a single entry — the first version of your `test.txt` file — you can use the plumbing command `git update-index` 



### CommandChecker

用来验证指令的正确性。

### StagingArea

作为数据暂存区，成员变量

- stageAdd, HashMap
- stageRemove, HashMap





## 2. Algorithms

> This is where you tell us how your code works. For each class, include a high-level description of the methods in that class 







## 3. Persistence

**==Folder Designment==** 

setup `.gitlet` folder to keep persist of the state of program( all the objects).

参考`.git`文档内容，以及lab6中的文档操作使用方法

> readObject \ writeObject \ join 等函数的使用

![1696140255494](.\CS61B-Gitlet.assets\1696140255494.png)

**==文件夹：logs, objects, refs, index==**

1. `HEAD`: This file is essential as it points to the currently checked out branch or commit.

```
refs/heads/master2
```

2. `objects`: The `objects` directory is crucial because it stores all the Git objects, including blobs (file content), trees (directory structures), and commits. `objects文件夹以哈希的形式存储不同的文件，首先取每个对象的40位哈希值，以前两位字符创建文件夹，后38位字符作为文件名。`

3. `refs`: The `refs` directory contains references to commits, branches, tags, and remote references. Without this, Git wouldn't know where different references point. 在gitlet里，只需要一个heads，heads里的文件代表有几个branch

4. `index`: The `index` file is fundamental because it serves as the Git staging area, keeping track of changes you intend to commit in the next commit. The index file contains information like:
   - File paths: The paths to the files in your repository.
   - File permissions and modes: Information about file permissions, such as whether a file is executable.
   - SHA-1 hash values: Git uses these hashes to track the content of the staged files.
   - Staging status: Whether a file is added (staged) for the next commit or not.
   - ==问GPT== tell  me the internal processing in index when making a git add 

5. `logs`: The `logs` directory is important for keeping a history of different references in your repository, such as branch history.

**注意**





## 4. Tips

1.在java里，可用` System.getProperty("user.dir") `获取当前文件工作路径





## 5.需要明确

1、内部文件结构，每个文件有什么用。git怎么回溯文件，怎么引用，怎么新建一个branch

2、git的操作流程，包括init, add, commit每一步怎么运作

**git回溯文件内容**

When you need to retrieve an object, Git simply hashes the object's content, looks for the corresponding directory and filename structure, and retrieves the object 

**git回溯流程**

> **Retrieval**: When you check out a commit, Git follows this process:
>
> - It reads the commit object for the commit you're checking out to get the hash of the associated tree object.
> - It then reads the tree object to get the list of files and their corresponding blob hashes.
> - Finally, Git retrieves the content of each file by looking up the blob object associated with its hash

3、staging area 怎么表示？





## 实现过程

必要的文件夹：refs,  objects, HEAD, index, logs

0、命令格式检查

> - 没有输入命令
> - 命令不存在
> - 命令格式不匹配
> - 文件夹不符合需求

**1、init**

需要生成初始的commit，以及一个.gitlet文件夹，文件夹内包括所有需要的子文件夹。对init而言，只需要包括：refs,  objects, HEAD

> 所有操作包括
>
> - setting persistence--->refs,  objects, HEAD，HEAD内容`ref: refs/heads/master`
> - create and store an initial commit--->message, timestamp

**2、add**

是一个数据暂存区，把需要更新的文件信息存储在该文件里，这里参照git的设计，写在`.gitlet\index`文件里

https://shafiul.github.io//gitbook/7_the_git_index.html

https://towardsdatascience.com/how-git-truly-works-cd9c375966f6

实现流程如下

> 1. 命令的格式检查
> 2. 检查是否存在`.gitlet`文件夹
> 3. 实现blob类，把需要的文件内容保存
> 4. 保存上述的blob对象
> 5. 在index(staging area) 内写入blob信息

往stage for add添加blob要分情况

> 1. 计算将要添加的文件hash
> 2. 上一个commit中不存在该文件，直接添到stage for add
> 3. 上一个commit中存在该文件，但是版本不同，添到stage for add
> 4. 上一个commit中存在该文件，且版本相同，不添加。并且对stage for add再检查，如果有同样的hash，把他从stage里删了。



**3、commit**

可参考https://towardsdatascience.com/how-git-truly-works-cd9c375966f6

可以理解为确认信息，记录下本次commit需要保存的所有文件内容。

==commit信息包括== ID \ message \ timestamp \ parent \ reference \ author

实现过程

> 1. 命令格式检查
> 2. 检查是否存在`.gitlet`文件夹
> 3. 创建root tree
> 4. 根据HEAD内容找到 parent commit，并复制
> 5. 创建最新commit，确定所有的内部meatadata
> 6. 清空暂存区（staging area）

**注意**

1、HEAD指向当前所在的branch，间接指明了当前commit的 parent commit（链表的末位就是最新的commit）

2、Gitlet把tree和commit进行了合并，并且不再考虑子文件夹的问题。

**问题**

怎么在commit表示对文件的引用关系？

> HEAD表示当前文件所处的branch，该branch存储在refs/heads文件夹中，其内容是当前branch下的最新commit，而每一个commit都包含有parent commit的SHA-1码，因此可以实现对历史上所有commit的回溯。



**4、rm**

> 1. 上一个commit中没有，但是stage for add有，直接删除相应blob
> 2. 上一个commit中没有，stage for add也没有，那么不存在这个文件，属于error。
> 3. 上一个commit中有，但是存在源文件，则删除源文件，移动到stage remove，
> 4. 上一个commit中有，但是不存在源文件，直接把文件移动到stage remove



**5、6 log\global-log**

循环

7、**find**

找到指定信息的commit，具体实现

> 0. 检查文件夹
> 1. 检查该信息是否存在
> 2. 输出具有指定消息的所有commit

8、status

输出当前gitlet存储文件相关信息

注意：需要按照字母顺序输出，由于branch和stage里的数据量是不定的，考虑用ArrayList来存储数据，ArrayList内部存储数据的矩阵大小可以进行动态调整。

10、checkout

根据输入参数的不同，共分以下三种情况

```
java gitlet.Main checkout -- [file name]
java gitlet.Main checkout [commid id] -- [file name]
java gitlet.Main checkout [branch name]
```

对于1，要检查验证几种情况

> 1. 当前commit中不存在该文件，输出错误信息
> 2. 当前文件夹删除了该文件，直接根据commit中的信息重建
> 3. 当前文件夹修改了该文件，内部信息重写

2和情况类似

3，将该分支下最新的commit中的文件全部写入当前工作文件目录，并将当前HEAD中的内容更新为当前分支。

错误情况

> 1. 目标分支不存在
> 2. 已经在目标分支
> 3. 工作目录中有目标分支中不存在的文件

实现

> 1. 检查上述1、2
> 2. 取出当前分支最新commit的文件（tree），检查上述3--->需要取出当前目录下所有文件的名字
> 3. 更新文件
> 4. 更新HEAD
> 5. 清空暂存区

在3中更新文件时，要分以下几种情况

> 1. A、B都有
> 2. A有，B没有
> 3. A没有，B有

10、branch

新建一个branch，该branch的内容是当前commit的sha-1。不需要修改HEAD中的值。

11、rm-branch



13、merge

| Branch1 | Branch2      | Spilt Point |
| ------- | ------------ | ----------- |
| BranchA | BranchC      | Init commit |
| BranchA | BranchMaster | Init commit |
| BranchC | BranchMaster | commit3     |

merge工作流程

> 1. 检查缓存区stage，存在缓存文件，输出`You have uncommitted changes.`
> 2. 检查给定的分支名，如果不存在，输出`A branch with that name does not exist.`
> 3. 如果给定的分支名和当前分支相同，输出`Can not merge a branch with itself.`
> 4. 存在仅被merge commit跟踪，将被覆盖的文件，输出 `There is an untracked file in the way; delete it, or add and commit it first. `
> 5. 找到当前commit，目标分支的最新commit
> 6. 找到split commit
> 7. 对上述三个时刻的文件进行对比，对工作目录的文件进行相应的增删
> 8. 保存最终的结果，存储最新的commit，更新HEAD。





| blob内容     | 哈希                                     | 所属文件 |
| ------------ | ---------------------------------------- | -------- |
| test content | d670460b4b4aece5915caf5c68d12f560a9fe3e4 |          |
| version 1    | 83baae61804e65cc73a7201a7252750c76066a30 | test.txt |
| version 2    | 1f7a7a472abf3dd9643fd615f6da379c4acb3e3a | test.txt |



| tree | 哈希                                     | 内容                                                         |
| ---- | ---------------------------------------- | ------------------------------------------------------------ |
|      | d8329fc1cc938780ffdd9f94e0d364e0ea74f579 | test.txt+83baae61804e65cc73a7201a7252750c76066a30      test.txt |
|      | 0155eb4229851634a0f03eb265b69f5a2d56f341 | 100644 blob fa49b077972391ad58037050f2a75f74e3671e92    new.txt<br/>100644 blob 1f7a7a472abf3dd9643fd615f6da379c4acb3e3a    test.txt |
|      | 3c4e9cd789d88d8d89c1073707c3585e41b0e614 |                                                              |



## bug记录

1、UID？

https://en.wikipedia.org/wiki/Unique_identifier

2、怎么确定当前的branch的？在HEAD-->refs-->branch里

3、https://stackoverflow.com/questions/10378855/java-io-invalidclassexception-local-class-incompatible

4、==add功能不完善==，对于hash相同的commit，不应该再添到暂存区。搞定

5、commit的构造函数没有给成员变量赋值。为什么？.....构造函数多写了个花括号

6、commit的hash功能有潜在问题。commit的定义问题，commit的名字应该和ID一致

7、log有bug，输出的信息会多

8、checkout输出时，文件读取路径不对。

![1696772690694](E:\master2\coding_notes\DSA\Gitlet-Expansion.assets\1696772690694.png)

原因：**文件路径少写了一个字母。**



## 测试

怎么测试？

1、在每一个指令下print特定信息，有问题了就单步debug

add：输出staging add area

rm：输出staging remove area

init：

commit：输出commit列表

2、给各个类对象添上便于调试的信息



3、自己写单元测试用例

rm测试

> 1. 上一个commit中没有，但是stage for add有，直接删除相应blob
> 2. 上一个commit中有，但是不存在源文件



commit测

怎么debug？

```
test01-init:
OK
test02-basic-checkout:
OK
test03-basic-log:
ERROR (incorrect output)
test04-prev-checkout:
ERROR (incorrect output)
test11-basic-status:
ERROR (java gitlet.Main exited with code 1)
test12-add-status:
OK
test13-remove-status:
OK
test14-add-remove-status:
ERROR (incorrect output)
test15-remove-add-status:
ERROR (incorrect output)
test16-empty-commit-err:
OK
test17-empty-commit-message-err:
ERROR (incorrect output)
test18-nop-add:
OK
test19-add-missing-err:
ERROR (java gitlet.Main exited with code 1)
test20-status-after-commit:
OK
test21-nop-remove-err:
ERROR (java gitlet.Main exited with code 1)
test22-remove-deleted-file:
OK
test23-global-log:
ERROR (incorrect output)
test24-global-log-prev:
ERROR (incorrect output)
test25-successful-find:
ERROR (incorrect output)
test26-successful-find-orphan:
ERROR (incorrect output)
test27-unsuccessful-find-err:
OK
test28-checkout-detail:
ERROR (incorrect output)
test29-bad-checkouts-err:
ERROR (incorrect output)
test30-branches:
OK
test30-rm-branch:
OK
test31-duplicate-branch-err:
ERROR (incorrect output)
test31-rm-branch-err:
ERROR (java gitlet.Main exited with code 1)
test32-file-overwrite-err:
OK
test33-merge-no-conflicts:
ERROR (file f.txt present)
test34-merge-conflicts:
ERROR (incorrect output)
test35-merge-rm-conflicts:
ERROR (incorrect output)
test36-merge-err:
ERROR (incorrect output)
test36-merge-parent2:
ERROR (java gitlet.Main exited with code 1)
test37-reset1:
ERROR (incorrect output)
test38-bad-resets-err:
ERROR (incorrect output)
test39-short-uid:
ERROR (incorrect output)
test40-special-merge-cases:
ERROR (incorrect output)
test41-no-command-err:
OK
test42-other-err:
OK
test43-criss-cross-merge-b:
ERROR (java gitlet.Main exited with code 1)
test43-criss-cross-merge:
ERROR (incorrect output)
test44-bai-merge:
ERROR (file A.txt has incorrect content)
```

- [x] log\status，不涉及文件操作的，不管

- [x] reset和checkout类似，不管

merge 9 error

reset

add



## 一周目总结

1. 程序 = 数据结构 + 算法，完成Gitlet后算是初步理解了这句话的深意。在整个gitlet的操作过程中，我们设计、使用的所有对象其实都是我们设计的数据结构，包括Commit, blob, stage，数据结构就是把我们需要用的特性和操作，在抽象之后统一封装到一个包里，然后我们就可以在其他程序里用这些数据结构的操作，所有的数据都有一定的数据结构，比较原始的数据 int double short等等，最常用的有数组、链表、二叉树等等。算法，我的一个粗浅理解就是做事的先后顺序，放在程序里就是先执行那个程序段、再执行那个程序段。
2. 代码风格，没有形成统一风格，好的风格可以极大提高可读性。这点我做的非常不好，抽时间把==《编写可读代码的艺术》==读一读。
3. 怎么编程能避免bug？首先要对代码的架构有良好的设计，并且避免拼写错误、顺序拼反等智障操作；再者就是，一定不要在脑子非常疲惫、不清楚的时候写代码，因为大脑的不清晰导致的一些低级错误，比如忘了写一个字母，可能需要后续debug几个小时。。。有这时间不如让大脑好好休息一下。
4. 多测试，验证功能可靠性，但是测试的频率怎么定？如果没有官方提供的测试用例，又该怎么去自己写测试？自己的考虑本身就不全面的情况下，怎么能写好测试呢？抽时间把==《驯服烂代码》==这本书研究、实现一下，这本书讲的就是测试驱动开发。
5. 写好设计文档，比较大的项目理清思路后，写起来快很多。算法设计好了之后，代码自然而然就出来了，但是前提是对代码实现的功能有正确理解，还是得多看、多写代码。
6. 怎么能快速定位bug？二分法查找，当然了，这只是针对于
7. 多看别人的代码，寻找思路和启发
8. debug时候，如果涉及多次的重复性操作，可以考虑写一个测试用例，可以避免每次debug时候都在脑子里想每一步的步骤，节约脑力。
9. 把问题差不多想清楚了，就可以直接动手尝试了，在尝试的过程中加深理解，然后再不断进行调整。
10. 确定一个明确的CS方向吧。

##项目描述

撰写简历时，强调你的项目经验是非常重要的，尤其是像你这样实现了一个版本管理工具这样的项目。这不仅展示了你的技术能力，还体现了你解决复杂问题的能力。以下是一些建议，帮助你更有效地在简历上展示这个项目：

1. **项目名称和简介**：
   - 为你的项目起一个吸引人的名称，比如“MiniGit: 一个轻量级的版本控制系统”。
   - 简短描述项目的目标和你为什么要启动这个项目。例如：“旨在通过Java和C语言实现Git的核心功能，深入理解版本控制系统的工作原理。”

2. **技术栈和工具**：
   - 列出实现项目时使用的主要编程语言和工具，比如Java, C, Git等。
   - 如果有，也可以提及用于测试、构建或部署的工具。

3. **关键特性和实现**：
   - 突出你实现的关键功能，例如：“实现了commit、branch、merge等核心Git操作”。
   - 描述一些技术难点及你是如何解决的，比如：“采用了优化的数据结构来高效存储文件的不同版本，解决了大量文件处理时的性能瓶颈。”

4. **个人贡献和团队合作**：
   - 如果这是一个团队项目，明确指出你的角色和贡献。例如：“在团队中担任主要开发者，负责整体架构设计和核心功能的实现。”
   - 如果是个人项目，强调独立工作能力和自我驱动力。

5. **成果和学习**：
   - 描述项目的成果，比如：“成功模拟了Git的基本功能，提高了对版本控制系统内部工作原理的理解。”
   - 反思你从项目中学到了什么，比如解决特定问题的新方法或技术，以及这个项目如何帮助你成长为一个更好的开发者。

6. **链接和演示**：
   - 如果可能，提供项目的GitHub链接或其他在线仓库，让招聘者可以直接查看你的代码。
   - 如果有，提供项目演示的视频链接或网站，这样招聘者可以直观地看到项目的成果。

将这些信息结合到你的简历中，可以有效地展示你的技术能力和解决问题的能力。记得保持简洁明了，让招聘者能够快速抓住重点。



项目名称：TinyGit， 轻量级版本管理工具

- 旨在通过实现Git的核心功能，深入理解版本控制系统的工作原理。
- 分别使用Java和C语言完成，实现了commit、branch、merge、add、status等git的核心操作，理解了Git的基本技术原理Blob、Tree、Commit。





