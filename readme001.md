What is Git ?
=============

<Add your description here>

Setup
-----
Follow this [guide](https://docs.github.com/en/get-started/quickstart/set-up-git)

Then, setup your [identity](https://docs.github.com/en/get-started/getting-started-with-git/setting-your-username-in-git).

    $ git config --global user.name "Your Name"

Your email should match your git email [settings](https://docs.github.com/en/account-and-profile/setting-up-and-managing-your-personal-account-on-github/managing-email-preferences/setting-your-commit-email-address)  

    $ git config --global user.email your.email@example.com

Getting Started
=============

Start by creating your own [repository on Github](https://docs.github.com/en/get-started/quickstart/create-a-repo) and make sure that you make it a private repo.

Then, copy-paste this raw file and commit it on the remote repo.   
Don't forget to name your file. `README.md` is a good name for this file.

Untracked changes
----------------

Then, continue by adding some new files into the project. 

Let’s create two files in your local machine named `bob.txt` and `alice.txt`.

These two files will be untracked since they are new.
In order to commit them, we ll have to `stage` them first.

In Git, you first add content to the `staging area` by using the `git add` command.  
Then, once you are happy with the content that you are about to commit, you seal the deal by using the `git commit` command.  
This finalize the process and record it into the git index.  
Dont forget to include a proper commit messages by using the `-m "commit message"`.  



Let’s add the files to the staging area

    $ git add alice.txt bob.txt

And commit them to the local repo

    $ git commit -m "Add alice and bob files"

What just happened ?
----------------------------

We should now have a new commit.  
To see all the commits so far, use `git log`

    $ git log

To view more information about a commit, use
`git show`.

    $ git show


Mixing things up
----------------------

Open `alice.txt` and type something goofy, or:

e.g. Lorem ipsum Sed ut perspiciatis, unde omnis iste natus error sit voluptatem accusantium doloremque laudantium

Hit **save**

Let's check what changed using `git diff`.

    $ git diff

You should see something like the following:

    diff --git a/alice.txt b/alice.txt
    index e69de29..2aedcab 100644
    --- a/alice.txt
    +++ b/alice.txt
    @@ -0,0 +1 @@
    +Lorem ipsum Sed ut perspiciatis, unde omnis iste natus error sit voluptatem accusantium doloremque laudantium

Staging area again
------------------

Now let’s add our modified file, `alice.txt` to the staging area. If you don't remember how, scroll up :P

Next, check the `status` of `alice.txt`.


Undo
-------

Let’s say we did not like putting Lorem ipsum into `alice.txt`. 

Here’s how to back out of the staging area :

    $ git reset HEAD alice.txt

    Unstaged changes after reset:
    M   alice.txt

Compare the `git status` now to the git status from the previous
section. How does it differ?


Your staging area should now be empty. What’s happened to the Lorem
Ipsum changes? 

Undo II
----------

Sometimes we did not like what we have done and we wish to go back to the last *recorded* state. 

In our case, lets say that we wish to go back to the state just before we added the Lorrem ipsum text to `alice.txt`.

To accomplish this, we use `git checkout`

    $ git checkout alice.txt

You have now undone your changes.  
Your file is empty again.

Branching
---------

Creating a branch in Git is easy. The `git branch` command, when used by
itself, will list the branches you currently have

    $ git branch

The `*` indicates the current branch you are on, which is `master` / `main`.

If you wish to start another branch, use `git checkout -b (new-branch-name)` :

    $ git checkout -b exp1

Try git branch again to check which branch you are currently on:


```
$ git branch
    master
  * exp1
```

Let’s perform some new commits:

    $ echo 'some content' > test.txt
    $ git add test.txt
    $ git commit -m "Added experimental txt"

Now, let’s compare them to the master branch. Use `git diff`

    $ git diff master

Basically what the above output says is that `test.txt` is present on
the `exp1` branch, but is absent on the `master` branch.

Bit of magic
-----------------------------

Git is good enough to handle your files when you switch between
branches. Switch back to the `master` branch

Try switching back to the master branch (Hint: It’s the same command we used to switch to the exp1 branch above)

Now, where is our `test.txt` file ?

    $ ls
    README.mc  alice.txt   bob.txt

As you can see the new file you created in the other branch has disappeared. 

Not to worry, your files are safe.

Now, switch back to the exp1 branch, and check that the `test.txt` is still there.


Merging
-------

We now try out merging.

At some point, you will need to merge two branches together after the conclusion of work.  
`git merge` allows you to do that.

Git merge works by first switching to the `target` branch (the one that you are looking to get the changes into), and then running the command to merge the branches togeter.

We now want to merge our `exp1` branch into `master`.  
First, switch to the `master` branch.

    git checkout master

Next, we merge the `exp1` branch into `master` :

    $ git merge exp1

You should see something like that:

```
Updating d73e1e9..3359d93
Fast-forward
test.txt |    1 +
1 files changed, 1 insertions(+), 0 deletions(-)
create mode 100644 test.txt
```


Merge Conflicts
---------------

Lets break things up

On master branch

Create a branch using the following command

    $ git branch shady-branch

Then, without checking out the branch that you just created.  
Edit bob.txt and add a new line `line change`  
Stage your changes and commit them to your `master` branch.

Proceed, by checking out your new branch `shady-branch`  
Edit bob.txt and add a line in the same location as before `oops`  
Stage your changes and commit them to `shady-branch`

Finally, move back to your `master` branch and try to merge the `shady-branch` into master.  
Things should go south at this point.

Your output should look something like this:
```
Auto-merging bob.txt
CONFLICT (content): Merge conflict in bob.txt
```

Fixing a conflict
-----------------

Open up the bob.txt file using your favorite editor.  
The file should look something like this.

```
<<<<<<< HEAD
line change
=======
oops
>>>>>>> shady-branch
```

The top part of the block, which is everything between `<<<<<< HEAD` and `======` is what was in your current branch.
While the bottom half is the version that is present from the incoming branch.

To resolve the conflict, you either choose one side or merge them as you see fit.

Once you are done with editing the file, you can resolve the conflict by creating a new commit message.  
`git add` and `git commit`.


Congratulations.  
You have fixed your first conflict.

Rolling back
------------

What about reverts?  
Well, when everything goes south, you can easily revert the changes by using the following command:

    $ git revert <commit hash>

Find the commit hash you want to revert by using the `git log` command.

NOTE: Merge commits can not be reverted by simply using the commit hash.
For reverting merges you will need the parent number, since the generated commit contains more than 1 parent git is not sure which parent to follow.
You can find the parents using `git log`

The output will look like this:
```
commit 9e6d56c8b59d5a76069d6eb17e88faaf67cb877f
Merge: fed19ca 545b073
Author: Ioannis Demelis <22951010+idemelis@users.noreply.github.com>
```

Keep an eye out for the line `Merge: fed19ca 545b073`

`git revert 9e6d56c -m 1` will get the tree as it was in `fed19ca`   
while `git revert 9e6d56c -m 2` will get the tree as it was in `545b073`


