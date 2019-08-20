# How to set up tensorflow for GPU and CPU (recommended: on virtual environment)
I tried setting up tensorflow for one of my projects and it was worse than what I expected. There were a lot of threads on the internet which I found useful but unfortunately they did not work for me (don't get me wrong, they all helped me to come up with a clean way to set up tensorflow to work fine with my GPU :P). So I decided to create this document for myself and anyone else who wants to set up tensorflow for their GPU but find themselves lost at the beginning. I hope this helps and saves you a couple days of searching on the internet and asking around :). At the end, I will put all the threads which helped me writing this. You may find them useful for your case.

These steps worked fine for me and I have a machine with Windows 10, my GPU is NVIDIA GEFORCE RTX 2080 Ti and I set this up on python 3.7 (time of writing: 8/17/2019).

If you want to run tensorflow on CPU just go to Step 12.

## Step 1. Install python on your machine 
You need to install python on your machine. It has to be 64-bit python, otherwise you will get the error "not a supported wheel on this platform" when trying to install tensorflow package at Step 10. For example, I have python 2.7 and python 3.7 installed on my PC.

## Step 2. Buy a GPU
Well if you want to set up tensorflow on your GPU you first need to have a GPU. This is the most expensive yet the most straightforward step. Make sure that your NVIDIA GPU will be CUDA-enabled with compute capability 3.5 or higher (see [this](https://developer.nvidia.com/cuda-gpus)). For example, I got the NVIDIA GEFORCE RTX 2080 Ti. All the following is assuming that you have a NVIDIA gpu.

## Step 3. Install drivers for your GPU
I used https://www.geforce.com/drivers to install my drivers. For example, my driver's version is 431.60. You can check your driver's version by right clicking on the screen on your desktop and choosing "NVIDIA control panel".

## Step 4. Find the compatible quadruple (tensorflow version, CUDA version, cuDNN version, python version) for your case
This was genuinely the hardest step. I did not know this was the case but I learnt it the hard way. The thing is to set up tensorflow-GPU on your machine you need CUDA and cuDNN and you cannot just install any aritrary version of tensorflow on your PC or download/install any random version of CUDA/cuDNN from the website. These four components (tensorflow version, CUDA version, cuDNN version, python version) should be compatible. There is a GREAT github repo (better than tensorflow documentation or any other documentation I found, I will call it "the GREAT github repo") for this:
* [The GREAT github repo](https://github.com/fo40225/tensorflow-windows-wheel)

This repo has all the details you want for this step (and Step 8). Simply scroll down and find your quadruple :).For example, I chose these settings:

* Tensorflow 1.13.1
* CUDA: 10.1.105_418.96
* cuDNN: 7.5.0.56
* Python 3.7

## Step 5. Download Visual Studio Express
Honestly, I'm not sure how important this step is or even it is necessary (looks like it is according to [Dr. Joanne Kitson](https://towardsdatascience.com/installing-tensorflow-with-cuda-cudnn-and-gpu-support-on-windows-10-60693e46e781)). So download Visual Studio Express from [here](https://visualstudio.microsoft.com/vs/express/). For example, I downloaded Visual Studio Express 2019 latest version at the time of writing.

## Step 6. Download and install CUDA
According to Step 4, download the CUDA version you need from [here](https://developer.nvidia.com/cuda-downloads). If you don't want the latest release, go to "Legacy Releases". You then need to Install CUDA, it is very straightforward. Just make sure to remember the installation path as you need it for Step 6. Say your CUDA path is "PATH/TO/CUDA". For example, in my case it was, "C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v10.1".

## Step 7. Download cuDNN and copy files into CUDA folder
According to Step 4, download the cuDNN version you need from [here](https://developer.nvidia.com/cudnn). You now have downloaded a zip file, just unzip it; I call this folder "cuDNN/CUDA" just to be different from "PATH/TO/CUDA" We then need to copy 3 files into the CUDA installation path from Step 5:

* Copy "cuDNN/CUDA/bin/cudnn64_7.dll" to "PATH/TO/CUDA/bin"
* Copy "cuDNN/CUDA/include/cudnn.h" to "PATH/TO/CUDA/include"
* Copy "cuDNN/CUDA/lib/x64/cudnn.lib" to "PATH/TO/CUDA/lib/x64"

## Step 8. Make sure your environment variables are correct
Make sure CUDA system variables are correct in "Control Panel ->Advanced System settings ->Environment Variables..." (the bottom half window titled "System Variables"). The correct environment variables should look like this depending on your CUDA version (if not just add them):

* Variable: CUDA_PATH   Value:  C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v10.1
* Variable: NVCUDASAMPLES_ROOT   Value:  C:\ProgramData\NVIDIA Corporation\CUDA Samples\v10.1
* Variable: PATH   one of Value:  C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v10.1\bin
* Variable: PATH   one of Value:  C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v10.1\libnvvp

## Step 9. Set up virtual environment for your project
Set up a virtual environment for your project. It should use a 64-bit python. I have made a document for this:

* [How to set up a virtual environment?](https://github.com/SalarAbb/Set-up-virtualenv-for-python)

Let's say your virtual environment is at "PATH/TO/VIRTUALENVS/PROJECT" for following steps.

## Step 10. Install correct verison of tensorflow package on your virtual environment
After activiating your virtual environment, you want to install tensorflow. Perhaps, pip install tensorflow-gpu=XXX won't work (for me it didn't). Again, we will use the GREAT github repo at Step 4 to download the corresponding wheel file (filename.whl). This github repo has all the .whl files for all tensorflow versions you may need (both CPU and GPU). For example the .whl file I used was [here](https://github.com/fo40225/tensorflow-windows-wheel/tree/master/1.13.1/py37/GPU/cuda101cudnn75sse2).

Let's say the .whl file is at "PATH\TO\WHEEL\filename.whl". You just need to do these:

* Activate your virtual environment (read [here](https://github.com/SalarAbb/Set-up-virtualenv-for-python)). You will see the ('PROJECT') icon next to your command line.
* Go to .whl file folder:
```
cd "PATH\TO\WHEEL"
```
* Install the .whl file with pip:
```
pip install filename.whl
```
Now tensorflow with GPU is installed on your machine. if you don't use python 64 bit you will get an error like ('not a supported wheel on this platform'). Imagine getting this error, after 10 steps including buying a gpu, which was the case for me until I found out the issue.

## Step 11. Make sure Tensorflow is using the GPU
Just run the following commands on python. It will show your GPU model, its memory and you will get the answer:
```
import tensorflow as tf
import os

os.environ['CUDA_VISIBLE_DEVICES'] = '0' # You need to tell CUDA
# which GPU you'd like to use. if you have one GPU probably your GPU is '0'

with tf.device('/gpu:0'):
    a = tf.constant([1.0, 2.0, 3.0, 4.0], shape=[2, 2], name='a')
    b = tf.constant([4.0, 3.0, 2.0, 1.0], shape=[2, 2], name='b')
    c = tf.matmul(a, b)

with tf.Session() as sess:
    print (sess.run(c))
```
I hope this works after 10 steps (10!).

## Step 12. Take a moment to digest how it was easier to set up tensorflow for CPU
To set up tensorflow on your CPU and virtual environment, you only need the following steps (make sure to create different virtual environments for CPU and GPU version if you would like to test both):
* Step 1
* Step 9
* Step 10



## References which helped me to understand things better and write this
* [A Medium post](https://towardsdatascience.com/installing-tensorflow-with-cuda-cudnn-and-gpu-support-on-windows-10-60693e46e781)
* [The GREAT github repo](https://github.com/fo40225/tensorflow-windows-wheel)
* [A stackoverflow page](https://stackoverflow.com/questions/45316569/how-to-install-tensorflow-on-python-2-7-on-windows)
* [Other stackoverflow post which helped my minor problems, which I unfortunatel forgot their direct pages](https://stackoverflow.com)
