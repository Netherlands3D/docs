Layers in Netherlands3D
=======================================

## Layers
Layers in Netherlands3D are objects that have an effect on the 3D environment. Layers are hierarchical, similar to GameObjects in Unity. Because of this. the Layer data structure makes use of Unity's GameObject/Transform hierarchy in order to minimize custom coding of the data structure, and to visualize the data structure in the Unity Editor hierarchy.

## Functionality of layers
All layers have the following basic functionality:
- Parenting: This works similar to the Unity Hierarchy
- Visibility: This works similar to setting a GameObject active/inactive in the Unity hierarchy.
- Properties: Each layer can have its own settings specific to the layer type, or even more specific to the content of the layer. This is similar to how the inspector in Unity will display all components and serialized fields of a GameObject.

## Layer types
Layers are split in 2 categories:
1. Regular layers that do not need their own GameObject hierarchy (basic layers)
2. Layers that do require a GameObject hierarchy (referenced layers)

## Basic layers
The first category of layers is the most simple. These layers are an extension of the `LayerNL3DBase` class.

* **Folder layer**: The most simple form of a Layer in this category is a `FolderLayer`. This layers has no functionality, except for providing the user with a way to organise other layers. It is similar to an empty GameObject.

* **Polygon layer**: A `PolygonLayer` is a layer that represents a polygon in the 3D environment. This layer can be created by providing a `List<Vector3>` of points (either by user input or from another source). This will create a layer in the hierarchy, with a visualisation and 

## Referenced layers
In case a layer has an internal hierarchy (such as a hierarchy of object parts, or a container with different tile objects) a layer can be integrated in the layer system as a referenced layer. A referenced layer exists outside of the Layer data structure, but has a connection to it through a `ReferencedProxyLayer`. The ReferencedProxyLayer will pass the required actions that affect the layer to its reference outside of the layer hierarchy, thereby making the referenced layer comply with the layer functionalities.
In order to do this, the layer object outside of the hierarchy must have an extension of the abstract class `ReferencedLayer` attached to it. This class will automatically create the `ReferencedProxyLayer`, thereby creating the connection to the layer system.

The following Layers are referenced layers:
* **Hierarchical object layer**:
A `HierarchicalObjectLayer` is an object that can be placed in the 3D environment. By default, this object can be moved by the transform handles. Objects of this type either come from a library of developer defined objects, or by user uploads. The most simple Hierarchical object layer is a GameObject with a `MeshFilter`, `MeshRenderer`, `Collider`, and `HierarchicalObjectLayer` component attached. More complex Hierarchical object layers can have nested GameObjects with each their own internal logic.

* **Cartesian tile layer**:
A `CartesianTileLayer` is a layer that makes use of the Netherlands3D custom tile file format. The objects and tiles in the layer are managed by the `TileHandler` and through the class `CartesianTileLayer` can interact with the layer system. For more information see the section on Cartesian Tiles. 

* **3D tile layer**:
A `Tile3DLayer2` is a layer that makes use of the 3D Tiles file format. These layers are managed by `Read3DTileset` and through the class `Tile3DLayer2` can interact with the layer system. For more information see the section on 3D Tiles. 

* **Object scatter layer**:
An `ObjectScatterLayer` is a special type of layer that will scatter the combined mesh of a `HierarchicalObjectLayer` in the area defined by a `PolygonLayer`. To do this, creat a `PolygonLayer` and make a `HierarchicalObjectLayer` the child of the `PolygonLayer`. This will convert the `HierarchicalObjectLayer` to an `ObjectScatterLayer`. Unparenting the ObjectScatterLayer from a PolygonLayer will revert the layer back to a `HierarchicalObjectLayer`. The `ObjectScatterLayer` has scatter settings that determine how the scattering should occur. The scattering is achieved through GPUInstancing, and therefore the mesh and material should support this. All objects in the ObjectLibrary support scattering.

## Layer UI
Each Layer can have an associated UI component defined by the class `LayerUI`. This component controls the UI elements associated with the layer. Interacting with the LayerUI will affect the corresponding `LayerNL3DBase`.
The Layer UI hierarcy is managed by `LayerManager`. This class holds a references to the layer type icons that are requested by LayerUI.

## Creating your own layers of an existing type
In order to add one of your own layers as an option through the existing UI, the following steps should be followed:
1. Create a prefab with the layer object. Make sure this object has a component of either `LayerNL3DBase` or `ReferencedLayer` attached to it.
2. Instantiate the prefab. If this is done in the existing LayerPanel UI, add a button or toggle in the prefab `AddLayerPanel.prefab` in the appropriate panel sub section. Then have the script that controls the button or toggle logic instantiate the prefab made in step 1.

## Creating your own layers of a new type
Follow the following steps to create a new layer type:
1. Choose if the new layer type can be a simple layer (direct extension of `LayerNL3DBase`) or it needs to be a more complex referenced layer (extension of `ReferencedLayer`).
2. Extend the chosen class and implement the required methods.
3. Add your own logic to the new class
4. Add a reference to the appropriate type and icon in LayerManager to ensure the new layer does not use the default `?` icon.
