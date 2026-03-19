Curved World
Curved World is a vertex-displacement shader for creating dramatic mesh-bending effects such as curved horizons, spherical worlds, little planet, cylindrical warps, spiral twists and more. It modifies mesh vertex positions only at render time, inside the camera view, leaving all original meshes untouched. Perfect for achieving the popular bending effects seen in games like Subway Surfers, Minion Rush, Animal Crossing and others.


Curved World uses a high-performance vertex displacement shaders built on precise mathematical methods, to apply non-destructive bending transformations to any mesh. All bending calculations rely solely on each vertex’s world-space position, ensuring consistent and predictable deformation across the entire scene.


Modifying mesh vertices using Curved World shader transformation method
Because Curved World is purely a shader-based effect, it never modifies the actual mesh it renders. A mesh that is physically “flat” before applying the shader will remain flat - only its rendered appearance is changed. As a result, Curved World has zero impact on physics, animations, colliders, AI behavior, path-finding, or any gameplay systems tied to real geometry. If an object needs to move from point A to point B along path C, it will still follow exactly the same path and only the camera’s visual interpretation is curved.


Original scene

The same scene, but rendered with Curved World shaders
Compatibility & Installation
Supported Unity versions (only LTS):

Unity version
Built-in (BiRP)
Universal (URP)
High Definition (HDRP)
6000.3

✅

✅

✅

6000.0

✅

✅

✅

2022.3

✅

✅

✅

2021.3 *

✅

✅

✅

2020.3 *

✅

✅

✅

2019.4 *

✅

✅

✅

Non-LTS versions, as well as alpha, beta, tech, or preview versions, are not supported.

*Support for Unity 2019.4, 2020.3 and 2021.3 ends on Jan 1, 2026.

Installation
After downloading and importing Curved World asset in the project, go to the Amazing Assets → Curved World → Installer folder and based on the used render pipeline import one of the 3 unitypackages:


In the case of changing project's render pipeline (for example from Built-in to URP), completely remove asset from the project and re-import appropriate package from the Installer folder.​

If changing Unity Editor version (for example from 2021.3 to 6000.0), completely remove asset from the project, clear the Asset Store cache folder (Amazing Assets sub-folder) and re-download package using Package Manager. Based on the used Unity Editor version, proper package will be downloaded.
Tutorial #1 - Introduction
Open Tutorial #1 - Introduction scene from the Curved World → Example Scenes folder and run it:


It is a simple 'runner' type scene. Objects are spawned at some distance from the player, moving along a path (X axis in this case) and are destroyed behind the camera. Nothing related to the Curved World.

As the bending effect created by the Curved World is a pure shader displacement effect, meshes requiring such bending have to use shaders with integrated Curved World vertex transformation. Currently all scene meshes are using Unity’s default shaders. We need to change them.

All mesh objects instantiated in this scene are listed inside Car Spawner and Chunk Spawner game objects (find them in the Hierarchy window). Go through each mesh there and change all materials shaders from default to the Amazing Assets → Curved World shaders.

Or use the editor tool from Unity Main Toolbar → Window → Amazing Assets → Curved World. Go to the Renderers Overview tab, here are displayed all scene materials and their shaders. Click on the Change button and change the used shader to the Amazing Assets → Curved World shader:


After changing shaders, make sure all materials are using Classic Runner (X Positive) bend type with Bend ID 1:


Inside material editor Curved World shaders have options only to choose Bend Type with Bend ID and have no other properties affecting mesh bending. All bending settings are updated from script.

Create empty gameObject (or select Curved World Controller gameObject in the Hierarchy window)  and from the Components menu add  Amazing Assets → Curved World → Controller script and make sure it also uses Classic Runner (X Positive) bend type with Bend ID 1:


That’s all. Everything is ready for 'world bending'. Enter game mode and change Horizontal & Vertical properties from the controller script:


That’s how simple the Curved World is. Nothing in the scene has changed (except material shaders) -scripts, physics, animations, lighting and game design elements - everything remains the same.

To summarize this simple tutorial, for enabling Curved World mesh bending it is necessary:

Mesh materials have to use Curved World shaders.

Use controller script for updating bending settings.

Package includes collection of ready to use shaders with integrated Curved World transformations, but if those shaders are not enough then Curved World vertex bending effect can be manually integrated into any shader.  

How to make custom shaders Curved World compatible is explained in the Custom Shaders chapter. 

Before explaining key features of the Curved World in more details, let’s try one more bend type.

While Tutorial #1 - Introduction scene still is open, change all scene materials bending type from Classic Runner to Spiral Horizontal (X Positive). Keep Bend ID to 1:


