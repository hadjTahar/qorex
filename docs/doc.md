# Doc:



### Modules:

- qx: core module
- examples
- tests: unit testing module
- benchmarks: benchmarks module

### namespaces

- Qx
- prv: is something that the end user probably should not use


### Build options and macros:

- QX_OPT: Options to control builds used in cmake files
- QX_CMK: cmake internal variables inside cmake files
- QX_DEF: C++ definitions added by cmake
- dbg_  : Debug Macros defined in source code, likely "common/dbg" folder

All options are editable at the cmake level, except QX_OPT_ENABLE_TRACKER, in : "debugtracker.h" (is this still valid???)


## Options
### Print debugging verbose level

QX_OPT_PRINT_LEVEL
  - 0 Disable printing
  - 1 Print message
  - 2 Print dbg prefix ( Debug, error, ...)
  - 3 Print method
  - 4 Print type name
  - 5 Print node name


### Loops:

- Event loop SDL_iterate
Loops:
   1 - windows loop
   2 - layers loop
   3 - Components process loop
   4 - Item updateItem loop
      Loops only if property flag values changed
   5 - Item render loops
         Loops only if property flag values changed
   6 - Views loops
   6 - Components render loops??


The property flag propagates upwards, from child items to parent until window, and can be changed with

If no changes are detected, only loops (1,2 and 3) are run

- Process
- Timers
- Events



### Classes
#### Objects (scene tree)

<pre>
App
 └─ Windows
     └─ Layers
         └─ Views (camera + bounds)
</pre>


- An app can have multiple windows
- Each window can have mutiple graphic layers
- Each graphic layer, can have multiple views( camera, bounds) 

- App
	- Windows
		- GraphicsLayer:
			- GraphicsItems: process, create RenderObjects and adds them to root or graphicslayer
			- ::updateItem, runs in a thread if "QX_OPT_THREADED_UPDATE_ITEMS" is enabled
			- ::Views
					- Camera: Transforms and renders the scene to the view
			- ::Spatial tree (quad, ....)
			- ::Canvas (2D backend)
					- An abstraction for the backend and holds the backend objects and types it gets passed to the, GraphicsItem::render,  it renders the same scene to multiple views
					- Skia, ThorVG, Blend2D
			- ::SceneView (3D backend)

### Properties

One downside is that , it does not work well with auto, but it can be circumvented, by using the getter equivilant.

Eg:

	const auto pos = item->transform.position; /// ## Will not return a vec3
	const auto pos = item->transform.getPosition(); /// ## Will return a vec3
	const vec3 pos = item->transform.position; /// ## Will return a vec3

Property flags propagates upward, to alert the parent about a child's state change ( transfomr, render, ...)

Property values, after checkPropertOptions, propagate downwards the values to the children, Eg.
  Transforms
  Visibility

### Rendering API's abstractions
- Canvas Back end API
	- ( skia, SDL, ... )
- Canvas API
	- Abstraction for the Canvas Back end
- Scene graph API
	- QML and Godot like

#### Rendering (backends)

### 2D VS 3D

- GraphicsItem
  - 
	- propagates ::render from "GraphicsItem::propertyChanged"      
  - GraphicsItem2D
    - ::render( Canvas)
    - ::canvas->draw
  - GraphicsItem3D
    - ::GraphicsLayer2D ::graphicsLayer
    - ::render( RenderContext )
    - ::RenderContext->addModel()

- Views
  - Has a viewport transform
  - HAs a camera transform
  - To render you, apply the viewport transform, then the camera transform to the canvas or context
  - Loop and render the items with a lod
  - You can’t remove views once added, only hide them.


Mouse events:
  - Maps the mouse coordinates to the views coords
  - Loop the mouse components and check

#### Dynamic behavior

- Timers
   - Looped Timers
   - Singleshot Timers
- Components:
   - ::process
   - DynamicEase ( Linear, Bezier, Damped and CSS like Polynomial Eases )

### Components

    When a new components is added, the components are sorted by their parentItem hierarchy,
    to ensure CoreComponent::process calls, are called properly.
    If there is a lag or a delay, probably it's a sorting issue
   
### Camel case vs snake case convention

For dev functions and names camel case is used, but snake case for debugging utilities, like dbg_print, dbg_assert


## Not yet ❌
----
### Spatial tree quad


Is this how a Spatial tree rendering work (quad)
Devide into rectangles tree
Each rectangle has a list of items as pointers


QuadNode {
    AABB bounds;
    vector<EntityID> items;
    QuadNode* children[4];
}

#### Frustum culling (2D camera view)

Without QuadTree, You do this every frame:

	for (auto& obj : allObjects) {
	    if (obj.bounds.intersects(cameraBounds))
	        visible.push_back(obj);
	}

If you have:
	- 10,000 objects
	- Camera sees 200

You still test 10,000 AABBs every frame. With QuadTree You do this

	quadtree.query(cameraBounds, visible);


#### Spatial queries (area checks)

Example: explosion radius. Without QuadTree

	for (auto& enemy : enemies) {
	    if (enemy.bounds.intersects(explosionRadius))
	        damage(enemy);
	}


Even if the explosion is tiny, you check everyone. With QuadTree

	auto nearby = quadtree.query(explosionAABB);
	for (auto& e : nearby)
	    damage(e);

#### Options (choose by usage)

| System    | Query shape          | Purpose         |
| --------- | -------------------- | --------------- |
| Rendering | Camera AABB          | Frustum culling |
| Collision | Object AABB          | Broad-phase     |
| AI        | Vision AABB / radius | Awareness       |
| Picking   | Ray AABB             | Selection       |


#### Options (choose by dimension & dynamics)

| Use case               | Structure                        |
| ---------------------- | -------------------------------- |
| 2D static              | **Quadtree**                     |
| 2D dynamic             | **Loose Quadtree**               |
| 3D static              | **BVH**                          |
| 3D dynamic             | **Dynamic AABB Tree**            |
| Terrain / large worlds | **Octree**                       |
| GPU culling            | **Bounding hierarchy + compute** |

### Pipeline

Scene Graph (Hierarchy tree)
  (world transforms)
       ↓
QuadTree (objects)
  (spatial filtering)
       ↓
Visible Set from camera(objects)
       ↓
Render Tree (final selected objects)
  (draw order & batching)
       ↓
GPU


