Zip folder of files you want to move from ARC4 to Aire:
```
tar -czvf <name_zip_folder>.tar.gz
```
Copy to Aire:
```
scp  <name_zip_folder>.tar.gz <user>@aire:<aire_path> (just : defaults to /users/<user>)
```
Login to Aire and unzip folder:
```
tar -xzvf <name_zip_folder>.tar.gz
```
