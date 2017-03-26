# Canvas

The Canvas is the area that all UI elements should be inside.The Canvas is a Game Object with a Canvas component on it, and all UI elements must be children of such a canvas.

The Canvas area is shown as rectangle in the Scene View. This makes it easy to position UI elements without needing to have the Game view at all times.

> Canvas uses the EventSystem object to help the Messaging System.

## Draw order of elements

To change which element appear on top of other elements, simply reorder the elements in the Hierachy by dragging them. The order can also be controlled from scripting by using these methods on the Transform component: ```SetAsFirstSibing, SetAsLastSibling, and SetSiblingIndex```.

## Render Modes

### Screen Space - Overlay

This render mode places UI elments on the screen rendered on top of the scene. If the screen is resized or changes resolution, the Canvas will automatically change size to match this.

### Screen Space - Camera

This is similar to *Screen Space - Overlay*, but in this render mode the Canvas is placed a given distance in front of a specified *Camera*. The UI elements are rendered by this camera, which means that the Camera settings affect the appearance of the UI. If the Camera is set to Perspective, The UI elements will be rendered with perspective, and the amount of perspective distortion can be controlled by the Camera *Field of View*. If the screen is resized, changes reolution, or the camera frustum changes, the Canvas will automatically change size to match as well.

> 摄像机 -> 截取空间可视区域 -> 呈现给用户屏幕

### World Space

In this render mode, the Canvas will behave as any other object in the scene. The size of the Canvas can be set mamually using its Rect Transform, and UI elements will render in front of or behind other objects in the scene based on 3D placement. This is useful for UIs that meant to be a part of the world. This is also known as a "diegetic interface".

> [参考一](http://www.jianshu.com/p/dbefa746e50d)