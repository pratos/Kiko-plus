---
layout: post
published: true
title: Setting up Deep Learning GPU environment
---

<div align="center"><img src="https://i.ytimg.com/vi/UQAwOx5UxYU/maxresdefault.jpg" height="600px;" width="900px;"/></div>
<p align="center"><b>Source: YouTube: <a href="https://www.youtube.com/watch?v=UQAwOx5UxYU">@HeroTech</a></b></p>

Setting up a Deep Learning Environment for a GPU Enabled system is a headache. It is harder compared to the simple CPU setups that take hardly much time. This post is for you, if you have struggled to get things working on Ubuntu.

(Note: There might be a few changes in getting things working for your system, since this might get very obsolete in a month or few months time)

You'll be going step-by-step in how not to sweat it out while getting things working. GPUs don't come cheap and it is obvious that no one wants to screw things up while working on the same. Below are the sections that we'll go through:

***

- Basic checks for installations needed for CUDA 8.0 and cuDNN.
- Installing the NVIDIA Drivers along with CUDA 8.0.
- Installing cuDNN.
- Installing Python using MiniConda.
- Installing GPU Version of TensorFlow.
- Installing the other required libraries to work with (Keras, Lasagne).
- Checking whether everything works.

***

Of all the challenges, NVIDIA Driver installation is the biggest headache involving the most tedious workaround.

Our machine specs:

<script src="https://gist.github.com/pratos/7a1c53fe6ed888dde178787d512806cf.js"></script>


_The GPU we have isn't an NVIDIA one, but from MSI (so be careful while following this blog post if you have an NVIDIA GPU)._

Let's start off:

### Basic checks for installations needed for CUDA 8.0 and cuDNN

***

- Check for the nvidia enabled drivers:

    `lspci | grep -i nvidia`
    
- Check the machine name and in the CUDA documentation check whether it is supported or not (We have 16.04 which is supported for CUDA 8.0):

    `name -m && cat /etc/*release`
    
- Check for the version of gcc (We need to install version less than 5.0 for some issues with Theano):

    `gcc --version`
    
- If version is above 4.9, downgrade the existing version from 5.3 to 4.9
    
    ```
    sudo apt-get install gcc-4.9
    sudo apt-get install g++-4.9
    ```
- Need to remove the initial installation for gcc:

    ```
    sudo rm -rf /usr/bin/gcc
    sudo rm -rf /usr/bin/g++
    ```
    
- Create hard links for the gcc versions:

    ```
    sudo ln -s /usr/bin/gcc-4.9 /usr/bin/gcc
    sudo ln -s /usr/bin/g++-4.9 /usr/bin/g++
    ```
    
- Check whether things are working or not:

    ```
    gcc --version
    g++ --version
    ```
    
### Installing the NVIDIA Drivers along with CUDA 8.0

***

