# nvida_drivers
1) Verify the system has a CUDA capable GPU:


        lspci | grep -i nvidia #NVIDIA Corporation GK208M [GeForce GT 740M] 
    
        ubuntu-drivers devices

                == /sys/devices/pci0000:00/0000:00:01.0/0000:01:00.0 ==
                modalias : pci:v000010DEd00001292sv0000103Csd0000219Abc03sc02i00
                vendor   : NVIDIA Corporation
                model    : GK208M [GeForce GT 740M]
                driver   : nvidia-driver-390 - distro non-free recommended
                driver   : nvidia-340 - distro non-free
                driver   : xserver-xorg-video-nouveau - distro free builtin

Cleanup all nvidia package. At this step we will remove all nvidia related packages.

    sudo apt-get remove nvidia* && sudo apt autoremove




2) [Blacklist Nvidia nouveau driver](https://linuxconfig.org/how-to-disable-nouveau-nvidia-driver-on-ubuntu-18-04-bionic-beaver-linux)

Create the blacklist file, [Nvidia official](https://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html#runfile-nouveau) 

      sudo bash -c "echo blacklist nouveau > /etc/modprobe.d/blacklist-nvidia-nouveau.conf"
      sudo bash -c "echo options nouveau modeset=0 >> /etc/modprobe.d/blacklist-nvidia-nouveau.conf"

Update kernel initramfs

      sudo update-initramfs -u

3) Disable Nouveau Driver:
Check if using NEUVEAU drivers
      
      
      lsmod | grep nouveau

Disable **nouveau driver** by changing the configuration **/etc/default/grub** file. Add the **nouveau.modeset=0** into line starting with **GRUB_CMDLINE_LINUX** . Or alternativly add the **rd.driver.blacklist=nouveau nouveau.modeset=0**
    
    ....
    GRUB_CMDLINE_LINUX="crashkernel=auto rhgb quiet nouveau.modeset=0"  
    or
    GRUB_CMDLINE_LINUX_DEFAULT="quiet splash modprobe.blacklist=nouveau"
    ....

If you change this file, run **update-grub** afterwards to update. 

    update-grub
  
The above line ensures that the nouveau driver is disabled the next time you boot your Linux system.


Reboot




