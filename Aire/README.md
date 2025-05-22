### Instructions for running the Mars PCM on Aire.

Aire Documentation: https://arcdocs.leeds.ac.uk/aire/welcome.html
HPC1 Course Material: https://arctraining.github.io/hpc1/welcome.html

Copy svn files from Wuhu to your home directory:
```
cp -r /users/earfw/svn_aire/ ~/svn_aire
```
Use svn to download the Mars PCM:
```
mkdir PCM_Mars_r3571

/users/<user_name>/svn_aire/bin/svn checkout https://svn.lmd.jussieu.fr/Planeto/trunk --depth empty

cd trunk

/users/<user_name>/svn_aire/bin/svn update LMDZ.MARS LMDZ.COMMON UTIL
```
Revert to desired version of the PCM:
```
/users/<user_name>/svn_aire/bin/svn update -r r3571
```

Copy files needed for running on Aire from Wuhu:
```
cp /scratch/earfw/git_PCM_MARS_r3769/PCM_ONEAPI_MPI_LIBS/MARS_PCM_ONEAPI_MPI.sh trunk/LMDZ.COMMON
```

Change environment:
```
source /scratch/earfw/git_PCM_MARS_r3769/PCM_ONEAPI_MPI_LIBS/bashrc_oneapi_mpi_pcm
```

Build the model in /LMDZ.COMMON:
```
./makelmdz_fcm -arch PCM_ONEAPI_MPI -parallel mpi -d 64x48x73 -p mars gcm
```

Create a run directory in /scratch with an /OUTPUT sub directory and copy over run files:
- callphys.def
- chemthermos_reactionrates.def
- diagfi.def
- run.def.ref
- start0.nc
- startfi0.nc
- stats.def
- traceur.def
- z2sig.def

Set datadir path in callphys.def to:
```
/scratch/earfw/PCM_AIRE/PCM_Mars_datadir/datadir
```
Create a job script (see example ‘PCM_Mars_r3571_4_metals_MY24_01.run’) and run the job using:

```
sbatch PCM_Mars_r3571_4_metals_MY24_01.run
```
To see the progress of your run use:
```
squeue -u $USER
```
