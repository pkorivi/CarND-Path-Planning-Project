## CarND-Path-Planning-Project

The project is an implementation of trajectory planning to drive the car around the track in a simulator avoiding the obstacles. 

The implementation is modelled in 3 phases

1. Read sensor information from simulator - This data contains the odometry of the car, assumptions about traffic position etc. 
2. Plan the path abiding to rules. 
3. Feed back the planned path to the simulator for the ego to drive ahead. 

The implementation details for the planning model are described in below steps.

1. Prediction - For each traffic participant [line 257 to line 296](./src/main.cpp#L257)
    1. Assign lane based on assumption that each lane is 4 meters wide
    2. Predict the future position of the car based on current speed (uniform speed model, better representations could be used)
    3. Check if any of the these participant is too close either ahead or could lead to collision in case of lane shift. This is done by checking the gap between the ego and traffic for same lane vehicle and for the vehicles on left and right by also considering the safe distance behind and ahead of car. In this case minimum safety distance asusmption is 30 mts. This assumption could be improved by considering the time to collison based on velocities of the participants. 
    4. Set the flags if any of the traffic participant qualifies the criteria mentioned in above steps.
2.Plan Behavior [line 300 to line 328](./src/main.cpp#L300) If there is a slow moving vehicle ahead, then check if its possible to change lane and change the lane. If lane change is not possible slow down the ego vehicle. 
3. If the ego vehicle is not in the center lane, bring it back to center lane. (This may not be ideal and result in too many lane shifts but falls inline with general driving rules if driving at optimal speed).
4. Trajectory [line 330 to line 428](./src/main.cpp#L330) - Create a path (spline) into the future by consdering the last two points from previous path and points placed at 30meters interval into future.
5. Fill the path with points left from previous cycle.
6. Fill the remaining of 50 points from the newly created spline.