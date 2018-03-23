# Git FAQ

- `git clone` into an existing folder?
  ```sh
  git clone https://myrepo.com/git.git temp
  mv temp/.git code/.git
  rm -rf temp
  ```
