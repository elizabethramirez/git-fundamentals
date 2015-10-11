## Useful sites:

http://www.eecs.harvard.edu/~cduan/technical/git/
http://longair.net/blog/2009/04/16/git-fetch-and-merge/
http://book.git-scm.com/3_basic_branching_and_merging.html
http://book.git-scm.com/4_rebasing.html

## General commands:

*** Always commit changes before doing a checkout ***

to see differences between the index file (index is what has been staged for a commit) and the HEAD commit use

    git status

to view all options use 

    git config --list

to view only local repository options use
  
    git config --local --list

to view only global options use
 
    git config --global --list


to view a list of the remotes use

    git remote -v

to add a new remote use

    git remote add 'alias' 'URL'

to delete a remote use

    git remote rm ‘alias’


to see all local and remote branches use

      git branch -a

to see past comits use
  
     git log

to reset the working directory to the last commited state do

    git reset --hard

When you want to start out a branch to do some work that you are not sure if it is going to work yet

    git checkout -b newwork

Then work on the branch and commit stuff there.  When you are finish and want to merge it back to the master then do

    git checkout master
    git merge newwork

## When you want to incorporate some changes into a branch even if they cannot be ‘fast-forward’ merged

 If you made a branch, then did some changes both to the master and to the branch.
Then, tens of commits later, I realized the branch is in much better state than the master, so you want the branch to "become" the master and disregard the changes on master. Then do

    git checkout better_branch
    git merge --strategy=ours master    
    # keep the content of this branch, but record a merge
    git checkout master
    git merge better_branch             
    #fast-forward master up to the merge


# When multiple people are working on the same code and you want to pull some code:

This works both when someone does a pull request or when a collaborator wants to get the latest version from the main repository.  The best thing to do is to first fetch the remote branch, to do this first add the remote repository (see above), and then use

    git fetch [remote name]

After doing this then using git branch -a  , the remote branch should appear listed.    Then make sure you are in the local master (or in the branch that you want to merge the changes to). 

The difference between the changes to be merged and the local master can be seen by typing

    git diff master [remote-name]/master

If you want to checkout the remote changes and test them out before you merge them (this is possibly the best idea)  you need to create a new local branch that contains the remote changes. This is done by typing 

    git checkout -b  [new-branch-name]  [remote-name]/master

You will notice that if instead you just typed git checkout [remote-name]/master git will give a message complaining about being in detached HEAD state. 

Now that the remote branch has a local name and it is checked out the code can be tested.  If the code is good then it can be merged.   If the code is not working, then it can be modified and the changes can be commited in the new branch.  After fixing any problems then it can be merged.

To merge first go back to your master (or to wherever you want to merge the new-branch to):

    git checkout master

and then do 

    git merge [new-branch-name]

If there are conflicts during the merge,  then you can see them using

    git status  
    git diff

More information on how to solve merge conflicts can be found here:
http://www.kernel.org/pub/software/scm/git/docs/v1.7.3/user-manual.html#resolving-a-merge

To resolve conflicts, edit the files to fix the conflicting changes. Then run git add to add the resolved files, and run git commit to commit the repaired merge. Git remembers that you were in the middle of a merge, so it sets the parents of the commit correctly.

Finally push the local master to github using 

    git push origin master

The local branch called [new-branch] is not pushed to github.  There is no need to make branches on github every time something is merged.  The [new-branch] was created locally only so that the code could be checked out and tested.   If everything goes well, then the local branch can be deleted by doing 

    git branch -d [new-branch] 
