# Git Notes
> Free and open source version control

---
## Git Commands
* Clone -> Bring  a repository that is hosted somewhere like Github into a folder on your local machine
```
git clone (ssh address)
git clone git@github.com:RShivathanu/gitDemo.git
```
* add -> Track your files and changes in Git
* commit -> Save your files in git 
* push -> Upload Git Commits to a remote repo, like Github
* pull -> Download changes from remote repo to your local machine, the opposite of push
* add -> This is used to add all new and modified file to the repository
```
git add .
git add sample.html
```
---
* ### Commit func
```
git commit -m "Added sample.html" -m "some description" 
```
> Here -m is used to add some messages to the commit.
---
* ### Git Push
```
git push origin master
or
git push origin main
```
---
## **Git Branching**

* Branching is creating sub branch to save the code.
* Here, the main branch is master or master branch
* And the sub branch is feature branch

```
git branch
```
> This is to checkout what are the branches
```
git checkout -b subBranch
```
> This is to switch from main branch to another branch