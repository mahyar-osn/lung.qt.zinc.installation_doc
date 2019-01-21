# A general instruction for setting up Opencmiss.Zinc API, PyQt (4 or 5), PySide, and some other Python package tools for Python 2.7.


## Python and Virtualenvironment
It is always best practice to run Python applications within a virtualenv.
We will go through virtualenv setup on different platforms. 
Please note that you will need Python and PIP installed on your machines. If you don't know how to get them installed, please use Google! 

#### Virtualenv Setup
In a terminal or a command prompt install virtualenv:
	
	pip install virtualenv
	
Create a virtualenv:

	virtualenv <virtualenv-name>
	
Activate the virtualenv

	source <path-to-virtualenv-name>/bin/activate
	
The virtualenv should now be setup and running. We proceed to actual installation of opencmiss.zinc and the rest of packages. 

NOTE: Make sure your virtualenv is still active while following the steps below.

### Install Zinc API:

First of all, make sure you have the required software for OpenCMISS.ZINC SDK installed on your machine. They are:

- git
- python-numpy
- cmake (recommended but not essential for zinc)
- mpich (recommended but not essential for zinc)

To install these packages you could simply do: 

	sudo apt-get install git cmake mpich python-numpy

on ubuntu or similar commands on other platforms.

Now download the OpenCMISS SDK from: 

	http://opencmiss.org/downloads.html

Make sure you download the correct installation depending on the OS of your machine. 

Create a directory and name it opencmiss-sdk

	mkdir opencmiss-sdk

Unzip/untar the content of the downloaded SDK into the opencmiss-sdk directory that you just created.

If you are using Windows, open a command prompt type:

	pip install <SDK_DIR>\lib\pythonX.Y\(Release|Debug)\opencmiss.zinc
	
On Ubuntu, open a terminal and type:

	pip install -e <SDK_DIR>/lib/pythonX.Y/(Release|Debug)/opencmiss.zinc

And on OS X, open a terminal and type:

	pip install <SDK_DIR>/lib/python2.7/Release/opencmiss.zinc

Note that on OS X, if you have created the virtualenv using the --sytem-site-package option, the you would need to do:

	pip install --user <SDK_DIR>/lib/python2.7/Release/opencmiss.zinc

Here, SDK_DIR is the installation root of your SDK, the X, and Y are placeholders for the major and minor version of Python that the bindings have been built for. The major and minor version number defined in the path must match the major and minor version number of the Python intreperter in use, and (Release|Debug) refers to the build type.

IMPORTANT NOTE: The current OpenCMISS SDK installation for Mac OS X is not behaving correctly. For this reason the work around is to include the following as environment variables:

	export PYTHONPATH=<path-to-virtualenv-name>/lib/python2.7/site-packages/opencmiss/zinc:$PYTHONPATH
	
	export DYLD_LIBRARY_PATH=<path-to->opencmiss-sdk/lib


Here, path-to-opencmiss-sdk is the path to the opencmiss-sdk directory that you created above.

### Install ZincPythonTools:

	pip install git+https://github.com/OpenCMISS-Bindings/ZincPythonTools

### Install PySide and PyQt:
On Ubuntu these packages can be installed by:

	sudo apt-get install qtcreator python-pyqt5 pyqt5-dev-tools
	pip install PySide

On Mac OS X, the easiest way is to download and install PyQtX package (which is the PyQt binary for Mac OS X) from [here](https://sourceforge.net/projects/pyqtx/). This will install the version 4 of PyQt. Then you should specify the PYTHONPATH with:

	export PYTHONPATH=/usr/local/lib/python2.7/site-packages:$PYTHONPATH

For PySide installation on Mac OS X, you should be able to simply install via:

	pip install -U PySide
	
If this didn't work, you may have to build PySide from the source code. You could do this by downloading PySide tar file from [here](https://pypi.python.org/packages/source/P/PySide/PySide-1.2.4.tar.gz). Then untar the file:

	tar -xzvf PySide-1.2.4.tar.gz
	
cd into the directory and build it by:

	python setup.py bdist_wheel
	
This will take a while usually around 30-40 minutes. Once done, verify that the `dist` directory is created and it has PySide wheel in it:

	cd dist
	ls
	
You should be able to spot a `.whl`. Now install PySide using:

	pip install PySide-1.2.4-cp27-cp27m-macosx_10_12_x86_64.whl

Once installation is done, you need to setup the correct DYLD_LIBRARY_PATH for PySide. 

### Installing other packages (specific to Lung group):

Install Scaffoldmaker using:

	pip install git+https://github.com/ABI-Software/scaffoldmaker.git
	
Scaffoldmaker is used for finite element mesh creation and manipulation within Zinc. It has nice functions that are lepful for building mesh, evaluating fields, fitting, etc.

Install Enum for python 2.7

	pip install enum34
	
Install lungsim:
	
	git clone https://github.com/LungNoodle/lungsim
	mkdir lungsim-build
	cd lungsim-build
	cmake ../lungsim
	make
	pip install -e src/bindings/python

### Test an example

Clone the following repo to test and see if everything works fine:

	git clone https://github.com/tdewolff/grow_airway_tree.git
	cd grow_airway_tree
	python run.py
	
You should see a GUI pop up looking similar to the image below:
	







































