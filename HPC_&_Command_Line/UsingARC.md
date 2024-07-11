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

The university has recently (as of 11-07-24) changed how you can connect to systems remotely via ssh. Info on how to set this up in the article here:
https://it.leeds.ac.uk/it?id=kb_article_view&sysparm_article=KB0014674#issues

From off campus you can also connect to the University VPN via Pulse Secure and then ssh in the usual way you would do from campus. Information about how to set up the VPN can be found here:
https://arcdocs.leeds.ac.uk/getting_started/logon/logon-off-campus.html


