# Deploying ORB-SLAM in a mobile robot

This repository describes the installation and usage of ORB-SLAM in a mobile robot using Raspberry Pi and Pi Camera V2, courtesy of Gautham Ponnu & Jacob George who implemented this project for their MEngg Design Project @ Cornell. Find their thesis report [here](https://courses.ece.cornell.edu/ece6930/ECE6930_Spring16_Final_MEng_Reports/SLAM/Real-time%20ROSberryPi%20SLAM%20Robot.pdf).

## Required Packages

I use Ubuntu 16.04.5 LTS. For making use of [ORB-SLAM2](https://github.com/raulmur/ORB_SLAM2), we will need the following packages:

* [Pangolin](https://github.com/stevenlovegrove/Pangolin)
* [OpenCV](https://opencv.org/)
    * OpenCV install instructions described below. I use version 3.4.6.
* [Eigen](http://eigen.tuxfamily.org/index.php?title=Main_Page)
    * Latest stable Eigen 3.2.x version is recommended. I use version 3.2.10.

The steps to install Pangolin and Eigen are described in the respective website/repository. 

### OpenCV Installation
For OpenCV, we'll first need virtualenv and virtualenvwrapper. In the terminal, type

	sudo pip install virtualenv virtualenvwrapper  
	sudo apt-get update`

Go to your bashrc file `gedit .bashrc` and paste the following:

	export VIRTUALENVWRAPPER_PYTHON=/usr/bin/python3
  	export VIRTUALENVWRAPPER_VIRTUALENV=/usr/local/bin/virtualenv
  	export WORKON_HOME=$HOME/.virtualenvs
  	source /usr/local/bin/virtualenvwrapper.sh

Now, close the terminal and open a new one. We'll create a new virtual environment (called cv) to install openCV. 
	
	mkvirtualenv cv -p python3


If you now type `workon cv`, your terminal display will start with the cv virtual environment like

 	(cv) username@desktopname:~$

Here, we'll install a few important packages:
	
	pip install wheel numpy scipy matplotlib scikit-image scikit-learn ipython dlib

To exit the virtual environment, type `deactivate`. 

Go to the official [OpenCV Releases](https://opencv.org/releases/) page and download the source for version 3.4.6. Then, download an extra package called opencv\_contrib-3.4.6 from [here](https://github.com/opencv/opencv_contrib/releases). Place these compressed folders in the home folder and extract them. 

Finally, we'll need to install the dependencies for openCV and then build openCV from source. For this, download the file installcv2.sh. In this file, you need to change <USERNAME> to your machine's username in the following line that's a part of cmake command:

	-D OPENCV_EXTRA_MODULES_PATH=/home/<USERNAME>/opencv_contrib-3.4.6/modules \

Save the file. Open a new terminal and type the following:

	chmod +777 installcv2.sh  
 	bash installcv2.sh

This will take some time to execute (~20-30 mins). 

After openCV is built and installed, go to `/home/<USERNAME>/opencv-3.4.6/build/lib`. Open in terminal and rename the file as
cv2.so by typing

	sudo mv cv2.cpython-35m-x86_64-linux-gnu.so cv2.so

**Note**: This file was called `'cv2.cpython-35m-x86_64-linux-gnu.so'` for me, it might be named differently for you. 

Next, copy this cv2.so file and paste it to `~/.virtualenvs/cv/lib/python3.5/site-packages/`, `/usr/local/lib/python2.7/site-packages`, `/usr/local/lib/python3.5/site-packages`, `/usr/local/lib/python3.5/dist-packages`.

**Note**: If there is no `site-packages` folder in the path `/usr/local/lib/python3.5`, create one using `sudo mkdir site-packages`, then copy paste the cv2.so file to the newly created folder. 

After renaming and performing the copy-paste operations to the respective folders, sym-link the file by typing

	cd ~/.virtualenvs/cv/lib/python3.5/site-packages/  
  	ln -s /usr/local/lib/python3.5/site-packages/cv2.so cv2.so

To check if openCV is working, activate the cv virtual environment and type
  
	python3  
  	import cv2
  	cv2.__version__

The terminal should return `'3.4.6'` which means openCV is functioning properly.

### Building ORB-SLAM2 library and examples
All the required packages are now installed. Now, download the [ORB-SLAM2](https://github.com/raulmur/ORB_SLAM2) repository to your home folder, rename the folder as 'ORB_SLAM2' and then execute:
	
	cd ORB_SLAM2
	chmod +x build.sh
	./build.sh

### Monocular Test
Test if the installation works by executing the Monocular Examples (TUM, KITTI, EUROC). I tested by downloading TUM's [fr1/xyz](https://vision.in.tum.de/data/datasets/rgbd-dataset/download) -- the first dataset under 'Testing and Debugging'. Place the uncompressed folder inside the ORB_SLAM2 folder and execute:

	cd ORB_SLAM2
  	./Examples/Monocular/mono_tum Vocabulary/ORBvoc.txt Examples/Monocular/TUMX.yaml /home/<USERNAME>/ORB_SLAM2/rgbd_dataset_freiburg1_xyz

