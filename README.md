# We Have To Go Deeper 

## aws p2.xlarge GPU optimized deep learning cluster grenade: 

### ami-e29fc5f5

# Greetings, traveler.  

Here we have a Deep Learning system image (AWS AMI) custom built for the p2.xlarge system.  
I've had my share of difficulties with building various modules/packages/drivers personally, so I thought
I would share this for any fellow ml/dl/ai researchers/professionals who feel they might benefit from not
having to go through the lengthy process of debugging/hacking that I did..


## Design Philosophy

I wanted to build a flexible system that would allow myself and others to seamlessly explore whatever machine learning / 
deep learning system I wanted to, while side-stepping the plethora of dependency conflict issues / custom 
build issues that inevitably seem to arise each time I try a new package.  To that end, I wanted to start 
off with a clean system, and do a simul-build of all of the latest, bleeding-edge packages around.  I'm 
sure I inevitably missed a few things here and there, but feel free to shoot me an email at mark.woods (at) 
gmail (dot) com if theres something you'd like to see added to this image.

This build is somewhat weighted towards python-facing systems because of my background, but I tried to also
include many of the other deep learning environments that I've been hearing of around town.  

#### Some hilights of the build:

CUDA 8.0
cuDNN 5.1
OpenCV 3.1
Spark 2.0
Caffe 1.0
Tensorflow
Theano
Keras  
Torch 7
Microsoft CNTK V2
R
Scala
Julia

also it comes with a virtual desktop set up via vncserver in case you're into that kind of thing.


### Additional Notes

All installed packages/modules/languages/etc have been tested and confirmed to work properly with GPU (at least for simple tests).  

Also, the GPU has been max-clocked ala:

http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using_cluster_computing.html#optimize_gpu


### Getting Started

1) Get an aws account, initialize it with this machine image from 'community amis' : ami-e29fc5f5

2) 
username: icarus
password: changetheworld

3) Reboot the instance. (this starts the init scripts properly)

4) run '$ aws configure' so that you can use the super handy-dandy aws-cli.

5) To access the vncserver-hosted desktop, I personally like VNC Viewer, since it's pretty lightweight and simple to use.
You can find it here: https://www.realvnc.com/download/viewer/

(I still haven't quite worked out the issue with the super button not opening the search menu properly, but oh well.  If you have tips on getting that to work, let me know.)

Then, to load the desktop, just boot up VNC Viewer, put your public DNS in the 'VNC Server:' box, followed by ':1' (without quotes)
to access the virtual desktop loaded on screen :1.  

password: godeeper

6) If you are a master sysadmin and you think the word 'desktop' is dirty, you can disable the auto-load script at /etc/init.d/vncserver .  
Alternatively, you can configure it as you like there.  It's currently set to create a 1920x1080 desktop at 24-bit color depth.


7) A few things still dont work quite properly due to the system being a headless server with the bleeding-edge drivers required for the installed packages.
For example, gnome-control-center will give an ominous error about a BadRequest from the X Window System.  I honestly recommend not trying to get too deep into debugging that, it's a pain.
Instead, there are packages around that will allow you to do what you need to in the system settings, or as an alternative:

Log into the instance via something like putty + xming, '$ vncserver -kill :1' to shut down the virtual desktop and run gnome-control-center from your terminal login. That will load a gui interface for that if you need it.
(If anyone knows a way to get it working in the vnc desktop, I'd be happy to hear.  For time's sake though, I chose to just adopt the workaround)


#### 8) Do all the machine learning.

In general, a decent way to use this system is just to assume that what you need is already installed on it and configured properly.

Just try some of these commands in the terminal:

$ th
$ pyspark
$ digits-devserver
$ cntk
$ python
>>> import caffe
>>> import tensorflow   
>>> import theano
>>> import keras


9) For more information on build paths and such, please reference ~/.bashrc .  I tried to include most (if not all) of the relevant directories there.

10) Additionally, I set up a little script in the home directory to run commands sequentially followed by shutting down the instance.
It's not pretty code, but for something that took less than 2min to make, it comes in handy when you want to get more work done in a night, but dont want to waste $$ by leaving your instance running.  Don't forget to alter it so that it uses your own instance ID !


## And that's it!  Enjoy folks!

If you find this useful, please shoot me an email over at mark.woods89 (at) gmail (dot) com .  Hopefully it will save some of y'all a lot of the headaches I went through trying to get it set up myself.

Oh yeah, and if you're just getting into machine learning, remember: change the world.


### System
aws p2.xlarge
Ubuntu 14.04 LTS (Trusty Tahr)
NVIDIA Tesla K80
50 Gb storage


### AWS cli
aws-cli 1.11.10

### NVIDIA things.
NVIDIA CUDA 8.0
NVIDIA cuDNN 5.1
NVIDIA CUB 1.5.5
NVIDIA DIGITS 5.1-dev
NVIDIA CNMeM 1.0 (?)

### Pythony things.
Anaconda 2
python 2.7
opencv 3.1.0
spark 2.0.1
caffe 1.0.0-rc3
keras 1.1.0
tensorflow 0.11.0rc0
tf-learn 0.2.2
theano 0.8.2

### Torch.
torch 7
##### See file luarocks_installs_versions.txt for included packages


### Other languages/packages for ml/dl
R 3.0.2
shiny 0.14.1


Scala 2.11.8
scala sbt 0.13.12


Julia 0.5


IntelliJ IDEA 2016.2.5 # For DeepLearning4J
maven 3.0.5


Microsoft CNTK
kaldi 1.0(?)


### Just a few BLAS/math/etc libs...
ATLAS 3.10.1
OpenBLAS 0.2.20
MKL
Microsoft MKLCustom
openMPI 1.10.3
zlib 1.2.8
libzip 3.0
Boost 1.54.0
protobuf 3.1.0
