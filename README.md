# Blood_Pressure_Gadget
# OBJECTIVE:
The objective of this project work was to build a semi-automated blood pressure/HR measuring device. The device built must be capable of measuring two measurements, namely Systolic Pressure and Diastolic Pressure in measurements are in units of mm/Hg. Along with the mentioned values the device should also be capable of measuring the heart rate and displaying it to the user.

# APPARATUS:
For implementing the functionality multiple hardware components were used,namely:
1. STM32F429 DISCOVERY Board
2. Honeywell Pressure Sensor (MPRLS0300YG00001BB)
3. Manually inflatable blood pressure Cuff
4. F-F Jumper Wires, Connection Tubes.

# PROCEDURE:
The apparatus was set up and the below steps were followed, in the given order:
1. Wear the cuff on any one of the arms.
2. Start the program.
3. Keep increasing the pressure of the cuff until it reaches 200mmHg.
4. After reaching the threshold value of 200mmHg, start releasing the pressure by opening the pressure relief valve and keeping the pressure deflation at an ideal rate as displayed on the LCD Screen.
5. Record the data till the pressure drops to about 30mmHg.
6. Check for the final result, color coded according to the severity of the Blood Pressure values.

# CODE IMPLEMENTATION:
Main objective was to find the systolic and diastolic pressure using the pressure values obtained during the deflating process. First the calculates and stores the pressure in the cuff and at a time interval of 0.2 seconds. These values are again used to calculate the systolic and diastolic pressure. Also with the number of ripples , a good estimate of heartbeats per minute can be obtained.

Approach to calculate the Systolic Pressure:
1. A ripple is a positive slope. When the blood starts flowing again as we go below the systolic blood pressure, we see ripples in our graph.
2. From the point of systolic blood pressure, we would see ripples continuously, thus the graph would alternate between positive and negative slope.
3. The first positive slope in this pattern of positive and negative slopes is the systolic pressure. 
4. Keeping this approach in mind, it is also possible to eliminate "noise", which prevents the device from detecting an outlier positive slope.
5. The logic used for avoiding outlier is:
a) The sampling rate is 0.2ms. Hence, we get 5 readings in 1 second.
b) When we read a positive slope, we check for consistency in the next 20 readings. Assuming heartbeats to be at an average of 70-100, We would get about 1 to 2 heartbeats every second
c) As each heartbeat would cause a positive slope; In 20 readings, which is 4 seconds, we should at least see 4 positive slopes
d) Hence, a positive slope will indicate a systolic value when the next 20 pressure readings have at least 4 positive slope readings

Approach to calculate the Mean Arterial Pressure(MAP):
1. Mean Arterial Pressure is at the point where we see the longest/highest ripple when the blood is flowing while deflating.
2. We implement the algorithm "Longest Increasing Subsequence" to find this pressure value.
3. Our goal is to find the longest positive slope. Hence, we first look for a positive slope reading.
4. Then we check the next reading, and if that slope is also positive, it means the ripple is higher than just the previous difference.
5. At the end , we calculate the magnitude of all positive slopes and retrieve the highest, for the Mean Arterial Pressure.

Approach to calculate the Diastolic Pressure:
Since MAP is roughly the midpoint of Systolic and Diastolic Blood Pressure, we can calculate diastolic conveniently, once we have the other two values. Assuming MAP as midpoint, evaluate diastolic from systolic and MAP values

Approach to calculate the Heartbeat BPM:
Theory: The number of peaks or ripples in our readings in a minute time will give heartbeats per minute for a person
Approach:
1. From the raw readings, sample 75 readings.
2. 75 readings take 15 seconds, which should be sufficient for calculating heartbeats.
3. We use the readings around our MAP value/index, considering that's the mean point.
4. A peak can be identified by checking the change in slope. Every time our consecutive slope reading goes from positive to negative, we can infer that we just crossed a peak.
5. We count all peaks in these 75 readings, which give us the number of heartbeats in 15 seconds. 6. Multiplying that by 4, we get heartbeats per minute.

# ADDITIONAL FUNCTIONALITIES IMPLEMENTED:
1. Smooth and fun user interface which makes the user interaction better.
2. Clear display of messages on the LCD screen which guides the user through the process.
3. Real time pressure tracking system available along with the Deflation Rate status which
can be used to control the valve’s pressure to get the accurate final pressure values.
4. Final pressure values displayed with color codes based on the severity.
5. Display of alert messages when the pressure reaches the threshold and also when the deflation rate is fast.
