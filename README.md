# UNREALEASED DO NOT USE YET

# Kliper on Prusa MK3s
Structured Klipper config for Prusa MK3s/MK3s+ 3D printer, inspired by https://github.com/Rat-OS/RatOS-configuration

## Hardware needed
- the best option is to use a Rpi zero (2w preferred) but if you prefer raspberry pi 2/3/4 can also be used with a bit more work and a few more wires
- adxl345 either fysetc [portable shaper](https://amzn.to/3CZuCLm) for pi 2/3/4 or
-  standard [adxl345](https://amzn.to/3XMmTZa) 
-  I recommend using these headers [male](https://www.digikey.com/en/products/detail/adam-tech/BHR-08-VUA/9832409) and [female](https://www.digikey.com/en/products/detail/adam-tech/FCS-08-SG/9832361) when using the standard ADXL345
-  if using pi zero you'll need [male headers](https://amzn.to/3XND883) so can mount directly to einsy board
-  

## Pre-Check

- Get Z offset value from your current firmware (Menu -> Calibration -> Z-offset), you will need it for the Klipper config.
- Your bed needs to be perpendicular (based on XYZ Calibration). If not you will have to do the skew calibration before printing or you risk crashing your nozzle to the bed.
- Read https://github.com/dz0ny/klipper-prusa-mk3s/blob/main/printer.template.cfg
- Read https://www.klipper3d.org/Installation.html#building-and-flashing-the-micro-controller

## Install
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
6. Power off your printer and plug in your pi to the einsy board per [Prusa's official instructions](https://help.prusa3d.com/en/article/raspberry-pi-zero-w-preparation-and-installation_2180)

7. Power on your printer and navigate to "yourHostName".local/ 
8. Navigate to the "Machine" tab and scroll down the "Update Manager" if you see a lot of red buttons that say "invalid" or "error" then give the board 5 minutes and then reboot the board using the power button in the top right of the Mainsail interface
9. navigate back to the update manager and if the buttons are still red, then wait about 5 minutes. They will eventually populate. There will be a button at the bottom that says "Update All Components" Click this to update all packages to the latest. 
10. Update all components under Machine tab, otherwise config might not be able to load
11. {stopped here need to finish updating instructions}Clone config ```git clone https://github.com/dz0ny/klipper-prusa-mk3s.git ~/klipper_config/config```
12. Add the following to the to `moonraker.conf` to enable automatic updates

```yml
[update_manager prusa]
type: git_repo
origin: https://github.com/dz0ny/klipper-prusa-mk3s.git
path: ~/klipper_config/config
primary_branch: main
is_system_service: False
managed_services: klipper
```

2. Copy https://github.com/dz0ny/klipper-prusa-mk3s/blob/main/printer.template.cfg to `printer.cfg` in your klipper config
3. Adjust config to your hardware
4. Flash Klipper to your printer https://www.klipper3d.org/Installation.html#building-and-flashing-the-micro-controller

You will still need a USB cable as you cannot flash via an internal serial port. You can also use any other computer to compile your firmware.

To use this config, the firmware should be compiled for the AVR atmega2560. To use via serial, in "make menuconfig" select "Enable extra low-level configuration options" and select **serial1** (the RasPi serial) or **serial0** when you plan to connect via the USB.

To flash:
`make flash FLASH_DEVICE=/dev/serial/by-id/usb-Prusa_Research__prusa3d.com__Original_Prusa_i3_MK3_CZPX0620X004XK70128-if00`

7. Print


## Nice things
[Klipper mesh on print area only install guide](https://gist.github.com/ChipCE/95fdbd3c2f3a064397f9610f915f7d02)


## Screenshots
![image](https://user-images.githubusercontent.com/239513/141822711-2818978e-2b87-4110-9b93-e5f489c9cdc7.png)
![image](https://user-images.githubusercontent.com/239513/141831204-89ced257-e67f-4b1f-add7-a3806cdd2617.png)
![image](https://user-images.githubusercontent.com/239513/141831245-11476041-240d-424a-8ff8-ffd8a03c08be.png)
![image](https://user-images.githubusercontent.com/239513/141831272-31b88652-ab3f-4978-8a4c-c54a83817dd1.png)
