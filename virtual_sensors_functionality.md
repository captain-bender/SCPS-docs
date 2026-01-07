# Virtual Sensor Bridge Node — Functional Description

The node acts as a **software sensor bridge** that listens to the turtle pose from the Turtlesim simulator and republishes equivalent information using standard ROS 2 interfaces. The purpose is educational: to show how the simple output of a simulator can be interpreted as if it were produced by real navigation hardware.

## Input Source

The node subscribes to the topic:

**/turtle1/pose — turtlesim/msg/Pose**

This message provides only planar localisation data:

- 2D position: $x$, $y$ in meters  
- orientation angle: $\theta$ in radians  
- no linear acceleration or angular rates  
- no vertical coordinate

The simulator therefore plays the role of a simplified localisation system whose data must be transformed to generate richer sensor messages.

## Odometry Output

The node publishes:

**/virtual/odom — nav_msgs/Odometry**

Meaningful values are assigned only for the members that correspond to the available Turtlesim information. All other fields in the odometry interface retain their default zero values.

### Position Members

The received planar position populates directly:

- `pose.pose.position.x = x_meas`  
- `pose.pose.position.y = y_meas`  
- `pose.pose.orientation` derived from $\theta$

### Orientation as Quaternion

Since odometry messages use quaternions, the node converts the yaw angle $\theta$ assuming roll and pitch are zero:

$$
q_z = \sin(\theta/2), \qquad
q_w = \cos(\theta/2)
$$

These values are published inside `orientation`.

### Derived Velocities

The node computes velocities using **finite differences** between consecutive pose messages. Let $\Delta t$ be the elapsed time between two updates.

#### Linear Speed

The instantaneous linear velocity magnitude is:

$$
v(t) = \frac{\sqrt{[x(t)-x(t_{prev})]^2 + [y(t)-y(t_{prev})]^2}}{\Delta t}
$$

This value populates:

- `twist.twist.linear.x`

#### Angular Rate

The angular velocity around the vertical axis is:

$$
\omega(t) = \frac{normalize\big[\theta(t)-\theta(t_{prev})\big]}{\Delta t}
$$

This populates:

- `twist.angular.z`

All lateral or vertical velocity fields remain zero because they have no equivalent in Turtlesim.

## IMU Output

The node also publishes:

**/virtual/imu — sensor_msgs/Imu**

Again, only planar-equivalent members are filled with meaningful values.

### Orientation

- `orientation = quaternion(\theta_{meas})`

### Angular Velocity

- `angular_velocity.z = \omega(t)`

### Forward Linear Acceleration

The node derives planar linear acceleration from the change of speed:

$$
a_x(t) = \frac{v(t)-v(t_{prev})}{\Delta t}
$$

This is published in:

- `linear_acceleration.x`

Lateral and vertical accelerations remain zero as they are not available from the simulator.

## Gaussian Noise

To mimic imperfect real sensors, the node adds **Gaussian noise** to all generated signals:

$$
x_{meas} = x + \mathcal N(0, \sigma_{xy}), \qquad
y_{meas} = y + \mathcal N(0, \sigma_{xy})
$$

$$
\theta_{meas} = \theta + \mathcal N(0, \sigma_{yaw})
$$

$$
v_{meas} = v + \mathcal N(0, \sigma_v), \qquad
\omega_{meas} = \omega + \mathcal N(0, \sigma_\omega)
$$

$$
a_{x,meas} = a_x + \mathcal N(0, \sigma_a)
$$

The standard deviation $\sigma$ is configurable in the task description and may be hard-coded. Noise introduces realistic behaviour such as small fluctuations even when the turtle moves smoothly.

## Useful functions:

```
def normalize_angle(a: float) -> float:
    """Wrap angle to [-pi, pi]."""
    while a > math.pi:
        a -= 2.0 * math.pi
    while a < -math.pi:
        a += 2.0 * math.pi
    return a
```


```
def yaw_to_quaternion(yaw: float) -> Quaternion:
    """Convert yaw (rad) to quaternion, assuming roll=pitch=0."""
    q = Quaternion()
    q.x = 0.0
    q.y = 0.0
    q.z = math.sin(yaw * 0.5)
    q.w = math.cos(yaw * 0.5)
    return q
```
