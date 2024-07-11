# Table of Contents

[0) First Time Usage](#0-first-time-usage)

[1) Create a New Case](#1-create-a-new-case)
   - [Compsets](#compsets)
   - [Resolution](#resolution)

[2) Change the Number of Cores (Optional)](#2-change-the-number-of-cores-optional)

[3) Case Set Up](#3-set-up-the-case)

[4) Edit SourceMods](#4-edit-sourcemods)

[5) Modify .xml Files](#5-modify-xml-files)
   - [env_build.xml](#env_buildxml)
   - [env_run.xml](#env_runxml)
   - [Run Types](#run-types)

[6) Edit user_nl_cam File](#6-edit-user_nl_cam-file)

[7) Build the Model](#7-build-the-model)
   - [Check Input Data (Optional)](#check-input-data-optional)

[8) Run the Model](#8-run-the-model)

[9) Archiving](#9-archiving)

[10) Log Files](#10-log-files)

[11) Checking, Monitoring, and Deleting Jobs](#11-checking-monitoring-and-deleting-jobs)

[12) Continuing or Re-running the Same Case](#12-continuing-or-re-running-the-same-case)



## 0) First time usage

- If you have not used WACCM before, create a directory to contain your cases e.g.

    ```mkdir /nobackup/[username]/cesm2/```
    
    ```mkdir /nobackup/[username]/cesm2/cases/```

- If you do not already have a user directory in /nobackup/ you'll need to make one using

    ```mkdir /nobackup/[username]```


## 1) Create a new case

- Navigate to the folder you created to store your cases in /nobackup/

    ```cd /nobackup/[username]/cesm2/cases/```

- Create a new case using the `create_newcase` command from the relevant model folder. If you've not ported the model yourself you will need to be granted the correct permissions to use this 
(currently working versions are either ported by Wuhu Feng or CEMAC. This document is correct for Wuhu's ported version).
Note you will also need permissions to the input data folder, and if using WACCM-X compsets, you will need access to the ESMF libraries. 

    ```/home/home01/earfw/release_cesm2_1_3/cime/scripts/create_newcase --case /nobackup/$USER/cesm2/cases/[descriptive caes name e.g. modelversion_compset_resolution] --compset [compset_name] --res [resolution_name] --machine arc4```
    


    e.g. ```/home/home01/earfw/release_cesm2_1_3/cime/scripts/create_newcase --case /nobackup/$USER/cesm2/cases/CESM213_FX2000_f19_f19_mg16_arc4 --compset FX2000 --res f19_f19_mg16 --machine arc4 --run-unsupported```

- N.B. Depending on the combination of compset, resolution etc you may need to add `--run-unsupported` but the terminal output will notify you of this if so

### Compsets
- `--compset` is the argument that specifies the component set. The compset specifies the component models and if they are active or not, forcing scenarios, and physics options for the models. 

### Resolution

- `--res` specifies the model resolution.

- Compsets determine which grid is required.

- Available compsets, CESM supported grids and [here](https://docs.cesm.ucar.edu/models/cesm2/config/) (select the model version in the top right hand corner)


- After running create_newcase, a $CASEROOT directory is created that contains the model script needed to build and run the case.

- Once you've created a new case, use `cd` to navigate to the new case directory.

    `cd /nobackup/$USER/cesm2/cases/[case_name]`


## 2) Change the number of cores (optional)

- Before calling ./case.setup, changes to NTASKS, NTHRDS, ROOTPE, PSTRID and NINST must be made, as well as any changes to the env_mach_specific.xml file, which contains some configuration for the module environment and environment variables.

- **NTASKS variable**
    - I think NTASKS is the ony option in env_build I've ever changed.

    - **N.B.**, it is always best to change NTASKS using `./xmlchange` rather than manually editing the files, since using this method will change NTASKS for all linked components ('CPL:80', 'ATM:80', 'LND:80', 'ICE:80', 'OCN:80', 'ROF:80', 'GLC:80', 'WAV:80') whereas it's easy to forget if doing this manually.

    - 80 cores are used as default (2 nodes), but you can change this to 40 if you wanted (e.g. if you can see only 40 cores are free in the job queue and you want your job to be run faster)
    ```./xmlchange NTASKS=40```

## 3) Case Set Up

- ```./case.setup``` will create various scripts (e.g. case.run, case.st_archive), directories (e.g. the /run/ directory), and files (e.g. user_nl_cam file (where you can customise component namelist options and set parameters for model output) 

## 4) Edit SourceMods

- If you want to make modifications to the CESM code, the best practice is to copy the relevant sub-routine into the SourceMods directory in your case directory. For example, if you wanted to modify a CAM subroutine, you would copy that subroutine to the following location `/nobackup/$USER/cesm2/cases/[case_name]/SourceMods/src.cam/`

- You can copy files from the source code for the particular model version you are using (e.g. at /home/home01/earfw/release_cesm2_1_3/components/cam/src/ ) and edit them in the sourcemods folder if you want to adjust any processes, rates etc. Do **NOT** edit files directly in the model folder there.

- For example, subroutines for any chemistry, physics etc can be changed e.g.
    - mo_photo.F90 - photolysis
    - mo_usrrxt.F90 - reactions


##  5) Modify .xml files

- Now, you will want to make any changes to the .xml files e.g. env_build.xml and env_run.xml. 

- When modifying an *.xml file, you can use the **./xmlchange** tool. This is done using the syntax `./xmlchange VARIABLE=VALUE` in your case directory. If you want to change multiple variables at a time then it’s `./xmlchange KEY1=value1,KEY2=value2,KEY3=value3`

- The xml files can also be edited manually with your chosen text editor, but you should **take care not to introduce any typos** and this method can get complicated if there are lots of **dependencies**. 
Using a text editor to take a look in the xml files, you can see descriptions of each variable (including permitted values), which can be  helpful in understanding what they control.

- If you're not sure the name of a xml variable, you can search using **`./xmlquery -p [XXXX]`** where XXXX can be a variable name or part of one e.g. 
    ```
    $./xmlquery -p TASK
    
      Results in group mach_pes
              NTASKS: ['CPL:40', 'ATM:40', 'LND:40', 'ICE:40', 'OCN:40', 'ROF:40', 'GLC:40', 'WAV:40', 'ESP:40']
              NTASKS_PER_INST: ['ATM:80', 'LND:80', 'ICE:80', 'OCN:80', 'ROF:80', 'GLC:80', 'WAV:80', 'ESP:1']
      
      Results in group mach_pes_last
              MAX_MPITASKS_PER_NODE: 40
              MAX_TASKS_PER_NODE: 40
      
      Results in group run_pio
              PIO_NUMTASKS: ['CPL:-99', 'ATM:-99', 'LND:-99', 'ICE:-99', 'OCN:-99', 'ROF:-99', 'GLC:-99', 'WAV:-99', 'ESP:-99']
    
    $ ./xmlquery -p TIME
    
      Results in group case.run
              JOB_WALLCLOCK_TIME: 48:00:00
              USER_REQUESTED_WALLTIME:
      
      Results in group case.st_archive
              JOB_WALLCLOCK_TIME: 0:20:00
              USER_REQUESTED_WALLTIME:
      
      Results in group run_flags
              TIMER_DETAIL: 2
              TIMER_LEVEL: 4
    
    ```

### env_build.xml 

- Depending on your run type you will probably want to add an **initialisation file** to the **CAM_CONFIG_OPTS** e.g. `<entry id="CAM_CONFIG_OPTS" value="-phys cam4 -waccmx -ionosphere wxie -chem waccm_ma -usr_mech_infile /nobackup/earfw/acp-21-15619-2021/acp-21-15619-2021.in">` I think this is the only thing I've ever changed in here though

### env_run.xml
- There are various variables to change in env_run. 

- When a CESM model run is first initialized, it is called an initial run. If an initial run has finished and you want to continue to run the case, you can change the variable **$CONTINUE_RUN** and submit the run again.
For an intial run, $CONTINUE_RUN must be set to FALSE. If the model continues a run, $CONTINUE_RUN is set to TRUE.

- An initial CESM run can be initialized in one of **three run types**: startup, branch,or hybrid. The **$RUN_TYPE** variable is set to startup, hybrid, or branch accordingly (see section below). 
Note that the $RUN_TYPE setting is only important for the initial run of a production run when the $CONTINUE_RUN variable is set to FALSE. 

- **$STOP_N** and **$STOP_OPTION** control the **length of the run** per job submission. 

    - $STOP_OPTION : sets the run length time interval type, i.e. nmonths, ndays, nyears.
    - $STOP_N : sets the number of intervals (set by STOP_OPTION) to run the model during each submission within the specified wallclock time.
    - e.g.
        - ```./xmlchange STOP_N=6```
        - ```./xmlchange STOP_OPTION='nmonths'```

### **Restart files** 
- Restart files are written by each model component at intervals dictated by **$REST_OPTION** and **$REST_N** in env_run.xml. If a run encounters problems and crashes, restart files function as a back up point - you can continue the run from the latest restart point.
- Restart files allow the model to stop and then start again with bit-for-bit exact capability (i.e. the model output is exactly the same as if it had never been stopped).
- The default values for these two variables are set to be the same as $STOP_OPTION and $STOP_N.
- In most cases, you don't need to change these variables, but it can be useful if you want to be able to start the model again from a particular point, or if you want restart files more often than you are running the model for at each submission.

    - $REST_OPTION -  sets frequency of model restart writes (same options as STOP_OPTION)
    - $STOP_N -  sets the number of intervals (set by REST_OPTION)

- Note, Whenever a component writes a restart file, it also writes a restart pointer file of the form, rpointer.$component (i.e. rpointer.atm). The restart pointer file contains the restart filename that was just written by the component. Upon a restart, each component reads its restart pointer file to determine the filename(s) to read in order to continue the model run. 


### Run Types

#### STARTUP
- In a startup run (the default), all model components are initialized from the basic default initial conditions. The coupler does not need an initial file.

#### BRANCH
- In a branch run, all components are initialized using a set of restart files from a previous run (determined by the $RUN_REFCASE and $RUN_REFDATE variables in env_run.xml). 
Branch runs are typically used when sensitivity or parameter studies are required, or when settings for history file output streams need to be modified while still maintaining bit-for-bit reproducibility.

    - **$RUN_REFCASE** is the name of the case you're branching from. 
    - **$RUN_REFDATE** is the date of th restart files, the start of your new branch run.

- In a branch run, setting $RUN_STARTDATE is ignored because the model components obtain the start date from their restart datasets. Therefore, the start date cannot be changed for a branch run. This is the same mechanism that is used for performing a restart run (where $CONTINUE_RUN is set to TRUE in the env_run.xml file).

#### HYBRID
- In a hybrid run, CESM is initialized more like a startup, but uses initialization datasets from a previous case. This is somewhat analogous to a branch run with relaxed restart constraints.

- I'm not very familiar with hybrid runs but there is more info in the Online tutorial [here](https://ncar.github.io/CESM-Tutorial/notebooks/modifications/xml/modify_run_type/hybrid_branch_restart.html).



## 6) Edit user_nl_cam file

- Edit the user_nl_cam file in your chosen text editor to make changes to the standard options.

- This file results in the creation of component namelists, the *_in files (i.e. atm_in, lnd_in, and so on). The *_in files are located in $CASEROOT/CaseDocs/ and in $RUNDIR.

- The *_in files should not be directly edited. Any manual changes of the *_in files will be overwritten when you compile or submit the run. You cannot change the namelist variables after the run is submitted or when CONTINUE_RUN=TRUE.

- Generate the namelists by running ./preview_namelists to create the *_in files. Note that this step is optional as the script preview_namelists is also called when you build the model.

- Documentation is available on the different variables, options etc on the CESM webpage [here](https://docs.cesm.ucar.edu/models/cesm2/settings/current/cam_nml.html). Search for relevant variables to see info on them.


- **Below are some common options to change. You don't necessarily need all of these, and note this is not comprehensve**

### Output Frequency/ Variables

- avgflag_pertape, fincl1, fincl2, fincl13 etc, mfilt, and nhtfrq together control the ouput data contents/frequency.

- **avgflag_pertape** sets the averaging flag for all variables on a particular history file series. Valid values are:
    - A ==> Average,
    - B ==> GMT 00:00:00 average
    - I ==> Instantaneous
    - M ==> Minimum
    - X ==> Maximum
    - L ==> Local-time
    - S ==> Standard deviation
- **fincl1,2,3**, etc contain list of fields to include on the first/second/third history file (by default the name of fincl1 contains the string "h0", fincl2 is "h1" etc)

- **mfilt** specifies the maximum number of time samples that are written to each history file. 

- **nhtfrq** controls the write frequencies - if nhtfrq(1) = 0, the file will be a monthly average (Note only the first file series may be a monthly average).
    - If nhtfrq(i) > 0, frequency is specified as number of timesteps
    - If nhtfrq(i) < 0, frequency is specified as number of hours
 
  e.g.
```
&cam_history_nl
 avgflag_pertape                = 'A', 'I', 'A'
  fincl1 = 'Fep','Mgp','Nap','Fe','Na','Mg','e','T','Z3','U','V','OMEGA'
  fincl2 = 'Fep','Mgp','Nap','Fe','Na','Mg','e','T','Z3','U','V','OMEGA'
  fincl3 = 'Fep','Mgp','Nap','Fe','Na','Mg','e','T','Z3','U','V','OMEGA'
  fincl4 =' '
  fincl5 =' '
  fincl6 =' '
  fincl7 =' '
 mfilt          =              1,  24, 8 
 nhtfrq         =             0,   -1,  6
/
```
In this example:
- fincl1 (h0) is a monthly average
- fincl2 (h1) is hourly instantaneous with 24x hourly outputs per file
- fincl3 (h2) is 3hourly average (specified in timesteps rather than hours. Standard WACCM time step is 30 minutes). Output is 8x 3hrly outputs per file (spanning one 24h period)


### ncdata file

- Specify the ncdata input file with:
```
&cam_initfiles_nl
 ncdata         = '/resstore/b0154/Data/earfw/acp-21-15619-2021/new_Na_Fe_Mg_Si_K_Ca_f.c54137.FX2000climo.f19_f19.ZGTest.001.cam.i.0002-01-01-00000_c170817.nc'
/
```

###  Forcing files

- MIF files need to be specified in ext_frc_specifier, and any other forcing files e.g. 
        ```
          &chem_inparm
          ext_frc_cycle_yr               = 2000
          ext_frc_specifier              = 'NO2 -> /resstore/b0154/Data/earfw/acp-21-15619-2021/CCMI_emissions_aircraft_NO2_1850-2100_1.9x2.5_c130314.nc',
                 'Fe -> /home/home02/sestay/WACCM_Input_Files/acp-21-15619-2021/jd_grl55292_2016_FeMIF_divd5_FSDW5_23Aug2018.nc',
                 'Mg -> /home/home02/sestay/WACCM_Input_Files/acp-21-15619-2021/jd_grl55292_2016_MgMIF_divd5_FSDW5_15May2019.nc',
                 'Na -> /home/home02/sestay/WACCM_Input_Files/acp-21-15619-2021/jd_grl55292_2016_NaMIF_divd5_FSDW5.nc',
          ext_frc_type           = 'CYCLICAL'
          /
      ```

### Solar Cycle files

- If you want to run at solar minimum or maximum conditions you can specify the conditions for a particular day using solar_data_ymd. This will pull the solar spectral irradiance (ssi) and total solar irradiance (tsi) from the 'solar_irrad_data_file' file, and solar parameters (f10.7, f10.7a, kp, ap) from the 'solar_parms_data_file' files for the date specified. e.g. to pick a particular date from a previous solar cycle
- Input data can be downloaded from [here](https://svn-ccsm-inputdata.cgd.ucar.edu/trunk/inputdata/atm/cam/solar/)
- user_nl_cam e.g.
        ```
          &solar_data_opts
          solar_data_type                = 'FIXED'
          solar_data_ymd         = 20000101
          solar_htng_spctrl_scl          = .true.
          solar_irrad_data_file          = '/home/home02/sestay/WACCM_Input_Files/spectral_irradiance_Lean_1950-2014_daily_GOME-Mg_Leap_c150623.nc'
          solar_parms_data_file          = '/home/home02/sestay/WACCM_Input_Files/SolarParmsEPP_CMIP6_daily_s18491230_e20150101_c190109.nc'
          /
          ```

- An alternative method is to save your chosen ssi and tsi in .nc files with the same structures/attributes etc as above. You can then specify your chosen parameters specifically in your file. e.g f10.7 parameters for an average solar min or max, rather than those specifically from a previous date e.g.
    - **Solar Max:** f10.7=200, f10.7a=200, kp=3.0, ap=15
    - **Solar Min**: f10.7=70, f10.7a=70, kp=0.3, ap=1.8


## 7) Build the Model 

- The command `./case.build` builds the model. 
- For Wuhus ported version, you need to use `./case.build --skip-provenance-check`

### Check Input Data (optional)

- The command `./check_input_data --download` checks whether all required input data has been downloaded, and if not, downloads it
- This step isn't neccessary, but can be useful as a check before running the model
  
## 8) Run the Model 

- **Optional:** You can preview commands that will be run at submission time using `./preview_run`

- Create a submission script to contain all the required options and job scripts for running the model e.g. 

```
#Run with current environment (-V)
#$ -V
# Run in the current directory (-cwd)
#$ -cwd
Request some time- min 15 mins - max 48 hours
#$ -l h_rt=48:00:00

#$ -P feps-cpu
#$ -j y
#$ -l h_vmem=4G
#$ -l np=80
# ----------------------------------------
cd /nobackup/sestay/cesm2/cases/SMax_3M_FX2000_f19f19mg16/
python case.submit
```

- More info on job scripts can be found in the arc docs [here](https://arcdocs.leeds.ac.uk/usage/batchjob.html#job-scripts)

- Then, to run, change working directory to the case directory: `cd /path/to/case/directory/` (if you are not in there already) and run the model using: `qsub -cwd submission_script.run`
- This returns some text to confirm our job has been submitted and provides us with the jobs unique ID number.


## 9) Archiving

- If the model run is successful, the CESM netcdf output history files are automatically moved to a short term archive ($DOUT_S_ROOT) by case.st_archive e.g. /nobackup/$USER/cesm2/archive/SMin_3M_FX2000_f19f19mg16/logs/
- If a model run was unsuccessful the output remains in the Run Directory ($RUNDIR) and the short term archive is not created.
- If both $RUNDIR and $DOUT_S_ROOT are in the arc /nobackup/$USER/ file space, it's a good idea to move your model output files to a more permanent location as soon as you are able.
- You can also back up key files from your case directory (e.g. any sourcemods, user_nl_cam, env_build, env_run) to a repo in the Github organisation as a branch from the repo for the relevant model verion (e.g. cesm2.13). **Be very careful not to overwrite anything from the main branch when pushing changes!**

## 10) Log Files

- The log files are files in the format $model.log.* e.g. cesm.log.240414-023702.gz,  atm.log.240414-023702.gz and cpl.log.240414-023702.gz

- When the model is running, it produces the log files in the run directory (RUNDIR) e.g. $USER/cesm2/cases/SMin_3M_FX2000_f19f19mg16/run/. When the run completes successfully, the model moves the log files into the archive directory $DOUT_S_ROOT 

- If the model fails, the log files remains in the run directory.
- If a run completed successfully, the last several lines of the cpl.log.* file will have a string like SUCCESSFUL TERMINATION OF CESM. If you don’t see this message, it means the run has failed.
- 


## 11) Checking, monitoring, and deleting jobs 

- Check what queues can you use with `$ for list in $(qconf -sul);do qconf -su $list |& grep $USER >& /dev/null && echo $list;done`
- List available nodes (to find out which queues are free before you submit your job): `$ qstat -g c`
- List your or another user jobs to check the status (queuing, running etc) with `$ qstat -u $USER`
- If your job is not running yet, you can see when it is scheduled to go on using `qsched -u $USER`
- Jobs can be cancelled using `qdel <job_id>`

- See also the UsingARC.md file in the repo here: General_Documentation/HPC_&_Command_Line/[UsingARC.md](https://github.com/UoL-Planetary-Modelling/General_Documentation/blob/main/HPC_%26_Command_Line/UsingARC.md)


## 12) Continuing or re-running the same case

- If the model has been successful and you want to carry it on with no changes, change $CONTINUE_RUN to TRUE and resubmit (see section 5).
- If the model has crashed, and you need to make changes, you will need to 'clean' the case, make any edits and then re-set up/re-build (one or both depending on what edits you need to make).
- You can do this using the options `./case.build --clean` and/or `./case.setup --clean`
