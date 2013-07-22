

## title.txt ##
Using GIT with subversion
##

## when.txt ##
long ago...
##

## content ##
Git is another source management system, similar to cvs, subvertion and many others. Why do I tell
"another", not "new", "faster" and so on? I believe that source management tool is just tool - no
more; and this tool must suit your needs as developer  and team member.  
Let's discuss "core "features we need in code management. We want to have history of changes, we
want to be able to roll back to any point, we want to be able to make branches.  
CVS and Subversion have these options and, probably, you use them every day. You use a set of commands
like: update, commit, merge. When you run this commands, all actions actually occur on server, and
other developers can share your results. Is it cool? Yes, it is. Teams can work in one office, in
different offices and they still work on the same system.  
This approach has small disadvantage: all your actions are shared, even if you don't want then to.
You may want to make some temporary branch, for example, to check some idea. As the repository is
centralized, entire team will see this branch...Now imagine, you have 10 developers, and every developer
makes a few branches to check some features or fix some bugs - you will get a great number of useless
branches...  
So the next generation for source management is to offer developer common features to be available
locally. These features are the same; commit, branch and so on, but now developers can execute this
commands without affecting some server's state. As a result every developer has his own local
repository and entire project will have set of distributed repositories.  
This architecture brings us to distributed source management systems, and git is one of them.  
You may read a lot of articles about distributed source management, how to synchronize repositories
and so one. But we will focus on useful options for developers in projects based on cvs or subversion.
For now you have no need in understanding distributed processes, synchronization and so on.


## Typical subversion use case.

Every morning you do 'svn update' to update you local working copy with latest changes from central
repository. Next you get task from tracker an start thinking. You suppose that task has two ways to
be solved and don't know which one is optimal; would be great to test each one...  
First you do all code changes for first idea; compile, run and get some results....and now you want to
check second way. Here problems appear. You can not commit code as it is not ready, and you don't want
to create branch for this couple-of-hours task...You may comment out changes and apply new one, make copy
of code on your hard drive and so on.  
Anyway, you will code second way and test it. After choosing optimal solution you commit your changes to
subversion. Would be great to optimize local development process...

Git is great tool to help you with local code management, local branching and local commiting - other team
members will not know, that you are git user!

I will give step by step use case with git, to demonstrate main aspects.


## Start with Git

Let's say that you project lives in directory ACME. This directory is working copy for subversion, so it
has .svn folder (and .svn present in any subfolder).  
'svn status' will show current state for subversion.  
First of all we have to tell git about his project. Go to ACME dir and run:

    git init

This will create local repository for git and this repo will live in .git directory. Note, project subfolders
have no .git directories.

Run:

    git status

It will show you all files as untracked: as we did not added any files to git. Before adding files, let
exclude .svn folders from git tracking. Open file: .git/info/exclude and add line: '.svn*'. Now git status
shows no .svn.

Add all project's files to git:

    git add .

Git 'status' shows all files as ready to be commited. Do it:

    git commit -am "initial file import"

Run: git status - it will show everything is ok

'svn status' will show .git as untracked item. As you do not need it in svn, you may igore it by:

    svn propset svn:ignore .git .

Now you have all you project's files tracked by two systems: subversion and git.

From subversion point of view it is no changes to working process: you use update, commit and so on
as usual.  
For example, you want to test some new idea, let's look how git helps and why it's cool.  
Run git status to be sure that everything is up to date. If you see any untracked or changed items (as a result
of svn update) just add them and run git commit.  
Also note: status says that you are in branch 'master'.  

Create branch for your idea.

    git branch idea
    git checkout idea

or

    git checkout -b idea

Run git status, it will show you are in new branch.  
Now you can make all you changes, commits (any number of commits), tests and so on.  
Any time you can switch back to main branch:

    git checkout master

and back

    git checkout idea

Being in idea branch you may want to create more branch, for sub ideas...  
Now imagine that you added all changes, test them and want to share with other developers via subversion.
First of all you should run svn status to check changes, and can run svn commit to save.
But I would offer to make commits only form branch master: master will be mirror for you subversion and
other branches is your own artifacts.
To make commit from master branch you have to move all changes made in idea. It is really easy:

    git checkout master
    git merge idea

As we did not make any changes to master, system will make simple merge forward and update your sources.
svn commit will share you code with team.
To drop unuseful branch run:

    git branch -d idea

If you confused with git, you should just remove .git foders, and reinit.


## The end...

Now you have ideal instrument to check ideas, fix bugs, and you are free to use it, even if you company
uses other source management software.
As a best practice I would offer to make branch fo every change you do, every feature or bug fix.
Also read more info in web about git commands: checkout, add, branch and merge.