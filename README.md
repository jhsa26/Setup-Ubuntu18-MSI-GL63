# Setup-Ubuntu18-MSI-GL63
Setup ubuntu18 on MSI-GL63

```
Summary for deeplearning setup
install nvidia-410 driver
install cuda-9.0
install cuDNN-7.0
install tensorflow-gpu
install pytorch-gpu
```

1. Using the UltraISO to burn the ubuntu18.0 system into USB disk on the window platform

2. Access to the bios GUI, select the UEFI boot and USB boot

3. Reboot, click the F11 button, select USB boot

4. When the mouse touch the install ubuntu option, then click the "e" on the keyboard
   
   Add " nomodeset" after "quiet splash". then click F10 to save this configuration and reboot
   
   Because the ubuntu use nouveau drivers  in default.
  
   You must do the step4.Otherwise, when you login, the screen will be freezed.
5. You can delete all hard disk and divide the hard disk into several part. home, swap area, /, and efi (storage - 300M,this must be done in here, otherwise there is no space to install grub2. )

6. Now, you can restart your computer to install nvidia card. When the mouse on the Ubuntu, click e and add the " nomodeset" after "quiet splash". there is space between "quiet splash" and "nomodeset.
7. edit blacklist.conf
```
sudo gedit /etc/modprobe.d/blacklist.conf
```
Add those terms in the end of file
```blacklist nouveau
blacklist lbm-nouveau
options nouveau modeset=0
```
save the blacklist.conf

8. Update  
```
sudo update-initramfs -u
```
9. Add nvidia source and update
  ```
  sudo add-apt-repository ppa:graphics-drivers/ppa
  sudo apt update
  ```
10. Search nvidia drivers
```
sudo ubuntu-drivers devices
```
```
/sys/devices/pci0000:00/0000:00:01.0/0000:01:00.0 ==
modalias : pci:v000010DEd00001C20sv00001462sd00001214bc03sc00i00
vendor   : NVIDIA Corporation
model    : GP106M [GeForce GTX 1060 Mobile]
driver   : nvidia-driver-410 - third-party free
driver   : nvidia-driver-390 - third-party free
driver   : nvidia-driver-415 - third-party free recommended
driver   : nvidia-driver-396 - third-party free
driver   : xserver-xorg-video-nouveau - distro free builtin
```
11. You can choose the nvidia-driver-410 to install on your laptop; 
```
   sudo apt-get install nvidia-driver-410
   or 
   sudo ubuntu-drivers autoinstall (could install 415 version)
```
12. update the grub
```
    sudo update-grub
```
13. reboot, click "del" into bios then disable secure boot control from bios if the nvidia-smi failed.

14.install cuda9.0

https://developer.nvidia.com/cuda-90-download-archive?target_os=Linux&target_arch=x86_64&target_distro=Ubuntu&target_version=1704&target_type=deblocal

download cuda-repo-ubuntu1704-9-0-local_9.0.176-1_amd64.deb
```
sudo dpkg -i cuda-repo-ubuntu1704-9-0-local_9.0.176-1_amd64.deb
sudo apt-key add /var/cuda-repo-9-0-local/7fa2af80.pub
sudo apt-get update
sudo apt-get install cuda-9.0
sudo apt-get install cuda-libraries-9-0 # will install libcublas.so
```

install cudnn

https://developer.nvidia.com/rdp/cudnn-download # for tensorflow-gpu

download  libcudnn7_7.3.1.20-1+cuda9.0_amd64.deb
```
sudo dpkg -i libcudnn7_7.0.3.11-1+cuda9.0_amd64.deb
```

15. add path of cuda-9.0 in the ~/.bashrc
```
#cuda
cuda_10path="/usr/local/cuda-9.0"
cuda_path="/usr/local/cuda"
export LD_LIBRARY_PATH=${LD_LIBRARY_PATH}:${cuda_10path}/lib64
export LD_LIBRARY_PATH=${LD_LIBRARY_PATH}:${cuda_path}/lib64
export PATH="${cuda_10path}/bin:${PATH}"
export PATH="${cuda_path}/bin:${PATH}"
```
```
source ~/.bashrc #then run nvcc command
```
17. install anaconda 
https://mirrors.tuna.tsinghua.edu.cn/anaconda/archive/

download Anaconda3-4.3.0-Linux-x86_64.sh
```
bash Anaconda3-4.3.0-Linux-x86_64.sh
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
conda config --set show_channel_urls yes
```
install pytorch, choose 0.4 version

https://pytorch.org/get-started/previous-versions/

18. install tensorflow
```
pip3  install tensorflow-gpu
```
