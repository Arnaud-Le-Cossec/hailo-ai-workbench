# HAILO MODEL COMPILER ENVIRONEMENT

This project aims at creating an environement for HAILO compiler

## Requirements

**GPU**







https://hailo.ai/developer-zone/software-downloads/?product=ai_accelerators&device=hailo_8_8l


## install

### Install pyenv

We will need specific versions of python. If you don't already have it; I recommand you to install **Pyenv** :

```bash
curl -fsSL https://pyenv.run | bash
```

and copy those lines into your `~/.bashrc` file:

```bash
export PYENV_ROOT="$HOME/.pyenv"
[[ -d $PYENV_ROOT/bin ]] && export PATH="$PYENV_ROOT/bin:$PATH"
eval "$(pyenv init - bash)"
```
