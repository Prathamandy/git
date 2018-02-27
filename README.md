# Useful Git Commands

## Escape Commit Message/Screen

For those new to command line, getting the commit message screen (when you forget to add -m "Message") is confusing because pressing escape (or CTRL + C) does not exit the screen. Instead, keep pressing escape (if you've started attempting to type something) and type the following command:

    :q!

And press enter, and you'll return to where you were.

## Upload all files in a local directory to a new Git repository

If you have a project on your computer and you just created an empty Git repository in GitHub, use these commands to upload everything to Git.

    cd <directory>
    git init
    git remote add origin https://github.com/you/repo
    git add .
    git commit -am "Message"
    git push origin master

## Download all files from Git repository to a local directory

The opposite of the above option - for example, if your repository exists in GitHub, and you're working on it in a different local computer. Run this command outside of where you want the new directory to appear (not within the directory you want it to appear).

    git clone https://github.com/you/repo

## Remove One File from Git Cache

Remove one cached file.

    git rm -r —-cached <filename.file>

## Override Entire Local Directory

If you have some merge conflicts, or accidentally started to make a change to your local directory before pulling the changes from the master, here's how you can revert your local directory to what's on GitHub.

    git fetch --all
    git reset --hard origin/master

## Ignore a Directory

If you've been tracking a directory and later decide to ignore the whole directory, simply adding it to .gitignore isn't enough. First you must add the directory to .gitignore, then run this command:

    git rm -r --cached <directory>

Then push the changes.

## Add .gitignore to Existing Repository

Similar to above, but if you've added a .gitignore with a lot of changes.

    git rm -r --cached .
    git add .
    git commit -am "Message"

## Create a Project Page with GitHub Pages

If you have **repo** set up, but you want `http://you.github.io/repo` to be a page about your project, here's the easiest way I've found to do it.

Commit and push all local changes to your existing repo.

Create your project page in a directory, like **project** or **dist**, that includes an `index.html`. I'll use **dist** for an example.

    git add dist
    git commit -am "Create GitHub Page"
    git subtree push -prefix dist origin gh-pages

Now `http://you.github.io/repo` will be the website you've created for your project. 

If the gh-pages branch already existed, because you tried to do it the way the documentation shows (through the front end) and realized there was no way to add folders (that I know of), you can run this command.

    git push origin `git subtree split —prefix dist master`:gh-pages --force

## Force a Push or Pull

When you really want your local repository to override the remote.

    git push -f origin master
    git pull -f origin master
    
## Override Local Changes

    git fetch --all
    git reset --hard origin/master
    
## Merging Changes from Remote Pull Request with Conflicts

Make a new branch with their changes.

    git checkout -b <their-branch> master
    git pull <their.git> master
    
Play with the files and commit them.    
    
    git add <files>
    git commit -m “Message"
    git push origin master
  
Merge back into your branch.  
  
    git checkout master
    git merge --no-ff <their-branch) (:wq!)
    git push origin master

## Remove Branch

    git push origin :master
    
## Replace Master with Contents of Another Branch

    git checkout <branch>
    git merge -s ours master
    git checkout master
    git merge <branch>
    
# Remove All Local Branches Except Master

    git branch | grep -v "master" | xargs git branch -D
    
More than one branch may be added to the grep. To remove all local branches except "master" and "develop":

    git branch | grep -v "master\|develop" | xargs git branch -D
    
 # Allow Empty Commit 
 
 Fix the problem of git hooks claiming everything is "Up-to-date".
 
 ```
 git push production master
 git commit --allow-empty -m 'push to execute post-receive'
 git push production master
 ```
