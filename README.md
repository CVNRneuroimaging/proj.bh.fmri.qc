Below: notes from my work supporting Ben Hampstead's ad hoc requests for FMRI quality control.


1. SCOPE OF WORK
================
Perform FBIRN QC on a few of Ben's task-based FMRI runs.


1.1. original request
---------------------
Ben's original request (F, 20140502): Would you mind running some QC on AM80 Encoding 1 & Encoding 2 (CSI server)? I'm getting unusual errors (
// ERROR: a or b too big, or MAXIT too small in betacf) when I run the single study GLM (for each run). It looks like there's less than 1mm movement in any direction for most of the runs, so I'm worried that something was funky w/ the scanner. A couple other patients have similar issues from that same time period AM82 (again just looking at E1 & E2 right now)...


1.2. task list
---------------
- DONE: get session identifiers & dicom location
- DONE: download dicoms
- DONE: generate FBIRN quality control report for each session
- DONE: review qcReports
- DONE: advise Ben


2. PROJECT NOTES
=======================

2.1. contacts and reference numbers
----------------------------------------------
n/a


2.2. budgets and deadlines
---------------------------------------
n/a 


2.3. file paths and URLs
------------------------
- this repository's public browsable URL: https://github.com/stowler/proj.bh.fmri.qc
- zip file containing all qcReports and code: https://github.com/stowler/proj.bh.fmri.qc/archive/master.zip
  - unzip, and then open the `index.html` file in each session's QC folder to view the QC results as a web page
  - does not include nifti files (too large)
- work was performed off-server in `stowler-local@pano.birc.emory.edu:~/temp-BH-QC`


2.4. software notes
--------------------------------------------------------------------------
PLATFORMS AND VERSIONS:

- `Linux pano.birc.emory.edu 3.2.0-53-generic #81-Ubuntu SMP Thu Aug 22 21:01:03 UTC 2013 x86_64 x86_64 x86_64 GNU/Linux`
- FBIRN QA tools: `bxh_xcede_tools-1.10.7-lsb30.x86_64`
- fslreorient2std from FSL version 5.0.4


COMMAND/GUI NOTES AND IDIOSYNCRASIES:
  - n/a


2.5. participant and session IDs (non-sensitive)
-------------------------------------------------
Sessions of interest per Ben. Task-based FMRI timeseries with five runs per session:

```
participantID,sessionID
AM53,E1
AM53,E2
AM80,E1
AM80,E2
AM82,E1
AM82,E2
AM84,E1
```


3. PROJECT LOG
=================================================
(ordered recent to older)

T, 20140506
---------------
- created github repo
- added Friday's QC scripts to repo
- adapted Friday's QC scripts for dicoms in non-MoCo folders (e.g., "Encode#2Run1_2")
- executed non-MoCo QC
- found that the hash symbol in input folder names screws up the FBIRN scripts
- removed the hash symbol from folder names and scripts, executed again
```
stowler-local@pano:~/temp-BH-QC$ du -sh acqfiles/*noMoCo
353M    acqfiles/AM53-E1-taskfmri.noMoCo
354M    acqfiles/AM53-E2-taskfmri.noMoCo
355M    acqfiles/AM80-E1-taskfmri.noMoCo
357M    acqfiles/AM80-E2-taskfmri.noMoCo
357M    acqfiles/AM82-E1-taskfmri.noMoCo
357M    acqfiles/AM82-E2-taskfmri.noMoCo
354M    acqfiles/AM84-E1-taskfmri.noMoCo
stowler-local@pano:~/temp-BH-QC$ du -sh qcReports/*noMoCo
12M     qcReports/AM53-E1-taskfmri.noMoCo
12M     qcReports/AM53-E2-taskfmri.noMoCo
12M     qcReports/AM80-E1-taskfmri.noMoCo
12M     qcReports/AM80-E2-taskfmri.noMoCo
12M     qcReports/AM82-E1-taskfmri.noMoCo
12M     qcReports/AM82-E2-taskfmri.noMoCo
12M     qcReports/AM84-E1-taskfmri.noMoCo
```
- this work required manual per-session script edits due to input file naming, so after execution I confirmed that I didn't make any manual entry errors:
  - confirmed no mis-match between script name and dicom source dir: `grep "dicomParentDir\=\~" *.sh`
  - confirmed no mis-match between script name and participant ID: `grep "participant\=A" *.sh`
  - confirmed no mis-match between script name and session ID: `grep "session\=E" *.sh`
  - confirmed no mis-match between script name and sequence group label : `grep "sequences\=task" *.sh`
  - confirmed no mis-match between script name and dicom child dirs: `grep "^fmriSeriesList\=" *.sh`
- email to Ben: Try processing the error sessions from the non-MoCo files. Console's motion correction seems to introduce strange smoothness characteristics (screenshot: http://note.io/1iotflT)


F, 20140502
---------------
- received Ben's reqeust for QC
- downloaded dicoms from CSI: /projects/adrcol/[sessionID]
- adapted existing QC script, executed for dicoms in MoCo folders per Ben:
```
stowler-local@pano:~/temp-BH-QC$ du -sh acqfiles/*taskfmri
355M    acqfiles/AM53-E1-taskfmri
355M    acqfiles/AM53-E2-taskfmri
356M    acqfiles/AM80-E1-taskfmri
359M    acqfiles/AM80-E2-taskfmri
358M    acqfiles/AM82-E1-taskfmri
358M    acqfiles/AM82-E2-taskfmri
357M    acqfiles/AM84-E1-taskfmri
stowler-local@pano:~/temp-BH-QC$ du -sh qcReports/*taskfmri
12M     qcReports/AM53-E1-taskfmri
12M     qcReports/AM53-E2-taskfmri
12M     qcReports/AM80-E1-taskfmri
12M     qcReports/AM80-E2-taskfmri
12M     qcReports/AM82-E1-taskfmri
12M     qcReports/AM82-E2-taskfmri
12M     qcReports/AM84-E1-taskfmri
```
- sent Ben screenshots of outlier percentages:
  - The QC data don't look great, but I have seen worse. Here's a link to a screenshot of the %outliers in each volume (note where I highlighted the high spots on the y-axis): http://note.io/1kCRwZK . I'm not seeing that AM84-E1 looks significantly different than the others. 53's outliers don't look that different from the dysfunctional sessions: http://note.io/1ncoK2W
