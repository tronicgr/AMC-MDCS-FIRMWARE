# AMC-MDCS_FIRMWARE - Latest release can be found here:
https://github.com/tronicgr/AMC-MDCS-FIRMWARE/tree/master/Latest_Firmware


### Firmware procedure AMC-MDCS:
https://www.youtube.com/watch?v=yE1EBL972JY


### ======= enc_8dof_AMC-MDCS_2_03.hex =======

release date: 03/10/2020:

-Added spike filter that can be adjusted by two parameters:
- 1. Level: it defines the distance in data points that the input position have to exceed for the spike filter to kick in. This is absolute 16bit value 0-65535, default value 32767. If the spike filter kicks in too often, increase this value.
- 2. Enable or Disable the Spike Filter
- 3. Also added two stage positioning algorithm to move faster for large distances and slow down when near target for these artificial movements.
- 4. Fixes the transition to online motion data if the Force-Offline switch is used, avoiding jumps or jolts in the motion.
- 5. Fixed the position seek during spike filtering to to a range instead a point to allow catching up to real time motion data immediately after a crash.
- 6. Added Range parameter for the Spike Filter to better fine tune for lower values of the spike filter value.
- 7. The YELLOW LED will light steady during spike filter being engaged and will revert to GREEN LED when it catches up to live data. To enable the indication LEDs you need to set DIP switch #2 to ON position.
- 8. The LED's are not PWM, they are solid ON now.

This new Spike Filter works by detecting spikes in position data larger that the defined level (most likely crashes) and it will automatically activate the Force Offline mode, that places the actuators in seek position mode until all motors catch up with live motion data. So for the duration of the crash the motors will avoid doing any intense jolts and most likely the motion will come back on when the vehicle is standing still. It should work wonders in long duration crashes like when you have rollover down a bank for example...




### ======= enc_8dof_AMC-MDCS_2_01.hex =======
```
release date: 08/01/2019: 
-Small bugfix where the the stroke position was set to 100% with Emergency Switch activated on power on.
```


### ======= enc_8dof_AMC-MDCS_2_00.hex =======
```
release date: 7/3/2019:
  -OFFICIAL Release 2.00
  -Fixed delayed start on power on for 4DOF + TL to allow slow movement of traction loss actuator to middle position
  -Provides access to parameters via AMC Config utility
  -Added indication LEDs outputs for the various states of the controller
    Green = Motor online
    Yellow = Standby
    Yellow+Red = Park
    Red = Forced Offline
  -Added wiring diagram for the indication LEDs, see below.
```
  
### Wiring Diagram for Indication LEDs

![Alt Text](https://github.com/tronicgr/AMC-MDCS-FIRMWARE/blob/master/Indication%20LEDs%20for%20AMC-MDCS.jpg)

### The mosfet driver for the 12v indication LEDs can be found here:
https://www.amazon.com/dp/B07NWD8W26/
```
Search for: "Anmbest 5PCS DC 5V-36V Dual High-Power MOSFET Drive Module 0-20KHz PWM Brightness Control"
```

### ======= enc_6dof_AMC-MDCS_SERVO_1.4_rev3.9_Auto_Park_4DOF_TL.hex ======= 
```
release date: 5/2/2019: 
  -New ID (FF FF) and support for 8DOF
  -Soft start of the platform from Park position
  -Automatic Park when motion software stops sending motion data.
  -Optional Button to switch between Park and Standby positions of the platform 
    (configurable by parameters in LCD menu)
  -Emergency Switch that disables drives
  -Force Offline switch if you need to pause motion and manually park the platform 
    without closing the motion software or game.
  -Slow movements manual test buttons (from the AMC panel)
  -Added parameters access via terminal
  -Added restoring default parameters by holding Menu Exit button while pressing Reset button
   A message should appear on the screen that "default parameters are restored", 
   you can release the Menu Exit button then.
```


### Videos related to enc_6dof_AMC-MDCS_SERVO_1.4_rev3.9_Auto_Park_4DOF_TL release added functions:
https://www.youtube.com/watch?v=NtrdQnRq1GI

### Wiring Diagram for emergency buttons and switches
![Alt Text](https://github.com/tronicgr/AMC-MDCS-FIRMWARE/blob/master/AMC-MDCS%20park-standby-emergency-force-offline%20diagram2.jpg)



### The parameters can be changed via terminal (250000 bps)

### ---List of commands---

Command Number | Display Parameter | Save Parameter
------------| ------------ | -------------
CMD01 |Motornumber: | spv012-spv018
CMD04 |Park Position: | spv04001-spv04254
CMD05 |Park Move Speed: | spv05001-spv05100
CMD06 |Park Move Timeout: | spv0601-spv0690
CMD07 |Standby Position: | spv07010-spv07245
CMD08 |Standby Speed: | spv08000-spv08100
CMD09 |Standby Timeout: | spv0901-spv0990
CMD10 |Disable park type: | spv111-spv114
CMD13 |Actuator Limits: | spv1300-spv1350
CMD14 |Kill switch mode: | spv141-spv142
CMD44 |Display all parameters 
CMD45 |Print this help page 
CMD55 |Print delimited parameter list for simtools
spv45 |Saves all parameters at once
RQM |  Displays model,revision and number of motors
Park | Parks the actuators if in standby mode

```
  Some Commands may not change value - locked

The CMD$$ displays each parameter, and spv$$### saves each parameter with the value indicated. 
To actually store the parameters in the flash memory you need to send "spv45" to save all 
parameters at once. The "$$" on the spv is the command number, and the "###" is the value, 
Some parameters have single digit value, some two digit value and some 3 digit value. 
All values are characters!

Here is a list of the default parameters values you should get when you issue the CMD44 command
(if not like this, you may reset the default parameters via button combination)

01.Motornumber 2-8: 4
04.Park Position 0-254: 1
05.Park_Move_Speed 1-100%: 11
06.Park_Move_Timeout 1-90: 5
07.Standby Position 10-245: 127
08.Standby Speed 0-100%: 24
09.Standby Timeout 1-90: 5
10.Disable park type 1-4: 1
13.Actuator Limits 0-50%: 1
14.Kill switch mode 1-2: 1

CMD55 returns the following numeric values separated by colon ( : ) punctuation mark:
"data:" <Motornumber> ":" <Parkposition> ":" <Parkmovespeed> ":" <Parkmovetimeout> ":" <StandbyPosition> ":" <StandbySpeed> ":"<StandbyTimeout> ":" <Disableparktype> ":" <ActuatorLimits> ":" <Killswitchmode> ":" <Firmwareversion> ":" <AMCModel>

```
