```git diff _old_file new_file_```  
可以比较两个Repository中两个文件的不同，但需要文件的commit ID。 

|working directory|staging area|Repository|
```git diff```:可以比较所有working directory（当前工作区）中文件前后的不同，不需要文件ID。  
```git diff --staged```: 比较staging area和最新的(the most recent)commit中文件的不同，同样不需要文件ID。
```git reset --hard```撤回working directory和staging area所有没有commit的文件更改。  

```git clone _file_ID/URL_```  
可以根据file_ID或者URL克隆git上的文件，同时还可以拷贝下文件的修改历史，即log。

```git log```  
显示文件的修改历史，置顶的文件是最新的。  
```git log --graph --oneline```显示各个版本的分支。

```git checkout file_ID```  
将文件还原至只某个历史版本，file_ID可以从git log中查找。

```git init```  
创建一个git库，生成*.git*文件。  

```ls -a```  
查看隐藏目录、文件。  

```git status```  
显示git repository的状态。  

```git add file_name```  
将要commit的文件逐个添加到staging area中，这样可以不断地缓存文件，但最终只需要提交一次。  
如果你意外地将某个文件添加到暂存区中，可以使用 ```git reset``` 删除它，e.g.```git reset lesson_2_reflections.txt```  


```git commit```  
为staging area缓冲区的文件创建一个commit。该命令输入后会跳出之前设置的默认编辑器，在最顶一行，添加commit中的对文件修改的简要描述，这很重要，便于今后回查代码的更改。  

```git branch```
查看分支内容，即有哪些分支。目前所在处的分支前会有一个*符号。  
```git branch branch_name```，通过在命令后添加一个创建的分支的名称，创建一个分支。  
如果要跳转到哪个分支上，使用```git checkout branch_name```就可以了。   

```git merge branch_name```  
首先要使用checkout指定一个分支，然后输入需要合并进来的分支名称。注：也可以同时合并多于2个分支的情况，比如处于branch1，需要将branch2,branch3也同时合并进来，则先执行```checkout branch1```，再使用```git merge branch2 branch3```。
**使用原始版本合并文件**，当得知原始文件后，之后被删除/添加的行将被删除/添加，一直保留的行将被保留。  
```git merge branch_name1 branch_name2```将branch_name1,branch_name2两个分支合并  
```git show commit_id```,使用时机：在合并分支之后，想要查看某个版本与其父级之前的变动，但无法得知其父级id时，使用git show命令。会显示该commit与其父版本之前的差别，而无需输入父级id。  

```git remote -v```查看当前添加到远程库的目录
```git remote add remote_repository_name(自己命名的) https://****```在本地的git上添加一个远程的repository.  
```git pull remote_repository_name branch_name```将远程库上的分支从网站上拉(pull,同步与合并)回本地。  
```git push remote_repository_name branch_name```将本地的分支同步(push,推)到远程库上。  
```git fetch remote_repository_name branch_name```将远程库上的分支从网站上复制回本地，但不会合并。  
Note: *pull = fetch + merge*  

Updating a Cross-Repository Pull Request  
* add the original repository as a remote in your clone, named "upstream"  
* pull the *master* branch from the original repository into your clone master, 
* merge the *master* branch into your *change* branch, the not-master branch(非master分支).means "update your clone master", and fix the conflicts in your local machine.    
* push your *change* branch to your fork.
