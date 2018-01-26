# Git tutorial part-1

Git is a version control system. There are so much stuff that goes with the previous statement, 
but in this tutorial I will walk though using it practically.

This Tutorial assumes that you have a github account. if not you can signup [here](https://github.com/join?source=code-freaks.github.io). It's free.<br>
[The tutorial will also work with other git enabled repositories link BitBucket, GitLab etc ...]

### Download git
You can download git from [here](https://git-scm.com/), or you can use your package manager to install git.<br>
[We will be using git CLI(command line interface) not GUI ]

for Mac
```
$ brew install git
```
for linux
```
$ sudo apt install git #Debian based distro
$ sudo yum install git #Redhat based distro
```
for Windows you can use download from [here](http://gitforwindows.org/).

### Setup git

open terminal / cmd and give the following commands.
Make sure to replace `<username>` and `<email>` with your username and email.
```
git config --global user.name <username>
git config --global user.email <email>
```

### Create your own repository

Go to [github.com](www.github.com) and login.
To create a new repository you can click the `+` at the navbar and click create new repository.

Create give a name for your repository and click `create repository`

once that is created your repo should be acceble by `https://github.com/username/repo-name`

### Clone your repository to local system'

Now lets make a copy of your repository to your local system so that you can play with it.<br>
To make a local copy you should **clone the repository**

```
git clone https://github.com/username/repo-name.git
``` 
[add a .git to end of the URL]<br>
you might get a warning saying that its an empty repository. but its ok cos we havn't added any files yet.

### Add a file to the repository
First cd into the local repo clone

```
cd repo-name
```

To check the status of the repository

```
git status
```

create a file `README.md` inside the current folder and open it with a text editor like notepad.<br>
add text 
```
# My first repository
```
save it. and go back to terminal / cmd and check status again with `git status`.
```
$ git status

On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)

	README.md

nothing added to commit but untracked files present (use "git add" to track)
```

The file is not tracked by git. So we should add it 
```
git add README.md
```
Now check status.
```
$ git status
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

	new file:   README.md
```

# Commit your change
Once you have created, modified the contents you can commit the changes.
**think of commit as a save point**, each commit will give you a commit hash which you can use to restore the files to that commit state [*save point*] <br>
```
git commit -m "Added Readme"
```
`-m` parameter is used to give a message for commit.
this will save the change only in your local system.

# Push changes to Github
Now you can upload the commits that you made to Github
```
git push origin master
```
Here 
* `origin` is an alias that points to your github repo
* `master` is the default branch in the repository. More on `branches` later.

Congratulations you have uploaded your fist file to github. You can use the same procedure to add multiple files att the same time and then commit and push.

**Quick tip** you can use wild cards to select files to add for commit.<br>
example to add all the files
```
git add *
```
Stay tuned for the part-2 of this git tutorial.