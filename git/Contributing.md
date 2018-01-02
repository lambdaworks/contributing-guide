# Contributing

In this file is explained the best practice for contributing to a Git version control repository hosting service such as _Github_, _Gitlab_, _Bitbucket_, etc.
Also, there's explained flow for application development.

In the flow given in the post named [Successful Git Branching Model](http://nvie.com/posts/a-successful-git-branching-model/) there's detailed explanation of proposed flow, which is described briefly bellow.

## Branches

One of the most powerful features of Git development flow are **branches**. After the repository is created, `master` represents predefined branch. This branch is defined for the _production_ version of an application. Furthermore, it should be **protected** for force pushes (`git push origin master -f`).

Another important branch is `dev`. One way of creating `dev` branch is typing the following command:
```
git checkout -b dev
```

The `dev` branch serves for the purpose of holding finished **tasks** which need additional testing before their **push** to **production**. In practice, this branch is used for some kind of **staging** environment.

Beside these two branches (`dev` and `master`) there are other three types of branches that will be explained in the **Flow** section:
- `feature` branches
- `hotfix` branches
- `release` branch

## Flow

This section explains _Git branching model_ with branches management and releasing process.  

### Feature branches

Let's start with the `feature` branches. They represent some **feature** that needs to be implemented. Depending on which _project manager tool_ is used for a project, they are named based on internally agreed naming convention. One of the naming examples would be - `feature-login-support` or `feature-21-login-support`. Every feature **must** start from `dev` branch and **must**, eventually, be merged to `dev` branch. This is achieved via following commands:

```
git checkout -b feature-example dev
git add .
git commit -m "Implement example feature"
git push origin feature-example
```

With this commands **feature** branch is created starting from `dev`. Implemented feature is added and pushed to the remote repository. **Pull request** must be created via UI, since there is no support for _Pull request_ creation via the command line. When feature **passes tests**, is **approved** and **merged**, we **STRONGLY recommend** to delete the merged branch by typing following commands:

```
git checkout dev
git pull origin dev --rebase
git branch -d feature-example
```

This will keep local repository clean.

Just to be clear, when richer semantic in the flow is needed, **feature** branch can be started with other words such as `feature-*`, `task-*`, `bug-*`, `fix-*`, etc. It's useful because branch names contain type of task that's finished.

### Hotfix branches

This branch must be created when there's some **critical** bug in **production**. This branch is created from `master` branch and should be merged to the `master` and `dev` branches.

### Release branch

**Release** branch is used to transfer code from `dev` to `master` branch, with addition of some small configuration changes (e.x. version increment). The complete process of releasing is given in section [Releasing Process](Releasing%20Process.md). Don't forget to **tag** release.
The tag is nothing more than a **named commit**. It is strongly recommended to tag  _releases_, along with a list of changes made, for easier project tracking.
