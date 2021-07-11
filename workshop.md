Starting your journey
---------------------

First, open up a terminal and clone this repository using SSH:

    $ git clone git@github.com:PennCreativeLabs/git-workshop.git

You may want to fork (create your own copy of) the project on github and
clone from your own repo. You can find the fork button at the top right of
the screen on a github repository, or more help about doing that [here](https://help.github.com/articles/fork-a-repo/).

Once you have cloned your repository, you should now see a directory
called `git-workshop`. This is your `working directory`

    $ cd git-workshop
    $ ls


For the curious, you should also see the `.git` subdirectory. This is
where all your repository’s data and history is kept.

    $ ls -a .git

You will see :

    branches  config  description  HEAD  hooks  info  objects  refs

The staging area
----------------

Now, let’s try adding some files into the project. Create a couple of
files.

Let’s create two files named `song.txt` and `movie.txt`.

    $ touch song.txt movie.txt

Let’s use a mail analogy.

In Git, you first add content to the `staging area` by using `git add`.
This is like putting the stuff you want to send into a cardboard box.
You finalize the process and record it into the git index by using
`git commit`. This is like sealing the box - it’s now ready to send.

Let’s add the files to the staging area

    $ git add song.txt movie.txt

or 
    $ git add .

Committing
----------

You are now ready to commit. The `-m` flag allows you to enter a message
to go with the commit at the same time.

    $ git commit -m "add two new txt files"


Let’s see what just happened
----------------------------

We should now have a new commit. To see all the commits so far, use
`git log`

    $ git log

The log should show all commits listed from most recent first to least
recent. You would see various information like the name of the author,
the date it was commited, a commit SHA number, and the message for the
commit.

You should also see your most recent commit, where you added the two new
files in the previous section. However git log does not show the files
involved in each commit. To view more information about a commit, use
`git show`.

    $ git show

You should see something similar to:

    commit 5a1fad96c8584b2c194c229de7e112e4c84e5089
    Author: <your_name> 
    Date:   <curr_date>

        add two new txt files

    diff --git a/song.txt b/movie.txt
    new file mode 100644
    index 0000000..e69de29
    diff --git a/song.txt b/movie.txt
    new file mode 100644
    index 0000000..e69de29

A necessary digression
----------------------

In this section, we are going to add more changes, and try to recover
from mistakes.

Be forewarned, this next step is going to be hard. We will need to add
some content to song.txt.

Open `song.txt` and type in your favourite line from a song, or:

e.g. Lorem ipsum Sed ut perspiciatis, unde omnis iste natus error sit
voluptatem accusantium doloremque laudantium

Then **save** the file

What did we change? A very useful command is `git diff`. This is very
useful to see exactly what changes you have done.

    $ git diff

You should see something like the following:

    diff --git a/song.txt b/song.txt
    index e69de29..2aedcab 100644
    --- a/song.txt
    +++ b/song.txt
    @@ -0,0 +1 @@
    +Lorem ipsum Sed ut perspiciatis, unde omnis iste natus error sit voluptatem accusantium doloremque laudantium

Staging area again
------------------

Now let’s add our modified file, `song.txt` to the staging area. Do you
remember how ?

Next, check the `status` of `song.txt`. Is it in the staging area now?


Undoing
-------

Let’s say we did not like putting Lorem ipsum into `song.txt`. One
advantage of a staging area is to enable us to back out before we
commit - which is a bit harder to back out of. Remembering the mail
analogy - it’s easier to take mail out of the cardboard box before you
seal it than after.

Here’s how to back out of the staging area :

    $ git reset HEAD song.txt

    Unstaged changes after reset:
    M   song.txt

Compare the `git status` now to the git status from the previous
section. How does it differ?


Your staging area should now be empty. What’s happened to the Lorem
Ipsum changes? It’s still there. We are now back to the state just
before we added this file to staging area. Going back to the mail
analogy, we just took our letter out of the box.

Undoing II
----------

Sometimes we did not like what we have done and we wish to go back to
the last *recorded* state. In this case, we wish to go back to the state
just before we added the Lorrem ipsum text to `song.txt`.

To accomplish this, we use `git checkout`, like so:

    $ git checkout song.txt

You have now un-done your changes. Your file is now empty.


Branching
---------

Most large code bases have at least two branches - a ‘live’ branch and a
‘development’ branch. The live branch is code which is OK to be deployed
on to a website, or downloaded by customers. The development branch
allows developers to work on features which might not be bug free. Only
once everyone is happy with the development branch would it be merged
with the live branch.

Creating a branch in Git is easy. The `git branch` command, when used by
itself, will list the branches you currently have

    $ git branch

The `*` should indicate the current branch you are on, which is
`main`.

If you wish to start another branch, use
`git checkout -b (branch-name)` :

    $ git checkout -b dev

Try git branch again to check which branch you are currently on:

    $ git branch
      dev
    * main

The new branch is now created. Now let’s work in that branch. To switch
to the new branch:

    $ git checkout dev

`git checkout (branch-name)` is used to switch branches.

Let’s perform some commits now,

    $ echo 'some content' > test.txt
    $ git add test.txt
    $ git commit -m "Added experimental txt"

Now, let’s compare them to the main branch. Use `git diff`

    $ git diff main

Basically what the above output says is that `test.txt` is present on
the `dev` branch, but is absent on the `main` branch.


Now you see me, now you don’t
-----------------------------

Git is good enough to handle your files when you switch between
branches. Switch back to the `main` branch

Try switching back to the main branch (Hint: It’s the same command we
used to switch to the dev branch above)

Now, where’s our `test.txt` file ?

    $ ls
    README.textile  song.txt   movie.txt     gamow.txt

As you can see the new file you created in the other branch has
disappeared. Not to worry, it is safely tucked away, and will re-appear
when you switch back to that branch.

Now, switch back to the dev branch, and check that the `test.txt` is
now present.


Merging
-------

We now try out merging. Eventually you will want to merge two branches
together after the conclusion of work.\
`git merge` allows you to do that.

Git merging works by first switching the branch you want to *into*, and
then running the command to merge the other branch in.

We now want to merge our `dev` branch into `main`. First, switch to
the `main` branch.

    git checkout main

Next, we merge the `dev` branch into `main` :

    $ git merge dev

Do you see the following output ?

    Merge made by recursive.
     test.txt |    1 +
     1 files changed, 1 insertions(+), 0 deletions(-)
     create mode 100644 test.txt

You have to be in the branch you want merge *into* and then you always
specify the branch you want to merge.

Fin
---

You have learnt :

1.  Clone a repository
2.  Commit files
3.  Check status
4.  Check diff
5.  Undoing changes
6.  Branching and merging
7.  Fixing conflicts


Now, we will tackle interacting with public repos:

Part II
========

GitHub
------
But, wait. There’s more. What about this distributed sharing thing with
Git ?

To be able to share, we’ll need a server to host our git repositiories.
GitHub (<a href="https://github.com/">github.com</a>) is probably the
easiest place to begin with.

Login or sign up with GitHub
----------------------------

If you've already got an account you can skip on to creating the repo on
github, or forking this repository and cloning it down to your local machine.

Otherwise...

Go <a href="https://github.com/signup">sign up for an account</a> at
GitHub; Or login into your GitHub account if you had previously signed
up.

Hint: You may need to setup git cache your GitHub password - see
<a href="https://help.github.com/articles/set-up-git">https://help.github.com/articles/set-up-git</a>

Then come back here, we’ll wait.

Create your first GitHub repository
-----------------------------------

A repository (repo) is a place where you would store your code. You were
practising on your very own repo just now in Part 1!

The following <a href="https://help.github.com/articles/create-a-repo">
tutorial</a> will show you how to create a GitHub repo - which you can
then share with others

Then come back here, we’ll wait.

Fork a repo
-----------

Go to [this tutorial](https://help.github.com/articles/fork-a-repo)
Then come back here, we’ll wait.

Let’s collaborate !
-------------------

Check out the `pull_request` branch on this repository for further instructions!
You can always get back to this version of the readme by checking out the main branch.

Congrats!
---

You have learnt:

1.  Forking a repo at GitHub
2.  Git push
3.  Git pull
