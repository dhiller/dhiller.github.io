# dhiller.dev

[![.github/workflows/main.yml](https://github.com/dhiller/dhiller.github.io/actions/workflows/main.yml/badge.svg?event=workflow_run)](https://github.com/dhiller/dhiller.github.io/actions/workflows/main.yml)

This is the source to my [website](https://www.dhiller.dev). It's based on [Jekyll](https://jekyllrb.com/) and hosted with [Github Pages](https://pages.github.com/).

## Local development

### System setup

```sh
$ # Install rbenv: https://github.com/rbenv/rbenv
$ rbenv install $(cat .ruby-version)
$ source ~/.zshrc
$ bundle install
```

### Running locally

```sh
bundle exec jekyll serve
```
