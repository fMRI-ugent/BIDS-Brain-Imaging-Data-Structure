## HOW TO CREATE A BIDS COMPATIBLE DATASET (using dcm2bids)

A tutorial is offered on the [dcm2bids github page](https://unfmontreal.github.io/Dcm2Bids/docs/tutorial/first-steps/)

#### Before to start let's install dcm2bids and its dependencies
[Here](https://unfmontreal.github.io/Dcm2Bids/docs/get-started/install/) all the instruction to install dcm2bids and its dependencies.

After following the instructions you arrive to the last 2 steps (run in the terminal):
1. Activate dcm2bids
```diff
@@ conda activate dcm2bids @@
```
3. Verify that dcm2bids works
```diff
@@ dcm2bids --help @@
```

#### If everything works, let’s start to use dcm2bids to bidsify your data: 

-	Create a folder for your BIDS_project (e.g.”my_BIDSproject”);
-	Activate dcm2bids (if you didn't do that  already) Type in the terminal:
```diff
@@ conda activate dcm2bids @@
```
-	Cd in the project folder e.g >> cd /Users/stefaniamattioni/Documents/data/my_BIDSproject
```diff
@@ [HereGoesThePathTo/myBIDSproject @@
```
-	(If you need to check your current working directory type:
```diff
@@  pwd @@
```
-	Type this line to create the typical scaffold for the BIDS structure (this command is only creating a BIDS compatible structure of empty folders, you need to do that only for your sub1):
```diff
@@  dcm2bids_scaffold @@
```
-	Copy into the “sourcedata” folder the source data acquired from the scanner: e.g.”Dicom-GIfMI-UZ” (you can do this manually).

-	Convert the dicom files to nifti files and create the sidecar Jason file for each data file: on the terminal type
e.g. dcm2bids_helper -d sourcedata/Dicom-GIfMI-UZ

```diff
@@  dcm2bids_helper -d sourcedata/[NameOfYourSourceDataFolderGoesHere] @@
```
this command will create a temporary folder “temp_dcm2bids” in which all the converted files + json files are stored. 

Note that if you already ran the conversion on one or more subject and you did not empty the temporary folder “tmp_dcm2bids”, you will get this error when running this step:

*dcm2bids_helper: error: Output directory /Documents/data/my_BIDSproject/tmp_dcm2bids/helper isn't empty, so some files could be overwritten or deleted.
Rerun the command with --force option to overwrite existing output files.*

You can delete the temporary folder after the conversion of each subject. If you are not sure if you did it well and you would like to keep this folder, maybe change the folder name (e.g. “tmp_dcm2bids_sub01”) and move it in another folder. Once you have the “tmp_dcm2bids” empty folder in your project folder you can re-run the command e.g. >>dcm2bids_helper -d sourcedata/[NameOfYourSourceDataFolderGoesHere].


#### -	Prepare the configuration file:
•	This is a json file where you specify which nifti files you want to convert and which name to assign them. 
•	In this repository you find an example of the configuration file for a previous experiment (not run at GIfMI), in which I wanted to select and convert: one anatomical file, 2 functional files for a localizer task and 12 functional files for an MVPA task.  
•	You can check that your json file is valid on an online validator: e.g   https://jsonlint.com.
•	A good location for the configuration file is in the ‘code’ folder (e.g: my_BIDSproject/code/dcm2bids_config.json)

SOME FURTHER DETAILS ABOUT THE CONFIGURATION FILE:
- In the first step of the convertion, you already converted all the source data from dicom to nifti files. Each of these files represent one MRI acquisition and for each file you created a json file with all the info for that specific acquisition.
However, you do not want all the acquisitions to be  converted into BIDS --> **You use the configuration file to select the files that you want to convert in BIDS.**
- the string you put in **“criteria”** will be the info that will be looked for within the json files, only if there is a match that file will be selected for bids conversion.
![Screenshot 2023-02-14 at 10 48 54](https://user-images.githubusercontent.com/50380749/218699606-ab5065bc-5038-4f72-884b-979fdc6bdf46.png)

- So, in the criteria section you need to put voices that are specific for each sequence --> *“SeriesDescription”* is a good example because it normally defines the name of your sequence. 
- In this example, to select the data from anatomical sequence I selected 2 criteria: *SeriesDescritption* and *EchoTime*. 
  To select the first functional run for localizer task I used only *SeriesDescription*
- Note that the label under SeriesDescription depends on how we named the sequence on the scanner —> this is why it would be good to use always the same name (e.g. Localizer_run1) for each participant. Otherwise at every conversion we will need to adapt this configuration file!

![Screenshot 2023-02-14 at 10 47 01](https://user-images.githubusercontent.com/50380749/218699126-4865adfb-31b1-4465-af1c-5775524286c7.png)


-	Once your configuration file is ready you can run the dcm2bids on the terminal: 
```diff
@@ dcm2bids -d path/to/source/data -p subject_id -c path/to/config/file.json @@
```
-	Running this command should automatically create a folder in your project folder with the subject ID name (e.g: sub-01) in which you should find the data you specified in your configuration file now converted in BIDS format. 
-	In addition, you will find also in the temporary folder a folder named as the subject ID in which you can find all the temporary files that have NOT been converted into BIDS. 
-	If you have functional data with a task (no resting-state) you should also include in the BIDS data folder a .tsv file for each run including the onset and duration of each events. Find here an example for one of my functional run (localizer task). 
-	Finally use an online BIDS validator to check that your data structure is now BIDS compatible. 


#### @Created by Stefania Mattioni. You can contact me at stefania.mattioni@ugent.be for any question.