Bend type can be changed directly from the material editor or by using Curved World’s editor window. Inside Renderers Overview tab select menu ≡ button and choose Change Curved World Bend Settings:


Inside the opened window choose Spiral Horizontal (X Positive) for the Bend Type and 1 for the Bend ID:


Click on the Change button. Now all materials and shaders listed in the Renderers Overview tab (in our case all material used in this tutorial scene) will use a new bend type.

After changing bend types for scene materials it is time to update bend settings from the script. As the scene already contains Curved World Controller script, select it and change its bend type to the Spiral Horizontal (X Positive), keep ID to 1. For the Rotation Center select a game object with the same name in the scene. Set Angle to 660 and Minimal Radius to 10.

That’s all. Everything is ready. Enter game mode and change Angle and Minimal Radius properties from the controller script, move the Rotation Center game object:


In the next tutorial we will create the Little Planet bending effect and scene with multiple bend types, but before that let’s check some key features of the Curved World.
How it really works
Curved World is a collection of vertex transformation functions, where each one is unique (by its name, ID and used properties) and is described in its own .cginc file. Those functions are used inside shaders, that displace mesh vertices and create bending effect.

Shader can use any number of Curved World vertex transformations. They can be mixed or used separately and enabled/disabled using keywords at any time from material editors or run-time scripts. For example, in the previous chapter (Tutorial #1 - Introduction) initially was used material with Classic Runner bend type and later it has been changed it to the Spiral Horizontal.




In this case material editor just disabled keyword for Classic Runner bend type and enabled it for the Spiral Horizontal. After that shader began using different function for calculating bending effect.

By default, Curved World package doesn't come with all bend types installed and shaders do not include all of them. They are added by user request. For example, if in the above material for the bend type choose Spiral Vertical Double (Z), material editor will display a warning message, as the bend type with such name and ID is not installed:


By clicking on the Add Now button, all required files will be generated and selected bending effect will be added to this shader.

Generating Curved World .cginc files and automatically adding their methods to the shader directly from the material editor, is possible only for shaders that are included in the package or are hand-written. Custom shaders created using Shader Graph or Amplify Shader Editor have to manage this process manually.

Package included shaders can use only one bend type at a time. Mixing multiple bending effects is possible only in custom shaders.

How to add Curved World vertex transformation to a custom shader is explained in the Custom Shaders chapter.

Curved World vertex transformation properties (bend settings) are always updated from script, not from the material editor. Bending properties are described in the .cginc files as global parameters and in Unity updating global shader properties is possible only from the script. 

Advantage of using global shader properties is that they are updated once and all shaders and materials automatically receive them, without the need to modify all of them separately.

For updating Curved World's global bending settings, package includes Curved World Controller script:


Which bending type Curved World Controller script updates depends on the Bend Type and Bend ID properties.  

Bend Type is the name of bending effect. For example, on the image above script updates Classic Runner (X Positive) bending. 

Bend ID is used for creating bend effect variations. For example, if scene needs several bending effects of the same type, but each one with separate bending properties, like on the image below:


This is a scene with 4 Classic Runner bend types used simultaneously on different sets of meshes, and settings for each bending effect is updated and controlled separately. 

All materials in this scene are using one shader containing 4 transformation methods: 

CurvedWorld_ClassicRunner_X_Positive_ID1 

CurvedWorld_ClassicRunner_X_Positive_ID2 

CurvedWorld_ClassicRunner_X_Positive_ID3 

CurvedWorld_ClassicRunner_X_Positive_ID4 

Each of those methods are unique and while they all create the same Classic Runner bend types, their properties are different, updated separately from each other and as a result create different bending effects.

We will create similar scene in the chapter Tutorial #4 - Multiple Bends.
Bend Types
Curved World offers 11 bending effects divided into two groups by used bending equation:

Parabola equation:

Classic Runner

Little Planet

Cylindrical Tower

Cylindrical Rolloff

Spiral equation:

Spiral Horizontal

Spiral Horizontal Double

Spiral Horizontal Rolloff

Spiral Vertical

Spiral Vertical Double

Spiral Vertical Rolloff

Combination of parabola and spiral equations:

Twisted Spiral

Main difference between those two groups is that the Parabola equation is faster and uses less instructions, but this algorithm ‘pushes’ vertices in the bend direction and they gain more distortion further they are from a pivot point. This limits camera placement in a scene. With those bending effects camera can be in a First Person, Third Person or Top Down positions. But observing a scene from a big distance or making 360° orbital rotation is not a good idea. 

Images below display Classic Runner, Little Planet and Cylindrical Rolloff bend types from camera position and editor (real) view:


Classic Runner bend type - view from camera and editor view.

Little Planet bend type - view from camera and editor view.

Cylindrical Rollof bend type - view from camera and editor view.
Meshes using Spiral bend equation can be observed from any point and camera can be placed anywhere in the scene:



Negative part of the Spiral equation is that it uses more instruction than Parabola and it doesn’t work well with tall objects, when the curvature arc is small. Image below demonstrates this problem. Vertices are stretched along the rotation arc and distortion increases proportionally by the distance from the rotation center:


However this problem can be fixed by calculating such objects position using script - not shader, as described in Non Shader Bending chapter and demonstrated in the Non - Shader Bending example scenes:


Parabola and Spiral equations allow to place bending origin and meshes anywhere in the scene. Using this, can be achieved multiple bending effects in one scene (demonstrated in the Tutorial #4 - Multiple Bends chapter).

Unfortunately the Twisted Spiral effect, that is a combination of parabola and spiral equations, lacks this possibility and the effect becomes distorted further its origin (pivot point) is from the scene center:


Keep origin of the Twisted Spiral bending effect at the scene (0, 0, 0) position.
Curved World bend effect name consists of two parts: 1) Bend Type and 2) Bend Axis. For example, Classic Runner (X Positive) – means that Classic Runner type bending effect will affect only those vertices that are distributed along the X (world X) axis and are on the positive side of the pivot point. As we did in the Tutorial # - Introduction chapter. This scene initially was oriented along the X axis and that's why we have used Classic Runner and Spiral Horizontal bend types working in X Positive axis direction. Using the Z axis (Positive or Negative) will be incorrect and would not give the desired bending effect.

