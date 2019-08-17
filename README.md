# How to set up tensorflow (cpu and gpu) on your machine (recommended: on virtual environment)
I tried setting up tensorflow for one of my projects and it was worse than what I expected. There were a lot of threads on the internet which I found useuful but unfortunately they did not work for me (don't get me wrong, they all helped me to come up with a clean way to set up tensorflow to work compatibly on my gpu :P). So I decided to create this document for myself and anyone else who wants to set up tensorflow for their gpu but find themselves lost at the beginning. I hope this helps and saves you a couple days of searching on the internet and asking around :). At the end, I will put all the threads which helped me writing this. You may find them useful for yor case.

## STEP 1. Install python on your machine 
You need to install python on your machine. It has to be 64-bit python, otherwise you will get the error "not a supported wheel on this platform" when trying to install tensorflow package. For example, I have python 2.7, python 3.7 installed on my PC.
## STEP 2. Buy a GPU
Well if you want to set up tensorflow on your gpu you need to have a gpu first. This is the most expensive yet the most straightforward step. For example, I got the NVIDIA GEFORCE RTX 2080 Ti. So all the following is assuming that you have a NVIDIA gpu.
## STEP3. Install drivers for your gpu
I used https://www.geforce.com/drivers to install my drivers. For example, my driver's version is 431.60. You can check your driver's version by right clicking on the screen on your desktop and choosing "NVIDIA control panel".
## 
