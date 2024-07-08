# File Transfer Using SCP, rsync, nohup and screen on Linux
This guide covers how to use **scp** and **rsync** for transferring files and how to ensure transfers are not interrupted using **nohup** and **screen**. 
These tools are helpful for maintaining uninterrupted file transfers, especially when dealing with large datasets.

## SCP (Secure Copy)
`scp` is a command-line utility for securely copying files and directories between two locations, either on the same machine or between different machines. 
It uses SSH for data transfer and provides the same level of security and encryption.

#### Basic Usage
To copy files from a remote server to your local machine:
```scp username@remote_host:/remote/path/to/file /local/path```

To copy files from your local machine to a remote server:
```scp /local/path/to/file username@remote_host:/remote/path```


## Rsync for File Transfer
rsync is another utility for efficiently synchronizing files between different systems. It's useful when you need to avoid copying files that already exist.

#### Basic Usage
To transfer files from a remote server to your local machine without copying existing files:
```rsync -aP username@remote_host:/remote/path/ /local/path/```



## Copying Large Number of Files:
For transferring a large number of files, you'll want to use methods that won't be interrupted if the connection drops. Two effective options are nohup and screen.

## Using Nohup
nohup allows a command to run even after the user has logged out. This is useful for long-running processes.

#### Steps to use nohup
Start the scp transfer with nohup:
```nohup scp -rp username@remote_host:/remote/path/* /local/path > nohup.out 2>&1 &```

Suspend the command:
Press Ctrl + Z

Restart the command in the background:
`bg`

#### Managing Background Jobs
To stop a background job and bring it to the foreground:
`jobs`

Bring the job to the foreground:
`fg [job_number]`

Terminate the job:
Press Ctrl + C

## Using Screen
screen allows multiple terminal sessions to be created, accessed, and controlled from a single terminal. 
Screen windows stay connected if the connection to the main session is lost, so its useful for file transfer

#### Steps to Use Screen
Start a new screen session:
```screen -S session_name```

Detach from the session (to return to the main window):
Press Ctrl + A, then D

List all screen sessions:
```screen -ls```

Reattach to a screen session:
If only one session is running:
```screen -r```

If multiple sessions are running:
```screen -r session_id```

Kill a specific screen session:
```screen -S session_id -X quit```

Kill all screen sessions:
```pkill screen```


#### Using SCP within a Screen Session
To transfer files using scp within a screen session:

Start a screen session and name the session 'file_transfer':
```screen -S file_transfer```

Run the scp command:
```scp -rp username@remote_host:/remote/path/* /local/path```

Detach from the session:
Press Ctrl + A, then D

Reattach to check the progress:
```screen -r file_transfer```


