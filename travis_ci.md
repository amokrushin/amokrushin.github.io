# Travis CI

### CLI

- [github.com/travis-ci/travis.rb](https://github.com/travis-ci/travis.rb)
- [github.com/skandyla/docker-travis-cli](https://github.com/skandyla/docker-travis-cli)

```
# add bash command alias to ~/.bashrc
# alias travis-cli='docker run --rm -it -v $(pwd):/project skandyla/travis-cli'

# setup npm deploy
travis-cli setup npm

# add encrypted env variable to .travis.yml
travis-cli encrypt ENV_VAR=value --add

# lint .travis.yml
travis-cli lint
```
