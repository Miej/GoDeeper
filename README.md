# We Have To Go Deeper 

## AWS p2.xlarge GPU optimized deep learning cluster-grenade.
### All installations are bleeding-edge as of Halloween 2016

### Region-specific AMIs

| Region                  | AMI ID       |
| ----------------------- |:------------:|
| US East (N. Virginia)   | `ami-a195cfb6` |
| US East (Ohio)          | `ami-86277de3` |
| US West (N. California) | `ami-3e22685e` |
| US West (Oregon)        | `ami-da3096ba` |
| EU (Ireland)            | `ami-afa3e9dc` |
| EU (Frankfurt)          | `ami-8f906be0` |
| Asia Pacific (Tokyo)    | `ami-32d37e53` |
| Asia Pacific (Seoul)    | `ami-6a20f404` |
| Asia Pacific (Singapore)| `ami-20e54543` |
| Asia Pacific (Sydney)   | `ami-6f2b170c` |
| Asia Pacific (Mumbai)   | `ami-8da5d1e2` |
| South America (São Paulo)| `ami-b3a438df` |

# Greetings, traveler.  

Here we have a Deep Learning system image (AWS AMI) custom built for the p2.xlarge system, which at ~$1 per hour is a GREAT price for a machine as powerful as it is.  I've had my share of difficulties with building various modules/packages/drivers personally, so I thought I would share this for any fellow ml/dl/ai researchers/professionals who feel they might benefit from not
having to go through the lengthy process of debugging/hacking that I did.


## Design Philosophy

I wanted to build a flexible system that would allow myself and others to seamlessly explore whatever machine learning / deep learning system I wanted to, while side-stepping the plethora of dependency conflict issues / custom build issues that inevitably seem to arise each time I try a new package.  To that end, I wanted to start off with a clean system, and do a simul-build of all of the latest, bleeding-edge packages around.  I'm sure I inevitably missed a few things here and there, but feel free to shoot me an email at mark.woods89 (at) gmail (dot) com if theres something you'd like to see added to this image.

This build is somewhat weighted towards python-facing systems because of my background, but I tried to also include many of the other deep learning environments that I've been hearing of around town.  

On that note, please let me know if there's anything you'd like to see added to this image.  I'll be keeping track of requests for the next iteration of this image at the bottom of this page under TODO.

#### Some highlights of the build:

* CUDA 8.0
* cuDNN 5.1
* Microsoft CNTK V2
* OpenCV 3.1
* Spark 2.0
* Caffe 1.0
* Tensorflow
* Theano
* Keras  
* Torch 7
* R
* Scala
* Julia

Also it comes with a virtual desktop set up via vncserver in case you're into that kind of thing.


### Additional Notes

All installed packages/modules/languages/etc have been tested and confirmed to work properly with GPU (at least for simple tests).  

Also, the GPU has been max-clocked ala:

http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using_cluster_computing.html#optimize_gpu


## Quick Start Guide

1. Get an aws account, initialize it with this machine image from 'Community AMIs' e.g. ami-a195cfb6 for US East (N. Virginia).  Make sure you have your security groups set up appropriately.  I personally like to allow all inbound/outbound traffic to/from my personal ip, but you can customize this how you want.  port 22 for ssh, ports 5900/5901 for vnc.

2. User credentials: 
    - username: `icarus`
    - password: `changetheworld`
    
    - Note: The software packages are on a user-specific install, so you need to log in as `icarus` in order to access them properly.  I plan to update this for the next release.

3. Reboot the instance. (this starts the init scripts properly).  For SSH-only access, you will need to connect with your cert file from aws, as described here: https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EC2_GetStarted.html#ec2-connect-to-instance-linux .  You can login as the default username `ubuntu`, no password.  Then switch to user: `icarus` via ` $ su icarus ` .  Alternatively, I've also set up a virtual desktop on this image that can be accessed by vnc.  See Step 5.

4. Run `aws configure` so that you can use the super handy-dandy aws-cli.

