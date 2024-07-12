# Contents

  - [Windows Install](#windows-install)
  - [Install on Arc or Wavechasm](#install-on-arc-or-wavechasm)
  - [Creating a New Environment](#creating-a-new-environment)
  - [Installing Conda Packages](#installing-conda-packages)
  - [Open Jupyter Lab on Arc4](#open-jupyter-lab-on-arc4)


#
#

# Anaconda & Python
To run Python you need a package/environment management system called Anaconda. The full version is quite big to download so you can download a mini version with just the essential bits - called miniconda. 
There’s lots of documentation on both Anaconda and miniconda online e.g. 
https://docs.conda.io/en/latest/miniconda.html

### Windows Install
Not ideal as you will have to copy .nc files you want to work with to your local computer. Files can be very large so it’s better to use them on one of the computing facilities e.g. arc4 or Wavechasm (skip to section below for instructions of how to set up on there)

1.	Open https://www.anaconda.com/products/individual#download-section with your web browser.
2.	Download the Anaconda for Windows installer with Python 3. (If you are not sure which version to choose, you probably want the 64-bit Graphical Installer Anaconda3-...-Windows-x86_64.exe)
3.	Install Python 3 by running the Anaconda Installer, using all of the defaults for installation except make sure to check Add Anaconda to my PATH environment variable.
4.	Once Anaconda has installed, open the Anaconda navigator, and create a new environment (called anything you like).
5.	Open your new environment, select ‘install under the ‘Jupyter Notebook’ icon to install. You may also wish to install Spyder
6.	Install relevant packages (listed in the conda packages section below) by following the instructions listed
7.	In the Anaconda Prompt window, type `Jupyter notebook -- generate config` to create a config file in C:\Users\<your_username>\.jupyter 
8.	Open the file (called jupyter_notebook_config.py) using a text editor (N.B. can do this in the Windows system file explorer)
9. Search for the line: `# c.NotebookApp.browser = ‘’` and change it to: `c.NotebookApp.browser = 'C:/"Program Files (x86)"/Google/Chrome/Application/Chrome.exe %s'` to change the default browser from internet explorer to Chrome
10.	Create a ‘home’ directory somewhere in your C drive user folder (e.g. C:/Users/<your_username>/) where you want your new Jupyter notebooks to be located. Name this anything you like though don’t make it too long.
11. In your jupyter_notebook_config.py file, search for the line: `# c.NotebookApp.notebook_dir = ‘'` and change it to: `c.NotebookApp.notebook_dir = 'C:/Users/<your_username>/Jupyter_Notebooks'` to change your Jupyter Notebook home directory to your new/chosen folder
12. To open Jupyter Notebooks `jupyter-notebook'

### Install on Arc or Wavechasm
First you will need terminal software such as Mobaxterm. To install, follow the instructions here on the video titled ‘Getting MobaXTerm and connecting to ARC’. 
This covers the install process and then logging into arc (which would be an equivalent process for Wavechasm or any other workstation).

Install miniconda in your home directory using the following commands:

```wget https://repo.anaconda.com/miniconda/Miniconda3-py39_4.9.2-Linux-x86_64.sh```

```chmod 700 Miniconda3-py39_4.9.2-Linux-x86_64.sh ```

```./Miniconda3-py39_4.9.2-Linux-x86_64.sh ```

```source .bashrc```



### Creating a New Environment
To manage different sets of packages for different projects or purposes, Anaconda allows you to create isolated environments. 
These environments ensure that packages installed in one environment do not affect others. 

Create a new environment:

`conda create --name myenv`

Press Y when prompted to proceed.

Once created, activate the new environment using:

`conda activate myenv`

Your terminal should now show the active environment name (myenv).


### Installing Conda Packages
If you have installed your own version of conda, you will need to install certain packages before you can use them. Below are some common packages containing useful functions etc. 
There are way more than this available and the list is not at all comprehensive, but should be a good starting point. You can find out more info about functions contained within each package by googling.

In the anaconda prompt window or linux terminal, activate your chosen environment using `conda activate [environment_name]`

Then use ```Conda install [package_name]``` to install relevant packages e.g. 

-	jupyterlab
-	netcdf4
- xarray
-	numpy
-	pandas
-	matplotlib
- cartopy
-	cftime
-	nc-time-axis
      -	For this package, the command above didn’t work for me for some reason but you can use the following instead: `conda install -c conda-forge nc-time-axis` or `pip install nc-time-axis`
datetime
-	geos
-	dask
-	

Now you’re ready to open a notebook and get started!




### Open jupyter lab on arc4:

**Window 1**

jupyter lab --no-browser --port=1112 &

Note you can choose any 4digit number for the port

**Window 2**

ssh -L 1112:localhost:1112 [username]@arc4.leeds.ac.uk

Next, open the link that popped up in the first window in your browser
