# tensorflow-from-source-avx2-windows
A guid to compile tensorflow from source with avx2 on windows

This is a guid for compiling tensorflow from source with avx2 instruction set. 
I have uploaded the whl file, which I have compiled using this guid as proof of my own guid.
it took me 2 hours to prepare everything + 3 hours to compile.
I did this for benchmarking purposes. I want to benchmark the avx2 capability of cpus.

The required STEPS:

1. Download Python 3.6.8 64-bit release for Windows from https://www.python.org/downloads/release/python-368/

2. Install the python 3.6.8 and choose add python to environment variable path during the installation

3. Install Visual C++ Build Tools 2017(simpler to install community version of Visual studio 2019 or 2017)
    Microsoft Visual C++ 2017 Redistributable
	Microsoft Build Tools 2017
    there is a very good guid for installing the rquired packages under step 3 in following links 
	https://medium.com/@amsokol.com/update-2-how-to-build-and-install-tensorflow-gpu-cpu-for-windows-from-source-code-using-bazel-61c26553f7e8
	
4. install bazel 0.26.1
   download the bazel exe file from https://github.com/bazelbuild/bazel/releases/tag/0.26.1
   rename it to bazel.exe and put it in a folder on c drive
   add the path to windows enviroment variable path (example--> C:\tools\bazel)

5. install mysys2 64 bit from https://www.msys2.org/
   download the 64 bit version and install it like the instruction on the page.(Example--> C:\tools\msys64)
   update the packages by "pacman -Syu" then close the windows
   open msys2 64bit again and run "pacman -Su"
   install prequisits for bazel by "pacman -S zip unzip patch diffutils git"
   
6. set mysys2 enviroment variables
BAZEL_SH:
    Go to Start Menu > Settings.
    Find the setting “Edit environment variables for your account”
    Look at the list on the top (“User variables for <username>”), and click the “New…” button below it.
    For “Variable name”, enter BAZEL_SH
    Click “Browse File…”
    Navigate to the MSYS2 directory, then to usr\bin below it.
    For example, this might be C:\tools\msys64\usr\bin on your system. 
    Select the bash.exe or bash file and click OK
    The “Variable value” field now has the path to bash.exe. Click OK to close the window.
    Done.

BAZEL_VC
	Go to Start Menu > Settings.
	Find the setting “Edit environment variables for your account”
	Look at the list on the top (“User variables for <username>”), and click the “New…” button below it.
	For “Variable name”, enter BAZEL_VC
	Click “Browse Directory…”
	Navigate to the VC directory of Visual Studio.
	For example, this might be C:\Program Files (x86)\Microsoft Visual Studio\2019\Community\VC on your system, if you have installed the community version 2019.
	Select the VC folder and click OK
	The “Variable value” field now has the path to VC. Click OK to close the window.
	Done.
	
7. Install tensorflow dependencies:(run cmd as administrator)
	pip3 install six numpy wheel
	pip3 install keras_applications==1.0.6 --no-deps
	pip3 install keras_preprocessing==1.0.5 --no-deps
   
8. Download the TensorFlow source code
	git clone https://github.com/tensorflow/tensorflow.git
	go to directory where tensorflow source code is cloned
	python ./configure.py
	answer the question as default by pressing enter but two:
	- Do you wish to build TensorFlow with CUDA support? [y/N]: N
	- Please specify optimization flags to use during compilation when bazel option "--config=opt" is specified [Default is /arch:AVX]: /arch:AVX2

	wait till it is over
	
9. run bazel build with right flags
	bazel build --config=opt //tensorflow/tools/pip_package:build_pip_package

10. Post build 
	after the Build is completed successfully, make a folder for whl-output outside of source directory(Example --> D:\Development\whl-output)
    build the whl file and save the output in whl-output folder. inside the source folder run:
		bazel-bin\tensorflow\tools\pip_package\build_pip_package path\to\whl-output)
	install the package: (run cmd as administrator)
		pip3 install path\to\whl-output\tensorflow-version-cp36-cp36m-win_amd64.whl
