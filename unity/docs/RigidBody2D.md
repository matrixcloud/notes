# Rigidbody2d

> Adding a **Rigidbody 2D** allows a sprite to move in a physically convincing way by applying forces from the scripting api. When the appropriate collider component is also attached to the sprite GameObject, it's affected by collisions with other moving GameObjects. Using physics simplifies many common gameplay mechanics and allows for realistic behavior with minimal coding.

## Body Type: Dynamic

A Dynamic Rigidbody 2D is designed to move under simulation. It has the full of properties available to it such as finite mass and is affected by gravity and forces. A Dynamic body will collide with every other body type, and is the most interactive of body types. This is the default body type for a Rigidbody 2D, because it is everything around it. All Rigidbody 2D properties are available with this body type.

Dont use the Transform component to set the position or rotation of a **Dynamic** Rigidbody 2D. The simulation repositions a **Dynamic** Rigidbody 2D according to its velocity; you change this directly via forces applied to its by scripts, or indirectyly via collisions and gravity.

## Body Type: Kinematic

A **Kinematic** Rigidbody 2D is designed to move under simulation, but only under very explicit user control. While a **Dynamic** Rigidbody 2D is affected by gravity and forces, a **Kinematic** Rigidbody 2D isn't. For this reason, it's fast and has a lower demand on system resources than a **Dynamic** Rigidbody 2D. **Kinematic** Rigidbody 2D is designed to be repositioned explicity via ```Rigidbody2D.MovePosition``` or ```Rigidbody2d.MoveRotation```. Use physics queries to detect collisions, and scripts to decide where and how the Rigidbody 2D should move.

A **Kinematic** Rigidbody 2D does still move via its velocity, but the velocity is not affected by forces or gravity. A **Kinematic** Rigidbody 2D does not collide with other **Kinematic** Rigidbody 2Ds or with Static Rigidbody 2Ds; it only collides with **Dynamic** Rigidbody 2Ds. Similar to a **Static** Rigidbody 2D, a **Kinematic** Rigidbody 2D behaves like an immovable object (as if it has infinite mass) during collisions. Mass-related properties are not availabel with this Body Type.