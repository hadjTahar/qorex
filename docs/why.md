### Why I Created Qx


I created **Qx** because none of the existing tools fully matched how I like to work or what I want to build.

I come from a **Qt Scene Graph** background, and I genuinely like its architectural ideas. Many concepts in Qx are inspired by Qt’s approach to scenes, nodes, and rendering. However, over time Qt has grown into a very large framework. It’s powerful, but for many projects—especially lightweight apps and games—it can feel **bloated**, with a lot of functionality you don’t always need, and lacks game dev.

**Unreal Engine** is extremely capable, but it is primarily focused on high-end game development. It’s heavy by design, not well suited for general app development, and while mobile support exists, it comes with performance, size, and workflow trade-offs that make it less appealing for small or hybrid app/game projects.

**Godot** is a solid engine, but its workflow is highly editor-driven. For developers who prefer writing systems and code directly, the amount of clicking through menus, inspectors, and editor panels can slow things down and break flow.

**Unity** is powerful and flexible, but it comes with its own complexity, ecosystem lock-in, and design decisions that don’t necessarily align with how I want to structure an engine or framework.

That’s why I created **Qx**.

Qx is designed to be:

* **Lightweight by default**
* Capable of handling **both 2D and 3D**
* Suitable for **apps and games**, not just games
* **Code-centric**, with minimal friction
* Modular, so you only use what you actually need


Qx exists for developers who:

* Want to understand their engine, not fight it
* Prefer writing systems over clicking through panels
* Care about performance, binary size, and determinism
* Need one tool for apps, tools, and games

#### Qx is trying to solve
- Bloat in game engines and frameworks
- One framework to rule them all 2d, 3d, apps and games

#### Qx is not, and not trying to be:
- Large games engine
- Large world game engine
- Certainly not AAA game engine

Qx takes inspiration from scene-graph–based systems like Qt and Godot, where structure, hierarchy, and determinism matter. But unlike large, all-encompassing frameworks, Qx is intentionally lightweight. It avoids bundling features by default and instead focuses on being a clean foundation.

Qx is not trying to compete head-on with massive engines. Instead, it’s built to give developers a clean, efficient foundation—combining the structural elegance of scene graphs with modern rendering and physics—without the overhead and constraints of larger platforms.

Qx exists because modern development tools have drifted away from simplicity. Over time, engines and frameworks have grown into massive ecosystems. They are powerful, but heavy. Feature-rich, but rigid. Optimized for every use case—except the one where a developer wants clarity, control, and speed.

Qx was created as a response to that. It is built on the belief that **an engine should serve the developer, not dictate the workflow**.





