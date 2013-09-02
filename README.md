GithubHub
=========

Use private repository with a free account.

Overview:
=========
This project can encrypt any number of bare git repositories into another git repository which is hosted on Github.com.

How this project work:
======================
0. Beforehand
There must be a repository called "root" on your Github.

1. Prepare
After you extract the github.sh from this project into a certain directory(e.g. some_dir). Please use git clone to fetch the root repo from your Github at the same dir contains github.sh.  So some_dir looks like:
some_dir --
          |- github.sh
          |- root/

Initially the root should be empty besides .git.

Then you should call "./github.sh init" to create pem files and creat leaf/. After this, some_dir looks like this:
some_dir --
          |- github.sh
          |- git.private.pem
          |- git.public.pem
          |- root/
          |- leaf/

All the prepare work is finished!

2. Usage
Suppose I want to create a project called 'secret' which I would like to put onto Github while nobody else can read it.

Goto leaf/ and create a directory 'secret'. Then goto secret/ and init it as a bare repository: git init --bare. So this git repo will work like a remote one.(A bare repo is a git repo withou index and work space which is often used as center repositroy.)

After that, goto some other directory and git clone the newly created git bare repo: git clone dirs/some_dir/leaf/secret. Great! You are can work as usual now! Add some content and do some change. Then git add && git commit && git push. 

This git push will only push all the changes to your local git bare repos. To push the bare repo to your Github. Please use "github.sh push secret". The repo name following push should be exactly same with the directory name under leaf. Remeber this. This process will compress the secret and encrpyt it into a file under root/. Then push the update root to Github. During this process git.public.pem will be used.

Maybe you change the conent under 'secret' from other places with the similar method above and want to fetch the update content to your current PC. Please use "github.sh pull secret". This process will pull the content from Github to root/ and decrypt it into a normal directory under leaf/. During this process git.private.pem will be used.

IMPORTANT:
==========
After the pem files generated with "github.sh init", please take care these *.pem files carefully. Once they are lost, you have no way to decrypt the file on you Github which means you lost them forever!!
