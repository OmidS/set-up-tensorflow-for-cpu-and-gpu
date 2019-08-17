# How to set up tensorflow (cpu and gpu) on your machine (recommended: on virtual environment)
I tried setting up tensorflow for one of my projects and it was worse than what I expected. There were a lot of threads on the internet which I found useuful but unfortunately they did not work for me (don't get me wrong, they all helped me to come up with a clean way to set up tensorflow to work compatibly on my gpu :P). So I decided to create this document for myself and anyone else who wants to set up tensorflow for their gpu but find themselves lost at the beginning. I hope this helps and saves you a couple days of searching on the internet and asking around :). At the end, I will put all the threads which helped me writing this. You may find them useful for your case.
These all worked fine for me and I have a machine with Windows 10, my gpu is NVIDIA GEFORCE RTX 2080 Ti and I set this up on python 3.7.

## STEP 1. Install python on your machine 
You need to install python on your machine. It has to be 64-bit python, otherwise you will get the error "not a supported wheel on this platform" when trying to install tensorflow package. For example, I have python 2.7, python 3.7 installed on my PC.

## STEP 2. Buy a GPU
Well if you want to set up tensorflow on your gpu you need to have a gpu first. This is the most expensive yet the most straightforward step. For example, I got the NVIDIA GEFORCE RTX 2080 Ti. So all the following is assuming that you have a NVIDIA gpu.

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
