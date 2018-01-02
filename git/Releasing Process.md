# Releasing process

Let's assume there are only two branches present in the repository - `master` and `dev`. Type the following commands to make **release**:

1. `git checkout master`
2. `git pull origin master --rebase`
3. `git checkout dev`
4. `git pull origin dev --rebase`
5. `git checkout -b release dev`
6. Next step is to update application/project version in the configuration files (e.g. _config file_, `README.md`, `version.sbt`, etc.). The version uses `major.minor.patch` format. When changes contain the new feature, increment `minor` digit and set `patch` to **0**. When changes are **bug fixes**, increment `patch` digit. The `major` digit should be changed only when there are ground breaking changes without backward compatibility. One of the examples would be changing our **API** (_Application Programming Interface_).
7. Then type `git commit` and format the message like in the example below:
    ```
    Release major.minor.patch

    - Example of some functionality we've just added.
    - Some bug we've fixed.
    ```
8. `git checkout master`
9. `git merge --no-ff release`
10. `git push origin master`
11. Then type `git tag -a major.minor.patch` and type the same message as in the example above.
12. `git push origin major.minor.patch`
13. `git checkout dev`
14. `git merge --no-ff release`
15. `git push origin dev`
16. `git branch -d release`
