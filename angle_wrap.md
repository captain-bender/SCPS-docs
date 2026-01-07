## The suggested function:

```
def normalize_angle(angle):
    """Wrap angle to [-pi, pi]."""
    while angle > math.pi:
        angle -= 2.0 * math.pi
    while angle < -math.pi:
        angle += 2.0 * math.pi
    return angle
```

### Angles are periodic:
a full rotation is 2π.

So angle θ and angle θ + 2πk (for any integer k) represent the same direction.

The goal is to express any angle inside the compact interval:

[−π,π]

This interval is special: it is centered on zero, symmetric, and contains exactly one representation of every direction.

What the function does

Suppose angle is:

greater than +π:
subtract 2π until it falls into the interval.

less than −π:
add 2π until it falls into the interval.

The two while-loops ensure this wrapping.

### Visual intuition

Think of a number line that loops like a circle.
If the angle wanders too far to the right, you push it left by one full circle.
If it wanders too far to the left, you push it right by one full circle.

```
      -3π      -2π      -π       0       π       2π       3π
--------|--------|--------|-------|-------|--------|--------
                    <----- Allowed region ----->
                          [-π    ...     π]
```