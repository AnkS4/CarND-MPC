# Model Predictive Control (MPC) Project

## Model Description
This project uses Kinematic model which ignores complex elements like gravity, air resistance tire forces.

The state variables: x, y, psi, v describes the state of the vehicle.
The actuator inputs: steering_angle, throttle allows the change of inputs.

The model tries to reduce the distance between the reference trajectory and the vehicle's actual path.
The actuator inputs are optimized at each timestep to minimize the cost of predicted trajectory. As the vehicle moves forward with new state, the model keeps calculating new optimal trajectory.

The reference state & reference control inputs are set to penalize the vehicle with the more cost, if the vehicle doesn't follow them.

### State is updated using following equations:

```
x = x0 + v0 * cos(psi) * dt
y = y0 + y0 * sin(psi) * dt
psi = psi0 + v0 * delta0 * dt / Lf
v = v0 + a0 * dt
cte = cte0 + v0 * sin(epsi0) * dt 
epsi = epsi0 + v0 * delta0 * dt/ Lf
```

where [x, y, psi, v, cte, epsi] are future state and actuator values and [x0, y0, v0, cte0, epsi0] are current state and actuator values.

### N and dt values:
'N' is the length of timesteps & 'dt' is elapsed duration between the timesteps.

The higher value of N will predict longer path for the vehicle while smaller N will only predict vehicle path for less distance.
Also, the higher N will slow down the prediction as it'll take more time for predicting more distance.
I found that N=20 works well, although predicting more distance.
dt=0.1 worked well for this project.

### Latency

The model deals with the latency by predicting the state and actuator control values with 100 millisecond latency.
