Turtlesim lives in a square world roughly 11 × 11 meters.

The controller chooses a reference corner near the centre of the window:
```
(x0, y0) = (5.54, 5.54)
```

Let's select a side: 
```
side = 2.0 m
```

First 2 corners:
```
P1 = (x0, y0)
P2 = (x0 + side, y0)
```

Sketch:
```
P3
   *
  / \
 /   \
*-----*
P1    P2
```

Third corner

We can calculate the height as:
```
h=side⋅sin(60°)
```
and then:
```
h = side * sin(pi/3)
x3 = x0 + side/2
y3 = y0 + h
```






