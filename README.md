# Pixel gain calibration

Code to run the gain calibration and obtain pedestals (offsets) and gains (slopes) per pixel.


## Software setup

Prepare your working directory with CMSSW

```
export SCRAM_ARCH=slc7_amd64_gcc820
cmsrel CMSSW_11_2_0
cd CMSSW_11_2_0/src
cmsenv
git clone https://github.com/CMSTrackerDPG/SiPixelTools-GainCalibration.git SiPixelTools/GainCalibration
scram b -j 8
cd SiPixelTools/GainCalibration/test
```

## Running the code

To run, the syntax is ./Run.sh -create 2381 inputfolder outputfolder, e.g

```
./Run.sh -create 2381 /store/group/dpg_tracker_pixel/comm_pixel/GainCalibrations/Phase1/Run_2381  /store/group/dpg_tracker_pixel/comm_pixel/thaarres/
```
To run from and store to default Phase1/Run_* directory, which is what you will do for any "real" gain calibration, simply do
```
./Run.sh -create 2381
```

This creates a folder with a config file storing information about input/output folders and creates the job scripts for the submission (from the template).
If you want to run over BPIX only, change the module input array here in L622 or Run.sh

```
./Run.sh -submit 2381 #submit
./Run.sh -resubmit 2381 #resubmit if fails (all jobs MUST finish!)
```

You can check the status of your jobs with
```
bjobs | grep cmscaf1nw
```
Now, hadd the output
```
./Run.sh -hadd 2381
```
and create a summary pdf
```
./Run.sh -summary 2381
```
Then create a tar file with the output and print some output to be posted at the TWiki (https://twiki.cern.ch/twiki/bin/viewauth/CMS/SiPixelGainCalibrationDoc)
```
./Run.sh -twiki 2381
```

The last step is to produce the payload (a database object)
```
./Run.sh -payload 2381 YEAR VERSION
```

where YEAR is the year the calibration was taken and VERSION is the number of payloads produced in that year.
 
After this is done, please notify the following people of the location of the database object:

Currently (this list might change)

Danek Kotlinski <danek.kotlinski@psi.ch>  
Tamás Vámi <vami.tamas@wigner.mta.hu>  
Tanja Susa <Tatjana.Susa@cern.ch>