Choosing the bending axis depends only on the game/scene design.

Some bend types have no option for choosing Positive/Negative sides of an axis, for example, Cylindrical Rolloff (X) or Spiral Horizontal Double (Z), etc. This means that the bending effect works along those axes on both sides of a pivot point.
Instructions count
Chart below displays Curved World vertex transformation cost - shader instructions count.  


Based on the provided data, is Curved World expensive and heavy effect? – No it is not. For comparison, below are some of the Unity Shader Graph nodes and their instructions count:


When creating bending effect, Curved World transforms mesh vertices only, but for better lighting and view-angle related effects, transforming of the mesh normals may be also desirable . This is optional feature for all shaders and it can be enabled from the material editor:


Normal transforming is not necessary for all shaders, but it can add more realism and details to the bent meshes using lit or reflective shaders. Image below demonstrates Plane mesh - on the left side mesh uses default shader. In the middle mesh uses Curved World shader, but without normals transformation. And on the right side material has enabled normals transformation:


Transforming mesh normals adds more realism to the bent meshes. 
Note, transforming normals in Curved World shaders relays on mesh tangent data.
Mesh disappearing and early culling
For vertex displacement shaders it is a very common problem when at some camera view angle mesh suddenly disappears. This happens when the original mesh goes beyond the camera's field of view and because of it, it is excluded from the rendering pipeline. In this case, shader has nothing to render.

For example, below is demonstrated the usage of the common (non Curved World) shaders:


And here is how the same meshes are rendered with the Curved World vertex displaced shader. Meshes outside of the camera's original view frustum are not rendered at all:


Curved World offers two solutions for fixing this problem:

Curved World Camera script - Overrides camera’s culling matrix before it begins frame rendering, that allows capturing objects outside its initial field view.



Increased Field Of View property brings culled objects back to the render pipeline.
Curved World Bounding Box script - Scales bounding box of MeshRenderer & SkinnedMeshRenderer components and makes meshes visible to a Camera.


This script is also necessary if a mesh is not visible to the light source (lights have their own field of view), to avoid excluding mesh from shadow receiving/casting pass.

It is very easy to find out why mesh with Curved World shader is not rendered or not receiving/casting shadows - just disable Curved World bending effect in the scene and check in the camera view if mesh is visible there. If it is not, then it means that it is outside of a camera view and will not be rendered by Curved World shader. In this case, one of the above solutions can be used to bring mesh back to the render pipeline.
Tutorial #2 – Little Planet (part 1)
Open Tutorial #2 - Little Planet (part 1) scene from the Curved World → Example Scenes folder and  and run it. 



It is the Unity’s Tanks! Tutorial asset from the Asset Store. Use W,A,S,D keys for tank navigation and Spacebar for shooting. Currently this scene does not use any Curved World transformations. We will fix that.