- You need to download the CUDA Installer kit from [this location (For Linux distribution)](https://developer.nvidia.com/cuda-downloads)
    * Path: `Linux --> x86_64 --> Ubuntu --> 16.04 --> runfile(local) --> Base Installer (1.4 GB)`
    
- Create a folder to extract contents of the installer file (Optional)
    ```
    mkdir ~/Downloads/nvidia_installers
    cd ~/Downloads
    ```
- Instead of this directly copy-paste
    
    `sudo sh ./cuda_8.0.44_linux.run -extract=~/Downloads/nvidia_installers`

- Follow this step only if you have an existing nvidia installer in your system, after this is done you will have the open source driver coming in picture. In the next few steps we would take care of that:

    `sudo apt-get --purge remove nvidia-<version name>`
    
- I'm not sure if the issue: [Graphics issues after/while installing Ubuntu 16.04/16.10 with NVIDIA graphics](https://askubuntu.com/questions/760934/graphics-issues-after-while-installing-ubuntu-16-04-16-10-with-nvidia-graphics) got resolved (in 2017). Updating the Graphics Driver from GUI Console didn't help the cause. So we had to do a work-around for that (follow along).

- Need to create the xorg.config file. You would have it in the location `/etc/X11`. Not needed for a fresh install, still run it.

    `sudo rm /etc/X11/xorg.conf`
    
- Blacklist the Nouveau module: 
    - To list the available modules in the system: `lsmod`
    - To check for the dependencies of that module: `modinfo -F depends nouveau`
    - To remove without the dependencies, create `/etc/modprobe.d/blacklist.conf` and add the lines below:
        ```
        blacklist nouveau
        options nouveau modeset = 0
        ```
- Download the Nvidia driver for 16.04 (Recommended nvidia-367). Copy everything in the `Downloads/nvidia_installers`

- Run the commands below
    
    `sudo update-initramfs -u`

- Reboot the computer after this to boot into `TTY` mode:
    * Reading for `TTY` mode:
        * [What does TTY stand for?](https://askubuntu.com/questions/481906/what-does-tty-stand-for)
        * [TTY Demystified](http://www.linusakesson.net/programming/tty/)
    * Reading on how to boot into `TTY` mode: [Link](http://ubuntuhandbook.org/index.php/2014/01/boot-into-text-console-ubuntu-linux-14-04/)
    * While booting up, press <kbd>Crtl</kbd>+<kbd>Alt</kbd>+<kbd>F1</kbd>
    * You should see a black screen terminal (see below):
        ![tty](http://1.bp.blogspot.com/-1rcSFf7QRKU/UPhb_GPwZsI/AAAAAAAALAw/X1f7pOdLHUY/s1600/tty-screenshot.png)

- The next step is to: _Stop the XServer_. Reading [What is XServer](https://askubuntu.com/questions/7881/what-is-the-x-server).
    * Run the command: 
    
        `sudo service lightdm stop`
        
- You have to run the CUDA installation in the `TTY` for mode: `sudo sh ./cuda_8.0.44_linux.run`

- The above command installs the driver as well as the tool kit, driver is the one for 16.04. We had downloaded the 16.04 version.

- You will encounter the following questions:
    * Scroll through the entire document, there will be a few questions:
        - EULA Acceptance? Accept
        - CUDA Driver installation? Y
        - Do you want to install OpenGL Libraries? N (Y for Nvidia Drivers)
        - Do you want to run nvidia-xconfig? Y
        - Install Cuda 8.0 toolkit? Y
        - Enter tool kit location? Enter a new one or default
        - Do you want to install a symbolic link at <link>? Y
        - Install the CUDA Samples? Y
        - Enter CUDA Sample location: Enter the path of Cuda directory/samples
        - Missing recommended libraries: Upto you.
- Export the paths: `sudo nano .bashrc` and put the export comments (below) at the end of the file:
    
    ```
    export PATH=/usr/local/cuda-8.0/bin:$PATH
    export LD_LIBRARY_PATH=/usr/local/cuda-8.0/lib64:$LD_LIBRARY_PATH
    ```
- Run: `source .bashrc`

- Check the driver version: `cat /proc/driver/nvidia/version`
    * Should be whatever version you install, our system has:
        ```
        NVRM version: NVIDIA UNIX x86_64 Kernel Module  375.39  Tue Jan 31 20:47:00 PST 2017
        GCC version:  gcc version 4.9.3 (Ubuntu 4.9.3-13ubuntu2) 
        ```
- Check the CUDA Version: `nvcc -V`
    * If it is successful, you should have the output below:
        ```
        nvcc: NVIDIA (R) Cuda compiler driver
        Copyright (c) 2005-2016 NVIDIA Corporation
        Built on Sun_Sep__4_22:14:01_CDT_2016
        Cuda compilation tools, release 8.0, V8.0.44
        ```
    
- Restart the lightdm service, this will take you back to your GUI console.
    
    `sudo service lightdm start`

- Make sure you remember where you saved your CUDA Samples. We saved ours in: `/usr/local/cuda-8.0/samples/1_Utilities/deviceQuery`

- Fire up the terminal and run the commands below:
    
    `cd /usr/local/cuda-8.0/samples/1_Utilities/deviceQuery`
    
- Run: `sudo make`. This would compile the files for you, for the program to be run.

- To run the `deviceQuery` sample, run: `./deviceQuery`. It should give you the details of your device (below):

    <script src="https://gist.github.com/pratos/159160b5833032c8aa0f7b19a298b8c9.js"></script>
    
    
### Installing cuDNN

***

- Download the cuDNN zip folder from this location: [cuDNN Download](https://developer.nvidia.com/rdp/cudnn-download)

(_Note: First time download needs a login and need to create Nvidia Developer account – going through the motions of creating an account, answering the survey and other things and also activating the account after which the downloads would be available for us mortals._)

- Choose “__cuDNN v5.1 Library for Linux__” and follow the steps below:

    ```
    tar xvzf ~/Downloads/cudnn-8.0-linux-x64-v5.1.tgz
    cd cuda
    sudo cp include/cudnn.h /usr/local/cuda/include/
    sudo cp lib64/* /usr/local/cuda/lib64/
    ```
- __cuDNN v5.1__ should now be available for you.


Now you are done through with the most difficult part, the next set shouldn't be taxing!

### Installing Python using MiniConda

***

- It is always better to use Continuum's Anaconda distribution, but it is best to use [Miniconda](https://conda.io/miniconda.html) as the minimal base python version.

- Use any version from [this link]
    
    ```
    wget https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh
    bash Miniconda3-latest-Linux-x86_64.sh
    ```
- A set of questions would follow, answer them appropriately. If interested in reading more and getting a pre-designed Python environment follow [this link](https://github.com/soumendra/python-machinelearning-setup)
    
- Test it once on the terminal, to make sure you are running the Miniconda Version.

- Create the python3.5 environment: `conda create --name dlgpu python=3.5`

- Activate the environment: `source activate dlgpu`
    
### Installing GPU Version of TensorFlow

***

- Visit the site: [OS Setup - TensorFlow](https://www.tensorflow.org/versions/r0.11/get_started/os_setup.html)

- Follow the steps below:
    
    ```
    export TF_BINARY_URL=https://storage.googleapis.com/tensorflow/linux/gpu/tensorflow_gpu-1.0.1-cp35-cp35m-linux_x86_64.whl
    ```

- Update setuptools (through conda to avoid install_path.pth issues)
    
    `conda install setuptools`
    
- Now install TensorFlow GPU using the command: 
    
    `pip install --ignore-installed --upgrade $TF_BINARY_URL`
    
- For this issue: [NVIDIA DIGITS Issue](https://github.com/NVIDIA/DIGITS/issues/8)
    
    `sudo ldconfig /usr/local/cuda/lib64`

### Installing the other required libraries to work with (e.g. Keras, Lasagne)

***

- Install other libraries that you'll need, we'll be going for `keras` & `lasagne`

    ```
    pip install keras
    pip install lasagne
    ```

### Checking whether everything works

***

<script src="https://gist.github.com/pratos/d53d44c594660ec92bcb23865ccdf119.js"></script>

If you are here, I felt the same as you!

<iframe src="//giphy.com/embed/Mp4hQy51LjY6A" width="480" height="320.4" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="https://giphy.com/gifs/smile-done-Mp4hQy51LjY6A">via GIPHY</a></p>

This should get your started with Deep Learning using GPU. Do let me know in the comments if there was any issue during the installation, would try to answer the questions to the best of my knowledge.

__Sources:__
1. [NVIDIA Blog: Installation](http://www.nvidia.com/object/gpu-accelerated-applications-tensorflow-installation.html)
2. [Install GPU TensorFlow Ubuntu 16.04](http://deeplearning.lipingyang.org/2017/01/18/install-gpu-tensorflow-ubuntu-16-04/)
3. [floydhub/dl-setup](https://github.com/floydhub/dl-setup)
4. [Setup for Ubunutu16.04](http://www.computervisionbytecnalia.com/en/2016/06/deep-learning-development-setup-for-ubuntu-16-04-xenial/)
5. [How to solve the graphics issues caused by X11](https://askubuntu.com/questions/760934/graphics-issues-after-while-installing-ubuntu-16-04-16-10-with-nvidia-graphics)
6. [Installing GPU Tensorflow in Xenial16.04](https://alliseesolutions.wordpress.com/2016/09/08/install-gpu-tensorflow-from-sources-w-ubuntu-16-04-and-cuda-8-0-rc/)
7. [Older setup](http://kislayabhi.github.io/Installing_CUDA_with_Ubuntu/)
8. [Deep Learning Installation](https://www.born2data.com/2016/deeplearning_install-part1.html)
9. [Deep Learning setup for Windows](https://github.com/philferriere/dlwin)
10. [NVIDIA Driver issues](http://technozed.com/fix-nvidia-graphical-issues-installing-ubuntu-16-04/)
11. [How to chose the driver for Linux](https://linuxconfig.org/how-to-install-the-latest-nvidia-drivers-on-ubuntu-16-04-xenial-xerus).
