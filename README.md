# Design resources

ENG5105 is a design unit at Monash University, where students are required to design and prototype something cool using a Raspberry PI, and additional sensors, microcontrollers and actuatros. This year students are tasked with making wildlife monitors. This repository contains a set of useful jupyter notebooks to get started with the Raspberry PI.

* [Servo motor control](https://github.com/MonashRobotics/ENG5105/blob/main/Servo_motor_control.ipynb)
* [Getting frames from the Pi Camera](https://github.com/MonashRobotics/ENG5105/blob/main/Camera_image_retrieval.ipynb)

There are also some nice [c++ programming resources](./cpp_resources) if you prefer to program using this language.

# Setting up your raspberry pi to run these notebooks

If you would like to configure your raspberry pi to use jupyter follow the steps below.

## Initial Setup

I'll assume you have an operating system with a terminal, and that can ssh. On windows, check out the [windows subsystem for linux](https://docs.microsoft.com/en-us/windows/wsl/install-win10), and [putty](https://www.putty.org/). I will be using Ubuntu, which generally ships with terminal and ssh.

I [installed Ubuntu 20.04 server](https://ubuntu.com/tutorials/how-to-install-ubuntu-on-your-raspberry-pi#1-overview) on my raspberry pi, because I wanted a 64 bit operating system for PyTorch (see below). You could follow a similar process for the Raspbian Buster distribution pre-installed on your sd cards, but you won't be able to use PyTorch if you do this. 

Let's log into our Ubuntu raspberry pi using ssh
```
ssh ubuntu@ip_address
```
Once you're in, it's time to install some useful software. [tmux](https://github.com/tmux/tmux/wiki) is a terminal multiplexer for running multiple programs in one terminal and remotely executing programs that will continue to run if the network drops. [htop](https://htop.dev/) is a terminal process viewer that will be useful to monitor processor load/ memory use. [vim](https://vim-adventures.com/) is a text editor for people who hate [emacs](https://en.wikipedia.org/wiki/Editor_war). Most of the rest are things we'll use in python.

```
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install git python3-scipy jupyter libatlas-base-dev avahi-daemon tmux htop vim python3-pip python3-matplotlib libgl-dev net-tools python3-gpiozero
```

Since we've installed the [avahi-daemon](http://avahi.org/), it's a good idea to change the hostname to something unique, eg. `ECE4191GROUPNO`. Use the command 
```
hostnamectl set-hostname ECE5105GROUPNO
```
to do this. You may need to reboot `sudo reboot` for this to take effect.

Now, assuming you're on the same network, logging in to the pi remotely will be a lot easier going forward, just using:
```
ssh ubuntu@ECE5105GROUPNO.local
```

Ok, lets continue to install some more packages on the pi.
``` 
pip3 install --upgrade pip
pip3 install --user jupyter opencv-python torch
pip3 install --user numpy --upgrade
```

Next, we'll clone this repository
```
git clone https://github.com/MonashRobotics/ECE5105/
```

# Running the notebooks

Now we'll run jupyter notebook in a headless fashion
```
jupyter notebook --no-browser --port 8888 --ip 0.0.0.0
```

You can now access the notebook running on the raspberry pi using any other computer on the network, by simply typing
```
http://raspberry_pi_ip:8888/
```
into a browser address bar, where `raspberry_pi_ip` is the ip address of your raspberry pi. Eg. mine is `192.168.20.14`.

You can find out your raspberry pi ip address by typing `ifconfig` in a raspberry pi terminal, or going to your router settings pages and looking at the addresses of connected devices.

Use `ctrl-c` in the terminal to kill a running process (eg. if you want to close the notebook). Shutdown your pi cleanly with `sudo shutdown -h now`

### Running PyTorch on your raspberry pi

Note that the raspberry pi is really not suited to deep learning (Consider the [Nvidia Jetson](https://developer.nvidia.com/buy-jetson) if you want a better embedded computer for this). You can run models, but I wouldn't try to train bigger models on the pi.


