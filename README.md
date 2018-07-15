# Tflite-Image-Recognition
Simple image recognition using tflite for android


## STEPS:
    1. Train Image Classifier
    2. Convert Trained Model to TFlite format using TFlite Converter
    3. Run as Android App using TFlite Interpreter

***Sidenote for Windows' user,*** as of what I've tried which took me about 3 days STEP 2 works only via Linux Virtual Machine using VirtualBox so we'll use it for this tutorial.

## STEP 1 & 2: Convert Trained Model to TFlite format using TFlite Converter
  ### A. For Windows
    1. Setting up Linux Virtual Machine using VirtualBox
      i. For installing Ubuntu-16.04 LTS on Virtual Box (Desktop version), follow tutorial at https://medium.com/@tushar0618/install-ubuntu-16-04-lts-on-virtual-box-desktop-version-30dc6f1958d0
    2. Setting up the environment and installing TensorFlow:
      i. For installing Python 3.6 using apt-get following the answer from @https://askubuntu.com/questions/865554/how-do-i-install-python-3-6-using-apt-get, input in the cli the ff:
         sudo add-apt-repository ppa:deadsnakes/ppa
         sudo apt-get update
         sudo apt-get install python3.6
      ii. For installing pip(PIP Installs Packages), input:
         sudo apt-get install python3-pip python3-dev 
      iii. For installing tensorflow, input:
          sudo pip3 install tensorflow==1.5.0
         
