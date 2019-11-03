# ORB-SLAM

This repository describes the installation and usage of ORB-SLAM in a mobile robot, courtesy of Gautham Ponnu & Jacob George who implemented this project for their MEngg Design Project @ Cornell. Find their thesis report [here](https://courses.ece.cornell.edu/ece6930/ECE6930_Spring16_Final_MEng_Reports/SLAM/Real-time%20ROSberryPi%20SLAM%20Robot.pdf).

## Required Packages

We use Ubuntu 16.04.5 LTS. For making use of [ORB-SLAM2](https://github.com/raulmur/ORB_SLAM2), we will need the following packages:

* [Pangolin](https://github.com/stevenlovegrove/Pangolin)
* [OpenCV](https://opencv.org/)
    * OpenCV install instructions described below. We use version 3.4.6.
* [Eigen](http://eigen.tuxfamily.org/index.php?title=Main_Page)
    * Latest stable Eigen 3.2.x version is recommended. We use version 3.2.10.

The steps to install Pangolin and Eigen are described in each website/repository. 

### OpenCV Installation
For OpenCV, we'll first need virtualenv and virtualenvwrapper. In the terminal, type

	sudo pip install virtualenv virtualenvwrapper
	sudo apt-get update

Go to your bashrc file `gedit .bashrc` and type the following:

	export VIRTUALENVWRAPPER_PYTHON=/usr/bin/python3
	export VIRTUALENVWRAPPER_VIRTUALENV=/usr/local/bin/virtualenv
	export WORKON_HOME=$HOME/.virtualenvs
	source /usr/local/bin/virtualenvwrapper.sh

Now, close the terminal and open a new one. We'll create a new virtual environment (called cv) to install openCV. 
	
  `mkvirtualenv cv -p python3`

If you now type `workon cv`, your terminal display will start with the cv virtual environment like

`(cv) username@desktopname:~$`

Here, we'll install a few important packages:
	
  `pip install wheel numpy scipy matplotlib scikit-image scikit-learn ipython dlib`

To exit the virtual environment, type `deactivate`. 

Go to the official [OpenCV Releases](https://opencv.org/releases/) page and download the source for version 3.4.6. Then, download an extra package called opencv\_contrib-3.4.6 from [here](https://github.com/opencv/opencv_contrib/releases). Place these compressed folders in the home folder and extract them. 

Finally, we'll need to install the dependencies for openCV and then build openCV from source. For this, download the file installcv2.sh. In this file, you need to change <USERNAME> to your machine's username in the following line that's a part of cmake command:

  `-D OPENCV_EXTRA_MODULES_PATH=/home/<USERNAME>/opencv_contrib-3.4.6/modules \`

Save the file. Open a new terminal and type the following:

  `chmod +777 installcv2.sh`
	`bash installcv2.sh`

This will take some time to execute (~20-30 mins). 


All the required packages are now installed. Test if the installation works by executing the Monocular Examples (TUM, KITTI, EUROC) as described in the [README](https://github.com/raulmur/ORB_SLAM2) of the ORB-SLAM2 directory.
