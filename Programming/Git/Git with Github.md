https://www.youtube.com/watch?v=RGOj5yH7evk

### Terms
- Directory -> Folder
- Terminal or Command Line -> Interface for Text Commands
- CLI -> Command Line Interface
- cd -> Change Directory 
- Code Editor -> Word Processor for Writing Code
- Repository -> Project, or the folder/place where your project is kept
- Github -> A website to host your repositories online

### Git Commands
- Clone -> Bring a repository that is hosted somewhere like Github into a folder on your local machine
- add -> Track your files and changes in Git (file staging?)
- commit -> Save your files in Git
- push -> Upload Git commits to a remote repo, like Github
- pull -> Download changes from remote repo to your local machine, the opposite of push

ls but with hidden files: ls -la

```
git clone
git status

git add . (all files)
git add index.html

// Commit is a local save
git commit -m "message" -m "description"  //second message is optional

git push
```

### [SSH Keys](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)
Need to generate key to prove to github you are owner

ssh-keygen -t rsa -b 4096 -C "jordan@dmpnet.org"

ls | grep main-key  // Search for new key, testkey.pub is the public key you want, other is private key on local machine. Public key goes on github, private key is used to prove the public key generation

cat main-key.pub   // shows key

pbcopy < path to key "~/.ssh/main-key.pub"      // to copy file  

edit config by going 

vim ~/.ssh/config

```
Host *
	AddKeysToAgent yes
	UseKeychain yes
	IdentifyFile ~/.ssh/id_rsa
```

//Side note: 
mkdir ~/.ssh    // creates directory
touch ~/.ssh/config   // creates plain file?


### Back to gitting


git push origin main

cd ..   // moves up a file
cd ../demo2    // moves up a file then back down a file

git init      // initialize git repo in new folder

make repo on github first

git remote add origin [link]   // link to clean github repo

git remote -v   // shows all remote repos connected to this repo

// Set an upstream so you don't have to write origin main each time
git push -u origin main

![[Git Workflow.png]]
![[Git Workflow 2.png]]