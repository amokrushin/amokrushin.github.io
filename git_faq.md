# Git FAQ

- `git clone` into an existing folder?
  ```sh
  git clone https://myrepo.com/git.git temp
  mv temp/.git code/.git
  rm -rf temp
  ```
- push git commits & tags simultaneously
  ```sh
  git config --global push.followTags true
  ```
  > If set to true enable `--follow-tags` option by default. You may override this configuration at time of push by specifying `--no-follow-tags`.