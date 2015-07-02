# GithubHub

Use private repository with a free account.

## Overview

This project can encrypt any number of bare git repositories in to a single `root` git repository which is hosted on Github.com.

## How this project works

### 0. Beforehand

There must be a repository called `root` on your Github.

### 1. Preparation

After you extract the `github.sh` from this project into a certain directory (e.g. `some_dir`). Please use `git clone` to fetch the `root` repo from your Github to the directory containing `github.sh`.  So `some_dir` looks like:

```
some_dir/
    |- root/
    |- github.sh
```

Initially the root should be empty besides `.git`.

Then you should call `./github.sh init` to create pem files and create `leaf/`. After this, `some_dir` looks like this:

```
some_dir/
    |- leaf/
    |- root/
    |- git.private.pem
    |- git.public.pem
    |- github.sh
```

All the preparation work is finished!

### 2. Usage

Suppose I want to create a project called `secret` which I would like to put onto Github while nobody else can read it.

Go to `leaf/` and create a directory called `secret`. Then enter that directory and init it as a bare repository: 

``` bash
cd secret
git init --bare
```

This git repo will work like a remote one (A bare repo is a git repo without an index or workspace which is often used as center repository.)

After that, go to some other directory and git clone the newly created `secret` repo: 

``` bash
cd somewhere_else
git clone path/to/some_dir/leaf/secret
```

Great! You can work as usual now! Add some content and make some changes. Then:

``` bash
git add .
git commit -am
git push
```

This git push will only push all the changes to your local git bare repos. 

To push the bare repo to your Github please use `github.sh push secret`. 

The repo name following push should be exactly same with the directory name under leaf. Remember this. This process will compress the secret and encrypt it into a file under `root/`, then push the updated `root` to Github. During this process `git.public.pem` will be used.

Maybe you change the content under `secret` from other places with the same method as above and want to fetch the updated content to your current PC. Please use `github.sh pull secret`. This process will pull the content from `root` on Github to `root/` and decrypt it into a normal directory under `leaf/`. During this process `git.private.pem` will be used.

## IMPORTANT

After the pem files generated with `github.sh init`, please take care these `*.pem` files carefully. Once they are lost, you have no way to decrypt the file on you Github which means you lost them forever!!
