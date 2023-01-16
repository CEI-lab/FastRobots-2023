# Fast Robots @Cornell, Spring 2023

[Return to main page](index.md)

# Lab 5: Motors and Open Loop Control

## Objective

The purpose of this lab is for you to change from manual to open loop control of the car. At the end of this lab, your car should be able to execute a pre-programmed series of moves, using the Artemis board and two dual motor drivers. 

*Note that this is an extensive lab. If you finish labs 3-4 early, consider getting an early start on steps 1-4 below.*

## Parts Required

* 1 x [R/C stunt car](https://force1rc.com/products/cyclone-remote-control-car-for-kids-adults)
* 1 x [SparkFun RedBoard Artemis Nano](https://www.sparkfun.com/products/15443)
* 1 x [USB cable](https://www.amazon.com/SUMPK-Charging-Braided-Compatible-Samsung/dp/B08R68T84N/ref=sr_1_4?keywords=usb+c+to+c&qid=1636380583&qsid=147-6677549-1776715&refinements=p_n_feature_ten_browse-bin%3A23555327011&rnid=23555276011&s=pc&sr=1-4&sres=B08D9SB161%2CB08R68T84N%2CB01CZVEUIE%2CB01FM51812%2CB07VCZV3R4%2CB075V68NVR%2CB075GMKZWW%2CB093BVBRJT%2CB09BBBJ33F%2CB09C2D9Z7T%2CB012V56D2A%2CB092CYFQMP%2CB081L4V3DN%2CB07Y6ZJT1D%2CB07Y2XKPX5%2CB07VPYJV8V%2CB07THJGZ9Z%2CB08W2TP2TT%2CB0744BKDRD%2CB07THFJ1J5&srpt=ELECTRONIC_CABLE)
* 1-2 x [Li-Ion 3.7V 400mAh (or more) battery](https://www.amazon.com/URGENEX-Battery-Rechargeable-Quadcopter-Charger/dp/B08T9FB56F/ref=sr_1_3?keywords=lipo+battery+3.7V+850mah&qid=1639066404&sr=8-3)
* 2 x [Dual motor drivers](https://www.digikey.com/en/products/detail/pololu-corporation/2130/10450426)

## Prelab

Check out the [documentation](https://www.pololu.com/product-info-merged/2130) and the [datasheet](https://www.ti.com/general/docs/suppproductinfo.tsp?distId=10&gotoUrl=https%3A%2F%2Fwww.ti.com%2Flit%2Fgpn%2Fdrv8833) for the dual motor driver. 

Note that to deliver enough current for our robot to be fast, we will parallel-couple the two inputs and outputs on each dual motor driver, essentially using two channels two drive each motor. This means that we can deliver twice the average current without overheating the chip. While it is a bad idea to parallel couple motor drivers from separate ICs because their timing might differ slightly, you can often do it when both motor drivers exist on the same chip with the same clock circuitry.  
In your lab write-up, discuss/show how you decide to hook up/place the motor drivers. 
* What pins will you use for control on the Artemis? (It is worth considering both [pin functionality](https://cdn.sparkfun.com/assets/5/5/1/6/3/RedBoard-Artemis-Nano.pdf) and physical placement on the board/car).
* We recommend powering the Artemis and the motor drivers/motors from separate batteries. Why is that? 
* Consider routing paths given EMI, wire lengths, and color coding. Long wires may not fit in the chassis, and lead to unnecessary noise. Wires that are too short, will make repair harder. Using solid-core wire can cause problems when the car undergoes high accelerations. 


## Lab Procedure

1. Unpack your RC car and try it out! The purpose of this step is to get a feel for what the car is capable of before you take it apart. Be sure to capture a video, so you can judge things like speed, turn radius, stability, etc. 
2. Put the car aside and focus on the motor drivers next.
3. Connect the necessary power and signal inputs to run one motor driver (with parallel coupled outputs) from the Artemis. For now, keep the motor driver (VIN) powered from an external power supply with a controllable current limit; this will make debugging easier. 
   - What are reasonable settings for the power supply? 
4. Use analogWrite commands to generate PWM signals and show (using an oscilloscope) that you can regulate the power on the output for the motors. 
5. Take your car apart!
   - Remove the battery from your car. Unscrew and remove the top (blue) shell from your car. You may have to cut the wires for the chassis LEDs (we will not be using them in this class). *Don't loose the screws!!*
   - Locate and unmount the control PCB and cut wires to the motors and the battery connector as close to the board as possible.
   - Connect the 650mAh battery that came with the car to the Artemis board. You will need to solder on the JST connector. Be *careful* not to short the battery leads when you do this. Cut, solder, and add heat shrink to one wire at a time. 
   - Keep the motor driver powered on an external power supply for now, but remember to connect all grounds in your circuit. 
6. Place your car on its side, such that the spinning wheels are elevated, and show that you can run the motor in both directions. 
7. Hook up the motor driver to the original battery leads from the car, and attach a 850mAh battery instead of a power supply (double check color codes before you plug it in), and make sure your code works when the circuit is fully battery powered. 
8. Repeat the process for the second motor and motor driver. One 850mAh battery should be enough to power both motors. 
9. Install everything inside your car chassis, and try running the car on the ground. 
   - Remember, the car may flip, so try to avoid having components that stick out beyond the wheels.
   - Also, the car is very fast, so test in in the hallway and add a timer in code so that it stops automatically after a short amount of time. That way you don't have to try to catch it when it gets away from you!
   - Here is an example of a car with everything hooked up (note that we did not use QWIIC connectors in this one). Remember that the implementation details are entirely up to you:

<p align="center"><img src="Figs/MotorDriver.jpg" width="400"></p>

10. Explore the lower limit for which each motor still turns while on the ground; note it may require slightly more power to start from rest compared to when it is running. 
11. If your motors do not spin at the same rate, you will need to implement a calibration factor. To demonstrate that your robot can move in a fairly straight line, record a video of your robot following a straight line (e.g. a piece of tape) for at least 2m/6ft. The robot should start centered on the tape, and still partially overlap with the tape at the end. 

12. Demonstrate open loop, untethered control of your robot - add in some turns. 


## Additional tasks for 5000-level students

1. Consider what frequency analogWrite generates. Is this adequately fast for these motors? Can you think of any benefits to manually configuring the timers to generate a faster PWM signal?

2. Write a program that ramps up and down in speed slowly. Reporting the values to your computer using Bluetooth either during operation or when your ramp up/down procedure is over. Use this setup to document accurately what range of speeds you can achieve. (If your sensors are still attached, it may be easiest to use them to help you determine speed). 

## Write-up
To demonstrate that you've successfully completed the lab, please upload a brief lab report (<800 words), with code snippets (not included in the word count), photos, and videos documenting that everything work and what you did to make it happen. 

