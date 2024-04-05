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