As in the previous tutorial (Tutorial #1 - Introduction), for enabling mesh bending effect it is necessary to use shaders with Curved World vertex transformation and update bend settings from the controller script.

Open Curved World editor window and change scene materials shaders from default to Amazing Assets → Curved World shaders:


After that, update properties for all scene materials to use Little Plane (Y) for Bend Type and 1 for Bend ID.

When all scene materials are set up to use Curved World shaders, create Curved World Controller script with the same bend settings:


Now, if trying to change the Curvature parameter inside the controller script, the scene will begin transforming but ground plane mesh will not be correctly affected. This is because, ground plane mesh hasn't enough vertices for creating smooth curvature:


As Curved World bending effect is calculated per-vertex, mesh needs to have enough vertices for the effect to be smooth. Image below demonstrates how vertex count affects curvature smoothness:


More vertices mesh has, more smooth the Curved World curvature is.
Current mesh used by the ground plane has just 4 vertices. Such meshes will never be bent:


Inside the Hierarchy window select the GroundPlane game object and change its mesh to the Plane 20x20 mesh. Now the scene should look much better:


Enter game mode and try to move the player tank (using W,A,S,D keys). 

Now, the far tank is moved from the scene's center, more distorted meshes are becoming and an illusion of the Little Planet effect is destroyed. It is easily fixable.

Each Curved World bending effect has a Pivot Point. It is the origin for the bending algorithm and it needs to be updated from the Curved World Controller script. Pivot Point can be any dynamic or static object.

Inside the Curved World Controller script, assign the tank gameObject to the Pivot Point and enter game mode. Now bending effect should be correct, as the scene is bent around the tank and the player is always in the center of a “spherical world”:


If increasing the Curvature parameter inside the controller script, some meshes will begin disappearing and popping in and out. As described in the chapter Mesh disappearing and early culling chapter, that’s because meshes are excluded from the rendering pipeline and become invisible for the Curved World shaders. To fix it, select Main Camera and from the Components menu add Curved World Camera script. Set size of Field of View to 120 with Perspective type:


To summarize key features explained in this tutorial:

For the correct bending effect mesh needs to have enough vertices. More vertices mesh has, more smoothly they are bent. 

Using correct Pivot Point, as it is the origin for the bending effect.

When using Curved World shaders some meshes can disappear unexpectedly, creating mesh popping in and out effect. It is because meshes are culled in the "pre-rendering" stage and excluded from the rendering pipeline before camera draws them into a frame. However this can be easily fixed by using the Curved World Camera script.

In the next tutorial we will add one more player into this Little Planet scene.

Note, we have missed changing the shader for the tank bullet mesh. As it is instantiated only at run time and is not visible for Renderers Overview window in the editor mode. Pause the game when shooting a bullet, go to the Renderers Overview window and change its material shader to the Curved World and set proper bend type and ID for it too.
Tutorial #3 – Little Planet (part 2)
In this tutorial we will continue working on the same Little Planet scene created in the previous chapter (Tutorial #2) and add one more player.

Note, rendering method used in this tutorial works in Built-in and Universal render pipelines and may not work in HDRP.

For now, temporarily disable the Curved World Controller script to see the scene without bending effect. 

Duplicate Tank and Camera game objects.

For the new camera remove Audio Listener script (Unity will throw warnings in the Console window if there are more than one such scripts in the scene) and in the attached Camera Follow script change target to be the new duplicated tank.

For the new tank in the Tank Movement script change Player ID to 2.

For each camera change Viewport Rect properties as on the image below:



While the Curved World Controller script still is disabled, enter game mode and test this scene (for the second tank, moving keys are Left,Right,Up,Down arrows, and right control key for shooting). If everything is done correctly then Camera (1) should following Tank (1) and rendering its view to the left side of the split screen, and Camera (2) should follow Tank (2) and rendering its frame to the right side of the split screen. 

Now enable Curved World Controller script and enter Play mode.

If Tank(1) is assigned as a Pivot Point in the controller script, only for this player bend effect will be correct and completely distorted for the second player. If Pivot Point is not assigned at all, both camera renderings will be incorrect:


We need both players (tanks) to be rendered with correct Little Planet effect. For achieving it, each Camera (1) and Camera (2) need to update bend settings (including Pivot Point) inside their own frames.

Duplicate game object with Curved World Controller script.

For the Pivot Point for the first controller assign Tank(1) and for the second script Tank(2).

For both scripts enable Manual Update check box.

Attach Manual Controller Update script to both cameras and assign Curved World Controller scripts - Controller (1) to Camera(1) and Controller (2) to Camera(2) accordingly.

Now enter Play mode. Each player should be in the center of its Little Planet while moving and cameras following each one accordingly:


If examine the Manual Controller Update script, all it does, calling update function for the Curved World Controller before camera begins rendering its frame.

By default Curved World Controller script automatically updates bend settings in its own Update() method. But as we have two different cameras and need each one to be rendered with its own bend settings, we have to disable controller script’s own Update (by enabling Manual Update check box) and call Update method manually for each camera from our custom script just before they begin rendering their frames. In this case even all scene materials are using one bend type with the same ID, in each frame the bending effect is rendered with different settings.

In the next tutorial we will create a scene with multiple various bending effects.
Tutorial #4 – Multiple Bends
In the previous tutorial we have created a scene with one bend type Little Planet, and rendered it from different cameras with different settings. Goal of this tutorial is to create a scene with multiple various bend types working simultaneously.

Open Tutorial #4 – Multiple Bends scene and run it. It is exactly the same scene we have worked with in the Tutorial #1 - Introduction chapter when creating runner type game.

Make sure all scene materials are using Curved World shaders with Classic Runner (X Positive) bend type with ID 1, and the Curved World Controller script uses Player (1) for the Pivot Point:


Duplicate game objects: Camera (1), Player (1), Car Spawner (1) and Chunk Spawner (1).

Move duplicates away on 1000 units along the Z axis. Just to make sure that old and duplicated objects are not interacting with each other and are not visible for neighbor cameras.

Set Viewport Rect for each camera as on the image below:



Now we have two players visible in the Game view, each one rendered with its own camera:


Currently all scene materials are using Classic Runner (X Positive) bend type with ID 1 and changing bend settings from the controller script will affect both players. But we need different bending effects for each one. For achieving this, duplicated objects materials need to use use different bend method, that requires material changing for all of them.

Open Renderers Overview tab and choose Selected Objects from the drop down menu, then select Player (2) and Scene Manager (2) game objects inside the Hierarchy window. Now all actions performed in the Renderers Overview tab will affect only meshes that are currently selected, other scene meshes are not affected.

From the menu ≡ button choose Replace Materials With Duplicates:


In the pop up window set Classic Runner ID2 for the Subfolder name and click on the Create Duplicate button. This will create material duplicates for all selected meshes and replace them in their renderers:


While still inside Renderers Overview tab and Player (2) and Scene Manager (2) game objects are selected, from the menu ≡ button choose Change Curved World Bend Settings. 

Set Classic Runner (X Positive) for the Bend Type and 2 for Bend ID. Click on the Change button:


Now all materials used for the Player (2) meshes are using different bending method.

Note, materials still are using the same one shader. But now this shader will have one more keyword describing additional bending method and for the Player (2) materials this keyword is enabled.

After materials are ready, it is time to update their bending settings. Select Curved World Controllers game object and from the Components menu add one more Curved World Controller script. Set Bend Type to Classic Runner (X Positive) and Bend ID to 2, and set Pivot Point to be the Player (2).

Now enter game mode. If everything is done correctly, meshes for Player (1) and Player (2) should be bent differently according to their bend settings controlled from their Curved World Controller scripts.

To summarize this tutorial:

Curved World bend methods are differentiated by Bend Type and Bend ID.

Two bend effects with similar Bend Types but different Bend IDs – are two different bend effects.

Materials switch bend effects using keywords (from material editor or run-time).

Scene can contain any number of various materials with various bend types.

Each bend effect must be updated separately, using Curved World Controller script or from any other custom script.


In this tutorial we have used two bending effects in one scene, but we can use even more. Check Tutorial #4 - Multiple Bends Finished scene, where 4 different bending effects are used simultaneously inside one scene:
Non-shader bending
Bending effect created by the Curved World is a pure shader displacement effect and it is visible only in the rendered frame. Objects real world space positions are not modified. 

If it is a need to click on the bent mesh or pick it up, then it is not possible, because mesh's real position is not there where it is rendered by the Curved World. Image below demonstrates this 'problem' - bent meshes are rendered in red color and their real scene positions with green wire:


The Curved World asset includes a solution for this problem – calculating shader bending effect using script and helping transforming any ‘world space’ position (Vector3 variable) into the ‘Curved World’ space. This allows transforming almost any mesh and non-mesh object’s position to follow the Curved World’s curvature. 

Check Non-Shader Bending example scenes in the  Curved World → Example Scenes folder to see how colliders, point lights and objects not using Curved World shaders are bent.
Unity terrain and grass shaders
For making Unity terrain system work with Curved World, create new material with Curved World terrain shader and assign it to the terrain asset. Choose required Bend Type with ID and click on the Actions button to recompile shader:


Save scene and restart Unity editor (in some Unity versions entering game mode is enough). Now terrain system should work with Curved World.

Supporting terrain grass and details is quite problematic due to constant changes in rendering pipelines and various engine limitations. If steps described below don’t work as expected, then the only solution to make Unity terrain and its grass/details system work with Curved World is converting terrain into a mesh and using common mesh shaders.

Note, project may contain only one set of terrain grass and detail shaders. If using multiple Curved World terrain shaders with different bend types, those shaders may not work.

Built-in render pipeline
By default, after assigning Curved World material to the terrain and restarting Unity editor, grass and detail shaders with Curved World effect should be visible to the terrain system.

Universal render pipeline (URP)
For Unity terrain system to work with custom Curved World grass and detail shaders it is necessary to override and replace them inside the render pipeline asset. Create new Curved World render pipeline asset:


This render pipeline asset is the exact copy of Unity's built-in URP asset, but allows to use custom grass and detail shaders for terrain system. Inside the Renderer List select project’s default renderer data (asset’s name can be ForwardRenderer or UniversalRenderer):


Renderer data can be checked inside the used render pipeline asset:


Select three shaders for pipeline asset from the Curved World terrain shaders folder:


Choose new render pipeline asset inside project Graphics and Quality settings:


Now grass and detail shaders with Curved World effect should be visible to the terrain system.

High Definition Render Pipeline (HDRP)
Terrain systems in Unity 2019.4 and 2020.3 versions don’t support grass and details at all. 

Unity 2021.3 and later versions support only details (not grass) and terrain system uses their shaders directly from the details prefabs. For those prefabs use common Curved World shaders and make sure they use the same bend settings as the terrain material.
Editor Window
Curved World editor window can be open from the Unity Main Menu → Window → Amazing Assets → Curved World and it is designed to help and speed up working with Curved World shaders and controllers.

Controllers
Controllers tab allows adding, editing and managing Curved World Controller scripts in the currently opened scene. 


Shader Integration
Shader Integration tab provides all Curved World related resource requiring when working with custom shaders. Properties of this window are explained in the Custom Shaders chapter.


Renderers Overview

Renderers Overview tab displays all materials and shaders that are available in the currently opened scene, at the moment when this tab is opened. From this tab can be easily edited scene shaders, materials and their keywords. 

By default this window displays information about all scene materials and shaders, but it can be changed to display information about only for the currently selected objects in the Hierarchy window:


Filter bar can be used to filter displayed data by shader name, material or by used keywords:


Refresh button updates displayed materials and shaders data:


Replace Materials With Duplicates - Replaces all materials currently displayed in the Renderers Overview tab with duplicates.

Change Curved World Bend Settings – Modifies materials (displayed in this window) Bend Type and Bend ID properties. This action affects only those materials that are using Curved World shaders included in the package or are hand-written. Custom Curved World shaders created using shader graph tools or any other shaders, are not affected. 

Curved World Keywords

Curved World Keywords tab displays selected shaders Curved World keywords. Each Bend Type and Bend ID added to a shader has its own keyword. Those keywords are enabled and disabled from material editor or run-time scripts when switching bend types. This window helps to visualize all such keywords. 

Save button rewrites selected shader to have only those keywords (Bend Types and Bend IDs) that are currently selected in this window. Keywords inside shader can be saved as shader_feature or multi_compile.

Curved World Keywords tab works only with package included and hand-written shaders.

Activator

Activator tab helps to activate Curved World vertex transformation that is integrated into assets purchased from the Asset Store.

If publisher from the Asset Store made asset Curved World compatible, then by default vertex transformations inside its shaders are disabled and after importing that asset package into a project Curved World effect needs to be manually activated from this tab - select project related path for required asset and click on the Activate button.

Deactivate button removes Curved World compatibility from the shaders.

Activator works only with hand-written shaders. If asset's shaders are created using the graph tools, Curved World nodes must be added there manually.

Before using activator tool make sure asset is Curved World compatible.
Scripts and run-time API
Curved World Controller
Updates bending properties for all shaders with selected Bend Type and Bend ID properties.


For run-time use CurvedWorldController script can be brought into scope with this using directive:


Copy
using AmazingAssets.CurvedWorld;
DisableBend

Copy
public void DisableBend()
Disables bend effect. All materials controlled by CurvedWorldController script will be rendered without Curved World transformation.

EnableBend

Copy
public void EnableBend()
Enables bend effect. All materials controlled by CurvedWorldController script will be rendered with Curved World transformation.

ManualUpdate

Copy
public void ManualUpdate()
Forces updating of the CurvedWorldController script. This method should be called every frame manually if  CurvedWorldController script has Manual Update check box enabled inside editor.

TransformPosition

Copy
public Vector3 TransformPosition(Vector3 vertex)
Transforms world space position of a vertex into Curved World space.

Check Non - Shader Bending example scenes in the Curved World → Example Scenes folder to see how TransformVertex method is used there for updating various objects position to make them follow the Curved World curvature.

TransformRotation

Copy
public Quaternion TransformRotation(Vector3 vertex, Vector3 forwardVector, Vector3 rightVector)
Calculates vertex rotation that follows Curved World curvature.

Check Non - Shader Bending example scenes in the Curved World → Example Scenes folder to see how TransformRotation method is used there for updating various objects position to make them follow the Curved World curvature.

Curved World Camera

Overrides camera's culling matrix before it begins frame rendering, allowing it to capture objects outside its field view. 

Script is used for fixing problem described in the Mesh disappearing and early culling chapter and it must be attached to the Camera object.

Curved World Bounding Box

Script scales bounding box of a MeshRenderer and SkinnedMeshRenderer and makes mesh visible to a camera, even if a mesh is outside of its field of view. Script is also necessary if mesh is not visible to the light source (it has its own field of view), to avoid excluding mesh from the shadow receiving/casting pass.

Script must be attached to a gameObject with MeshRenderer or SkinnedMeshRenderer components.
Custom Shaders
Curved World vertex transformation can be integrated into Shader Graph, Amplify Shader Editor and hand-written HLSL files. 
Integrating Curved World into Shader Graph
For integrating Curved World vertex transformation inside Shader Graph, inside Curved World editor window select required Bend Type and Bend ID, and then click on the Vertex or Vertex + Normal buttons:


This will generated Shader Graph node file and highlight it inside Project window:


Drag & drop selected node file inside Shader Graph and connect its output to the master node:


For Curved World transformation node input (vertex) use Vertex Position node (object space).

If shader contains other per-vertex transformations, like texture displacement, wind, etc, Curved World node should be used after them. In this case make sure their resultant vertex position (used as input for Curved World node) is in the object space.

Inside Shader Graph can be used multiple Curved World nodes of various type, they can be used separately from each other (enabled and disabled using keywords) or mixed together for achieving other types of bending effect:


Note, each Curved World node contains its own bending properties and they all must be updated using their own CurvedWorldController scripts.
Integrating Curved World into Amplify Shader Editor
For integrating Curved World vertex transformation inside ASE, inside Curved World editor window select required Bend Type and Bend ID, and then click on the Vertex or Vertex + Normal buttons:


This will generated ASE node file and highlight it inside Project window:


Drag & drop selected node file inside ASE and connect its output to the master node:


For Curved World transformation node input (vertex) use Vertex Position node.

If shader contains other per-vertex transformations, like texture displacement, wind, etc, Curved World node should be used after them. In this case make sure their resultant vertex position (used as input for Curved World node) is in the object space.

Make sure, Vertex Output option inside General settings is set to Absolute:
Integrating Curved World into hand-written shader
Modifying hand-written shaders assumes knowledge of  HLSL. 

For integrating Curved World into hand-written shader it is necessary to: 

Declare Curved World variable inside Properties block:


Copy
[CurvedWorldBendSettings] _CurvedWorldBendSettings("0|1|1", Vector) = (0, 0, 0, 0)
Add Curved World definitions and path to the CurvedWorldTransform.cginc file before the vertex stage:


Copy
#define CURVEDWORLD_BEND_TYPE_CLASSICRUNNER_X_POSITIVE
#define CURVEDWORLD_BEND_ID_1
#pragma shader_feature_local CURVEDWORLD_DISABLED_ON
#pragma shader_feature_local CURVEDWORLD_NORMAL_TRANSFORMATION_ON
#include "Assets/Amazing Assets/Curved World/Shaders/Core/CurvedWorldTransform.cginc"
Using Curved World vertex transformation inside vertex stages of all passes:


Copy
#if defined(CURVEDWORLD_IS_INSTALLED) && !defined(CURVEDWORLD_DISABLED_ON)
   #ifdef CURVEDWORLD_NORMAL_TRANSFORMATION_ON
      CURVEDWORLD_TRANSFORM_VERTEX_AND_NORMAL(v.vertex, v.normal, v.tangent)
   #else
      CURVEDWORLD_TRANSFORM_VERTEX(v.vertex)
   #endif
#endif
That's all. Save file and if shader doesn't use custom editor then Curved World properties will be displayed inside material editor:


If shader uses custom editor, then _CurvedWorldBendSettings property should be drown there manually.


Copy
//Example of Unlit shader with Curved World support

Shader "Example Shader"
{
    Properties
    {
        [CurvedWorldBendSettings] _CurvedWorldBendSettings("0|1|1", Vector) = (0, 0, 0, 0)

        _MainTex ("Texture", 2D) = "white" {}
    }

    SubShader
    {
        Tags { "RenderType"="Opaque" }
        LOD 100

        Pass  
        {
            CGPROGRAM 
            #pragma vertex vert
            #pragma fragment frag   
            #include "UnityCG.cginc"     

            sampler2D _MainTex;
            float4 _MainTex_ST;

            struct v2f 
            {
                float4 vertex : SV_POSITION;
                float2 uv : TEXCOORD0;                
            };
            
            
            #define CURVEDWORLD_BEND_TYPE_CLASSICRUNNER_X_POSITIVE
            #define CURVEDWORLD_BEND_ID_1
            #pragma shader_feature_local CURVEDWORLD_DISABLED_ON
            #pragma shader_feature_local CURVEDWORLD_NORMAL_TRANSFORMATION_ON
            #include "Assets/Amazing Assets/Curved World/Shaders/Core/CurvedWorldTransform.cginc"


            v2f vert (appdata_full v)
            {
                v2f o;

                #if defined(CURVEDWORLD_IS_INSTALLED) && !defined(CURVEDWORLD_DISABLED_ON)
                    #ifdef CURVEDWORLD_NORMAL_TRANSFORMATION_ON
                        CURVEDWORLD_TRANSFORM_VERTEX_AND_NORMAL(v.vertex, v.normal, v.tangent)
                    #else
                        CURVEDWORLD_TRANSFORM_VERTEX(v.vertex)
                    #endif
                #endif

                o.vertex = UnityObjectToClipPos(v.vertex);
                o.uv = TRANSFORM_TEX(v.texcoord, _MainTex);
				
                return o;
            }

            fixed4 frag (v2f i) : SV_Target
            {
                fixed4 col = tex2D(_MainTex, i.uv);

                return col;
            }
            ENDCG
        }
    }
}
Pay attention to the vertex data semantics provided in the transformation methods. In the example above v.vertex, v.normal and v.tangent variables are described in the appdata_full class with provided names.

In custom shaders those names may be different.

Shader Integration tab of the Curved World editor window can greatly help when working with custom shaders, as it contains all files path, keywords, method, definitions and macros names associated with selected Bend Type and Bend ID properties: 
For Asset Store Publishers
Almost any asset from the Asset Store can be made Curved World compatible by integrating vertex transformation instructions inside its shaders. 


All required steps for integrating Curved World in custom shaders are provided in the Custom Shaders chapter.

If Curved World instructions are integrated into hand-written shaders, then before publishing asset it is necessary to comment out: 1) Curved World variable inside Properties block and 2) defines before the vertex stage. In this case shader will be compiled even if Curved World asset doesn't exist in project.


