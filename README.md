<div align=center>
  <h1>
    A∗ algorithm
  </h1>
  <p>
    <b>KAIST CS470: Introduction to Artificial Intelligence (Spring 2023)</b><br>
    Programming Assignment 5
  </p>
</div>

<div align=center>
  <p>
    Instructor: <a href=https://sites.google.com/site/daehyungpark target="_blank"><b>Daehyung Park</b></a> (daehyung [at] kaist.ac.kr)<br>
  </p>
</div>


## Setup

### Installation Guide
Before followings, you should have Ubuntu 20.04 and ROS Noetic.

#### Install dependencies for TurtleBot3 Packages
~~~~bash
sudo apt-get install ros-noetic-joy ros-noetic-teleop-twist-joy \
  ros-noetic-teleop-twist-keyboard ros-noetic-laser-proc \
  ros-noetic-rgbd-launch ros-noetic-rosserial-arduino \
  ros-noetic-rosserial-python ros-noetic-rosserial-client \
  ros-noetic-rosserial-msgs ros-noetic-amcl ros-noetic-map-server \
  ros-noetic-move-base ros-noetic-urdf ros-noetic-xacro \
  ros-noetic-compressed-image-transport ros-noetic-rqt* ros-noetic-rviz \
  ros-noetic-gmapping ros-noetic-navigation ros-noetic-interactive-markers
sudo apt-get install python3-tk python-numpy
sudo apt install ros-noetic-turtlebot3-msgs ros-noetic-turtlebot3
~~~~

#### Install Simulation Package

Before installing the simulation and compile the necessary packages, let's move to your assignment5 folder
~~~~bash
cd your assignment5 folder
~~~~

Then, clone the TurtleBot3 Simulation Package
~~~~bash
cd src
git clone -b noetic-devel https://github.com/ROBOTIS-GIT/turtlebot3_simulations.git
cd ..
catkin_make
~~~~

Note that you have source the environment setup for ROS before running catkin_make. For example,
~~~~bash
source /opt/ros/noetic/setup.bash
~~~~

### PROBLEM 1: Run ASTAR on gridworld

~~~~bash
source ./robot.sh
roscd py_astar_planner/src/py_astar_planner
python3 astar.py
~~~~


### PROBLEM 2: Run Turtulebot3 Simulation

We first load necessary environment variables pre-defined in the following bash script. Note that you have to run the following script when you often any new terminal from now!
~~~~bash
source ./robot.sh
~~~~

#### Terminal 1
Let's run the TurtleBot3 World
~~~~bash
roslaunch turtlebot3_gazebo turtlebot3_world.launch
~~~~

#### Terminal 2
Let's run the ASTAR planner with RViZ
~~~~bash
roslaunch py_astar_planner turtlebot3_navigation.launch
~~~~

#### Align the map with LDS sensor input

1. Click the "2D Pose Estimate button" in the RViz menu.
2. Click on the map where the actual robot is located and drag the large green arrow toward the direction where the robot is facing.
3. Repeat step 1 and 2 until the LDS sensor data is overlayed on the saved map.

#### 2D Navigation with ASTAR planner

1. Click the "2D Nav Goal button" in the RViz menu.
2. Click on the map to set the destination of the robot and drag the pink arrow toward the direction where the robot will be facing.
3. As soon as the goal is set, the path will be generated TurtleBot3 will start moving to the destination immediately.


#### Q&A

1. Why "NO PATH!" message after completion?
This py_astar_planner package iterlatively run the ASTAR algorithm. So, after reaching the goal, it still tries to find a path though it does not require path.




## Assignment 5

### Problem 1: A∗ algorithm [65 pts]
You will implement a representative informed search algorithm, A∗, to find the shortest path leveraging heuristics. The performance varies depending on the heuristics. You will compare the performance to Dijkstra algorithm that uses a path cost only. Please, fill your code in the blank section following the <b>“PLACE YOUR CODE HERE”</b> comments in the <b>py_astar_planner/src/py_astar_planner/astar.py</b> file following the sub-problems below.

#### 1.1. A∗ algorithm implementation [50 pts]
A∗ algorithm expands the search tree by selecting a node n with the lowest value of an evaluation function $f(n)$, which is the sum of a path cost $c(n)$ and heuristics $h(n)$ (i.e., $f(n) = c(n) + h(n)$) (See details on Algorithm 1). As a testbed of the implemented A∗ algorithm, you will use a simple path-finding problem in a 20 × 20 size <i>grid</i> map where the goal and initial states will be given. You need to fill in the <b>astar_planning()</b> function and run the <b>astar.py</b> file.

In your report, you must
```
• describe (justify within 100 words) your selected heuristics function for this grid-based search problem,
• count the explored nodes at the end of the search (start=[3,3] and goal=[4,18]),
• plot the explored nodes at the end of the search (same as above), and
• plot the obtained paths from a start to a goal. You have to show the results of two scenarios:
    (i) start=[2, 1], goal=[19, 19]
    (ii) start=[2, 18], goal=[18, 7].
```

