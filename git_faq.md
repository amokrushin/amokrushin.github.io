# Git FAQ

- #### `git clone` into an existing folder?
  ```sh
  git clone https://myrepo.com/git.git temp
  mv temp/.git code/.git
  rm -rf temp
  ```
- #### Push git commits & tags simultaneously
  ```sh
  git config --global push.followTags true
  ```
  > If set to true enable `--follow-tags` option by default. You may override this configuration at time of push by specifying `--no-follow-tags`.
  
- #### Undo last commit
  ```sh
  git reset HEAD~
  << edit files as necessary >>
  git add ...
  git commit -c ORIG_HEAD
  ```
  > _[Undo a commit and redo](https://stackoverflow.com/questions/927358/how-to-undo-the-most-recent-commits-in-git/927386#927386)_

- #### Syncing a fork
  ```sh
  git remote add upstream https://github.com/ORIGINAL_OWNER/ORIGINAL_REPOSITORY.git
  git fetch upstream
  git checkout master
  git merge upstream/master
  ```
  > _[Configuring a remote for a fork](https://help.github.com/articles/configuring-a-remote-for-a-fork/)_
  >  | 
  > _[Syncing a fork](https://help.github.com/articles/syncing-a-fork/)_

- #### Merging vs. Rebasing
  https://www.atlassian.com/git/tutorials/merging-vs-rebasing
  > The golden rule of `git rebase` is to never use it on *public* branches.
  
  > By default, the `git pull` command performs a merge, 
  > but you can force it to integrate the remote branch with a rebase by passing it the `--rebase` option.
  
