# Fast Robots @Cornell, Spring 2023

[Return to main page](index.md)

# Lab 8 Stunts!

## Objective
The purpose of this lab is to combine everything you've done up till now to do fast stunts. _This_ is the reason you labored all those long hours in the lab carefully soldering up and mounting your components!
Your grade will be based partially on your hardware/software design and partially on how fast your robot manages to complete the stunt (relative to everyone else in class). We will also have everyone vote on the coolest stunt and the best blooper video - the top picks will receive up to 2 bonus points.   

## Parts Required

* 1 x [R/C stunt car](https://force1rc.com/products/cyclone-remote-control-car-for-kids-adults)
* 1 x [SparkFun RedBoard Artemis Nano](https://www.sparkfun.com/products/15443)
* 1 x [USB cable](https://www.amazon.com/SUMPK-Charging-Braided-Compatible-Samsung/dp/B08R68T84N/ref=sr_1_4?keywords=usb+c+to+c&qid=1636380583&qsid=147-6677549-1776715&refinements=p_n_feature_ten_browse-bin%3A23555327011&rnid=23555276011&s=pc&sr=1-4&sres=B08D9SB161%2CB08R68T84N%2CB01CZVEUIE%2CB01FM51812%2CB07VCZV3R4%2CB075V68NVR%2CB075GMKZWW%2CB093BVBRJT%2CB09BBBJ33F%2CB09C2D9Z7T%2CB012V56D2A%2CB092CYFQMP%2CB081L4V3DN%2CB07Y6ZJT1D%2CB07Y2XKPX5%2CB07VPYJV8V%2CB07THJGZ9Z%2CB08W2TP2TT%2CB0744BKDRD%2CB07THFJ1J5&srpt=ELECTRONIC_CABLE)
* 2 x [Li-Po 3.7V 650mAh (or more) battery](https://www.amazon.com/URGENEX-Battery-Rechargeable-Quadcopter-Charger/dp/B08T9FB56F/ref=sr_1_3?keywords=lipo+battery+3.7V+850mah&qid=1639066404&sr=8-3)
* 2 x [Dual motor driver](https://www.digikey.com/en/products/detail/pololu-corporation/2130/10450426)
* 2 x [4m ToF sensor](https://www.pololu.com/product/3415)
* 1 x [9DOF IMU sensor](https://www.digikey.com/en/products/detail/pimoroni-ltd/PIM448/10246391)
* 1 x [Qwiic connector](https://www.sparkfun.com/products/14426)

## Lab Procedure

### Controlled Stunts

All of these stunts must be performed on the tracks setup in the lab (or hallway outside of the lab). We have set up crash pads which should help prevent excessive damage when vehicles go rogue. We will need video evidence that your stunt works at least three times, and graphs showing the sensor data, KF output (if applicable), and motor values with time stamps. 

#### Task A: Position Control

Your robot must start at the designated line (<4m from the wall), drive fast forward, and upon reaching the sticky matt with a center located 0.5m from the wall, perform a flip, and drive back in the direction from which it came. 
   - Your stunt will be considered successful if your robot manages to do a flip. The score will depend on how quickly (if at all) you make it back past the initial starting line. If your robot runs off at an angle without re-crossing the starting line this is a failed run. 
   - Disengage your PID position control, and give the robot a set, fast speed (to do your flip, you need the robot to go fast, therefore it should not be slowing down as it would if it was doing PID control on the position). If you want to go above and beyond and create a very robust solution, consider doing PID control on the speed of the robot instead of the position (this is optional).  
   - The TOF sensors are not all calibrated equally well; manually tuning the distance to the wall for which your robot initiates the flip is fine.
   - If you are using the Kalman Filter to estimate the distance to the wall quickly, do the following. When you have new sensor readings, run both the prediction and the update step. When you don't have new sensor readings, run only the prediction step (i.e. compute your estimated distance based only on the motor inputs and your dynamics model). Remember to adjust your discrete A and B matrices to the new sample rate and your input to the new PWM values.  

[![Lab 8, Task A](https://img.youtube.com/vi/cffupvOlyUM/1.jpg)](https://youtu.be/cffupvOlyUM "Lab 8, Task A")


#### Task B: Orientation Control

Your robot must start at the designated line (<4m from the wall), drive fast forward, and when the robot is within 3ft (914mm = 3 floor tiles in the lab) from the wall, initiate a 180 degree turn. 
   - The TOF sensors are not all calibrated equally well; manually tuning the distance to the wall for which your robot initiates the turn is fine.
   - If you are using the Kalman Filter to estimate the distance to the wall quickly, do the following. When you have new sensor readings, run both the prediction and the update step. When you don't have new sensor readings, run only the prediction step (i.e. compute your estimated distance based only on the motor inputs and your dynamics model). Remember to adjust your discrete A and B matrices to the new sample rate and your input to the new PWM values.  
  
[![Lab 8, Task B](https://img.youtube.com/vi/d2JvpHIE_Pg/1.jpg)](https://youtu.be/d2JvpHIE_Pg "Lab 8, Task B")

### Extra credit for coolest stunt and best bloopers!

Everyone, including your teaching team, will be the chance to vote for the coolest stunt video and the best blooper. We will send you a link to vote after Spring break. The highest scores will be given up to 2 extra credits!

---

## Write-up

To demonstrate that you've successfully completed the lab, please upload a brief lab report (<800 words), with code snippets (not included in the word count), photos, graphs, and/or videos documenting that everything worked and what you did to make it happen. 
