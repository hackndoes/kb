# Install Maually

```s
wget http://us.download.nvidia.com/XFree86/Linux-x86_64/367.57/NVIDIA-Linux-x86_64-367.57.run
chmod a+x NVIDIA-Linux-x86_64-367.57.run
./NVIDIA-Linux-x86_64-367.57.run --accept-license --no-questions --silent --no-opengl-files
rm ./NVIDIA-Linux-x86_64-367.57.run
# test it works
nvidia-smi
```

# Install as PPA

``` s
sudo apt-get purge nvidia*
sudo add-apt-repository ppa:graphics-drivers
sudo apt-get update
sudo apt-get install nvidia-370
sudo reboot
# test it works
nvidia-smi
```

[Source](http://www.linuxandubuntu.com/home/how-to-install-latest-nvidia-drivers-in-linux)