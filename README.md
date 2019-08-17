# How to set up tensorflow (cpu and gpu) on your machine (recommended: on virtual environment)
I tried setting up tensorflow for one of my projects and it was worse than what I expected. There were a lot of threads on the internet which I found useuful but unfortunately they did not work for me (don't get me wrong, they all helped me to come up with a clean way to set up tensorflow to work compatibly on my gpu :P). So I decided to create this document for myself and anyone else who wants to set up tensorflow for their gpu but find themselves lost at the beginning. I hope this helps and saves you a couple days of searching on the internet and asking around :). At the end, I will put all the threads which helped me writing this. You may find them useful for your case.
These all worked fine for me and I have a machine with Windows 10, my gpu is NVIDIA GEFORCE RTX 2080 Ti and I set this up on python 3.7. 

If you want to run tensorflow on cpu just go to STEP 12.

## STEP 1. Install python on your machine 
You need to install python on your machine. It has to be 64-bit python, otherwise you will get the error "not a supported wheel on this platform" when trying to install tensorflow package. For example, I have python 2.7, python 3.7 installed on my PC.

## STEP 2. Buy a GPU
Well if you want to set up tensorflow on your gpu you need to have a gpu first. This is the most expensive yet the most straightforward step. For example, I got the NVIDIA GEFORCE RTX 2080 Ti. All the following is assuming that you have a NVIDIA gpu.

## STEP 3. Install drivers for your gpu
I used https://www.geforce.com/drivers to install my drivers. For example, my driver's version is 431.60. You can check your driver's version by right clicking on the screen on your desktop and choosing "NVIDIA control panel".

## STEP 4. Finding the compatible quadruple (tensorflow version, CUDA version, cuDNN version, python version) for your case
This was genuinely the hardest step. I did not know this was the case but I learnt it the hard way. The thing is to set up tensorflow-gpu on your machine you need CUDA and cuDNN and you cannot just install any aritrary version of tensorflow on your PC or download any arbitrary version of CUDA from the website. These four components should be compatible. There is a GREAT github repo (better than tensorflow documentation or any other documentation I found) for this:
* https://github.com/fo40225/tensorflow-windows-wheel

This webstie has all the details you want for this step (and STEP 8). Simply scroll down and find your quadruple :). I chose this setting
* Tensorflow 1.13.1
* CUDA: 10.1.105_418.96
* cuDNN: 7.5.0.56
* Python 3.7

## STEP 4. Finding the compatible quadruple (tensorflow version, CUDA version, cuDNN version, python version) for your case
This was genuinely the hardest step. I did not know this was the case but I learnt it the hard way. The thing is to set up tensorflow-gpu on your machine you need CUDA and cuDNN and you cannot just install any aritrary version of tensorflow on your PC or download any arbitrary version of CUDA from the website. These four components should be compatible. There is a GREAT github repo (better than tensorflow documentation or any other documentation I found) for this:
* [The GREAT github repo](https://github.com/fo40225/tensorflow-windows-wheel)

This webstie has all the details you want for this step (and STEP 8). Simply scroll down and find your quadruple :). I chose this setting
* Tensorflow 1.13.1
* CUDA: 10.1.105_418.96
* cuDNN: 7.5.0.56
* Python 3.7

## STEP 5. Download visual studio express
Honestly, I'm not sure how important this step is or even it is necessary (it is according to [Dr. Joanne Kitson](https://towardsdatascience.com/installing-tensorflow-with-cuda-cudnn-and-gpu-support-on-windows-10-60693e46e781)). So download visual studio express from [here](https://visualstudio.microsoft.com/vs/express/). For ecample, I downloaded visual studio express 2019 latest version at the time of writing.

