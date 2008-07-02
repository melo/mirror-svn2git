How-to add a new Subversion repository
======================================

What you need:

 * the Subversion URL: this is not the trunk url, but the base URL.
 *

Step 1: prepare the git repo
----------------------------

mkdir ~/repos/NAME-OF-REPO
cd ~/repos/NAME-OF-REPO
git init

Tip: use .NAME_OF_REPO until everything is ready. gm-update-mirror will
ignore repos that begin with a dot.


Step 2: create the initial authors file
---------------------------------------

Use the script `gm-create-authors-file` with two parameters: repo and authors file

    gm-create-authors-file $svnrepo .authors

Edit the authors file to add the email address of each author. Separate the svn user and the email address with whitespace (spaces or tabs).

    # original line
    melo
    
    # becomes
    melo = Pedro Melo <melo@simplicidade.org>

Add the authors file to the git configuration:

    git-config --add svn.authorsfile  `pwd`/.authors

I like to put this file inside the .git directory for the repo.


Step 3: init and fetch the svn repository once
----------------------------------------------

Run:

    git-svn init -s --prefix=svn/ $svnrepo
    git-svn fetch
    git repack -d -a


Step 4: configure the git remote repo and push initial version
--------------------------------------------------------------

Use the instructions provided by github or your own repository hosting site. For example:

      git remote add origin git@github.com:USER/REPO_NAME.git
      git push


Thats it!


Automatic updates
-----------------
