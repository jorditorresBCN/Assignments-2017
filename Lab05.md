# Lab 5: Under the Hood of Cloud Advanced Analytics Services

In previous lab we suggested an "optional" task that uses the [Cloud Vision API](https://cloud.google.com/vision/) from Google as an example of Advanced Analytics as a Service in the Cloud. The platform to support this type of services require high performance computing resources as GPU accelerators and a powerful software, as TensorFlow, their advanced analytics engine. 

On this lab we will look under the hood of these types of advanced analytics services in the Cloud, either in terms of hardware and software, in order to understand how their high performance requirements can be provided. We will use a GPU cluster (thanks to the support of Barcelona Supercomputing Center) and we will review the main characteristics of [TensorFlow](https://www.tensorflow.org), the most popular framework to build Artificial Intelligent models nowadays. 

## Task 0: Install TensorFlow in your local laptop
* Follow the installation instructions from https://www.tensorflow.org/install/
* Clone or download this repository https://github.com/jorditorresBCN/FirstContactWithTensorFlow-2nEdition
* Validate your TensorFlow installation (assuming that target directory is ~/tensorflow ):

´´´
source ~/tensorflow/bin/activate 
git clone https://github.com/jorditorresBCN/FirstContactWithTensorFlow-2ndEdition.git
cd FirstContactWithTensorFlow-2ndEdition/
python ValidateYourInstallation.py
´´´

If the system outputs the following, 
´´´
Hello, TensorFlow!
´´´

then you are ready to begin writing TensorFlow programs!




#  Tasks of Lab 1 
## Task TF.1: 
Install Python in your local laptop.
## Task 1.2: 
Create a python code that uses the “random” library. We will value positively if you build a creative program in python. The minimum example accepted will be a code that generates a random number between 1 and 20. Then let the player guess the number introduced, displaying if the number is to low or high. The game ends either when the number is guessed correctly. The suggested program name is `Lab1.guessnumber.py`. 
## Task 1.3:  
Create a private repository CLOUD-COMPUTING-COURSE-2017 in your github account (use you student email (".upc.edu") for creating your github account in order to have private repositories or benefits as student pack).
## Task 1.4:  
Update your remote repository from your local repository on your laptop:

_*Detailed content of this lab will be added here soon (after Easter).*_
