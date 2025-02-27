# PlanktothrixCommunity
Metabolic install instructions

Metabolic has programs that only work with Linux, so you will have to use a linux environment. 

Windows
  If you have anaconda or miniconda installed on your windows computer, you will actually need to delete it. It will cause issue if its on both windows and the linux sub system
    	Go to control panel, programs, uninstall programs
	On the computer, search for command prompt and open
	You now need to download Linux on Windows with WSL for programs to run
	Type: “ wsl --install “
	You now need to go to the start menu on the computer and type “ turn windows features on or off “ in the search menu. Once here, if Windows Subsystem for Linux is unchecked, check it, click ok, and computer will restart. If it is already checked, uncheck, click ok, restart. Do the same process again, check it again, and restart. It should now run. 
	Back in command prompt type: “ wsl.exe “ to active the subsystem
	Type: “ wsl.exe --install Ubuntu “
	*Tip* once in wsl.exe, to paste, ctrl v does not work but a simple right click does
	This worked for me but if you keep getting errors, you might need to enable the "Virtual Machine Platform" feature to install Ubuntu
	Now follow the instructions on this webpage line by line for steps 1, 2, and 3 but do cd ~ instead of cd /tmp and also lookup the most recent version on anaconda to install, the one on this page is outdated https://phoenixnap.com/kb/install-anaconda-ubuntu
	conda create -n metabolic python=3.10
	Conda activate metabolic
	Type: “ conda config --set channel_priority flexible “
  	Kept getting this error “Error while loading conda entry point: conda-libmamba-solver (libarchive.so.19: cannot open shared object file: No such file or directory)”
  	So then did this and it fixed “ conda install -n base libarchive -c main --force-reinstall “
	Will now need to have appropriate channels which you probably don’t. Type: “ conda config --add channels conda-forge “ and “ conda config --add channels bioconda “ and “conda config --add channels ursky” “conda config --add channels r” “conda config --add channels main”
	Type: “conda update -n base -c main conda” (if it’s not there yet then conda install conda) then “conda update anaconda” (again if not there yet, then conda install anaconda)
	conda install -n base -c conda-forge mamba
  	Would highly recommend using mamba install from now on instead of conda install
	Now follow steps on this page https://github.com/AnantharamanLab/METABOLIC/issues/27  Check that you have all the appropriate channels installed that are listed on the page.  (warning, installing everything on this page takes quite a bit of time)
  	After conda install gtdbtk you will be given options for installing the required database, typing donload-db.sh is the easiest option by far. This process takes around 3 days so be patient, its a big database. 
    	Run: gtdbtk check_install to make sure its working properly
  	When installing r-tidyverse, it kept aborting because the computer was taking too long. I wound up needing to increase the wsl memory allocation, if this happens do these, chatgpt can also help with 
    	Outside of the wsl command, I opened a new powershell window and typed: notepad $env:USERPROFILE\.wslconfig
    	That will open the notepad application, then you’ll type copy and paste this:
[wsl2]
memory=6GB  # Adjust based on available RAM
swap=4GB
    	Click File → Save As...
    	Set Save as type to All Files
    	Save as .wslconfig in C:\Users\YourUsername\ (replace YourUsername with your actual username)
    	Then back in the powershell with wsl type: wsl --shutdown
    	Then: wsl
    	Then cd ~ then conda activate metabolic
  	Even tho bioperl seemed to install fine with mamba install, it still wasn’t working when I ran the metabolic test files, wound up having to do: cpanm --force Bio::Perl (literally nothing else worked)
	Also need: conda install samtools, conda install minimap2, conda install r-dplyr, conda install r-reshape2
	git clone https://github.com/AnantharamanLab/METABOLIC.git
	cd METABOLIC
	bash run_to_setup.sh
	Use these to test depending on which program you are interested in. As a note, metabolic-c does everything g does plus some, so no need to do both if you plan to use c. If you plan to use g, then just do g, as c takes longer. “ perl METABOLIC-G.pl -test true “ and “ perl METABOLIC-C.pl -test true “


In future when opening command prompt, type wsl.exe to activate the subsystem then cd ~ then conda activate metabolic then cd METABOLIC
