# UNREALEASED DO NOT USE YET

# Kliper on Prusa MK3s
Structured Klipper config for Prusa MK3s/MK3s+ 3D printer, inspired by https://github.com/Rat-OS/RatOS-configuration

## Hardware needed
- the best option is to use a Rpi zero (2w preferred) but if you prefer raspberry pi 2/3/4 can also be used with a bit more work and a few more wires
- adxl345 either fysetc [portable shaper](https://amzn.to/3CZuCLm) for pi 2/3/4 or
-  standard [adxl345](https://amzn.to/3XMmTZa) 
-  I recommend using these headers [male](https://www.digikey.com/en/products/detail/adam-tech/BHR-08-VUA/9832409) and [female](https://www.digikey.com/en/products/detail/adam-tech/FCS-08-SG/9832361) when using the standard ADXL345
-  if using pi zero you'll need [male headers](https://amzn.to/3XND883) so can mount directly to einsy board
-  micro-usb to usb-b (printer cable) or a [micro-usb adapter](https://amzn.to/3J5S6SW) if you already have a usb-b cable

## Pre-Check

- Get Z offset value from your current firmware (Menu -> Calibration -> Z-offset), you will need it for the Klipper config.
- Your bed needs to be perpendicular (based on XYZ Calibration). If not you will have to do the skew calibration before printing or you risk crashing your nozzle to the bed.
- Read https://github.com/dz0ny/klipper-prusa-mk3s/blob/main/printer.template.cfg
- Read https://www.klipper3d.org/Installation.html#building-and-flashing-the-micro-controller

## Installing Mainsail on your Raspberry Pi
1. Download [Pi Imager](https://downloads.raspberrypi.org/imager/imager_latest.exe), install it and open it
2. 
   a. Select "Choose OS" > "Other specific-purpose OS" > "3D Printing" > "Mainsail OS" > select the 32 bit version
   
   b. Select "Choose Storage" > select the sd card that you want to flash
   
   c. Next click the gear icon set the hostname, enable SSH, set a username and pass, put in your wifi credentials and select your country
   
   d. click "Save" and then you're ready to Write!
4. For Pi zero users, solder the headers onto the pi per [Prusa's official instructions](https://help.prusa3d.com/en/article/raspberry-pi-zero-w-preparation-and-installation_2180) but do not plug it into the einsy board yet. 
5. a. (optional step) If you do not want to use input shaping then you're done with the hardware setup and can move on to step 6 where we begin configuring Mainsail! 
   
   b. If you want to use input shaping then we'll solder on our header to this position [insert image of pi + pins here]
   
   c. [crimp wire instruction]
6. Power off your printer and plug in your pi to the einsy board per [Prusa's official instructions](https://help.prusa3d.com/en/article/raspberry-pi-zero-w-preparation-and-installation_2180) or power it with a micro USB

7. Power on your printer and navigate to "yourHostName".local/ 
8. Navigate to the "Machine" tab and scroll down the "Update Manager" if you see a lot of red buttons that say "invalid" or "error" then give the board 5 minutes and then reboot the board using the power button in the top right of the Mainsail interface
9. navigate back to the update manager and if the buttons are still red, then wait about 5 minutes. They will eventually populate. There will be a button at the bottom that says "Update All Components" Click this to update all packages to the latest. 
10. reboot the Pi again
11. in the "Machine" tab, open "printer.cfg" and adjust it to suit your hardware
    a. Go back to the github and download the repo
	b. go back to mainsail and select the "Machine" tab. Open the repo and drag everthing inside of the "config" folder into the "Config Files" section of the machine tab. 
	c. (optional) click the gear icon in "Config Files" and check the box for "Hidden Files" then drag and drop the .theme folder into the "Config Files" section. This will give you a cool Prusa Theme for Mainsail!
12.
13. (skip this step. it is not finished) Add the following to the to `moonraker.conf` to enable automatic updates

```yml
[update_manager prusa]
type: git_repo
origin: https://github.com/Fail-Fast-V0/klipper-prusa-mk3s/tree/config.git
path: ~/printer_data/config
primary_branch: main
is_system_service: False
managed_services: klipper
```
## Flashing Einsy and Setting up Communications between Rpi and Einsy
1. Power down your pi in mainsail. if you pi is plugged into your printer power that down too and remove the pi. Connect the a USB power source to the micro-usb port on the furthest edge of the board.  Plug your micro-usb to usb-b cable in the center most micro-usb port and then plug the other end into your Einsy mainboard. Turn your printer on if it isn't already. 
2. to flash klipper onto your einsy
    a. download putty and connect to your pi. Enter the following commands  

    ```yml
    cd ~/klipper/
    make menuconfig   
    ```
    Compile the firmware for  "AVR" "atmega2560". To connect your rpi zero over serial, select "Enable extra low-level configuration options" and select **UART1** (the RasPi serial) or **UART0** when you plan to connect via the USB.
    press "q" and exit

    ```yml
    make
    ```

    b. find the port for the usb cable

    ```yml
    ls /dev/serial/by-id/*
    ```

    Putty will return something like "/dev/serial/by-id/usb-1a86_USB2.0-Serial-if00-port0"
	
	copy this line and replace the corresponding line in the code below. Then run those commands in Putty
	
	```yml
	sudo service klipper stop
    make flash FLASH_DEVICE=/dev/serial/by-id/usb-1a86_USB2.0-Serial-if00-port0
    sudo service klipper start
    ```
	d. (optional) if you want to connect your rpi via usb instead of plugging it directly into the einsy. You need the "/dev/serial/by-id/...." line in the "[mcu]" section of your printer.cfg 

7. Print


## Nice things
[Klipper mesh on print area only install guide](https://gist.github.com/ChipCE/95fdbd3c2f3a064397f9610f915f7d02)


## Screenshots
![image](https://user-images.githubusercontent.com/239513/141822711-2818978e-2b87-4110-9b93-e5f489c9cdc7.png)
![image](https://user-images.githubusercontent.com/239513/141831204-89ced257-e67f-4b1f-add7-a3806cdd2617.png)
![image](https://user-images.githubusercontent.com/239513/141831245-11476041-240d-424a-8ff8-ffd8a03c08be.png)
![image](https://user-images.githubusercontent.com/239513/141831272-31b88652-ab3f-4978-8a4c-c54a83817dd1.png)
