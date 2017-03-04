# Basic Cocos2d-x Concpets

## Main Components

- Scene
- Node
- Sprite
- Menu
- Action

## Director

The **Director** controls the flow of operations and tells the necessary recipient what to do.

One Common **Director** task is to control Scene replacements and transitions.

The **Director** is a shared singleton object.

## Scene Graph

A **scene graph** contains **Node** objects in a tree structure.

![](./imgs/node_tree.jpg)

> Cocos2d-x uses the ``` in-order walk ``` algorithm.
> An ``` in-order walk ``` is the left side of the tree being walked, then the root node, then the right side of the tree. Since the right side of the tree is rendered last, it's displayed first on the scene graph.

> Another point to think about is elements with a ``` negative z-order ``` are on the left side of the tree, while elements with a ``` positive z-order ``` on the right side.

In Cocos2d-x, you build the scene graph using the **addChild()** API call:

```cpp
// Adds a child with the z-order of -2, that means
// it goes to the 'left' side of the tree.
scene->addChild(title_node, -2);

// When you don't specify the z-order, it will use 0
scene->addChild(label_node);

// Adds a child with the z-order of 1, that means
// it goes to the 'right' side of the tree.
scene->addChild(sprite_node, 1);
```

## Sprites

Sprites are the objects that you move around the screen. You can manipulate them.

***Sprites*** are easy to create and they have configurable properties like: **position**, **rotation**, **scale**, **opacity**, **color** and more.

```cpp
auto mySprite = Sprite::create("mysprite.png");
mySprite->setPosition(Vec2(500, 0));
mySprite->setRotation(40);
mySprite->setScale(2.0);
mySprite->setAnchorPoint(Vec2(0, 0));
```

## Actions

For a game to be a game we need to make things move around! **Action** objects integral part of every game.You can change **Node** properties like position, rotation, and scale. Example Actions:

- MoveBy
- Rotate
- Sacle

```cpp
auto mySprite = Sprite::create("Blue_Front1.png");   

// Move a sprite 50 pixels to the right, and 10 pixels to the top of over 2 seconds
auto moveBy = MoveBy::create(2, Vec2(50, 10));
mySprite->runAction(moveBy);

// Move a sprite to specific location over 2 seconds.
auto MoveTo = MoveTo::create(2, Vec2(50, 10));
mySprite->runAction(moveTo);
```

## Sequences and Spawns

Just like it sounds, a **Sequence** is multiple **Action** objects run in a specified order.
   
Take a look at the flow of an example **Sequence** for moving a **Sprite** gradually:

!()[./imgs/moving_sequence.png]

```cpp
auto mySprite = Node::create();

auto moveTo1 = MoveTo::create(2, Vec2(50, 10));
auto moveBy1 = MoveBy::create(2, Vec2(100, 10));
auto moveTo2 = MoveTo::create(2, Vec2(150, 10));

auto delay = DelayTime::create(1);

mySprite->runAction(Sequence::create(moveTo1, delay, moveBy1, delay.clone(), moveTo2, nullptr));
```

**Spawn** will take all the specified **Action** objects and executes them at the same time. Some might be longer than others, so they won't all finish if this is the case.

```cpp
auto myNode = Node::create();

auto moveTo1 = MoveTo::create(2, Vec2(50,10));
auto moveBy1 = MoveBy::create(2, Vec2(100,10));
auto moveTo2 = MoveTo::create(2, Vec2(150,10));

myNode->runAction(Spawn::create(moveTo1, moveBy1, moveTo2, nullptr));
```

## Parent Child Relationship

Cocos2d-x uses a **parent and child relationship**. This means that properties and changes to the parent node are applied to its children.

## Logging as a way to output messages

```cpp
// a simple string
log("This would be outputted to the console");

// a string and a variable
string s = "My variable";
log("string is %s", s);

// a double and a variable
double dd = 42;
log("double is %f", dd);

// an integer and a variable
int i = 6;
log("integer is %d", i);

// a float and a variable
float f = 2.0f;
log("float is %f", f);

// a bool and a variable
bool b = true;
if (b == true)
    log("bool is true");
else
    log("bool is false");
```