#### 1.2. Comparison of A-star vs. Dijkstra algorithms [15 pts]
Dijkstra algorithm is another method for finding the shortest path but it uses a heuristic identically equal to 0. You need to fill in the <b>dijkstra_planning()</b> function and run the <b>dijkstra.py</b> file. Note

<img src="/Figure/algorithm1.png" width="50%" height="50%">

that you can obtain the Dijkstra’s algorithm by simply removing heuristics from A∗ code. 

In your report, you must
```
• count the explored nodes at the end of the search (same as Problem 1.1),
• plot the explored nodes at the end of the search (same as Problem 1.1),
• plot the obtained path from a start to a goal. You have to consider the same scenarios given in Problem 1.1 above, and
• describe the difference between Dijkstra and A-star methods based on the results.
```

### Problem 2: Running A∗ on Turtlebot3 [35 pts]
The A∗ has been used for various AI applications including navigation. In this problem, you will adopt the result of the Problem 1.1 for the navigation problem given a simulated mobile robot, Turtlebot3. Throughout the Gazebo simulator, you command your Turtlebot3 finds a global collision-free path from a current location to a goal via RViZ. Then, you can observe a computed path from your A∗ implementations and the robot’s tracking performance. 

In your report, you must
```
•  attach three different obstacle-avoidance paths captured from RViZ, and
•  attach a sequence of screen captures that show your robot is tracking a computed path

Note that you can select challenging start-goal configurations as you want for the captures.
```




## Results
### 1. A∗ algorithm [65 pts]
#### 1.1. A∗ algorithm implementation [50 pts]
- describe (justify within 100 words) your selected heuristics function for this grid- based search problem,
  - I have selected the Manhattan distance between the position and the goal as the heuristic function for this grid-based search problem. Although there are many metrics for measuring the distance between two points, such as Euclidean distance, Manhattan distance is the most suitable for grid-based situations because the movement of the robot is constrained to the grid.

- count the explored nodes at the end of the search (start=[3,3] and goal=[4,18]),
  - 82
 
- plot the explored nodes at the end of the search (same as above), and

  <center><img src="/Figure/answer1.png" width="50%" height="50%"></center>

- plot the obtained paths from a start to a goal. You have to show the results of two scenarios:
  - (i) start=[2, 1], goal=[19, 19]

  <center><img src="/Figure/answer2.png" width="50%" height="50%"></center>

  - (ii) start=[2, 18], goal=[18, 7]

  <center><img src="/Figure/answer3.png" width="50%" height="50%"></center>
  
#### 1.2. Comparison of A-star vs. Dijkstra algorithms [15 pts]
- count the explored nodes at the end of the search (same as Problem 1.1),
  - 189 (start=[3,3] and goal=[4,18])

- plot the explored nodes at the end of the search (same as Problem 1.1), (start=[3,3] and goal=[4,18])

  <center><img src="/Figure/answer4.png" width="50%" height="50%"></center>

- plot the obtained path from a start to a goal. You have to consider the same scenarios given in Problem 1.1 above, and
  - (i) start=[2, 1], goal=[19, 19]

   <center><img src="/Figure/answer5.png" width="50%" height="50%"></center>
    
  - (ii) start=[2, 18], goal=[18, 7]
 
    <center><img src="/Figure/answer6.png" width="50%" height="50%"></center>

- describe the difference between Dijkstra and A-star methods based on the results.
  - When comparing the Dijkstra and A-star algorithms, there is no significant difference in the length of the resulting path and its cost. However, we can observe a disparity in the number of explored nodes, represented by black dots (i.e., nodes in the CLOSED_SET), between the two methods. This observation suggests that the A-star algorithm is more computationally efficient than the Dijkstra algorithm in finding the shortest path. Additionally, the A-star algorithm requires storing fewer nodes in both the OPEN_SET and CLOSED_SET compared to Dijkstra. Thus, the A-star algorithm outperforms Dijkstra in terms of spatial efficiency. This is why the A-star algorithm is utilized when dealing with a large number of states, such as in the case of a 15-puzzle problem (4 by 4) that has approximately 20 trillion states.

### 2. Running A∗ on Turtlebot3 [35 pts]
- attach three different obstacle-avoidance paths captured from RViZ, and

  <center><img src="/Figure/answer7.png" width="50%" height="50%"></center>

  <center><img src="/Figure/answer8.png" width="50%" height="50%"></center>

  <center><img src="/Figure/answer9.png" width="50%" height="50%"></center>

- attach a sequence of screen captures that show your robot is tracking a computed path

  <center><img src="/Figure/answer10.png" width="50%" height="50%"></center>

  <center><img src="/Figure/answer11.png" width="50%" height="50%"></center>

  <center><img src="/Figure/answer12.png" width="50%" height="50%"></center>

  <center><img src="/Figure/answer13.png" width="50%" height="50%"></center>

  <center><img src="/Figure/answer14.png" width="50%" height="50%"></center>

  <center><img src="/Figure/answer15.png" width="50%" height="50%"></center>

# ETC
For educational purpose only. This software cannot be used for any re-distribution with or without modification. The lecture notebook files are copied or modified from the material of Siamak Ravanbakhsh. 

