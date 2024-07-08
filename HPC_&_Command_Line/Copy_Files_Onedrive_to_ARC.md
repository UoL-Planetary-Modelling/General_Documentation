Before transfering files between OneDrive and ARC servers, you need to add your OneDrive to rclone. 
In case you haven't done that before, instructions are here: https://arcdocs.leeds.ac.uk/getting_started/file_transfer/rclone.html  

After setting up the above, you can copy files from Onedrive to HPC systems. 

The following command should work for one file at a time:
`rclone copy onedrive:/Folder/to/Files/to/transfer --include "{filename_to_transfer.nc}" /Destination/Folder/Path/ &`
For example:
`rclone copy onedrive:/Post_Doc/Computing_Stuff/Jianfei_WACCMX_Files --include "{waccmx_Mg_Mgp_10.nc}" /usr/not-backed-up/sestay/Jianfei_WACCMX_files/ &`

Use the command:
`jobs` to check when the transfer is finished

Someone made a useful website where you can generate a command to do multiple files at once: https://foqueiroz.github.io/web/public/arc/index.html
& there is a video explanation here: https://web.microsoftstream.com/video/ae085b90-445a-4382-be50-52418dae6c98


