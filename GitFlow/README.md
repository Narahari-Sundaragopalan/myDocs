# Flow for Git

* Do the following steps to create feature branches and push code

* Once you have cloned the code from master branch, do the following steps

1) This step ensures you have a local copy of the ```develop``` branch

```
git checkout -b develop origin/develop
```

2) Pull the latest changes

```
git pull
```

3) Create a feature branch

```
git checkout -b <featureBranchNameHere>
```

4) Develop your feature and do the code changes on your feature branch

5) Add your changes

6) Check your branch status - This will show you all the files to which code changes were made by you (or any generated files if there are any)

```
git status
```

7) Once you have glanced all the files, see which files you want to add to your commit, either do 7a or 7b

7a) Incase you want to add all the files, to which changes are made, just do

```
git add .
```
The above command will add all files and stage them for commit. 
                      
7b) However, there might be cases when there are files you do not want to include as part of the commit like files related to your local virtual environment
In that case you want only some specific files to be added to your commit, so you can do

```
git add <fileName1> <fileName2> ...
```

8) Commit your changes
```
git commit -m 'Some Commit message'
```

9) Once committed, run the below command. This will push your local feature branch to github to a new remote feature branch

```
git push origin <remoteFeatureBranchName>
```

10) Next, create a pull request on GitHub, from your feature branch to the develop branch.

Note: Next time you create a new feature, or any fix, go back to your ```develop``` branch on the local machine, make sure you do a ```git pull``` to pull in latest changes. Since you already created a ```develop``` branch in Step 1, all you have to do is

```
git checkout develop
git pull
```
This will pull the latest changes onto your local develop branch

* And then follow the same steps from creating a new feature branch (Step 3) and continue.

### Some more helpful commands
  #### Abandon specific file changes in the current branch
  ```
  git checkout <fileName1> <fileName2> ...
  ```
  
  #### Abandon all changes in all the files in the current branch
  ```
  git checkout -f
  ```
  
  #### Delete a local branch
  ```
  git branch -d <yourLocalBranchNameHere>
  ```
  
  #### Merge a branch
  
  * If you are working on a feature branch for a long time, and not ready to push your code yet, there is a high possibility that new features have been developed and pushed to the ```develop``` branch by other members. In that case your local repo  would not be up to date. 
Say you need the new features from the develop branch onto your local feature branch so that you can test your feature on the latest code, so in order to not disrupt your work too much, you can do the following and get the latest code on your local feature branch

* Let's say your local feature branch name is ```loginFeature```, and you created this branch a week ago from ```develop``` branch
* In the past one week, many new changes and features have been pushed to the ```develop``` branch and it has been updated.
* Now you need these latest features in your develop branch but you are not done with your ```loginFeature``` yet and would like to test it on the latest code.
* All you have to do is first commit the changes on your ```loginFeature```. So do,
```
git status
git add .
git commit -m 'Login Feature - UI part'
```
* Now to pull latest changes from the remote ```develop``` branch (and all remote branches), do
```
git fetch
```
This will fetch all the changes from the remote ```develop``` branch and update your local ```develop``` branch

* Next merge the ```develop``` branch with your feature branch which is ```loginFeature``` in this case
```
git merge origin/develop
```
Now your feature branch is up to date with all the latest changes. You can go back to working on your feature and continue to make code changes, and commit and push the feature as normal. 
If you keep our local branch up to date, it will reduce the chances of conflicts in code
