# Travis CI

### CLI

- [github.com/travis-ci/travis.rb](https://github.com/travis-ci/travis.rb)
- [github.com/skandyla/docker-travis-cli](https://github.com/skandyla/docker-travis-cli)
- [npm: How to Work with Authentication Tokens](https://docs.npmjs.com/getting-started/working_with_tokens)

```bash
# add bash command alias to ~/.bashrc
# alias travis-cli='docker run --rm -it -v $(pwd):/project skandyla/travis-cli'

# setup npm deploy
travis-cli setup npm

# add encrypted env variable to .travis.yml
travis-cli encrypt ENV_VAR=value --add

# lint .travis.yml
travis-cli lint
```
