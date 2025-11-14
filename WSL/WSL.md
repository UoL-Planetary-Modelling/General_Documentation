# Using WSL in VSCode for Windows users

## Purpose

* Ability to open a persistent SSH tunnel (native to Linux and Mac users) to avoid repeated password prompts in VSCode. No password prompts for opening new folders, reloading window, or getting 'kicked off'. 
* Able to log into Aire and Wavechasm without DUO

1. Request WSL installation from IT services and install. Create username and password when prompted.

```
wsl --install
```

2. Ensure you use Ubuntu 20.04

```
wsl --setdefault ubuntu-20.04
```

3. 