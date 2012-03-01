Usage:

    # rebase mybranch and all its children onto origin/master
    git rebase-all origin/master mybranch
    # if you need to resolve a conflict, resolve it and run
    git rebase-all --continue # (or --skip, or --abort: these flags will be passed through to the underlying `git rebase`)

`git-rebase-all` is my solution to
[this StackOverflow
question](http://stackoverflow.com/questions/9407234/git-maintaining-many-topic-branches-on-a-frequently-moving-base), pasted here for convenience.

In my day-to-day git workflow, I have many topic branches, like so:

<pre>
              o--o--o (t2)
             /
         o--o (t1)
        /
 o--o--o (master)
        \
         o--o--o (t3)
</pre>

When I pull from upstream,

<pre>
              o--o--o (t2)
             /
         o--o (t1)
        /
 o--o--o--n--n--n (master)
        \
         o--o--o (t3)
</pre>

I want to **rebase** all my topic branches on top of the new master:

<pre>
                        o'--o'--o' (t2)
                       /
                  o'--o' (t1)
                 /
 o--o--o--n--n--n (master)
                 \
                  o'--o'--o' (t3)
</pre>

Currently I do this by hand, using `git rebase --onto`. In this scenario, the whole update process would be:

    $ git checkout master
    $ git pull
    $ git rebase master t1
    $ git rebase --onto t1 t2~3 t2
    $ git rebase master t3

This gets even hairier when jumping between various topic branches and adding commits.

Dependencies between topic branches in my case are purely tree-like: no branch depends on more than a single other branch. (I have to eventually upstream dependent patches in some particular order, so I choose that order a priori.)

Are there any tools that can help me manage this workflow? I've seen [TopGit](http://repo.or.cz/w/topgit.git?a=blob;f=README), but it seems to be tied quite heavily to the `tg patch` email-based workflow, which isn't relevant to me.
