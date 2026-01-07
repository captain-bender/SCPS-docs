## Triangle Motion Driven by a Proportional Apex-to-Apex Loop

The strategy requires a ROS 2 node that connects to the turtlesim communication channels. The node must **listen** to the topic that reports the turtle pose and **publish** to the topic that accepts velocity commands. These two channels form the bridge between sensing and actuation.

### Listening to State Feedback

The controller subscribes to **turtle1/pose**, which provides the turtle position in the 2D world (x, y) and its orientation $θ$. This stream is the measurement signal for the control loop.

### Publishing Actuator Commands

The node publishes messages to **turtle1/cmd_vel** of type **Twist**.

- `linear.x` represents forward speed.  
- `angular.z` represents turning rate.

### Describing the Triangle as Waypoints

Three fixed corners in the same (x, y) frame define the desired triangle. The controller treats one corner as the **current apex goal**. The turtle does not know anything about ideal edges; it only believes in the vertex it is chasing at that moment.

---

## The Journey Around the Triangle

After the node starts, the controller enters a **continuous loop from vertex to vertex**.

### 1. Approach the Active Apex

For the selected apex the controller calculates geometric errors.

**Euclidean distance error**

The position error is computed using the Euclidean metric:

$e_d = \sqrt{(x_{goal}-x)^2 + (y_{goal}-y)^2}$

This value expresses how far the turtle is from the goal corner.

**Direction to the apex using atan2**

The bearing toward the corner is obtained with

$θ_{target} = atan2(y_{goal}-y,\; x_{goal}-x)$

**Heading error**

The angular error between the desired direction and the current orientation is

$e_θ = θ_{target} - θ$

The difference is interpreted in the interval [–π, π] so that the turtle always turns through the shortest rotation.

### 2. Apply the Proportional Controller

- Linear velocity: $v = Kp_{lin} \cdot e_d$  
- Angular velocity: $w = Kp_{ang} \cdot e_θ$

The gains scale the corrections. Larger errors produce larger velocity commands.

### 3. Push Commands to cmd_vel

A Twist message is created with

- `linear.x = v`  
- `angular.z = w`

and published to **turtle1/cmd_vel**. The turtle moves under this proportional pull.

### 4. Corner Rounding Emerges

As the turtle gets close to Apex 1 it begins to rotate toward the line leading to Apex 2. Because the controller always chases the next selected apex, the turtle leaves the vertex slightly early — the classic **apex-to-apex point-chasing effect** — and the corner becomes smooth rather than perfectly sharp.

### 5. Waypoint Switching

When the Euclidean distance error becomes smaller than a chosen tolerance, the controller declares the apex reached, selects the next triangle apex, and repeats the same process.

### Apex 2 → Apex 3 → Apex 1 → …

The turtle continues to circulate endlessly, drawing the triangle as a perpetual pilgrimage between its three vertices.