# Doc:



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

QX_OPT_PRINT_VERBOSE_LEVEL
  - 0 Print message
  - 1 Print dbg prefix ( Debug, error, ...)
  - 2 Print method
  - 3 Print type name
  - 4 Print node name


### Classes
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


