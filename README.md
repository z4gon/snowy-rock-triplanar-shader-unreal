# Snowy Rock Triplanar Shader

Implemented with Triplanar Projection and Normals Orientation in **Unreal Engine 5.1.0**

## Screenshots

![Picture](./docs/24.png)

### Table of Content
- [Implementation](#implementation)
  - [Model Meshes](#model-meshes)
  - [Setup Scene](#setup-scene)
  - [Import Textures](#import-textures)
  - [Rock Material](#rock-material)
    - [Basic Rock](#basic-rock)
    - [Triplanar Projection Material](#triplanar-projection-material)
      - [Normal Masking](#normal-masking)
      - [TriplanarProjection Material Function](#triplanarprojection-material-function)
  - [Material Instances](#material-instances) 
  - [Snow Material](#snow-material)
  - [Snowy Rock Material](#snowy-rock-material)
    - [Snow and Rock Material Functions](#snow-and-rock-material-functions)
    - [Masking Roughness](#masking-roughness)

### Resources

- [Unreal Engine Materials Course by Timothy Trankle](https://www.udemy.com/course/unlocking-the-unreal-engine-material-editor)
- [Rock Texure](https://3dtextures.me/2022/03/03/rock-044/)
- [Snow Texture](https://3dtextures.me/2018/02/27/snow-002/)

## Implementation
### Model Meshes

- Model Meshes in Blender for the Rocks and the Ground.
- Ensure Normals are pointing to the right direction.

![Picture](./docs/1.png)

- Export as FBX and then import into Unreal Engine.
  
![Picture](./docs/2.png)

### Setup Scene

- Set the terrain and the rocks.

![Picture](./docs/3.png)

### Import Textures

### Rock Material
#### Basic Rock

- Implement a basic material using each of the textures for the corresponding property in the shader.

![Picture](./docs/4.png)

#### Triplanar Projection Material

- Use the **World Position** vector to sample the texture 2D, using the plane coordinates as if they were UV coordinates.
- Using the Red and Green channels means using the X,Y plane, thus projecting from the Z direction (Up, Down).
- This projection doesn't depend on object scale, UV maps, or anything.

![Picture](./docs/5.png)
![Picture](./docs/6.png)

- Repeating the process for all the 3 dimensions gives us the 3 projections we need.

![Picture](./docs/7.png)

##### Normal Masking

- To blend together the 3 projections, we need to use the normals directions.
- Getting the absolute value of the Blue coordinate of the Normals, gives us a mask to blend in the Z projection.
- Using a power function we make the blending band smoother.

![Picture](./docs/8.png)
![Picture](./docs/9.png)

- Multiplying each mask to the corresponding projection, then blending them all together by adding the colors, gives us the triplanar projection.

![Picture](./docs/10.png)

##### TriplanarProjection Material Function

- Extracting a set of nodes into a sub graph means creating a **Material Function**.
- This function has inputs and outputs.
- We can then reuse this in other materials.
- In the main **Triplanar Surface** **Material**, we now use the **Triplanar Projection** **Material Function** for each of the texures.

![Picture](./docs/11.png)
![Picture](./docs/12.png)

### Material Instances

- By making instances of a base material, we just change the parametrized textures and values.
- Make a material instance of the Triplanar Surface Material, to implement the Rock Material Instance.

![Picture](./docs/13.png)
![Picture](./docs/14.png)

### Snow Material

- Implement a new **Material** using the **TriplanarProjection** **Material Function**.
- Change the light model to **Subsurface**, for subsurface scattering.
- The **Subsurface **Color represents the subsurface scattering color.
- **Opacity** will dictate how much subsurface scattering will happen.

![Picture](./docs/15.png)
![Picture](./docs/16.png)

### Snowy Rock Material
#### Snow and Rock Material Functions

- Implement two **Material Functions** that will output **MakeMaterialAttributes** for both the **Snow** and **Rock** **Materials**.

![Picture](./docs/17.png)
![Picture](./docs/18.png)

#### Masking Roughness

- Use a texture to clamp values of 0,1 based on the threshold desired for the amount of snow.

![Picture](./docs/19.png)

- Mask again, this time using the Blue channel of the vertex normal, to just get the sides that are pointing up in world space.

![Picture](./docs/20.png)

- Use a height map to make the transition more natural, following the shapes of the texture.

![Picture](./docs/21.png)

- Multiply all these masks together to get the final mask used to blend the two **Material Properties**.

![Picture](./docs/22.png)
![Picture](./docs/23.png)

- Final result.

![Picture](./docs/24.png)
