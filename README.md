# Eebot maze navigation

A program for an eebot to navigate through a maze with intersections and dead ends in order to reach a final goal. This is to be done through the use of
assembly code which will be mounted onto the MC9S12C32-led device. The robot will be tasked
to navigate through a guidance line made of black electrical tape and use guide sensors to initiate
which directions to take (forward, left, right or reverse).  

The initial line contains an S turn which mainly requires the use of E and F sensors to initiate left or right shifts to maintain on the S path.
After, the eebot will be implemented to prioritize the forward direction and will eventually
encounter a barrier at which the front bumper on the robot will be toggled. When the front
bumper is toggled the robot will move backwards a fixed (reverse state) then turn 180 degrees
(reverse turn state) then return to forward state.   

Another situation is when the robot comes to a dead end (T intersection) where there are only 2 branches to take, right or left. The robot will
randomly decide which branch to take till it reaches the endpoint. Then the robot will return to
the main loop and initiate the forward state which will continue following the guidance line.
Once at the endpoint, the operator will actuate the back bumper and will initiate the instructions
to return back to the start points without making any mistakes.       

## Requirements 
CodeWarrior
HCS12 microcontroller
Eebot
Maze board to test the eebot

## Project Description  

The navigation system is a sum of functions and concepts. These main components include a motor controller, a status
indicator, a state dispatcher, and a sensor reader. The sensor reader would take input from the
eebot’s optical sensors and allow it to follow a track of black tape through a determined range of
light value.   

These readings would then cause the state dispatcher to change accordingly, such as
the need to reverse in the case of a dead end from bumper input, turn in the case of a branching
path being read, or proceed. The dispatcher then calls on the motor controller in order to spin
both wheels in opposite or identical directions at the same time and speed in order to execute a
turn or to continue or stop moving.   

Throughout the process, the status of the eebot including
voltage, state, and sensor reading are displayed through the status indicator function. These
processes result in the unmanned device being able to navigate through the course without
interference and issue.


## Setting Up the Guider

<uL>
  <li>Set up the eebot so that it is powered up and connected to a HCS12 microcontroller board.</li>
  <li>Load and run the program read-guider using CodeWarrior.</li>
  <li>Make a full-sized copy of the template of figure 3. Place it under the eebot guider, lined up with the front
sphere and rear driving tires as marked on the template.</li>

  <img width="654" alt="Screenshot 2024-01-20 at 7 08 03 PM" src="https://github.com/CoboAr/Eebot-Navigation-Maze/assets/144629565/5652da1e-0e7d-4ade-b154-414908d0bc8b">

<li>Check that the 6 LED illumination circles match up with the large circles on the template.</li>
</uL>

## Code sections

### Variables section

The eebot will require variables that will be used to operate instructions in the
subroutines. The variables will be referenced and changed through the calling of many
subroutines which are necessary for sensor readings, motor control, etc.

### Initialize LCD
This code section has the instructions to initialize the LCD and will be called to display
the battery voltage and the current state the robot is in.   

### Dispatcher 
The state dispatcher of the program handles the movement part of the navigation system.
In order of start, forward, right turn, left turn, reversing, and stop, the active state is then
branched to form the dispatcher. The program will essentially loop through the dispatcher and
will execute a specific subroutine based on the current state mentioned previously. 

### Dispatcher States (Start, Forward, Right turn, Left turn, Reverse, All stop)
<ul>
  <li>Start state is necessary to avoid entering the forward state when the program is started.</li>
  <li>Forward state depends on the front, rear bumper and the sensors. If the front bumper is pressed,
the robot will go into the reverse state. When the rear bumper is pressed the robot goes into all
stop state. If sensor D value is 1 then the robot will go into the right turn state. If sensor B value
is 1 then the robot will go into the left turn state. If the EF sensor value is lower then the E
threshold then the robot will shift left, and if the value is greater then the F threshold the robot
will shift right. If the value of sensor A is lower than the base value, the robot will check the
values of sensors B or D and initiate the left or right turn depending on their base value,
respectively. If B and D are bigger than its base value the robot will prioritize the left turn.</li>
  <li>Right and left turn state work similarly. When making a right turn the starboard motor is off and
the port motor is on and the turn will continue until the counter is equal to the turning time.
When making a left turn the starboard motor is on and the port motor is on and the turn will
continue until the counter is equal to the turning time.</li>
  <li>Reverse state is initiated when the front bumper is actuated. The robot will move back a fixed
distance.</li>
  <li>Reverse turn state will initiate when the robot finishes the reverse state and turns on the starboard
motor until it completes a 180 degree turn then moves to forward state.</li>
  <li>All stop state will initiate when the rear bumper is actuated and goes into the start state.</li>
</ul>
### Initialize Subroutine (INIT_FWD, INIT_REV, INIT_RT_TRN, INIT_LT_TRN, INIT_ALL_STOP)
Based on current state the robot will initialize a state specific to the current state. For
INIT_FWD, the driver motors are turned OFF, resets the timer counter and sets the forward
direction for both motors.

### Motor Control
The motor control routine directly interacts with the ports of the eebot responsible for
toggling the motors on and off as well as their directions. It sets bits in order to turn a motor on
or reverse direction, and clears a bit to turn a motor off or set the direction to forward.

### Guider and Sensor
The guider and sensor subroutine handles the enabling and disabling of the LED sensors
as well as detection and reading updates per run cycle. It also loads these results into the RAM
registers to be read by the other processes in the main loop.

### Update Display
The display update subroutine converts the battery voltage reading off the eebot and
converts it into a readable form to be displayed on the LCD along with the current state.

### Other Subroutines  

The other subroutines not mentioned above, function as they did in the previous labs but
some modifications were done to accommodate this report. These additional subroutines exist to
accommodate displaying information on the LCD.


## Demo


https://github.com/CoboAr/Eebot-Navigation-Maze/assets/144629565/eaa497a2-24d4-4324-b07b-b0c1f2e51e94



The eebot is able to navigate through the dead ends and intersections as well as the winding paths present in the maze. The
programming of the robot led it to change states accordingly with respect to sensor readings,
allowing it to adhere to the electrical tape track and follow through the path and obstacles to its
final destination, while outputting its battery voltage and state on the LCD indicator.


Enjoy! And please do let me know if you have any comments, constructive criticism, and/or bug reports.
## Author
## Arnold Cobo
