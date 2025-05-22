# Copying files from ARC4 to onedrive

Instructions for setting up rclone can be found on the [IT services page](https://leeds.service-now.com/now/nav/ui/classic/params/target/kb_view.do%3Fsys_kb_id%3D8f07414afb63d6109eaffefbaeefdc88). Once rclone and the onedrive are set up, files can be copied using  
```sh
rclone copy [local file] [new location]
```
for example, to copy the local folder ```Fe_MIF``` and all its contents to the new onedrive folder ```Fe_results``` in Research/Modelling/ARC4_results/, use
```sh
rclone copy Fe_MIF onedrive:Research/Modelling/ARC4_results/Fe_results/ -P
```
where the -P flag means that rlcone will report the progress of the file transfer.

To transfer many folders, a bash script can be produced to transfer one after another, e.g.

```sh

#!/bin/sh

rclone copy Fe_MIF onedrive:Research/Modelling/ARC4_results/Fe_results/ -P
rclone copy Mg_MIF onedrive:Research/Modelling/ARC4_results/Mg_results/ -P
rclone copy Na_MIF onedrive:Research/Modelling/ARC4_results/Na_results/ -P
```
etc.

**NOTE:** all paths and names **must not contain spaces**. Trying to transfer to, e.g. ```onedrive:Research/Modelling/ARC4 results/Fe results/``` will not work! Use camelCase or underscores to keep names readable!
