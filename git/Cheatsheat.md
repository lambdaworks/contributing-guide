# Git Cheatsheet

This file contains guides such as **how to write good commit message**, **useful git commands** and their explanation. We'll start with **Writing Good Commit Message** section that contains [Erlang's guide](https://github.com/erlang/otp/wiki/writing-good-commit-messages) in a nutshell. Next section, **Useful Git commands and how to use them**, lists useful Git commands. Section **How to rebase properly?** explains the process of rebasing which is required in proposed Git flow given in [Contributing](Contributing.md) chapter.

## Table of Contents

- [Writing good commit message](Cheatsheat.md#writing-good-commit-message)
- [Useful Git commands and how to use them](Cheatsheat.md#useful-git-commands-and-how-to-use-them)
- [How to rebase properly?](Cheatsheat.md#how-to-rebase-properly)

### Writing Good Commit Message

Provided [Erlang's guide](https://github.com/erlang/otp/wiki/writing-good-commit-messages) gives rules describing how to write a good commit message. The most important rules from the above-mentioned guide are listed below.

- Write your commits in **imperative** style.

    **GOOD:** `Remove unused code`

    **WRONG:** ~~`Removed unused code`~~

- Don't put **punctuation marks** at the end of the commit message.

    **GOOD:** `Remove unused code`

    **WRONG:** ~~`Remove unused code.`~~

- Use short commit messages - up to 50 characters.

    **GOOD:** `Remove unused code`

    **WRONG:** ~~`In this commit we've removed unused code along with classes`~~

- Don't put more statements in one commit message. Use description instead. The description should be wrapped to 80 characters maximum.

    **GOOD:**
    ```
    Remove unused code

    Removed 'getAllUsers' function from 'UserDAO' class.
    Performed cleanup on 'Users' controller. Removed unused
    config file.
    ```

    **WRONG:** ~~`Remove 'getAllUsers' function. Cleanup 'Users' controller. Remove unused config.`~~

- Leave second line blank when writing descriptions.

    **GOOD:**
    ```
    Remove unused code

    Removed 'getAllUsers' function from 'UserDAO' class.
    Performed cleanup on 'Users' controller. Removed unused
    config file.
    ```

    **WRONG:**
    ```
    Remove unused code
    Removed 'getAllUsers' function from 'UserDAO' class.
    Performed cleanup on 'Users' controller. Removed unused
    config file.
    ```

### Useful Git commands and how to use them

As it is assumed that basic Git commands are widely known, the focus will move onto less known yet useful commands instead.

1. `git commit --amend`

    This command is used to avoid creation of two commits with the same or similar purpose. If commit was pushed and it is noticed that some small changes need to be done (e.g. there is an obsolete `print` statement in code), it makes no sense to create another commit with the message like `Remove unused 'print' command`. Instead, make appropriate small changes to the code and use `--amend` flag to update the previous commit. After `--amend` flag is used in `git commit` command, it's necessary to add `-f` flag to next `git push` command.
2. `git reset HEAD~`

    This command is used to **undo** last commit locally. If you type `git commit -m "Implement something"` and you notice you've made mistake, then you can use this command to reset your latest work.

### How to rebase properly?

The rebasing technique is mostly used when another branch is merged before current, for the sake of avoiding `Git default merge` commits during **resolving conflicts** process. It is recommended to perform rebase to **put** current branch on updated `HEAD` instead. Of course, this is not the only case where this technique is used, but it's the most common one.

Assume there are two branches, `branch-1` and `branch-2`:

```
git checkout -b branch-1 dev
git checkout -b branch-2 dev
```

If work is performed on `branch-2`, but `branch-1` is merged to `dev` in the meantime, then `branch-2` should be **rebased** according to Git flow given in [Contributing](Contributing.md) chapter. To do that, type following commands:

```
git checkout dev
git pull origin dev --rebase
git checkout branch-2
git rebase dev
```

Three different scenarios can occur:

1. Desired one. There are no conflicts, and current branch is successfully rebased.
2. Conflict exists and there are conflicted files in **index** with flag **both modified**. Then modify those files, add them to `index` and type `git rebase --continue`.
3. Conflict exists, but **index** doesn't show conflicted files. Then just type `git rebase --skip`.

After **rebase** it's mandatory to use `-f` flag (**force push**) during next `git push origin branch-name`.
