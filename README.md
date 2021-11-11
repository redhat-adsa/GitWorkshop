# Git Workshop

## What is this?

This is a workshop to help beginners through Advanced userst to understand how to use git with each of the major git hosting providers including Azure DevOps, Bitbucket, GitHub and Gitlab. In this lab you should learn how to pull, clone, fork, import, to merge requests and resolve conflicts.

## Introduction to Git Presentation

[PDF version of presentation](into_to_Git_slides.pdf)

[ODP slides of presentation](into_to_Git_slides.odp)

## Links

## Prerequisites

## Install GIT

### Installing on Linux

If you want to install the basic Git tools on Linux via a binary installer, you can generally do so through the package management tool that comes with your distribution. If you’re on Fedora (or any closely-related RPM-based distribution, such as RHEL or CentOS), you can use dnf or yum:

*NOTE:* If you are in a workshop with labs provided to you git is most likely installed and you can skip to the config section.

```shell
sudo dnf install git-all
```

If you’re on a Debian-based distribution, such as Ubuntu, use apt:

```shell
sudo apt install git-all
```

### Installing on macOS

If using a Mac with OSX, from version 10.9 onward, Git is preinstalled up to Big Sur (Latest OS release at this time) and can be verified by running the following:

```shell
git --version
```

For other operating systems, follow latest documentation on [git-scm.com](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)

## Create your first project

To create your first local project you will need to create a directory and run the init command with the name of the default branch as `main` otherwise it will default to `master`.

```shell
mkdir project-name
cd project-name
git init --initial-branch=main
```

*NOTE:* git init --initial-branch command was added in git 2.28. If you have an older version you will need to create the main branch and then set the global default to main in next step.

## Set main as global default

Extra credit so you don't have to set initial branch every time you create a new repository.

```shell
git config --global init.defaultBranch main
```

## Git Config

The git config command is used to get and set repository or global level options. Let's set some options for the repository you just created so that when you do a push it knows who made the changes. `NOTE:` If this is a shared machine ([vscodeserver](https://github.com/cdr/code-server) etc) remove the `--global` and it will only add this to the local repository.

```shell
git config --global user.name "john doe"
git config --global user.email "john.doe@example.com"
```

### Show all settings

```shell
git config --list
```

Output:

```shell
user.name=John Doe
user.email=johndoe@example.com
color.status=auto
color.branch=auto
color.interactive=auto
color.diff=auto
```
