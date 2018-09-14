# Flow for Git

* Do the following steps to create feature branches and push code

* Once you have cloned the code from master branch, do the following steps

* This step ensures you have a local copy of the ```develop``` branch

```
git checkout -b develop origin/develop
```

* Pull the latest changes

```
git pull
```

* Create a feature branch

```
git checkout -b <featureBranchNameHere>
```

* Do your feature changes
* Add and commit your changes

```
git add file1 file2 ...

git commit -m 'Some Commit message'
```

* Once committed, run the below command. This will push your local feature branch to github to a new remote feature branch

```
git push origin <remoteFeatureBranchName>
```

* Next, create a pull request on GitHub, from your feature branch to the develop.

Note: Next time you create a new feature, or any fix, go back to your ```develop``` branch on the local machine, make sure you do a ```git pull``` to pull in latest changes.

* And then follow the same steps from creating a new feature branch and continue.

* Some more helpful commands
  * Abandon specific file changes in the current branch
  ```
  git checkout <fileName1> <fileName2> ...
  ```
  
  * Abandon all changes in all the files in the current branch
  ```
  git checkout -f
  ```
  
  * Delete a local branch
  ```
  git branch -d <yourLocalBranchNameHere>
  ```
