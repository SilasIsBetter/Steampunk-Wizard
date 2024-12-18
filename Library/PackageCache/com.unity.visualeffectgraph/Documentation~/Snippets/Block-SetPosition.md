## Block compatibility

This Block is compatible with the following Contexts:

- [Initialize](Context-Initialize.md)
- [Update](Context-Update.md)
- Any output Context

## Block settings

| **Setting**               | **Type** | **Description**                                              |
| ------------------------- | -------- | ------------------------------------------------------------ |
| **Spawn Mode**            | Enum     | Specify how to distribute particles within the mesh. The options are:<br/>&#8226; **Random**: Calculates a per-particle random progress uniform sampling among the chosen primitive in **Placement Mode**.<br/>&#8226; **Custom**: Manually specify the sampling parameters. |
| **Placement mode**        | Enum     | Specify which primitive part of the mesh to sample from:<br/>&#8226; **Vertex**: Samples positions from all listed vertices.<br/>&#8226; **Edge**: Samples from an interpolation between two consecutives vertices that are part of a triangle on the mesh. <br/>&#8226; **Surface**: Samples from an interpolation between three vertices that define a triangle on the mesh. |
| **Surface coordinates**   | Enum     | Specify the method this Block uses to sample the surface of a triangle.<br/>&#8226; **Barycentric**: Samples the surface using raw barycentric coordinates. With this method, sampled positions are not constrained by the triangle edges which is useful if you have baked a position outside of the Visual Effect Graph.<br/>&#8226; **Uniform**: Samples the surface uniformly within the triangle area.<br/>This property only appears if you set **Placement mode** to **Surface** and **Spawn Mode** to **Custom**. |
| **Source**                | Enum     | **(Inspector)** Specify the mesh type to sample from.<br/>&#8226; **Mesh**: Sample from a mesh asset. The Block becomes a [Set Position (Mesh)](./Block-SetPosition(Mesh).md) Block.<br/>&#8226; **Skinned Mesh Renderer**: Sample from a [Skinned Mesh Renderer](https://docs.unity3d.com/Manual/class-SkinnedMeshRenderer.html) component. The Block becomes a [Set Position (Skinned Mesh)](./Block-SetPosition(SkinnedMesh).md) Block.|
| **Skinned Transform**     | Enum     | Specify the transform to apply to the sample from the Skinned Mesh Renderer.<br />&#8226; **None**: Apply no transform. The sample is the raw value from the geometry buffer of the Skinned Mesh Renderer.<br/>&#8226; **Apply Local Root Transform**: Apply the transform from the root bone, relative to the Skinned Mesh Renderer instance.<br/>&#8226; **Apply World Root Transform**: Apply the world transform of the root bone. <br/><br/>This property only appears if you set **Source** to **Skinned Mesh Renderer**. |
| **Apply Orientation**     | Enum     | **(Inspector)** Specify how Visual Effect Graph applies orientation to an attribute.<br/>&#8226; **Direction**: Sample the vertex normal, then apply the **Composition Direction** to the direction attribute.<br/>&#8226; **Axes**: Sample the vertex normal and tangent, then calculate normalized perpendicular axes (an orthonormal basis) and apply the **Composition Axes** to the AxisX, AxisY, and AxisZ attributes. |
| **Composition Position**  | Enum     | **(Inspector)** Specify how this Block calculates the position attribute.<br/>&#8226; **Overwrite**: Overwrites the position attribute with the new value.<br/>&#8226; **Add**: Adds the new value to the position attribute value.<br/>&#8226; **Multiply**: Multiplies the position attribute value by the new value.<br/>&#8226; **Blend**: Interpolates between the position attribute value and the new value. You can specify a blend factor between 0 and 1. |
| **Composition Axes**      | Enum     | **(Inspector)** Specify how this Block calculates the axisX, axisY, and axisZ attributes.<br/>&#8226; **Overwrite**: Overwrites the axis attribute values with the new value.<br/>&#8226; **Blend**: For each axis attribute, interpolates between the value and the new value. You can specify a blend factor between 0 and 1. The three axes might not be normalized and perpendicular to each other (orthonormal) after Visual Effect Graph blends the values.<br/>This property only appears if you set **Apply Orientation** to **Axes**. |
| **Composition Position**  | Enum     | **(Inspector)** Specify how this Block calculates the position attribute.<br/>&#8226; **Overwrite**: Overwrites the direction attribute with the new value.<br/>&#8226; **Add**: Adds the new value to the direction attribute value.<br/>&#8226; **Multiply**: Multiplies the direction attribute value by the new value.<br/>&#8226; **Blend**: Interpolates between the direction attribute value and the new value. You can specify a blend factor between 0 and 1. <br/>This property only appears if you set **Apply Orientation** to **Direction**. |

## Block properties