5. To access the vncserver-hosted desktop, I personally like VNC Viewer, since it's pretty lightweight and simple to use. You can find it here: https://www.realvnc.com/download/viewer/
    - (I still haven't quite worked out the issue with the super button not opening the search menu properly, but oh well.  If you have tips on getting that to work, let me know.)
    - Then, to load the desktop, just boot up VNC Viewer, put your public DNS in the "VNC Server:" box, followed by ":1" (without quotes) to access the virtual desktop loaded on screen :1.  (eg: `ec2-55-155-222-144.compute-1.amazonaws.com:1` )
    - Password: `godeeper`

6. If you are a master sysadmin and you think the word 'desktop' is dirty, you can disable the auto-load script at /etc/init.d/vncserver by deleting the file, or moving it to a backup or some such.
    - Alternatively, you can configure it as you like there.  It's currently set to create a 1920x1080 desktop at 24-bit color depth.

7. A few things still dont work quite properly due to the system being a headless server with the bleeding-edge drivers required for the installed packages.
    - For example, gnome-control-center will give an ominous error about a BadRequest from the X Window System.  I honestly recommend not trying to get too deep into debugging that, it's a pain.
    - Instead, there are packages around that will allow you to do what you need to in the system settings, or as an alternative:

    Log into the instance via something like putty + xming, '$ vncserver -kill :1' to shut down the virtual desktop and run gnome-control-center from your terminal login. That will load a gui interface for that if you need it.
    - (If anyone knows a way to get it working in the vnc desktop, I'd be happy to hear.  For time's sake though, I chose to just adopt the workaround)

8. **Do all the machine learning.**
    In general, a decent way to use this system is just to assume that what you need is already installed on it and configured to work.

    Just try some of these commands in the terminal:
    ```
    $ th
    $ pyspark
    $ digits-devserver
    $ cntk
    $ python
    >>> import caffe
    >>> import tensorflow   
    >>> import theano
    >>> import keras
    ```

9. For more information on build paths and such, please reference `~/.bashrc`.  I tried to include most (if not all) of the relevant directories there.

10. Additionally, I set up a little script in the home directory to run commands sequentially followed by shutting down the instance.
It's not pretty code, but for something that took less than 2 min to make, it comes in handy when you want to get more work done in a night, but dont want to waste $$ by leaving your instance running.  Don't forget to alter it so that it uses your own instance ID!

### Saving money with AWS spot instances.
Using AWS spot instances can save you 20-80% per hour. The downside is that AWS reserves the right to shutdown your instance if the demand for that instance type goes up. Zohar Jackson has provided a script to attempt to optimize aws spot instance choice and pricing if you want to go that route, you can check it out here: https://github.com/Jakobovski/aws-spot-bot It aims to find and launch the cheapest and least likely to get shutdown spot instances.  AWS also provides spot instances that can be reserved for a set length of time.


## And that's it!  Enjoy folks!

If you find this useful, please shoot me an email over at mark.woods89 (at) gmail (dot) com .  Hopefully it will save some of y'all a lot of the headaches I went through trying to get it set up myself.

Oh yeah, and if you're just getting into machine learning, remember: change the world.


### Getting Started with AWS, ML, CV, and DL

This will get fleshed out a little in the future, but for now, if you're new to aws/ml/etc, here are some links that will help you get set up in your exploration of this system, and the machinations

1.  Getting the image set up on aws:  first, log in to your amazon account on the aws site.  You may need to send a reqest to AWS to increase your p2 instance limit to 1.  Should only take a few days to get approved.  When you're clear to start a p2.xlarge instance, you can load the ami of your choice by going to the EC2 section of aws, clicking 'create instance', and searching for the ami of your choice.
    - ref: http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/finding-an-ami.html

2.  Once you've created your new instance, it will take a couple minutes to boot up.  Once it does so, reboot the instance once for good measure.  From there, you can access it either by ssh
    - ref: http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AccessingInstancesLinux.html
    - or by vnc (see step 5 in the quick-start guide)

3.  I personally use python for a lot of my work, and I especially like the jupyter notebook interface.  To use it, just type `jupyter notebook` into your terminal and hit enter.  From there, you can create new notebooks and run sections of code as you like.
    - ref: https://jupyter-notebook-beginner-guide.readthedocs.io/en/latest/execute.html

4.  Cool, so now you have the jupyter notebook interface up and running.  Next, you want to figure out what this whole 'machine learning' thing is.  There are a few tutorials/classes around the web, here's one by the notorious A.NG:
    - ref: http://openclassroom.stanford.edu/MainFolder/CoursePage.php?course=MachineLearning

5.  The python package 'scikit-learn' is wonderful and incredible and delightful and things.  Most things you want to do with non-deep learning models are already implemented quite nicely there. Check it out.  Here's a little tutorial page to get you started:
    - ref: http://scikit-learn.org/stable/tutorial/

6.  Machine learning tasks explored with sklearn also interface quite nicely with python's pandas package, and since it's built from a numpy foundation, you can do a lot of vaguely complex things to large datasets pretty quickly.  
    - ref: http://pandas.pydata.org/pandas-docs/stable/10min.html

7.  A lot of very interesting problems in the field of machine learning/ai involve building systems that will be able to process and "understand" visual data in order to perform a task/analysis.  A good starting point for this is the python package opencv.  It's very cool, and very powerful when used properly.  Check it out.
    - ref: http://docs.opencv.org/trunk/d9/df8/tutorial_root.html

8.  So now you're tired of that, and want to do some real deep learning and start making cool pictures like Google's inceptionism deep dreams.  For a little introduction into the current methods/models being used in the field, check out this site:
    - ref: http://deeplearning.stanford.edu/wiki/index.php/UFLDL_Tutorial

9.  Then, to start playing around with some more code and model-building:
    - ref: http://deeplearning.net/tutorial/gettingstarted.html
    - ref: http://machinelearningmastery.com/tutorial-first-neural-network-python-keras/

10.  Explore.  There are a lot of installations listed here, and they each perform certain tasks in different ways.  Figure out what you like, what sort of problems you want to work on, what tools are best-suited to solving them.  Google is your friend.  Machine learning/deep learning/ai is like a cave.  The entrance is well-lit and well-known, but there's still plenty of pathways that have yet to be discovered.  So grab yourself a torch(7) and find yourself a dark tunnel that nobody's been down yet.


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

##### See file luarocks_installs_versions.txt in the home directory of the system for included packages





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



#### TODO for next release:

build compatibility for python3

build docker image

add mxnet

add nolearn

add lasagne

add octave

flesh out tutorial section

look into turning this into an ansible playbook?

Include dl4j

shogun-tk ?

include perl

include https://github.com/usnistgov/F4DE because it is glorious.

Suggestion: Install Cuda and cuDNN using the -no-opengl-files option.
https://davidsanwald.github.io/2016/11/13/building-tensorflow-with-gpu-support.html




### Feeling generous?

Costs about 60 USD/mo to maintain the machine images across the world, which I'm generally happy to do in the name of making these tools easier to access.  That said, if you want to help contribute to the cause, any and all donations are highly appreciated.  You can use this link if you want to contribute to the ai/beer fund: www.paypal.me/miej
