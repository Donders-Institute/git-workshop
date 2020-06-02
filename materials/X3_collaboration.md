# Exercise 3: Collaborating in small teams

### Overview
In this exercise you will apply many of the commands you have learned so far in a more realistic collaborative situation. You will work with your team to solve a common task. The idea is that you get familiar with a collaborative workflow, which includes:

1) Organising your team and your tasks using GitHub
2) Assigning yourself to work on an issue
3) Pulling your colleagues changes
4) (optional) Creating a new branch
5) Solving merge conflicts
6) Pushing your changes to the remote
7) (optional) Create pull request for feedback on your branch
8) close the issue

Check out the tasks (called 'issues' on GitHub) we create for you [here](https://github.com/Donders-Institute/git-workshop/issues) and find the ones labeled according to your breakout room. Assign yourself to one of the issues and git away. As you will be working on the same files with multiple people things will become messy and chaotic. Don't panic - it is just a practice repository you will be messing up, no hard consequences.

![meme](/Users/sophiearana/Documents/Work/Teaching/GitWorkshop/Git_imprsconference2020/test/images/git_conflict2.jpeg =300x200)

Once you are done with the breakout-room specific issues you can choose from the other issue suggestions in the repo or create your own!

[Bonus!] Once you are familiar with the steps above you can try out what the collaborative workflow looks like if you are **not** a contributor of a project. For example if you would like to improve code of a public project, you will usually not be allowed to directly push your changes. Instead you will need to [fork & request a pull](https://github.com/susam/gitpr) from the main developers.

#  
#  
#  
#  
#  
#  
#  
 


# Issues

![issuespic1](/Users/sophiearana/Documents/Work/Teaching/GitWorkshop/Git_imprsconference2020/test/images/issues_pic.png)

You can think of issues like a task on a to-do list. With issues you can structure your collaborative project on GitHub by specifying what needs to be done. People can assign themselves to these tasks and comment on them for example to report their progress or discuss questions with the team.

### Reference an issue in your commits

When you commit a change that is related to an issue, you can mention the issue in your commit message as ```#xxx```. The commit will then be listed under the respective issue. If you write ```fix #xxx``` the issue will automatically be closed.

```console
>> git commit -m "changed copyright #xxx"

```
# Branching
Per default every Git repository only has one branch, called master. You can check the list of available branches and which branch you are currently working from:

```console
>> git branch
* master
```

Let's create a new branch to encapsulate our upcoming changes with the command ```git branch <name-of-branch>```. Try to give your branches descriptive names, that will make your history more readable! Once you have created a branch you can use the ```checkout``` command to switch to the branch. If you then run ```git branch``` again you will see your new branch appear in the list.

```console
>> git branch my-new-branch
>> git checkout my-new-branch
Switched to branch 'my-branch'
>> git branch
  master
* my-new-branch
```

Instead of using two commands to create and switch to a branch, you can also use the shortcut ```git checkout -b <name-of-branch>```, it will achieve the same.

Now that we have create a new branch locally, we need to sync our remote so that the branch also appears on GitHub. For this we use the ```push``` command, but specify the new branch name:

```console
>> git push origin my-new-branch
Enumerating objects: 10, done.
Counting objects: 100% (10/10), done.
Delta compression using up to 8 threads
Compressing objects: 100% (3/3), done.
Writing objects: 100% (6/6), 638 bytes | 638.00 KiB/s, done.
Total 6 (delta 0), reused 0 (delta 0)
remote:
remote: Create a pull request for 'my-new-branch' on GitHub by visiting:
remote:      https://github.com/aranas/my_first_remote/pull/new/my-new-branch
remote:
To https://github.com/aranas/my_first_remote.git
 * [new branch]      my-new-branch -> my-new-branch
```

Now you can push and pull to this new branch, like you were used to working on the master branch. If you like to switch back to master, simply type ```git checkout master```.

When you are completely satisfied with the work you have done on your new branch and you would like to bring those changes to the master branch, then you can merge the branch into master. For this you first checkout to the branch that should receive the changes, in your case master. Before merging it is good practice to make sure master is up-to-date by doing a ```git pull```.


```console
>> git checkout master
Switched to branch 'master'
>> git pull origin master
From https://github.com/aranas/my_first_remote
 * branch            master     -> FETCH_HEAD
Already up to date.
```

Finally, you can merge your new branch into master:

```console
>> git merge my-branch
Updating 383bce8..c35d762
Fast-forward
 newfile.txt | 1 +
 1 file changed, 1 insertion(+)
 create mode 100644 newfile.txt
```

# How to resolve merge conflicts

Generally it is good practice to always run ```git pull``` BEFORE you start making your own changes, this way you will be applying your own changes to the up-to-date files and no conflicts should occur. However, it can happen that someone pushes their changes while you are working on your own. If this is the case Git will ask you to pull remote changes before pushing your own.

```console
>> git push origin master
To https://github.com/aranas/my_first_remote.git
 ! [rejected]        master -> master (fetch first)
error: failed to push some refs to 'https://github.com/aranas/my_first_remote.git'
hint: Updates were rejected because the remote contains work that you do
hint: not have locally. This is usually caused by another repository pushing
hint: to the same ref. You may want to first integrate the remote changes
hint: (e.g., 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```

In the process of pulling changes, Git will merge any changes from the remote with your own and if there is a conflict it will stop and show you the following message:

```console
>> git pull origin master
From https://github.com/aranas/my_first_remote
* branch            master     -> FETCH_HEAD
Auto-merging John_Doe.md
CONFLICT (content): Merge conflict in John_Doe.md
Automatic merge failed; fix conflicts and then commit the result.
```

Merge conflicts occur when competing changes are made to the same line of a file, or when one person edits a file and another person deletes the same file. Conflicts are standard business when working collaboratively via GitHub and you need to learn how to deal with them.
If you check ```git status``` you will see that the conflicting file is listed as modified. The basic procedure for those files is to:

#### 1) Edit files to fix conflict  

Open your favorite text editor and navigate to the file that has merge conflicts.
To see the beginning of the merge conflict in your file, search the file for the conflict marker <<<<<<<. When you open the file in your text editor, you'll see the changes from the HEAD or base branch after the line <<<<<<< HEAD. Next, you'll see =======, which divides your changes from the changes in the other branch, followed by >>>>>>> BRANCH-NAME.   

Decide if you want to keep only your branch's changes, keep only the other branch's changes, or make a brand new change, which may incorporate changes from both branches. Delete the conflict markers <<<<<<<, =======, >>>>>>> and make the changes you want in the final merge.

#### 2) Add all resolved files  

You can use ```.``` to automatically add all tracked files. Before you use this option, make sure to  double check with ```git status``` that there are no tracked files that you don't want to add yet.

```console
>> git add .
```
#### 3) Commit to complete merge.

```console
>> git commit -m "Resolved merge conflict by incorporating both suggestions."
```

At this point all conflicts are resolved and you can continue to make changes and add them to your commit or directly push to a remote.
