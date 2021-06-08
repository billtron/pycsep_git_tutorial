# Working with shared projects on GitHub

This repository will be a playground to practice working with the fork and pull method of shared development on GitHub. The basic workflow will be described below. The text below are inspired by the references at the bottom of the document. 

## 1. Fork the repository

Forking a repository basically copies that repository to your personal GitHub account. On a forked repository, you can suggest changes to the shared repository using pull requests. The shared repository is also referred to as the *upstream* repository. You must synchronize changes from the *upstream* repository to your local fork. Your local fork is also referred to as *origin*. 

![fork_graphic](https://user-images.githubusercontent.com/1085365/121100542-f85ccb80-c7ae-11eb-9fc7-3d50133fa3e5.png)

## 2. Clone your forked copy 

Once you have cloned your forked copy and set up the remotes, you can start developing on your personal copy of the repository. Changes can then be suggested to the shared repository (*upstream*) using pull requests.

    $ git clone git@github.com:YOUR_USERNAME/pycsep_git_tutorial.git

If you do not see an `upstream` entry you can add the upstream with the following command. If your GitHub needs SSH keys for authentication use the SSH address instead.

    $ git remote add upstream https://github.com/billtron/pycsep_git_tutorial.git
    
Verify the remotes 

    $ git remote -v
    > origin    https://github.com/YOUR_USERNAME/pycsep_git_tutorial.git (fetch)
    > origin    https://github.com/YOUR_USERNAME/pycsep_git_tutorial.git (push)
    > upstream  https://github.com/billtron/pycsep_git_tutorial.git (fetch)
    > upstream  https://github.com/billtron/pycsep_git_tutorial.git (push)
    
## 3. Synchronizing your personal repository with the shared repository

There are two approaches for synchronizing the local and *upstream* repositories. The first and recommended approach uses `git merge` and the second uses `git rebase`. You should synchronize your local default branch with *upstream* often. Note: you must push the updated local branch to *origin* if you would like those updates to appear in your personal GiHub repository for the project. When using `git rebase` you might need to force push the changes to *origin*. Do this with care. 

If `git` spots that competing changes are made to the same line of a file you will be unable to merge the changes from *upstream*. This is known as a `merge conflict`. Read how to [resolve merge conflicts using the command line](https://docs.github.com/en/github/collaborating-with-pull-requests/addressing-merge-conflicts/resolving-a-merge-conflict-using-the-command-line).

### Using `git merge`

From the `git` documentation page: "`git merge` joins two or more development histories together". 

1. Navigate to local repository using command line
2. Fetch branches from *upstream*  
```
$ git fetch upstream
$ git merge upstream/main
> remote: Counting objects: 75, done.
> remote: Compressing objects: 100% (53/53), done.
> remote: Total 62 (delta 27), reused 44 (delta 9)
> Unpacking objects: 100% (62/62), done.
> From https://github.com/ORIGINAL_OWNER/ORIGINAL_REPOSITORY
>  * [new branch]      main     -> upstream/main
```
3. Check out default branch in local repository -- default in GitHub is `main`.
```
$ git checkout main
> Switched to branch 'main'
```
4. Merge changes from the upstream default (`main`) into your local default branch. After executing this command, your local repository should be in sync with the *upstream* repository.
```
$ git merge upstream/main
> Updating a422352..5fdff0f
> Fast-forward
>  README                    |    9 -------
>  README.md                 |    7 ++++++
>  2 files changed, 7 insertions(+), 9 deletions(-)
>  delete mode 100644 README
>  create mode 100644 README.md
```

### Using `git rebase`

`git rebase` can be a powerful tool for maintaining clean commit histories in open-source projects. It must be used with caution, because it re-writes the commit history of the repository. This has major advantages in shared development such as consolidating commits from a pull request with several commits or rewiting commit messages to better describe the suggested code changes. `git rebase` can also be very useful for updating an open pull request with new changes from the *upstream* default branch. This can occur if new changes are merge into *upstream* while a pull request is still open. 

The golden rule of rebasing is to **never rebase a shared (or public) branch**. You will be using `git rebase` on branches associated with your pull requests.

Make sure to read the references about rebasing before using this tool. 

1. Navigate to local repository using command line
2. Fetch branches from *upstream*  
```
$ git fetch upstream
> remote: Counting objects: 75, done.
> remote: Compressing objects: 100% (53/53), done.
> remote: Total 62 (delta 27), reused 44 (delta 9)
> Unpacking objects: 100% (62/62), done.
> From https://github.com/ORIGINAL_OWNER/ORIGINAL_REPOSITORY
>  * [new branch]      main     -> upstream/main
```
3. Check out default branch in local repository -- default in GitHub is `main`.
```
$ git checkout main
> Switched to branch 'main'
```
4. Rebase local changes on top *upstream* `main`
```
$ git rebase upstream/main
> First, rewinding head to replay your work on top of it...
> Fast-forwarded main to upstream/main.
```
5. Verify that local `main` was updated with changes from ``upstream/main`.
```
$ git status
> 
> On branch main
> Your branch is ahead of 'origin/main' by 1 commit.
>   (use "git push" to publish your local commits)
>  
> nothing to commit, working tree clean
```
For more information on how to use `git rebase` to clean up the commits in your pull request read [rebasing a pull request](https://www.digitalocean.com/community/tutorials/how-to-rebase-and-update-a-pull-request) and [merging vs. rebasing](https://www.atlassian.com/git/tutorials/merging-vs-rebasing). It is considered a best-practice to make pull requests clean by logically grouping related commits. For example, commits in a feature request can be grouped into *code* and *documentation* changes. This helps keeps the shared history clean.

## 4. Making a contribution

Now that our forked repository is configured to track *upstream* and we have synchronized our local default branch with the latest changes from *upstream*, we are ready to make our first commit. Most open-source projects contain a *Contribution Guidelines* or `CONTRIBUTING.md` file in the project. Take a look at these files to see if there are any project specific information. For example, a project might request that documentation be contributed with any new feature requests. It might also provide information on how to run the project's test suite. Many projects have continuous-integration build and testing configured to run automatically on commit. So, submitting pull requests (or updates to pull-requests) that do not pass the tests delays the coe review process.

### Opening a pull request 

1. Create a new branch for your feature / bug fix / **docs** contributions.
```
$ git branch my_new_branch
> Switched to a new branch 'my_new_branch'
```

2. Commit changes to local repository
```
$ touch hi_from_bill.txt && echo 'hello!' >> hi_from_bill.txt
$ git add hi_from_bill.txt
$ git commit -m 'commit to demonstrate pull request'
> 1 file changed, 0 insertions(+), 0 deletions(-)
>  create mode 100644 hi_from_bill.txt
```

3. Push changes to forked repository
```
$ git push -u origin my_new_branch
> Enumerating objects: 4, done.
> Counting objects: 100% (4/4), done.
> Delta compression using up to 8 threads
> Compressing objects: 100% (2/2), done.
> Writing objects: 100% (3/3), 294 bytes | 294.00 KiB/s, done.
> Total 3 (delta 0), reused 0 (delta 0)
> remote:
> remote: Create a pull request for 'my_new_branch' on GitHub by visiting:
> remote:      https://github.com/wsavran/pycsep_git_tutorial/pull/new/my_new_branch
> remote:
> To github.com:wsavran/pycsep_git_tutorial.git
>  * [new branch]      my_new_branch -> my_new_branch
> Branch 'my_new_branch' set up to track remote branch 'my_new_branch' from 'origin'.
```

4. Create pull request
![pull_request_graphic](https://user-images.githubusercontent.com/1085365/121115267-c147e380-c7c9-11eb-9c33-0908727acc0d.PNG)

### Code review and merging pull request

Completing a code review is the last step before the pull request will be merged into `main`. Each project will have its own code review standards. Pull requests can be opened at any point during development (even with an empty commit). Notes should be addeded to the pull request page that indicate this pull request is *in-development* and not ready for a code review.

![image](https://user-images.githubusercontent.com/1085365/121117266-b80c4600-c7cc-11eb-9307-40bde3ecc564.png)


GitHub provides an issues page in addition to the pull requests page. These pages are related in the sense that issues and pull requests can be cross-referenced with one another. If your pull requests fixes an open issue, linking the pull requests causes the issue to automatically be closed when the pull request is successfully merged. Here is some additional reading on [how to link a pull request to an issue](https://docs.github.com/en/issues/tracking-your-work-with-issues/creating-issues/linking-a-pull-request-to-an-issue).

Modifications to pull requests can be made by simply pushing additional commits to the forked repository explained in the previous section. Newly added commits will automatically be identified by GitHub and added to the pull request.

## 5. Accepted pull request and clean up

You are now a contributor to an open-source project! The last step to finish the pull request is to synchronize your forked repository with the new changes from *upstream* and delete the feature branch. You can do this using `git rebase`.

1. Synchronize forked repository
```
$ git checkout main
$ git pull --rebase upstream main
$ git push -f origin main
```
2. Delete the local copy of the branch
```
$ git branch -d new-branch
```
3. Delete the remote branch in forked repository
```
git push origin --delete new-branch
```

Now your local reposiory is clean and your changes are available in default branch of the shared repository. Typically, project maintainers will issue released when a sufficient amount of new contributions have been made. 

## References 

Most of the information in this tutorial was taken from the following references.

- [Git cheat sheet](https://about.gitlab.com/images/press/git-cheat-sheet.pdf)
- [Working with forks](https://docs.github.com/en/github/collaborating-with-pull-requests/working-with-forks)
- [Merging vs. rebasing](https://www.atlassian.com/git/tutorials/merging-vs-rebasing)
- [Rebasing a pull request](https://www.digitalocean.com/community/tutorials/how-to-rebase-and-update-a-pull-request)
- [Git documentation](https://git-scm.com/docs/)
- [How to contribute to open-source projects](https://opensource.guide/how-to-contribute/)
