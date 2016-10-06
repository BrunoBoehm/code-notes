## Git repository basics


```
~ $ mkdir next-big-thing
~ $ cd next-big-thing
next-big-thing $ ls
```

You created a new directory with `mkdir next-big-thing` and then moved into it by changing your directory with cd `next-big-thing`. You typed `ls` to see all the files and folders in your brand new project, and, as expected, it was empty. But this directory, `next-big-thing`, is where you're going to put all your code.

```
next-big-thing $ git init
Initialized empty Git repository in /Users/avi/next-big-thing/.git/
```

`git init` initializes a new Git repository in your current directory, will allow it to keep track of all the changes to your code and interact with other Git remotes such as GitHub. Git is letting you know that it created a new repository within the hidden .git folder in next-big-thing

Make your new file by typing touch README.md from within the next-big-thing directory. You won't see any output after touch, but you will see a new file has been created if you type ls.

```
next-big-thing $ touch README.md
next-big-thing $ ls
README.md
```

You have to explicitly tell Git about all the files you want it to keep track of.
We do this by adding the files to our Git repository with git `add <filename or path>`

```
next-big-thing $ git add README.md
next-big-thing $ git status
``

To capture all changes in a directory –– the standard way to do it –– type `git add .`, where the . refers to the entire current directory.

A commit is like a frozen copy of your code at a given point.

```
next-big-thing $ git commit -m "Added README.md"
```

To capture all outstanding changes in a commit, type `git commit -am "Your commit message"`, where -a refers to 'all changes' and -m (combined as -am) assigns a commit message of "Your commit message".

### github

GitHub does nothing special in the git universe. It's just another git repository in the cloud.

#### PUSHING

```
echo "# my-repo" >> README.md
git init
git add . 
git commit -m "first commit"
git remote add origin https...
git push -u origin master
```

What is origin? "Origin" is simply the default alias assigned to your new remote repo, but we could rename it to anything.

```
git remote -v
# view existing remotes

git remote rename origin destination
# change remote name to destination

git remote -v
# name has been changed!
```

Now that we added a remote repo, there are two actions. We can send our latest work to and retrieve the latest work from the remote.

git push takes two arguments. The first is the name of the remote repo. Remember, origin is just an alias that refers to the repository name. You don't actually have to enter the repository name. Instead, you can just use origin. The second is the name of the remote branch you want to send code to.

```
git push origin master
# we push to the master branch of our remote repo

git push
# this will also put your code implicitly

git push -u origin master
```
We only need to apply the `-u` flag (short for `--set-upstream) the first time we use git push. It tells the current local branch to track itself against the master branch of origin, the remote repo we're pushing to. After you've set the upstream link with -u, you can use git push and git pull without specifying any arguments (such as a target branch or repo).

To find all the remote branches names run `git branch -r`

#### PULLING

As we collaborate with other people, inevitably they will push some code. The only problem is that now the code on our machine (our local repo) is out of sync with the remote repo. To remedy this, we must pull down the new code from the remote repo to our local. No surprise here.

```
git pull origin master

git pull
```

#### FORKING

Forking a GitHub repository is just a way to create your own copy of it.
Every time you fork a lab, GitHub creates a copy that you can use as a sandbox to play around, all while maintaining a canonical repo where the original lab remains intact. In other words, forking creates another remote that allows you to work in your own world without messing with the core learn-co-students content.

#### CLONING

```
git clone your-copied-github-url
```

### git collaboration
A key to collaborating with git is to keep discrete and individual lines of work isolated from each other.

The master git branch is our default branch. One of the responsible ways to use git is to make sure that the master branch is always clean with working code so that if we ever need to add a bug fix, we can do it and deploy a new version of the application immediately. We don't put broken code in master so that we can always deploy master.

#### branching out

To keep master clean, when we want to start a new feature, we should do it in an isolated feature branch.

![branch out](https://dl.dropboxusercontent.com/s/d61r0fxyriaf5oj/2015-11-02%20at%2011.52%20AM.png)

```
git branch new-feature
```

and to list the branches we can type

```
git branch -a
```

We need to checkout or move into our new-feature timeline or branch so that git knows that all commits made apply to only that unit of work, timeline, or branch.

```
git checkout new-feature
```
You can create and checkout a new branch in one command using: `git checkout -b new-branch-name`. That will both create the branch new-branch-name and move into it by checking it out.

We can rename a file with `mv <original filename> <new filename>`

#### git merge

By merging the timelines, master will have all of the commits from the new-feature branch as though those events occured on the master timeline.
When we merge a branch with `git merge, it's important to be currently working on your target branch, the branch you want to move into.

```
git checkout master
git merge new-feature
```

Delete the branch, being on master by typin `git branch -d branch-to-delete`

#### git fetch and merge

Your local branches can attach to remote branches that live on the internet, generally on GitHub, that your team members might contribute to and you can download locally.
Whenever you want to update your local copy with all the branches that might have been added to the GitHub remote, you can type `git fetch`.

Even though git fetched a new commit from origin/master, it did not merge it into the local master.
After you fetch, you have access to the remote code but you still have to merge it.

How do you merge a change fetched from origin/master into your current master? From within your local master branch, type: `git merge origin/master`, referring to the branch's full path ("remote/branch")

```
git fetch
git merge origin/master
```

The commits fetched via `git fetch` are now merged from the origin/master branch into our local master branch.

git fetch is a pretty low-level git command we don't use that much because it always requires two steps, first git fetch and then git merge to actually integrate those changes into your working branch.

#### git pull is better

If you want to both fetch and merge, which is what you want to do 99% of the time, just type `git pull`. You will `git fetch` all remote changes, including those on the current branch, existing branches, and new branches. Any changes that are on a remote branch which is being tracked by your local branch, that is to say, if you are on master and there is a change to origin/master, those changes will be automatically merged.



