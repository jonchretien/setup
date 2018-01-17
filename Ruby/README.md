# Ruby
Install using [rbenv](https://github.com/rbenv/rbenv).

```
brew install rbenv
brew install openssl libyaml libffi
curl -fsSL https://github.com/rbenv/rbenv-installer/raw/master/bin/rbenv-doctor | bash
xcode-select --install
rbenv install 2.x.x
```

Add this to your .bash_profile:

```
# rbenv
eval "$(rbenv init -)"
```

## Gems
[Sass](http://sass-lang.com): `gem install sass`
