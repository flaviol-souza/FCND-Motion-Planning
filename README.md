# FCND - 3D Motion Planning
![Quad Image](./misc/enroute.png)

This a project of Motion Planning of the Fly Car course from Udacity.
Then, to execute it following the steps:
    1. colliders.csv contains the data that describe the environment of navigation the drone. If necessary, update that.
    2. through the variables of initialization (g_lot and g_lat) inform the goal location to the start location is defined in the first line of the colliders.csv
    3. after starting the solution, an algorithm the search way will run considering the variables of initialization (the start location and the goal location)
    4. With the way defined, a test the collinearity is applied under the waypoints list to remove the unnecessary waypoints.
    5. After passing the filter, the drone start following the waypoints planned.


---
## Check Rubric Points:
> The process of acceptance to Udacity's project following a specific Rubric (Read the Rubric to this project [here](https://review.udacity.com/#!/rubrics/1534/view)). In this section, I show a describe the coding of this project.

### Unlocking the code
This project is composed of two files (`motion_planning.py` and `planning_utils.py`). The scripts contain a set feature able to produce motion planning.

#### 1. `motion_planning.py` (Description of features the script)
The `motion_planning.py` script is an implementation of motion planning. The planning is defined with a search on the grid that represents the environment. For this it necessary to have a Start Point (check the first line of `colliders.csv`) and Goal Point (assigned arbitray on the code), in this way the planning is traced considering the grid's obstacles.
Once defined the planning, the script will control the drone until the Goal Point.

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

#### 2. `planning_utils.py`
The `planning_utils.py` is a script to helper the `motion_planning.py` process. Then this script has function to help the motion planning and the features are:

* **create_grid:** Returns a grid representation of a 2D configuration space based on given obstacle data, drone altitude and safety distance arguments.
* **Action Enum:** are constraints that are composed of 2 attributes (cost and delta) referring to the location. The `delta` use the first 2 values relative to to the current grid position, already the `cost ` is composed by the last value referring to the cost of performing the action.
* **valid_actions:** Returns a list of valid actions given a grid and current node.
* **a_star:** The A* algorithm is a simple yet elegant way of efficiently finding the lowest cost path from start to goal. In this way, the algorithm finds the lowest cost plan by considering the cost of each partial plan.
* **heuristic:** It's just a feature able calculates the Euclidean distance between two points (the current point and the goal). This function is used by the A* algorithm.
* **point:** Define a simple function to add a z coordinate of 1 and returns a 3D point as a numpy array.
* **collinearity_check:** Evaluate the collinearity for any three points that collinear within the threshold set by Epsilon.
* **prune_path:** this function is a filter to A* algorithm to remove unnecessary waypoints using creterion the `collinearity_check`.