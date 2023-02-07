## HOW TO CREATE A BIDS COMPATIBLE DATASET (using dcm2bids)

A tutorial is offered on the [dcm2bids github page](https://unfmontreal.github.io/Dcm2Bids/docs/tutorial/first-steps/)

#### Before to start let's install dcm2bids and its dependencies
[Here](https://unfmontreal.github.io/Dcm2Bids/docs/get-started/install/) all the instruction to install dcm2bids and its dependencies.

After following the instructions you arrive to the last 2 steps:
1. Activate dcm2bids
diff'''
@@ciao@@
diff'''

3. Verify that dcm2bids works


Now  let’s start to use dcm2bids to bidsify your data: 

-	Create a folder for your BIDS_project (e.g.”my_BIDSproject”);
-	Open the terminal >> conda activate dcm2bids
-	Cd in the folder e.g >> cd /Users/stefaniamattioni/Documents/data/my_BIDSproject
-	(To check your current working directory type: pwd )
-	Create the typical scaffold for the BIDS structure >> dcm2bids_scaffold (this command is only creating a structure of empty folders, you need to do that only for your sub1).
-	Copy into the “sourcedata” folder the source data acquired from the scanner: e.g.”DCM-GE_SaintLuc”.
-	 
-	Convert the dicom files to nifti files and create the sidecar Jason file for each file: on the terminal type>> dcm2bids_helper -d sourcedata/[NameOfYourSourceDataFolderGoesHere] (this command will create a temporary folder “temp_dcm2bids” in which all the converted files + json files are stored). 

Note that if you already ran the conversion on one or more subject and you did not empty the temporary folder “tmp_dcm2bids”, you will get this error when running this step:
dcm2bids_helper: error: Output directory /Documents/data/my_BIDSproject/tmp_dcm2bids/helper isn't empty, so some files could be overwritten or deleted.
Rerun the command with --force option to overwrite existing output files.

You can delete the temporary folder after the conversion of each subject. If you are not sure if you did it well and you would like to keep this folder, maybe change the folder name (e.g. “tmp_dcm2bids_sub01”) and move it in another folder. Once you have the “tmp_dcm2bids” empty folder in your project folder you can re-run the command e.g. >>dcm2bids_helper -d sourcedata/[NameOfYourSourceDataFolderGoesHere]


-	Prepare the configuration file:
•	This is a json file where you specify which nifti file you want to convert and which name to assign them. 
•	Here you find an example of the configuration file for my experiment, in which I wanted to select and convert: one anatomical file, 2 functional for a localizer task and 12 functional for an MVPA-sign language task.  
•	You can check that your json file is valid on an online validator: e.g   https://jsonlint.com.
•	A good location for the configuration file is in the ‘code’ folder (e.g: my_BIDSproject/code/dcm2bids_config.json)

 


-	Once your configuration file is ready you can run the dcm2bids on the terminal: dcm2bids -d path/to/source/data -p subject_id -c path/to/config/file.json
The exact command I used for my project is: dcm2bids -d sourcedata/DCM_GE_SaintLuc/sub-HLS03  -p HLS03 -c code/dcm2bids_config.json
-	Running this command you should create a folder in your project folder with the subject ID name (in my case it will be a folder named: sub-HNS02) in which you should find the data you specified in your configuration file now converted in BIDS format. 
-	In addition, you will find also in the temporary folder a folder named as the subject ID in which you can find all the temporary files that have NOT been converted into BIDS. 
-	If you have functional data with a task (no resting-state) you should also include in the BIDS data folder a .tsv file for each run including the onset and duration of each events. Find here an example for one of my functional run (localizer task). 
-	Finally use an online BIDS validator to check that your data structure is now BIDS compatible. 

