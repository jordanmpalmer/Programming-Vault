https://learngitbranching.js.org/
https://git-school.github.io/visualizing-git/#free-remote

| Git Command                         | Description                                                                     |
| ----------------------------------- | ------------------------------------------------------------------------------- |
| ```git init```                      | initialize project to use git                                                   |
| ```git add .```                     | add all changes to be changed                                                   |
| ```git add "filename"```            | add single file to be saved                                                     |
| ```git commit -m "message"```       | save changes with message (git commit -am "message" for no new files)           |
| ```git push origin master```        | push changes to github master                                                   |
| ```git push origin new-branch```    | push changes to github new-branch                                               |
| ```git pull origin master```        | pull changes from github master                                                 |
| ```git branch new-branch```         | create a new branch on active                                                             |
| ```git checkout -b new-branch```    | create a new branch on active while checking it out                                       |
| ```git checkout -b new-branch C2``` | create a new branch on C2 while checking it out                                                                                |
| ```git status```                    | check status of changes                                                         |
| ```git log```                       | see all previous saved changes                                                  |
| ```git checkout "commit hash"```    | travel back to old commit                                                       |
| ```git branch -d name```            | delete merged branch                                                            |
| ```git merge new-branch```          | merge new-branch into active branch                                             |
| ```git rebase "new-branch"```       | copies new-branch to after main to act as if developed in sequence vs. parallel |
| ```git rebase "new-branch" main```  | send main to new-branch                                                         |
| ```git checkout main^```            | checkout one commit above main                                                  |
| ```git checkout HEAD^```            | checkout one commit above HEAD (usually connected to main)                      |
| ```git checkout HEAD~4```           | checkout 4th commit above head                                                  |
| ```git branch -f main HEAD~3 ```    | directly reassign a branch to a commit (move by -force)                         |
| ```git reset HEAD^```               | reverses changes by moving a branch reference back commits (only good locally)  |
| ```git revert HEAD (no ^)```        | reverts a single commit and puts it after the most recent commit                |

| Git Command                               | Description                                                           |
| ----------------------------------------- | --------------------------------------------------------------------- |
| ```git cherry-pick Commit1 Commit2 ...``` | to copy a series of commits below your current location (`HEAD`)      |
| ```git rebase -i HEAD^```                 | interactive rebase - opens interface for rebasing multiple commits    |
| ```git commit --amend```                  | to make a slight change                                               |
| ```git tag "name" HEAD^```                | create project tags (milestones, anchors), these cannot be modified   |
| ```git describe```                        | how close you are to the closest tag (``<tag>_<numCommits>_g<hash>``) |
| ```git checkout main^2```                 | checks out second merge commit [[Technology/Coding/Git/Cheat sheet#^ref-1|REF]]            |
| ```git checkout HEAD~^2~2```              | ^x and ~x can be chained [[Technology/Coding/Git/Cheat sheet#^ref-2|REF]]                  |
| ```git branch new-branch HEAD~^2~2```     | New                                                                   |


| Git Command                                  | Description                                                                                              |
| -------------------------------------------- | -------------------------------------------------------------------------------------------------------- |
| ```git clone```                              | clone remote repo                                                                                        |
| ```git fetch```                              | get files from remote repo                                                                               |
| ```git pull```                               | git fetches then git merges in one command                                                               |
| ```git push```                               | send files to remote repo                                                                                |
| ```git pull --rebase```                      | shorthand for fetch and rebase                                                                           |
| ```git reset --hard o/main```                |                                                                                                          |
| ```git reset --mixed name```                 | default on git                                                                                           |
| ```git checkout -b totallyNotMain o/main```  | create custom remote tracking branch connection                                                          |
| ```git branch -u o/main foo```               | create custom remote tracking branch connection, can leave out foo if o/main is checked out              |
| ```git push origin main```                   | allows you to push from and to "main" and which remote "origin" you want                                 |
| ```git push origin <source>:<destination>``` | allows you to specific both source and destination of push (colon refspec) will even create a new branch |
| ```git fetch orgin main```                   |                                                                                                          |
| ```git fetch orgin <source>:<destination>``` |                                                                                                          |
| ```git push origin :<destination>```         | deletes destination name                                                                                 |
| ```git fetch origin :<destination>```        | makes source                                                                                             |
| ```git pull origin foo```                    |                                                                                                          |
| ```git pull origin bar~1:bugFix```           |                                                                                                          |

![[git-cheat-sheet-education.pdf]]

![[REF1.png]]^ref-1

![[REF2.png]]^ref-2