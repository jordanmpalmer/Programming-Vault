### Git Branching

Master (main) branch
Feature Branch
![[Git Branching.png]]

![[Git Merging.png]]

![[Hot Fix Branch.png]]

git branch 

// Will show below which indicates we have one branch and master is currently active. 
*master"

// New branch
git checkout -b name

git checkout master

![[Screenshot 2023-12-07 at 12.01.52 AM.png]]

git diff name  // shows what has been changed in file

![[Screenshot 2023-12-07 at 12.04.18 AM.png]]

git merge name // this works but not most common way, instead do PR

![[Screenshot 2023-12-07 at 12.07.08 AM.png]]

Pull Request (PR) - request to be pulled into main code
Can be done on github

git branch -d name // to delete merged branch

git commit -am "message" // can be used to commit changes to a pre-existing file

![[Screenshot 2023-12-07 at 12.15.51 AM.png]]


Does not allow checkout if file both branches have been modified. Must be committed first. 
![[Screenshot 2023-12-07 at 12.17.57 AM.png]]

git merge main // much more common to merge from master compared to  into master

When conflict arrises, fix in code editor then finally commit
![[Screenshot 2023-12-07 at 12.21.59 AM.png]]

## Undoing in Git

![[Screenshot 2023-12-07 at 12.26.39 AM.png]]


git add ()
git commit (can do commit and add together if pre-existing)
got push (send to github)
git reset     // to unstage add

![[Screenshot 2023-12-07 at 12.29.50 AM.png]]
git reset HEAD~1 // to unstage commit

git log    // shows all commits with messages

![[Screenshot 2023-12-07 at 12.32.13 AM.png]]
git reset log#code      // use log code to go back to older commit

git reset --hard log#code     // erases all updates even in code editor

Forking on github. Allows you to take a repo and duplicate it under your profile. 