## STEP 6. Download and install CUDA
According to STEP 4, download the CUDA version you need from [here](https://developer.nvidia.com/cuda-downloads). If you don't want the latest release, go to "Legacy Releases". You then need to Install CUDA, it is very straightforward. Just make sure to remember the install path as you need it for STEP 6. Say our CUDA path is "PATH/TO/CUDA". For example, in my case it was, "C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v10.1".

## STEP 7. Download cuDNN and copy files into CUDA folder
According to STEP 4, download the cuDNN version you need from [here](https://developer.nvidia.com/cudnn). You now have downloaded a zip file, just unzip it and. I call this folder "cuDNN/CUDA" just to be different from "PATH/TO/CUDA" We then need to copy 4 files into the CUDA folder from STEP5. 

* Copy "cuDNN/CUDA/bin/cudnn64_7.dll" to "PATH/TO/CUDA/bin"
* Copy "cuDNN/CUDA/include/cudnn.h" to "PATH/TO/CUDA/include"
* Copy "cuDNN/CUDA/lib/x64/cudnn.lib" to "PATH/TO/CUDA/lib/x64"

## STEP 8. Make sure your environment variables are correct

Make sure CUDA system Variables variables are correct in "Control Panel ->Advanced System settings ->Environment Variables..." (the bottoom half window titled "System Variables"). The correct environment variables should look like this (if not just add them):

* Variable: CUDA_PATH   Value:  C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v10.1
* Variable: NVCUDASAMPLES_ROOT   Value:  C:\ProgramData\NVIDIA Corporation\CUDA Samples\v10.1
* Variable: PATH   one of Value:  C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v10.1\bin
* Variable: PATH   one of Value:  C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v10.1\libnvvp

## STEP 9. Set up virtual environment for your project
Set up a virtual environment for your project. It should use a 64-bit python. I have a document teaching seetting up a virtual env on:

* [How to set up a virtual environment?](https://github.com/SalarAbb/Set-up-virtualenv-for-python)

Let's say your virtual environment is at "PATH/TO/VIRTUALENVS/PROJECT".

## STEP 10. Install correct verison of tensorflow package on your virtual environment
After activiating your virtual environment, you want to install tensorflow. Probably pip install tensorflow-gpu=XXX won't work (for me it didn't). Again, we will use this GREAT github repo at STEP 4 to download the corresponding wheel file (filename.whl). This github repo has all the .whl files for all tensorflow versions (both cpu and gpu). for example the .whl file I used was [here](https://github.com/fo40225/tensorflow-windows-wheel/tree/master/1.13.1/py37/GPU/cuda101cudnn75sse2).

Let's say the .whl file is at "PATH\TO\WHEEL\filename.whl". You just need to do these:
* activate your virtual environment (read [here](https://github.com/SalarAbb/Set-up-virtualenv-for-python)). You will see the ('PROJECT') icon next to your command line.
* go to .whl file folder:
'''
cd "PATH\TO\WHEEL"
'''
* pip install filename.whl

Now tensorflow with gpu is installed on your machine. 

## STEP 11. How to make sure Tensorflow is using your gpu:
Just run the following commands on python. It will shows your gpu model and its memory and you will get the answer. I got this piece of code from
'''
import tensorflow as tf
import os
os.environ['CUDA_VISIBLE_DEVICES'] = '0'

with tf.device('/gpu:0'):
    a = tf.constant([1.0, 2.0, 3.0, 4.0, 5.0, 6.0], shape=[2, 3], name='a')
    b = tf.constant([1.0, 2.0, 3.0, 4.0, 5.0, 6.0], shape=[3, 2], name='b')
    c = tf.matmul(a, b)

with tf.Session() as sess:
    print (sess.run(c))
'''
I hope this works after 10 (10!) STEPS.
## STEP 12. Take a moment to digest how it was easier to set up tensorflow for CPU
To set up tensorflow on your cpu and virtual environment you only need these steps (make sure to create different virtual environments for cpu and gpu version if you would like to test both):
* STEP 1
* STEP 9
* STEP 10


## References that helped me to understand things better and write this
* [A Medium post](https://towardsdatascience.com/installing-tensorflow-with-cuda-cudnn-and-gpu-support-on-windows-10-60693e46e781)
* [The GREAT github repo](https://github.com/fo40225/tensorflow-windows-wheel)
* [A stackoverflow page](https://stackoverflow.com/questions/45316569/how-to-install-tensorflow-on-python-2-7-on-windows)
* [Other stackoverflow post which helped my minor problems, which I unfortunatel forgot their direct pages](https://stackoverflow.com)
