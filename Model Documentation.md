I started the project with the code shown in [project walkThrough video](https://classroom.udacity.com/nanodegrees/nd013/parts/6047fe34-d93c-4f50-8336-b70ef10cb4b2/modules/27800789-bc8e-4adc-afe0-ec781e82ceae/lessons/23add5c6-7004-47ad-b169-49a5d7b1c1cb/concepts/3bdfeb8c-8dd6-49a7-9d08-beff6703792d).

The path planner shown in the video offers basic functions, such as avoid collision with car in front of us, and change the lane to left if the car before us is too slow.

I enhance the path planner by using cost functions to find the best path. The best path is selected from the lane on the left of our car, the lane which our car is running on and the lane on the right of our car.

There are 5 criteria in my cost functions:

1. Infeasible cost

  **Is the lane feasible?**

  If our car is on the leftmost lane, it can't not switch to the lane on its left because the lane is in opposite direction. If the car is on the rightmost lane, it can't go to the lane on its right because there is no lane there.

  I add a very high cost, `999`, to those infeasible lane. Such that the car will never think that the infeasible lane is the best option.

1. Speed cost

  **Is the car on that lane running in a high speed?**

  Our goal is running the car in 50 mph. If the car on the lane is running slow, then we should better avoid it.

  The difference between our target speed and the speed of the car running on the lane is the cost. However, if the car running on the lane is far from us, then I assume that there is no car on that lane because our speed on that lane is not yet limited by the car. And I give the lane with no penalty.

1. Collision cost

  **Is it possible to switch to the lane at that moment?**

  If there are cars on the target lane, then it may not be safe to switch to the lane right aways.

  If there is no enough space to switch to that lane, then I add `999` to the cost, which means the lane is infeasible. However, it should be always safe to use our current lane, so there is no penalty on keep using the current lane.

1. Non-middle lane cost

  **Is the lane closest to the middle lane?**

  There is only 3 lanes on the road. If our car is on the middle lane, then we do always have three feasible lanes can be used. We can always choice pass the car in front of us by changing to left lane or right lane, but there is only one lane can be used for passing a car if we are on the leftmost or rightmost lane.

  As a result, I give a little cost to the non-middle lanes. Such that the car prefers running on middle lane if it have options.

1. Lane Traffic cost

  **Is the lane has no car in front of us?**

  If there is no car in front of us, then we should better using that lane, because our speed is not limited to any car.

  I give negative cost to the lane in order to encourage the car to using that lane.

  This is similar to the cost for speed, however, this cost is more focus on the number of cars, and that on is focus on the speed of the cars.

When there is any data send to path planner, the planner will find out the path with the lower cost to use. The path on the left of the car, on the right of the car, and the lane which is using by the car will be considered. The cost of these three lane is calculated by the sum of these cost functions. The lane with the lowest cost will be used for the future.
