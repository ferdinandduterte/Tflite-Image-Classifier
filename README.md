# Tflite-Image-Classifier
Simple image recognition using tflite for android


## STEPS:
    1. Train the Image Classifier
    2. Convert Trained Model to TFlite format using TFlite Converter
    3. Run as Android App using TFlite Interpreter


***Sidenote for Windows' user,*** as of what I've tried which took me about 3 days STEP 2 works only via Linux Virtual Machine using VirtualBox so we'll use it for this tutorial.


## STEP 1 : Train the Image Classifier
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
    3.  Retraining the Model
      i. Clone the git repository
        git clone https://github.com/googlecodelabs/tensorflow-for-poets-2
        cd tensorflow-for-poets-2 
      ii. Download the training images
        curl http://download.tensorflow.org/example_images/flower_photos.tgz \
            | tar xz -C tf_files
      iii. Configure your MobileNet
        IMAGE_SIZE=224
        ARCHITECTURE="mobilenet_0.50_${IMAGE_SIZE}"
      iv. Using Tensor Board
        tensorboard --logdir tf_files/training_summaries &
      v. Retraining the Network
        python -m scripts.label_image \
            --graph=tf_files/retrained_graph.pb  \
            --image=tf_files/flower_photos/daisy/21652746_cc379e0eea_m.jpg
        or
        python -m scripts.label_image \
            --graph=tf_files/retrained_graph.pb  \
            --image=tf_files/flower_photos/daisy/21652746_cc379e0eea_m.jpg
            
   ### B. For Others(Linux, Mac, etc.): Follow https://codelabs.developers.google.com/codelabs/tensorflow-for-poets/
   
   
## STEP 2 : Convert Trained Model to TFlite format using TFlite Converter
    IMAGE_SIZE=224
    toco \
      --input_file=tf_files/retrained_graph.pb \
      --output_file=tf_files/optimized_graph.lite \
      --input_format=TENSORFLOW_GRAPHDEF \
      --output_format=TFLITE \
      --input_shape=1,${IMAGE_SIZE},${IMAGE_SIZE},3 \
      --input_array=input \
      --output_array=final_result \
      --inference_type=FLOAT \
      --input_data_type=FLOAT
      
    This should output a "optimized_graph.lite" in your "tf_files" directory which will be used for the next step. If you're using Linux virtual machine, upload "optimized_graph.lite" to cloud sharing site(i.e. Google Drive, Dropbox, etc.) and download it in your PC with android studio. 

***Before proceeding to STEP 3, Connect your mobile to your computer via usb cable. Remember, you can't load the app from android studio onto your phone unless you activate "developer mode" and "USB Debugging". This is a one time setup process.***

## STEP 3 : Run as Android App using TFlite Interpreter
    1. Open AndroidStudio. After it loads select " Open an existing Android Studio project" from this popup:
    2. In the file selector, choose tensorflow-for-poets-2/android/tflite from your working directory.
    3. You will get a "Gradle Sync" popup, the first time you open the project, asking about using gradle wrapper. Click "OK".
    4. Run a Gradle sync, , and then hit play, , in Android Studio to start the build and install process.
    5. Add your model files to the project:
        cp tf_files/optimized_graph.lite android/tflite/app/src/main/assets/graph.lite 
        cp tf_files/retrained_labels.txt android/tflite/app/src/main/assets/labels.txt 
    6. In Android Studio run a Gradle sync, so the build system can find your files, and then hit play, to start the build and install process as before.
    7. Now try a web search for flowers, point the camera at the computer screen, and see if those pictures are correctly classified. Or have a friend take a picture of you and find out what kind of TensorFlower you are !
