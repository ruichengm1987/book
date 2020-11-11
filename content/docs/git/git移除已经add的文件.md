# git 移除已经add的文件

## 使用 git rm 命令即可，有两种选择,
* 一种是 git rm --cached “文件路径”，不删除物理文件，仅将该文件从缓存中删除；
* 一种是 git rm --f “文件路径”，不仅将该文件从缓存中删除，还会将物理文件删除（不会回收到垃圾桶）。

## 请问 git rm --cache 和 git reset HEAD 的区别到底在哪里呢？
如果要删除文件，最好用 git rm file_name，而不应该直接在工作区直接 rm file_name。
### 如果一个文件已经add到暂存区，还没有 commit，此时如果不想要这个文件了
有两种方法：
* 1，用版本库内容清空暂存区，git reset HEAD 但要慎重使用
* 2，只把特定文件从暂存区删除，git rm --cached xxx