Copy
//Example of Unlit shader with disabled Curved World support

Shader "Example Shader"
{
    Properties
    {
        // Commented out Curved World variable
        // [CurvedWorldBendSettings] _CurvedWorldBendSettings("0|1|1", Vector) = (0, 0, 0, 0)

        _MainTex ("Texture", 2D) = "white" {}
    }

    SubShader
    {
        Tags { "RenderType"="Opaque" }
        LOD 100

        Pass  
        {
            CGPROGRAM 
            #pragma vertex vert
            #pragma fragment frag   
            #include "UnityCG.cginc"     

            sampler2D _MainTex;
            float4 _MainTex_ST;

            struct v2f 
            {
                float4 vertex : SV_POSITION;
                float2 uv : TEXCOORD0;                
            };
            
            
            // Commented out Curved World defines
            // #define CURVEDWORLD_BEND_TYPE_CLASSICRUNNER_X_POSITIVE
            // #define CURVEDWORLD_BEND_ID_1
            // #pragma shader_feature_local CURVEDWORLD_DISABLED_ON
            // #pragma shader_feature_local CURVEDWORLD_NORMAL_TRANSFORMATION_ON
            // #include "Assets/Amazing Assets/Curved World/Shaders/Core/CurvedWorldTransform.cginc"


            v2f vert (appdata_full v)
            {
                v2f o;
                
                #if defined(CURVEDWORLD_IS_INSTALLED) && !defined(CURVEDWORLD_DISABLED_ON)
                    #ifdef CURVEDWORLD_NORMAL_TRANSFORMATION_ON
                        CURVEDWORLD_TRANSFORM_VERTEX_AND_NORMAL(v.vertex, v.normal, v.tangent)
                    #else
                        CURVEDWORLD_TRANSFORM_VERTEX(v.vertex)
                    #endif
                #endif

                o.vertex = UnityObjectToClipPos(v.vertex);
                o.uv = TRANSFORM_TEX(v.texcoord, _MainTex);
				
                return o;
            }

            fixed4 frag (v2f i) : SV_Target
            {
                fixed4 col = tex2D(_MainTex, i.uv);

                return col;
            }
            ENDCG
        }
    }
}
Users can activate Curved World compatibility for such assets/shaders any time using Activator tool:
Help & Contact
In the case of any questions, feature request or bug report, contact us on the asset forum or using support email.

Forum: discussions.unity.com​

Email: support@amazingassets.world​​​

©2012 - 2025 Amazing Assets
​https://amazingassets.world