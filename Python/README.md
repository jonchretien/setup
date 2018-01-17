# Python
https://github.com/rbenv/rbenv

```
brew install python3
brew postinstall python3
pip3 install --upgrade pip setuptools wheel
sudo pip3 install virtualenv virtualenvwrapper
```

Add this to your .bash_profile:

```
# python 3
export VIRTUALENV_PYTHON=/usr/local/bin/python3
export VIRTUALENVWRAPPER_PYTHON=$VIRTUALENV_PYTHON
```

In your terminal, type:

```
source ~/.bash_profile
```
