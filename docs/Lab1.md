# Fast Robots @Cornell, Spring 2023

[Return to main page](index.md)


# Lab 1: The Artemis board

## Objective
The purpose of this lab is for you to setup and become familiar with the Arduino IDE and the Artemis board. After this lab, you should be comfortable programming your board, using the board LED, reading/writing serial messages over USB, and using the onboard temperature sensor and Pulse Density Microphone.

## Parts Required

(These will be handed out during lab 1 - note if you decide to drop the class please give these back to a TA.)

* 1 x SparkFun RedBoard Artemis Nano
* 1 x USB C-to-C or A-to-C cable

## Prelab
Install the [Arduino IDE](https://www.arduino.cc/en/main/software) on your computer. Please use the latest versions of ArduinoIDE and Sparkfun Appollo3 support software. Update them if necessary. If you have any issues, please contact the TA team.
While we only guarantee TA support on the lab computers this semester, you can likely do all Arduino-related tasks on your own computer which will save everyone time! 
   
Check out the Artemis description, features, and helpful forums here:
1. [SparkFun RedBoard Artemis Nano](https://www.sparkfun.com/products/15443). *Note that this is a 3V board, NOT 5V, but 3V. Please. Remember. 3V. Inputs. Only.*
2. [Setup instructions](https://learn.sparkfun.com/tutorials/artemis-development-with-arduino?_ga=2.30055167.1151850962.1594648676-1889762036.1574524297&_gac=1.19903818.1593457111.Cj0KCQjwoub3BRC6ARIsABGhnyahkG7hU2v-0bSiAeprvZ7c9v0XEKYdVHIIi_-J-m5YLdDBMc2P_goaAtA4EALw_wcB)
3. [Artemis forums](https://forum.sparkfun.com/viewforum.php?f=167&sid=b66b1e5b6aa9711dc4d064c60e6ad159)

## Instructions

*For all of the following tasks, think about how you will document that your code works. We cannot grade "I did this..". Instead, you can choose to upload photos, screenshots of the serial monitor, screen-recordings, videos of the board, and/or grab the data from the serial monitor and plot them in a graph. As always, feel free to check [last years solutions](https://cei-lab.github.io/ECE4960-2022/StudentPages.html) for examples.* 

1. Hook the Artemis board up to your computer, and follow the instructions from bulletpoint 2 above ("Introduction" and "Arduino Installation"). Typical issues encountered include...    
    1. Bad connections, because the USB connector needs to be pressed fully into the Artemis board.
    2. If you are running Windows and the first compilation takes a long time, try adding "C:\Program Files\Arduino" (or the particular project folder) to the antivirus exclusions. 
2. From the setup instructions linked above, follow the instructions in "Example: Blink it Up". (Note: you may need to slow the baud rate down for it to work.)
3. In File->Examples->Artemis Examples, run Example2_Serial. (Note: to view the output and provide input open the serial monitor in the upper right hand corner of the script window.)
4. In File->Examples->Artemis Examples, run Example4_analogRead to test your temperature sensor. Try blowing on or touching the chip to change its temperature. It may take a while to transfer your heat. 
5. In File->Examples->PDM, run Example1_MicrophoneOutput to test your microphone. E.g. try whistling or speaking to change the highest frequency.

# Additional tasks for 5000-level students

6. Program the board to turn on the LED when you play a musical "A" note over the speaker, and off otherwise. Use your phone, computer, or similar to generate the sound. If you're having fun you could even combine the microphone and the Serial output to generate an electronic tuner.

## Write-up
To demonstrate that you've successfully completed the lab, please upload to your site a brief lab report (<600 words), with pseudo-code/code snippets (not included in the word count), photos, graphs, and/or videos documenting all steps work and what you did to make it happen.  




