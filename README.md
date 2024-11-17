# DJI-Mavic-Pro-drone-Modeling

The drone is a four-propeller drone modeled after the DJI Mavic Pro. The control system was designed to get the drone to hover at the desired position regardless of the weather conditions. The forces at play in this model are the motors’ thrust, gravity, drag and gusts, and motor propeller torque. Since the plant takes four motor speeds as inputs to spin the propellers and generate forces and torques that affect the output, the state of the drone was estimated by manipulating the four motors using the sensors’ output which are fed back into the controller. The system states being estimated are the position, velocity, acceleration, attitude, and altitude. The controller takes in these set points as well as the battery performance to estimate its state and calculate the DC motor commands which will calculate the necessary forces and torques fed to the propellers. The propellers then generate a thrust vector depending on the motors’ rotation angle and speed. Furthermore, there are three feedback controllers, for roll, pitch, and yaw, and we are feeding the estimated roll angle into the roll controller, the estimated pitch angle into the pitch controller, and the estimated yaw angle into the yaw controller. Besides roll, pitch, and yaw in the output, we are also measuring the throttle. 

The DJI Mavic Pro uses brushless DC motors, however, in this model, I used the provided motor constant. The RPM constant KV is expressed in RPM/Volt. 1400RPM/V, which tells us the back-emf generated in the motor at a certain speed, and how much voltage the motor generates as it rotates. 
*Assuming a 0% RPM drop from KV at 0V and a 25% drop from KV at max voltage (11.4V). 

Using the datasheet published by APC (a company that manufactures propellers) that has a detailed performance of its propellers the calculations of the propellers’ performance made using the practical approach rather than the theoretical approach since the exact geometry of the propellers, and the pressure differential around it at different velocities is unknown. These were used to calculate the thrust and torque of the propellers at different rotational velocities generated by the motor. 

[Datasheet](https://lnkd.in/e4x3kVqU)

# Results

![1706863859748](https://github.com/user-attachments/assets/14f10d3a-8713-4161-811d-7d7afdfdcf0f)

![1706863861665](https://github.com/user-attachments/assets/2e2aa192-aa10-4f40-863f-ebdb30ee8b31)

![1706863860335](https://github.com/user-attachments/assets/7e06cd90-7cba-4ff6-ad07-0f87225e16f3)

![1706863859781](https://github.com/user-attachments/assets/d7360fae-e133-4cd1-971c-2c95acff1a7d)

![1706863860548](https://github.com/user-attachments/assets/8d2ae67d-bb41-4e12-9632-b2cffff1e36d)

![1706863859799](https://github.com/user-attachments/assets/3c258623-9700-4f06-aff3-e59026b32ffc)

![1706863861104](https://github.com/user-attachments/assets/b3333248-1c30-4488-89a0-d3e4c89c1e69)

![1706863860674](https://github.com/user-attachments/assets/5ffdaf77-960a-4044-b8ef-b71e0359d6be)

![1706863860940](https://github.com/user-attachments/assets/b82433b7-a087-4fdb-87bf-9a43faa4160e)


# Drone characteristics, linear dynamics, rotational dynamics and formulas

Shape and Profiles:

By assuming the drone consists of two beams placed perpendicularly to each other we get:

<img width="990" alt="image" src="https://github.com/user-attachments/assets/80ae957d-e424-428e-8123-576311d3bd1e">

![image](https://github.com/user-attachments/assets/877578f7-041b-4205-b155-b7f0930eea5c)

Other Characteristics of the DJI Mavic Pro:
Weight: 0.743kg
Fontal Area: 0.0197m2
Motor KV: 1400 rpm/V
Battery Capacity: 3830mAh
Voltage: 11.4V
Max Discharge Current: 20C/77A



## Linear Dynamics:
According to the 2nd law of Newton, the sum of external forces applied to an object of mass m is equal to its mass multiplied by its acceleration:
In the x direction: Fx = max
In the y direction: Fy = may
In the z direction: F2 = maz

## Rotational Dynamics:
Rotational Movements:

About the x-axis (roll): Mx = Ix × θx
About the y-axis (pitch): My = Iy × θy
About the z-axis (yaw): Mz = Iz × θz

M: The external moments or torques in Nm.
I: The moments of inertia in kg.m2.
θ: The rotational acceleration in rad/s2.

## Moments of Inertia:
The moment of inertia of a rod of length L can be calculated as follows:

<img width="713" alt="image" src="https://github.com/user-attachments/assets/325361c0-34a6-46a2-b5d4-210f1f018d48">

The moment of inertia of a point mass about a principal axis of rotation:

<img width="655" alt="image" src="https://github.com/user-attachments/assets/fdfb2ea3-ab90-4467-aeb1-91444016225c">


## Torque and Propeller Rotation:

![image](https://github.com/user-attachments/assets/694fe7d4-db97-4af1-ac14-56bc79727b0b)

Decreasing/increasing the power in each of these couples independently induces yaw.
Mz= T4 - T1 + T2 - T3

## Thrust and Moments:

<img width="411" alt="image" src="https://github.com/user-attachments/assets/ba72c1a6-ad08-481e-8b6b-ae9919fb11ae">

•	F1, F2, F3 and F4 each represent the thrust of their respective propellers.
•	mg is the force exerted by gravity on the center of mass of the drone with m the mass of the drone (0.743 kg) and g the gravitational acceleration 9.81 m. s^-2.
•	F1, F2, F4 and F3 are coupled to induce pitch.
•	F4, F1, F3 and F2 are coupled to induce roll.


## Pitch and Roll Motion:

<img width="275" alt="image" src="https://github.com/user-attachments/assets/3922fbc4-335b-49a5-ad85-5c6c369f0c26">

•	If the Pitch angle is positive   F1 and F2 decrease while F3 and F4 increase, leading the drone to pitch by θx.
•	If the Roll angle is positive  F3 and F2 increase while F4 and F1 decrease keeping total thrust

## Drag and Disturbances:

<img width="556" alt="image" src="https://github.com/user-attachments/assets/99db1301-0825-4eea-9a41-1c711cbf34b8">

- The constant projected area estimated used in the design in the x and y directions: A = 0.0197m2.
- The drag Coefficient Cd of a cube: 1.00
- The air density at sea level: p = 1.225 kg.m-3
 
Since we have two beams crossing each other in the z direction the area was doubled and the same coefficient of drag and air density is used, moreover, 30% is added because of the rotors  A= 0.0512m2. 
Drag = ½ × p × v^2 Wind × A × Cd

## Roll and Pitch Thrust Vectors:

<img width="383" alt="image" src="https://github.com/user-attachments/assets/b4c3bd58-2c4f-4cd0-9ea0-4e665c6fce32">

Fpropx = sin(θx) cos(θy) (F1 + F2 + F3 + F4)
FpropY = sin(θy) cos(θx) (F1 + F2 + F3 + F4)
FpropZ = (F1 + F2 + F3 + F4) cos(θx) cos(θy)

Sum of Linear Forces:
FX = Fpropx +I – Dragx
Fy = Fpropy +I – Dragy
Fz = Fpropz – mg +I – Dragz



