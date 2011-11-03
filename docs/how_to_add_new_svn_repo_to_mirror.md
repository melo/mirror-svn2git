How-to add a new Subversion repository
======================================

What you need:

 * the Subversion URL: this is not the trunk url, but the base URL.
 * Authors names and emails

This instructions assume a setup where you create a new account,
`gitmirrors`, just to run this software. All files will be owned by this
account, all services will run under this account, and the work
directory is also inside this account.

The recommended setup is:

 * place this software under `~gitmirrors/release`;
 * add `~gitmirrors/release/bin` to the account `PATH`.


Setting up a new svn-to-git mirror
==================================

Set the `svnrepo` variable to the SVN repo URL (the base URL):

    svnrepo=svn://host/base
    export svnrepo


We will reuse this variable on the following commands.


Step 1: prepare the git repo
----------------------------

Run:

    mkdir -p ~/repos/.NAME-OF-REPO/workdir
    cd ~/repos/.NAME-OF-REPO/workdir
    git init


Note: the .NAME_OF_REPO is used to make the update script ignore this
repo while we finish the setup.


Step 2: create the initial authors file
---------------------------------------

Use the script `gm-create-authors-file` to create the initial
authors file

    gm-create-authors-file $svnrepo > authors.lst


Edit the authors file to add the email address of each author. Separate
the svn user and the email address with whitespace (spaces or tabs).

    # original line
    melo

    # becomes
    melo = Pedro Melo <melo@simplicidade.org>


Add the authors file to the git configuration:

    git config --add svn.authorsfile `pwd`/../authors.lst


Step 3: init and fetch the svn repository once
----------------------------------------------

Run:

    git svn init -s --prefix=svn/ $svnrepo
    git svn fetch
    git repack -d -a


Step 4: configure the git remote repo and push initial version
--------------------------------------------------------------

Use the instructions provided by github or your own repository hosting
site. For example:

      git remote add origin git@github.com:USER/REPO_NAME.git
      git config --add remote.origin.mirror true
      git push


If you need to use a different SSH key, add the id_dsa key to the
~/repo/NAME/ directory and then create a new host in ~/.ssh/config
like this:

      Host perl-libnet.github.com
        HostName github.com
        IdentityFile ~/repos/perl-libnet/id_dsa


Then adjust the `git remote` command above to something like this:

      git remote add origin git@perl-libnet.github.com:USER/REPO_NAME.git


Thats it!


Automatic updates
=================

Every hour at 11 minutes past, the gm-update-mirrors runs, and for all
repos in `~/repos` the git-svn command will be used to update the local repo,
and then we push it to the remote origin.

The crontab entry looks like this:

    11 * * * * /home/gitmirrors/release/bin/gm-update-mirror /home/gitmirrors/repos
