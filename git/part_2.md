# Git tutorial part-2
In this tutorial I will walk you through using branches in git. And deploy a static website using github pages.

if you haven't checked out the part-1 of this tutorial please do check it out.
## Branches intro
Think of branches as multiple workspaces for the same project.
```
git branch
```
this lists all the branches in a repository.<br>
Each branch represents independent line of development. You can combine 2 branches with `merge` command <br>

Think of a scenario where you are developing a website. In such a case you will need at least 2 branches <br>
1. Master / deployment [ A branch which has stable codebase ]
2. Development [ A branch for your active development ]

Master branch will be used for deployment on your server which should be stable so that it will work properly 
for the end users.

The active development will be happening in the `Development` branch, Once a feature or a module that you are working on 
is stable in the development branch. you can `merge` the Development branch to the master branch. 
making the new code accessible to all the end users.

## Create a new repository.
[Create new a repository](https://github.com/new) in github. <br>
Give the repository name as `<username>.github.io` [replace username with your github username] <br>
this will automatically create a new website available under <username>.github.io.
An example is my website [code-freaks.github.io](https://code-freaks.github.io).

## clone the repository to your local system
```
git clone https://github.com/<username>.github.io
cd <username>.github.io
```

## Add necessary files and push to master
Now that you have cloned the repository, Lets add a landing page for your website.<br>
create an `index.html` file.


copy paste the contents to index.html 
```
<html>
    <body>
        <h1>My Website</h1>
        <p> welcome to my website </p>
    <body>
</html>
``` 

Lets add `index.html` to git
```
git add index.html
```

Once it is added, We should commit the changes
```
git commit -am "added index.html"
```

Now we should push to github
```
git push origin master
```

Now test your website, open up a web browser and go to `<username>.github.io`. This should show your index.html

### create a new branch
Now you cannot make changes in master branch for each new changes, because you do not want to accidentally break the website before complete and test the new feature that you are working on.

```
git checkout -b development
```
this will create a new branch and switch to that branch copying all the contents from the current branch from which you created the new branch

you can see which branch you are on by command `git branch`. the current branch will be highlighted
```
$ git branch
* development
  master
```
here `*` shows that am in development branch.

### Make changes in branch
Now lets make some changes, open up `index.html`
and add 
```
<a href="about.html">about</a>
```
now lets create `about.html`
```
<html>
    <body>
        This is my about page
    </body>
</html>
```
now commit and push to development branch
```
git add about.html
git commit -am "added about page"
got push origin development
```
Now check `<username>.github.io` in browser your changes wont be applied.

### Merge branch
Now lets merge development with master
```
git merge master
```
now switch to master branch
```
git checkout master
```
now push the changes to github
```
git push origin master
```
Now check the `<username>.github.io` it should contain the changes.

Congratulations you have created your first branch.
Here is a short cheatcheet for git branches
```
$ git branch # list all branches
$ git checkout -b <branch> # create new branch
$ git checkout <branch> # switch to other branch 
$ git merge <branch> # merge a branch to existing branch
```
