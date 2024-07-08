# Notes on using Leeds ARC HPC resource

## Useful links:

ARC4 documentation

https://arcdocs.leeds.ac.uk/systems/arc4.html#

Should you require assistance with Research/HPC-related matters, you can submit a ticket to Research IT at: https://bit.ly/arc-help.

## Useful commands:

List the available nodes (use this to find out which queues are under-utilized):
```
$ qstat -g c
```
List your or another user jobs
```
$ qstat -u $USER
```
What queues can you use?
```
$ for list in $(qconf -sul);do qconf -su $list |& grep $USER >& /dev/null && echo $list;done
```

## How to avoid deletion of files in /nobackup/user:
/:
Touch files (update file timestamps) recursively for an entire directory (-h includes symbolic links, -a only updates the access time not modification time):
```
$ find . -exec touch -ha {} \;
```

## Connecting from Off Campus:

Information here for creating a config file in your local Mobaxterm directory to connect via remote-access.leeds.ac.uk:
https://arcdocs.leeds.ac.uk/getting_started/logon/logon-off-campus.html

You can also connect to the University VPN via Pulse Secure and then ssh in the usual way. Information about how to set up the VPN can be found here:
https://arcdocs.leeds.ac.uk/getting_started/logon/logon-off-campus.html
