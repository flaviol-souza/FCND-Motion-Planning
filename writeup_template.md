## Project: 3D Motion Planning
![Quad Image](./misc/enroute.png)

---


# Required Steps for a Passing Submission:
1. Load the 2.5D map in the colliders.csv file describing the environment.
2. Discretize the environment into a grid or graph representation.
3. Define the start and goal locations.
4. Perform a search using A* or other search algorithm.
5. Use a collinearity test or ray tracing method (like Bresenham) to remove unnecessary waypoints.
6. Return waypoints in local ECEF coordinates (format for `self.all_waypoints` is [N, E, altitude, heading], where the drone’s start location corresponds to [0, 0, 0, 0].
7. Write it up.
8. Congratulations!  Your Done!

## [Rubric](https://review.udacity.com/#!/rubrics/1534/view) Points
### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---
### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one.  You can submit your writeup as markdown or pdf.  

You're reading it! Below I describe how I addressed each rubric point and where in my code each point is handled.

### Explain the Starter Code

#### 1. Explain the functionality of what's provided in `motion_planning.py` and `planning_utils.py`
These scripts contain a basic planning implementation that includes...

And here's a lovely image of my results (ok this image has nothing to do with it, but it's a nice example of how to include images in your writeup!)
![Top Down View](./misc/high_up.png)

The `motion_planning.py` script is an implementation of motion planning. The planning is defined with a search on the grid that represents the environment. For this it necessary to have a Start Point (check the first line of `colliders.csv`) and Goal Point (assigned arbitray on the code), in this way the planning is traced considering the grid's obstacles.
Once defined the planning, the script will control the drone until the Goal Point.

**Description of features  the script (`motion_planning.py`):**
    * **States Enum:** Contains the states of the states machine to control of flight from drone 
    * **MotionPlanning Class:** the core the motion planning process. The initialization and execution this is the default behavior from the Udacidrone API. This class is composed of several functions to control the flight of the drone, listed below:
        * *local_position_callback:* function the callback to update the current location of drone. If the state of flight is TAKEOFF then the `waypoint_transition` function is called. the `waypoint_transition` function also called when the state of flight is WAYPOINT and the waypoints list is not empty. Or else, the state of flight is WAYPOINT and the waypoints list is empty, then the `landing_transition` is called when the velocity the drone is less than 1.
        * *velocity_callback:* In this function trigger the `disarming_transition` function when the state of flight LANDING and the drone arrived on goal point.
        * *state_callback:* Always that the state of flight is updated this function is triggered to control the flight's flow.
        * *arming_transition:* Attributes the Arming state giving control to the simulator.
        * *takeoff_transition:* in this function update the state of the fight to TAKEOFF and level the target altitude.
        * *waypoint_transition:* After attributes, the state of flight to Waypoint, define a new waypoint and instructs the drone to there.
        * *landing_transition:* this function is triggered on end the flow of flight, here the drone is ordained to land and consequently, the state is updated to Landing.
        * *disarming_transition:* only updated the states to Desarmar and releases control of the drone. 
        * *manual_transition:* turn off the drone motors updating the state to Manual and deactivates the flag `in_mission`.
        * *send_waypoints:* send the list of the waypoints to simulator render it.
        * *plan_path:* this function is the main to the planning. it is responsible for build the waypoints planning.
        * *start:* 


### Implementing Your Path Planning Algorithm

#### 1. Set your global home position
Here students should read the first line of the csv file, extract lat0 and lon0 as floating point values and use the self.set_home_position() method to set global home. Explain briefly how you accomplished this in your code.


And here is a lovely picture of our downtown San Francisco environment from above!
![Map of SF](./misc/map.png)

#### 2. Set your current local position
Here as long as you successfully determine your local position relative to global home you'll be all set. Explain briefly how you accomplished this in your code.


Meanwhile, here's a picture of me flying through the trees!
![Forest Flying](./misc/in_the_trees.png)

#### 3. Set grid start position from local position
This is another step in adding flexibility to the start location. As long as it works you're good to go!

#### 4. Set grid goal position from geodetic coords
This step is to add flexibility to the desired goal location. Should be able to choose any (lat, lon) within the map and have it rendered to a goal location on the grid.

#### 5. Modify A* to include diagonal motion (or replace A* altogether)
Minimal requirement here is to modify the code in planning_utils() to update the A* implementation to include diagonal motions on the grid that have a cost of sqrt(2), but more creative solutions are welcome. Explain the code you used to accomplish this step.

#### 6. Cull waypoints 
For this step you can use a collinearity test or ray tracing method like Bresenham. The idea is simply to prune your path of unnecessary waypoints. Explain the code you used to accomplish this step.



### Execute the flight
#### 1. Does it work?
It works!

### Double check that you've met specifications for each of the [rubric](https://review.udacity.com/#!/rubrics/1534/view) points.
  
# Extra Challenges: Real World Planning

For an extra challenge, consider implementing some of the techniques described in the "Real World Planning" lesson. You could try implementing a vehicle model to take dynamic constraints into account, or implement a replanning method to invoke if you get off course or encounter unexpected obstacles.


