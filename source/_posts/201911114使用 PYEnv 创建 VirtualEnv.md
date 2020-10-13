title: 使用 PYEnv 创建 VirtualEnv
date: 2019-11-14 18:22:58
tags: [Python]

---

```bash
brew install pyenv
brew install pyenv-virtualenv

vim ~/.zshrc
# 设置环境变量 
#export PYENV_ROOT="$HOME/.pyenv"
#export PATH=$PYENV_ROOT/bin:$PATH
#eval "$(pyenv init -)"
#eval "$(pyenv virtualenv-init -)"

pyenv install 3.6.8
pyenv virtualenv 3.6.8 demoenv
pyenv activate demoenv
pyenv deactivate demoevn

# 外部 pip install 后
pyenv rehash

# 若果出现 python 依赖包出错的话，需要设置 PYTHONPATH
python
# in python
#import sys;
#print(sys.path); # ['', '/Users/username/.pyenv/versions/aws/lib/python3.6/site-packages', '/Users/username/.pyenv/versions/3.6.8/lib/python36.zip', '/Users/username/.pyenv/versions/3.6.8/lib/python3.6', '/Users/username/.pyenv/versions/3.6.8/lib/python3.6/lib-dynload']
#查看最后一个路径
# exit()

export PYTHONPATH=/Users/username/.pyenv/versions/3.6.8/lib/python3.6/lib-dynload #最后一个路径
```
