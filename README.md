By Stephanie Hachem 
May 27, 2020

Description: Use Vannucci et al. (2017) and Schneider's () muscle spindle models to predict muscle spindle afferent output given muscle length as an input. Predictions are compared with experimentally-measured muscle spindle afferents from scientific article figures and datasets. The associated paper is at [Teknos] and [GMU], and a flowchart of the programs is attached.

-Install Python 3 (Anaconda3 is a great way to install this), Python 3 packages (namely, Numpy, Matplotlib, Pathlib, OpenCV2), MATLAB (R2019b 9.7.0.1247435 or higher), Simulink, and Oracle Virtualbox (<https://www.virtualbox.org/wiki/Downloads>) on Windows (Windows 10 or higher)
-If you want to use the Cygwin .sh files (which allows you to run these files much faster and with a lot less work than if you don't use the .sh files), install Cygwin (<https://cygwin.com/install.html>) on Windows
-Make an Oracle (64-bit) Linux Server 7.5 Red Hat Enterprise virtual machine (VM) (Linux Kernel Version 4.1.12-124.15.2.el7uek.x86_64) in VirtualBox, 
-Install Python, Neural Simulation Tool (NEST) (https://nest-simulator.readthedocs.io/en/stable/getting_started.html>, and NEST's Python interface (PyNEST) <https://nest-simulator.readthedocs.io/en/stable/tutorials/> on the VM (this may be difficult.. don't give up)

If you installed Cygwin and therefore can run these files the simpler and faster way:
	See fiveClickVideo.mp4 for a demonstration of running through the below steps. (fiveClickVideo_long.mp4 is fiveClickVideo.mp4 without the time waiting for the simulations being sped up.) (Note that in the videos, the simulation of Blum's data with Vannucci's model (line 7 in OLshared_revisedSkeleton/runMe4_inLinuxVM.sh) isn't included, since that would exceed the time I can simulate with the screen recording software I'm using.)  
	
	-In Windows, run runMe3_inWindows.sh
		bash runMe1_inWindows.sh 
		type in some integer/number of your choice when Cygwin says "Please enter the supp number (a single integer/letter):" (at the very beginning) (in the video, I used "p")
	-Open VirtualBox and ensure the VM is shut down 
	-Add OLshared_supp<your integer/letter>/ as a shared folder with 'automount' checked 
	-Start up the VM 
	-Log in 
	-Open Terminal 
	-Run 
		cd /media/OLshared_supp<your integer/letter>/
		bash runMe2_inLinuxVM.sh 
	-In Windows, run runMe3_inWindows.sh	
		bash runMe3_inWindows.sh 
		type in the same integer/number of your choice when Cygwin says "Please enter the supp number (a single integer/letter):" 
	-In the VM, run runMe4_inLinuxVM.sh
		bash runMe4_inLinuxVM.sh 
	-In Windows, run runMe5_inWindows.sh
		bash runMe5_inWindows.sh 
		type in the same integer/number of your choice when Cygwin says "Please enter the supp number (a single integer/letter):" 

If you didn't/can't install Cygwin or, for some reason, you want to manually do what the 5 .sh files do:

	1. Copy a folder skeleton (OLshared_revisedSkeleton) 
	2. Rename the new folder to <new name>
	3. In the new folder, in the below files, replace all instances of "_suppX" with <new name> (in the video, I rename the folder to OLshared_supp1, so I replace all instances of "_suppX" with "_supp1")
		main.py
		prepareBlumDataForFalotico.m
		prepareDigitizedDataForSchneider.m
		useDataForSchneider.m

	4. Open main.py of the new path in Spyder (or some other code-editing software) (this step is necessary, but you're only likely to accidentally NOT do this if you've made several main.py files) 
	5. Uncomment the Python multiline commented lines at the top (lines 27-35, inclusive), and comment lines 16-24, inclusive 
	6. Edit main.py to have doDigitizedData==True, doDigitize==True, and the rest of lines 27-35 (inclusive) false	
	7. Run main.py 

	8. Make sure the VM is shut off
	9. Share the new folder you made in step 1 with the VM; check the 'automount' checkbox
	10. Turn on the VM
	11. Log in to whatever user has a working PyNEST installation (in the video, that user is root)
	12. Edit main.py to have doDigitizedData==True, doSimulate_Vannucci==True, and the rest of lines 27-35 (inclusive) false
	13. In the VM's terminal, run
		cd /media/sf_<new name>/ (substitute the name of the new folder named in step 1 for <new name>)
		python main.py 
	14. Move article 6 figure 1 files and article 6 figure 6 files from tempp/ to the correct folders by running these commands in Windows Command Prompt (although, in the video, I run these commands in Cygwin since Windows Command Prompt doesn't allow me to access files on an external hard drive, which is where I have the files):	
		cd F:\InteruserWorkspace_fDrive\interuser\getSpindleDataModels\data\digitizeSpindleData\<new name>\
		mv tempp\a_*.png digitizedData\data\6\1\VannucciInputPng\
		mv tempp\a_*.dat digitizedData\data\6\1\VannucciOutputDat\
		mv tempp\article6fig6cat*.png digitizedData\data\6\6\VannucciInputPng\
		mv tempp\article6fig6cat*.dat digitizedData\data\6\6\VannucciOutputDat\

	15. Edit main.py to have doDigitizedData==True, doWriteSortedLenFile==True, and the rest of lines 27-35 (inclusive) false
	16. In Windows, run main.py 

	17. Get the .txt files containing the filenames to be used by Schneider's model ready by replacing old paths in <new name>/digitizedData/MATLABinputCsvFilenamesTxt/*.txt files with new paths

	18. Open useDataForSchneider.m and prepareDigitizedDataForSchneider.m of the new paths in MATLAB (or some other code-editing software) (this step is necessary, but you're only likely to accidentally NOT do this if you've made several useDataForSchneider.m/prepareDigitizedDataForSchneider.m files)
	19. In MATLAB's GUI, run 
		addpath 'F:\InteruserWorkspace_fDrive\interuser\getSpindleDataModels\data\digitizeSpindleData\<new name>'
		prepareDigitizedDataForSchneider()
	20. In F:\InteruserWorkspace_fDrive\interuser\getSpindleDataModels\data\digitizeSpindleData\<new name>\digitizedData\SchneiderInputMat\, rename files which don't end with .mat to end with .mat 
	21. In MATLAB's GUI, run 
		useDataForSchneider(false,true)
	22. In MATLAB's GUI, run 
		prepareBlumDataForFalotico()
	23. In MATLAB's GUI, run 
		useDataForSchneider(true,false)

	24. Edit main.py to have doBlum==True, doSimulate_Vannucci==True, and the rest of lines 27-35 (inclusive) false
	25. In the VM's terminal, run main.py 

	26. Edit main.py to have doDigitizedData==True, doPlot==True, and the rest of lines 27-35 (inclusive) false
	27. In Windows, run main.py 
	28. Edit main.py to have doDigitizedData==True, doGetStats==True, and the rest of lines 27-35 (inclusive) false
	29. In Windows, run main.py 
	30. Edit main.py to have doBlum==True, doPlot==True, and the rest of lines 27-35 (inclusive) false
	31. In Windows, run main.py 
	32. Edit main.py to have doBlum==True, doGetStats==True, and the rest of lines 27-35 (inclusive) false
	33. In Windows, run main.py 			
	34. In Windows, run compareStats.py 
