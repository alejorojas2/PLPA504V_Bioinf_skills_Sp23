---
title: "Lecture 4"
author: "Alejandro Rojas"
output: 
  html_document: 
    keep_md: yes
---



## Modules at AHPCC

When multiple versions of software are installed, some method is needed
to use the version you want to use. The
Modules package is the overwhelming
choice of HPC centers for this. Modules is supplied on the system to
manipulate the users's environment variables to run a choice of the
needed programs and versions.


The most important of these variables are `$PATH`, telling the system where to find executable files such as blast or bwa, and $LD_LIBRARY_PATH, telling the system where
to find shared libraries that an executable calls and requires. 

The four critical module commands are:
 * list (currently loaded)
 * avail (all available)
 * load
 * purge
 
 Let's try module list to see what are the modules in use:
 
 ```
$ module list

Currently Loaded Modules:
  1) os/el7
 ```

Now, let's see the modules available to us:

```
$ module avail
```

If the avail list is too long consider trying:

* `module --default avail` to just list the default modules.
* `module overview` or "ml ov" to display the number of modules for each name.
* Use `module spider` to find all possible modules and extensions.
* Use `module keyword key1 key2 ...` to search for all possible modules matching any of the "keys".


Now let's load blast:

```
$ module load blast
```

Try now to do `module list` to see what is available.


## Conda

We will follow this tutorial by Astrobiomike: [An introduction to conda](https://astrobiomike.github.io/unix/conda-intro)


## Installing Git

__MacOs__

A different option to git on mac apart from Xcode - command line, you can also install it via a binary installer. A macOS Git installer is maintained and available for download at the Git website, at https://git-scm.com/download/mac.

__Windows__

There are also a few ways to install Git on Windows. The most official build is available for download on the Git website. Just go to https://git-scm.com/download/win and the download will start automatically. Note that this is a project called Git for Windows, which is separate from Git itself; for more information on it, go to https://gitforwindows.org.

After git is installed, please check:

```
git --version
```

## Git - How it works?

We’ll start by exploring how version control can be used to keep track of what one person did and when. Even if you aren’t collaborating with other people, automated version control is much better than this situation:

"Piled Higher and Deeper" by Jorge Cham, http://www.phdcomics.com

![][id1]

Version control systems start with a base version of the document and then record changes you make each step of the way. You can think of it as a recording of your progress: you can rewind to start at the base document and play back each change you made, eventually arriving at your more recent version.

Once you think of changes as separate from the document itself, you can then think about “playing back” different sets of changes on the base document, ultimately resulting in different versions of that document. For example, two users can make independent sets of changes on the same document. Unless multiple users make changes to the same section of the document - a conflict - you can incorporate two sets of changes into the same base document.

![][id2]

## Setting up git

When we use Git on a new computer for the first time,
we need to configure a few things. Below are a few examples
of configurations we will set as we get started with Git:

*   our name and email address,
*   what our preferred text editor is,
*   and that we want to use these settings globally (i.e. for every project).

On a command line, Git commands are written as `git verb options`,
where `verb` is what we actually want to do and `options` is additional optional information which may be needed for the `verb`. So here is how
Dracula sets up his new laptop:

```
$ git config --global user.name "Vlad Dracula"
$ git config --global user.email "vlad@tran.sylvan.ia"
```

Please use your own name and email address instead of Dracula's. This user name and email will be associated with your subsequent Git activity,which means that any changes pushed to:

- [GitHub](https://github.com/),
- [BitBucket](https://bitbucket.org/),
- [GitLab](https://gitlab.com/)

For this lesson, we will be interacting with [GitHub](https://github.com/) and so the email address used should be the same as the one used when setting up your GitHub account. If you are concerned about privacy, please review [GitHub's instructions for keeping your email address private][git-privacy]. 

This lesson is modified from [Software carpentry](https://swcarpentry.github.io/git-novice/02-setup/index.html).

We might run into issues with github, and we will need to set up a [SSH-Key](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/reviewing-your-ssh-keys).

## Let's use git and use the repository

Then we tell Git to make planets a repository – a place where Git can store versions of our files:
```
echo "# Test" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin git@github.com:alejorojas2/Test.git
git push -u origin main
```

## How to set up Token access in github

This is probably the most easy way to set up access to github.  Here is the link with the 
instruction to setup a [token access for github](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token).

In this case you want to setup a **Personal token access**, in this case personal access tokens (classic) are less secure. Once you have a token, you can enter it instead of your password when performing Git operations over HTTPS.

```
$ git clone https://github.com/USERNAME/REPO.git
Username: YOUR_USERNAME
Password: YOUR_TOKEN
```
Personal access tokens can only be used for HTTPS Git operations. **If your repository uses an SSH remote URL, you will need to switch the remote from SSH to HTTPS.**

## How to set up SSH key for github

We might run into issues with github, and we will need to set up a [SSH-Key](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/reviewing-your-ssh-keys).




[id1]: Images/phd101212s.png
[id2]: Images/Version_control.png
