Model Predictive Control (MPC)
--
## Student describes their model in detail. This includes the state, actuators and update equations.
The model has 6 states:
```
// x coordinates
double x = state[0];
// y coordinates
double y = state[1];
// angle
double psi = state[2];
// speed
double v = state[3];
// cross track error to reference line
double cte = state[4];
// orientation error to reference line
double epsi = state[5];
```

2 actuators:
`delta` is the turning speed and `a` is the acceleration (throttle).

The update functions (according to lectures):

![image](./update.png)

However, the sign of the `vt / Lf * delta * dt` term in both the `psi` and `epsi` update functions are used oppositely in the code. Not sure if it's a bug in the lectures. 

## Student discusses the reasoning behind the chosen N (timestep length) and dt (elapsed duration between timesteps) values. Additionally the student details the previous values tried.
Initially, I started with the following value:
```
N = 10
dt = 0.1
ref_v = 40
```
And the car could finish the lap easily with an average speed of 37mph. Then I want to run faster by increasing the speed to 80mph then the car will run off track. Even I set `ref_v` to 50 it still couldn't finish one lap.

Then I change `dt` to `0.05` and keep `ref_v = 50` and it can finish. I then increase `ref_v` to 100. It can still finish safely, the average speed however isn't increased (~51mph). I also tried using the same weight (500) for speed matching cost as `cte` and `epsi` but still no change. I suspect that it's because:
1. The solver is stuck at some local optima that increasing speed is not getting cost down
2. My laptop is not powerful so that the solver doesn't have the time before timing out to find the best solution

Anyway, this is something that I will continue to find out in the future

## If the student preprocesses waypoints, the vehicle state, and/or actuators prior to the MPC procedure it is described
Before fitting the polynomial, the x and y coordinates sent by the simulator are transformed to vehicle's coordinate system

## The student implements Model Predictive Control that handles a 100 millisecond latency. Student provides details on how they deal with latency.
I don't find much difference running with or without delay. Perhaps it is because the speed is never higher than 60mph.
