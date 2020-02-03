# i965-Driver-Fix
I put together a guide for fixing anyone's 9600/9700k that do not work with HW transcoding on ubuntu 18.04 or lower. All credits for the repo and packages used in this guide go to https://launchpad.net/~wsnipex.

## Where to Start: The Basics

If you are utilizing this fix on any truly unmanaged server you will need to set your system time first, otherwise you will run into errors with time being off and nothing will download or install.

You will also need to ensure that you have igpu enabled on your motherboard first.

1. `timedatectl list-timezones` - Find your time zone to enter on the next step.
2. `timedatectl set-timezone { America/Chicago } && timedatectl set-ntp true && sudo systemctl restart systemd-timesyncd` - In between the brackets ensure that you put your timezone and remove the brackets before running this command.
3. `sudo apt install software-properties-common` - This may already be installed but I'm assuming you have not run any commands. Ditto for a lot of these commands.
4. `sudo apt install vainfo` 
5. `sudo vainfo` - If you do not have the correct libva or i965 driver you will see a similar image to this: 

![vainfo](https://i.imgur.com/BBYmXaT.png)

6. `sudo add-apt-repository ppa:wsnipex/vaapi` - This is the repo needed for the backported driver.
7. `sudo apt-get update` - To refresh the packages after adding the one above.
8. `sudo apt-get install i965-va-driver` - This is the bread and butter here. This should install any dependencies needed for this driver as well such as libva. 
9. `sudo vainfo` - And now for the final step and hopefully everything is now working and your `vainfo` should now look like this: 

![vainfo2](https://i.imgur.com/rmADp1t.png)

Hopefully this is everything you need to get your igpu working on your server.

##Troubleshooting

If you have an issue where your vainfo still does not show the correct information there is two ways to try and resolve this issue.

1. You do not have your iGPU enabled in the BIOS of your motherboard. Ensure it is turned on and set correctly.
2. If your BIOS is 100% correct then this will be the next step.
   - `export LIBVA_DRIVERS_PATH=/usr/lib/x86_64-linux-gnu/dri/`
   - `export LIBVA_DRIVER_NAME=i965`

This should fix any issues and assign the i965 driver to be utilized instead of the i915 or any other installed on the machine.

I've had a question on why I do not use the iHD driver (intel media driver). The reason for this is it needs to be compiled on individual setups. This is not condusive for most people. This is the reason why.

So here is the link to the Intel media driver if you are interested in pursuing this route but I provide no support for this method.


https://github.com/intel/media-driver/wiki/How-to-build-and-use-media-driver%3F