| **Input**                 | **Type**              | **Description**                                              |
| ------------------------- | --------------------- | ------------------------------------------------------------ |
| **Mesh**                  | Mesh                  | The source mesh asset to sample.<br/><br/>This property only appears if you set **Source** to **Mesh**. |
| **Skinned Mesh Renderer** | Skinned Mesh Renderer | The source Skinned Mesh Renderer component to sample. This is a reference to a component within the scene. To assign a Skinned Mesh Renderer to this port, create a Skinned Mesh Renderer property in the [Blackboard](Blackboard.md) and expose it.<br/><br/>This property only appears if you set **Source** to **Skinned Mesh Renderer**. |
| **Vertex**                | uint                  | The index of the vertex to sample.<br/>This property only appears if you set **Placement mode** to **Vertex** and **Spawn Mode** to **Custom**. |
| **Index**                 | uint                  | The start index of the edge to sample from. The Block uses this index and the following index to select the line to sample from.<br/>This property only appears if you set **Placement mode** to **Edge** and **Spawn Mode** to **Custom**. |
| **Triangle**              | uint                  | The index of triangle to sample, assuming the index buffer contains a triangle list.<br/>This property only appears if you set **Placement mode** to **Surface**, **Spawn Mode** to **Custom**, and **Spawn Mode** to **Custom**. |
| **Edge**                  | float                 | The interpolation value the Block uses to sample along the edge. This is the percentage along the edge, from start position to end position, that the sample position is taken.<br/>This property only appears if you set **Placement mode** to **Edge** and **Spawn Mode** to **Custom**. |
| **Barycentric**           | Vector2               | The raw barycentric coordinate to sample from the triangle at. The input is two-dimensional (**X** and **Y**) and the block calculates the **Z** value with the values you input: **Z** = **1** - **X** - **Y**. This sampling method does not constrain the sampled position inside the triangle edges.<br/>This property only appears if you set **Placement mode** to **Surface**, **Surface coordinates** to **Barycentric**, and **Spawn Mode** to **Custom**. |
| **Square**                | Vector2               | The uniform coordinate to sample the triangle at. The Block takes this value and maps it from a square coordinate to triangle space. To do this, it uses the method outlined in the paper [A Low-Distortion Map Between Triangle and Square](https://hal.archives-ouvertes.fr/hal-02073696v2) (Heitz 2019).<br/>This property only appears if you set **Placement mode** to **Surface**, **Surface coordinates** to **Uniform**, and **Spawn Mode** to **Custom**. |
| **Blend Position**        | Float                 | The blend percentage between the current position attribute value and the newly calculated position value.<br/>This property only appears if you set **Composition Position** to **Blend**. |
| **Blend Axes**            | Float                 | The blend percentage between the current axis attribute value and the newly calculated axis value.<br/>This property only appears if you set **Composition Axes** to **Blend**. |
| **Blend Direction**       | Float                 | The blend percentage between the current direction attribute value and the newly calculated direction value.<br/>This property only appears if you set **Composition Direction** to **Blend**. |
| **Transform**             | Transform             | The transform to apply after sampling. If you set **Source** to **Skinned Mesh Renderer** and **Skinned Transfrom** to a setting other than **None**, Visual Effect Graph applies the transform after the root bone transform. If you set **Skinned Transform** to **Apply World Root Transform**, Visual Effect Graph converts the transform to world space. |

## Limitations

The Mesh sampling feature has the following limitations:

- It only supports [VertexAttributeFormat.Float32](https://docs.unity3d.com/ScriptReference/Rendering.VertexAttributeFormat.Float32.html) for all [VertexAttributes](https://docs.unity3d.com/ScriptReference/Rendering.VertexAttribute.html) except [Color](https://docs.unity3d.com/ScriptReference/Rendering.VertexAttribute.Color.html), which has to be a four component attribute using either [VertexAttributeFormat.UInt8](https://docs.unity3d.com/ScriptReference/Rendering.VertexAttributeFormat.UInt8.html) or [VertexAttributeFormat.Float32](https://docs.unity3d.com/ScriptReference/Rendering.VertexAttributeFormat.Float32.html) format.
- If the mesh isn't [readable](https://docs.unity3d.com/ScriptReference/Mesh-isReadable.html), the Block returns 0. For information on how to make a mesh readable in the Editor, refer to the [Import Settings for a model file](https://docs.unity3d.com/Manual/FBXImporter-Model.html).
- Visual Effect Graph can't sample the current Skinned Mesh Renderer if you disable **Update When Offscreen** in the [Skinned Mesh Renderer](https://docs.unity3d.com/Manual/class-SkinnedMeshRenderer.html) and the mesh bounds aren't visible. Visual Effect Graph uses the position, normal and tangent of the last visible frame.

## Reference list

* Heitz, Eric. 2019. "A Low-Distortion Map Between Triangle and Square". [hal-02073696v2](https://hal.archives-ouvertes.fr/hal-02073696v2)