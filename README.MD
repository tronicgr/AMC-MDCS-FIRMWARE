# AMC-MDCS_FIRMWARE - Latest release can be found here:
https://github.com/tronicgr/AMC-MDCS-FIRMWARE/tree/master/Latest_Firmware


### Firmware procedure AMC-MDCS:
https://www.youtube.com/watch?v=yE1EBL972JY


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
