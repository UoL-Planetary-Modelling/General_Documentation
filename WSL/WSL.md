# Using WSL in VSCode for Windows users

## Purpose

* Ability to open a persistent SSH tunnel (native to Linux and Mac users) to avoid repeated password prompts in VSCode. No password prompts for opening new folders, reloading window, or when you get 'kicked off'. 
* Able to log into Aire and Wavechasm without DUO

## WSL

1. Request WSL installation from IT services. They must install it for you.

https://it.leeds.ac.uk/it?id=sc_cat_item&sys_id=d6db6ef91b526d10d530eb53b24bcb80&table=sc_cat_item&searchTerm=cisc


2. Install at an in person appointment with IT.

Create username and password when prompted.

```
wsl --install
```

3. Ensure you use Ubuntu 20.04

```
wsl --setdefault ubuntu-20.04
```

## SSH config files - local

4. Exit WSL, and create an ssh.bat file in powershell using the following command.

```
exit
```
```
Add-Content -Path "$env:USERPROFILE\.ssh\test.txt" -Value 'C:\Windows\system32\wsl.exe ssh %*'
```

## Generate key pairs 

5. Generate key pairs for SSH in WSL. Use your username where it says user below.

```
wsl
```

```
ssh-keygen -t ed25519 -C "user@leeds.ac.uk"
```

6. Copy public key to servers

```
ssh-copy-id user@aire.leeds.ac.uk
```

```
ssh-copy-id user@uol-lin-213775.leeds.ac.uk
```

## SSH config files - WSL

7. Create your ssh config file for WSL. Mine looks like this. Replace Usernames and relevant servers/machines.

(The difference between wave and wavechasm is that wave is for when I am connected to the ethernet. My laptop is then a trusted part of the network and rash and passwords are not required. Wavechasm is when I am on the wifi and VPN. My terminal hangs indefinitely when I try to ssh into wavechasm using rash as a proxy while connected to the ethernet).

The Control settings allow for the persistent tunnel to be opened. Use Rash as a proxyJump to wavechasm rather than Aire in general as it allows for SSH keys to be used, whereas Aire requires 2FA every time.

```
AddKeysToAgent yes

ControlPath ~/.ssh/%r@%h:%p
ControlMaster auto
ControlPersist 120m

Host wavechasm
  HostName uol-lin-213775.leeds.ac.uk
  User hqmp0145
  ProxyJump rash
  IdentityFile ~/.ssh/id_ed25519
  ForwardAgent yes

Host wave
  HostName uol-lin-213775.leeds.ac.uk
  User hqmp0145
  IdentityFile ~/.ssh/id_ed25519
  ForwardAgent yes

Host rash
  HostName rash.leeds.ac.uk
  ForwardX11 yes
  ForwardX11Trusted yes
  User hqmp0145
  ServerAliveInterval 30
  ServerAliveCountMax 20

Host aire
  HostName aire.leeds.ac.uk
  User hqmp0145
  ForwardX11 yes
  ForwardX11Trusted yes
  ForwardAgent yes
  ServerAliveInterval 120
  IdentityFile ~/.ssh/id_ed25519
```


8. Check ssh connection from WSL. Passwords should not be required.

```
ssh aire
```

```
ssh wavechasm
```


## VSCode settings

9. Open your user settings (JSON) - Ctrl+Shift+P and type 'JSON'. Paste these lines into the file. Replace {USER} with your username.

The first line prevents repeated password prompts at the top of your VSCode window, and now will only request it in the terminal (if you do not use WSL).

```
"remote.SSH.showLoginTerminal": true,
"remote.SSH.lockfilesInTmp": true,
"remote.SSH.useExecServer": true,
"remote.SSH.connectTimeout": 60,
"remote.autoForwardPortsSource": "hybrid",
"security.allowedUNCHosts": [
    "wsl$"
],
"remote.SSH.enableDynamicForwarding": false,
"remote.SSH.defaultForwardedPorts": [],
"remote.SSH.path": "C:\\Users\\{USER}\\.ssh\\ssh.bat"
```

## Workflow

10. Your workflow should follow these steps

a. Open a powershell terminal
b. Open WSL
```
wsl
```
c. SSH into the server
```
ssh aire
```
d. Open VSCode and connect to host
Ctrl + Shift + P
Connect current window to host

This should connect without a login. You are now free to use VSCode without password prompts.

11. Use VSCode with WSL
If you are working remotely and would like to use a linux terminal, then you can connect to VSCode in WSL.

a. Open a powershell terminal
b. Open WSL
```
wsl
```
c. Open VSCode using the command
```
code
```

This should open VSCode in WSL - you will see WSL:Ubuntu in the bottom left corner. When you open a terminal it will be your WSL terminal, from your local machine.