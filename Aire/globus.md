### Instructions for setting up Globus to transfer large data between Aire Scratch and resstore
1) Create and account on Globus at https://www.globus.org/ .
2) Login to Aire and download Globus:
```
wget https://downloads.globus.org/globus-connect-personal/linux/stable/globusconnectpersonal-latest.tgz
```
3) Extract tarball:
```
tar xzf globusconnectpersonal-latest.tgz
```
4) cd into globusconnectpersonal directory (change numbers according to the version you have downloaded).
```
cd globusconnectpersonal-3.2.8
```
5) Run Globus Personal:
```
./globusconnectpersonal -setup --no-gui
```
6) Create a config paths file:
```
vim ~/.globusonline/lta/config-paths
```
7) Add your home and scratch directories e.g.:
```
~/,0,1
/scratch/cm19cjg/,0,1
```
8) Check that your Aire collection is present in 'Your Collections' in your 'File Manager' on globus.org.
9) Put your Aire collection online:
```
./globusconnectpersonal -start &
```
10) Create an endpoint for resstore.
11) You can now transfer data between Aire and your other created endpoints.

    
Infomation obtained from -> https://arcdocs.leeds.ac.uk/aire/usage/file_data_management/data_transfer.html 
