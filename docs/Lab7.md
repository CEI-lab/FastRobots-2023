
# Fast Robots @Cornell, Spring 2023

[Return to main page](index.md)

# Lab 7: Kalman Filter

## Objective

The objective of Lab 7 is to implement a Kalman Filter, which will help you execute the behavior you did in [Lab 6](Lab6.md) faster. The goal now is to use the Kalman Filter to supplement your slowly sampled ToF values, such that you can speed towards the wall as fast as possible, then either stop 1foot from the wall (if you chose Task A, position control) or turn within 2ft of the wall (if you chose Task B, orientation control). Note that part of the lab 8 grade is based on the speed of your solution.  

## Parts Required

* 1 x [R/C stunt car](https://force1rc.com/products/cyclone-remote-control-car-for-kids-adults)
* 1 x [SparkFun RedBoard Artemis Nano](https://www.sparkfun.com/products/15443)
* 1 x [USB cable](https://www.amazon.com/SUMPK-Charging-Braided-Compatible-Samsung/dp/B08R68T84N/ref=sr_1_4?keywords=usb+c+to+c&qid=1636380583&qsid=147-6677549-1776715&refinements=p_n_feature_ten_browse-bin%3A23555327011&rnid=23555276011&s=pc&sr=1-4&sres=B08D9SB161%2CB08R68T84N%2CB01CZVEUIE%2CB01FM51812%2CB07VCZV3R4%2CB075V68NVR%2CB075GMKZWW%2CB093BVBRJT%2CB09BBBJ33F%2CB09C2D9Z7T%2CB012V56D2A%2CB092CYFQMP%2CB081L4V3DN%2CB07Y6ZJT1D%2CB07Y2XKPX5%2CB07VPYJV8V%2CB07THJGZ9Z%2CB08W2TP2TT%2CB0744BKDRD%2CB07THFJ1J5&srpt=ELECTRONIC_CABLE)
* 2 x [Li-Po 3.7V 650mAh (or more) battery](https://www.amazon.com/URGENEX-Battery-Rechargeable-Quadcopter-Charger/dp/B08T9FB56F/ref=sr_1_3?keywords=lipo+battery+3.7V+850mah&qid=1639066404&sr=8-3)
* 2 x [Dual motor driver](https://www.digikey.com/en/products/detail/pololu-corporation/2130/10450426)
* 2 x [4m ToF sensor](https://www.pololu.com/product/3415)
* 1 x [9DOF IMU sensor](https://www.digikey.com/en/products/detail/pimoroni-ltd/PIM448/10246391)
* 1 x [Qwiic connector](https://www.sparkfun.com/products/14426)

We will have a few setups in and just outside of the labs with crash-pillows mounted along the wall to limit damages. If you practice at home, be sure to do the same!


## Lab Procedure

### 1. Estimate drag and momentum 

To build the state space model for your system, you will need to estimate the drag and momentum terms for your A and B matrices. Here, we will do this using a step response. Drive the car towards a wall while logging motor input values and ToF sensor output. 
  1. Choose your step-size to be of similar size to the PWM value you used in Lab 6 (to keep the dynamics similar). If you never specified a PWM value directly, but rather computed one using your PID controller, pick something on the order of the maximum PWM value that was produced.  
  2. Make sure your step time is long enough to reach steady state (you likely have to use active breaking of the car to avoid crashing into the wall).
  3. Show graphs for the TOF sensor output, the (computed) speed, and the motor input. Please ensure that the x-axis is in seconds.
  4. Measure both the steady state speed and the 90% rise time.

### 2. Initialize KF 

1. Compute the A and B matrix given the terms you found above, and discretize your matrices. Be sure to note the sampling time in your write-up.

   ```cpp
   Ad = np.eye(n) + Delta_T * A  //n is the dimension of your state space 
   Bd = Delta_t * B
   ```

2. Identify your C matrix. Recall that C is a m x n matrix, where n are the dimensions in your state space, and m are the number of states you actually measure.
   - This could look like C=np.array([[-1,0]]), because you measure the negative distance from the wall (state 0).

3. Initialize your state vector, x, e.g. like this: x = np.array([[-TOF[0]],[0]])

4. For the Kalman Filter to work well, you will need to specify your process noise and sensor noise covariance matrices. 
   - Try to reason about ballpark numbers for the variance of each state variable and sensor input. 
   - Recall that their relative values determine how much you trust your model versus your sensor measurements. If the values are set too small, the Kalman Filter will not work, if the values are too big, it will barely respond.
   - Recall that the covariance matrices take the approximate following form, depending on the dimension of your system state space and the sensor inputs.

   ```cpp
   sig_u=np.array([[sigma_1**2,0],[0,sigma_2**2]]) //We assume uncorrelated noise, and therefore a diagonal matrix works.
   sig_z=np.array([[sigma_3**2]])
   ```

### 3. Implement and test your Kalman Filter in Jupyter

1. To sanity check your parameters, implement your Kalman Filter in Jupyter first. You can do this using the function in the code below (for ease, variable names follow the convention from the [lecture slides](TBD)). 
   - Import timing, ToF, and PWM data from a straight run towards the wall (you should have this data handy from lab 6).  
   - You may need to format your data first. For the Kalman Filter to work, you'll need all input arrays to be of equal length. That means that you might have to interpolate data if for example you have fewer ToF measurements than you have motor input updates. Numpy's linspace and interp commands can help you accomplish this. 
   - Loop through all of the data, while calling the Kalman Filter.
   - Remember to scale your input from 1 to the actual value of your step size (u/step_size).
   - Plot the Kalman Filter output to demonstrate how well your Kalman Filter estimated the system state.
   - If your Kalman Filter is off, try adjusting the covariance matrices. Discuss how/why you adjust them. 


```cpp
def kf(mu,sigma,u,y):
    
    mu_p = A.dot(mu) + B.dot(u) 
    sigma_p = A.dot(sigma.dot(A.transpose())) + Sigma_u
    
    sigma_m = C.dot(sigma_p.dot(C.transpose())) + Sigma_z
    kkf_gain = sigma_p.dot(C.transpose().dot(np.linalg.inv(sigma_m)))

    y_m = y-C.dot(mu_p)
    mu = mu_p + kkf_gain.dot(y_m)    
    sigma=(np.eye(2)-kkf_gain.dot(C)).dot(sigma_p)

    return mu,sigma
```

### 4. Implement the Kalman Filter on the Robot

Integrate the Kalman Filter into your Lab 6 PID solution on the Artemis. Before trying to increase the speed of your controller, use your debugging script to verify that your Kalman Filter works as expected. Be sure to demonstrate that your solution works by uploading videos and by plotting corresponding raw and estimated data in the same graph. 

The following code snippets gives helpful hints on how to do matrix operations on the robot:

```cpp
#include <BasicLinearAlgebra.h>    //Use this library to work with matrices:
using namespace BLA;               //This allows you to declare a matrix

Matrix<2,1> state = {0,0};         //Declares and initializes a 2x1 matrix 
Matrix<1> u;                       //Basically a float that plays nice with the matrix operators
Matrix<2,2> A = {1, 1,
                 0, 1};            //Declares and initializes a 2x2 matrix
state(1,0) = 1;                    //Writes only location 1 in the 2x1 matrix.
Sigma_p = Ad*Sigma*~Ad + Sigma_u;  //Example of how to compute Sigma_p (~Ad equals Ad transposed) 
```

If you want to get a head start on Lab 8, try speeding up your robot using your new KF to increase the execution time of your control loop. 

### Additional tasks for 5000-level students

You're off the hook in this lab.

---

## Write-up

To demonstrate that you've successfully completed the lab, please upload a brief lab report (<800 words), with code snippets (not included in the word count), photos, and/or videos documenting that everything worked and what you did to make it happen. 
