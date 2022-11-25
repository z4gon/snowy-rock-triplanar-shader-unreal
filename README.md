# Snowy Rock Triplanar Shader

Implemented with Triplanar Projection and Normals Orientation in **Unreal Engine 5.1.0**

## Screenshots

### Table of Content
- [Implementation](#implementation)
  - [Model Meshes](#model-meshes)
  - [Setup Scene](#setup-scene)
  - [Import Textures](#import-textures)
  - [Materials](#materials)
    - [Basic Rock](#basic-rock)
    - [Triplanar Projection](#triplanar-projection)
    - [Normal Masking](#normal-masking)
  - [Material Function](#material-function)
  - [Material Instances](#material-instances) 

### Resources

- [Unreal Engine Materials Course by Timothy Trankle](https://www.udemy.com/course/unlocking-the-unreal-engine-material-editor)
- [Rock Texure](https://3dtextures.me/2022/03/03/rock-044/)
- [Ground Texture](https://3dtextures.me/2022/04/27/gravel-001/)
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

### Materials
#### Basic Rock

- Implement a basic material using each of the textures for the corresponding property in the shader.

![Picture](./docs/4.png)

#### Triplanar Projection

- Use the **World Position** vector to sample the texture 2D, using the plane coordinates as if they were UV coordinates.
- Using the Red and Green channels means using the X,Y plane, thus projecting from the Z direction (Up, Down).
- This projection doesn't depend on object scale, UV maps, or anything.

![Picture](./docs/5.png)
![Picture](./docs/6.png)

- Repeating the process for all the 3 dimensions gives us the 3 projections we need.

![Picture](./docs/7.png)

#### Normal Masking

- To blend together the 3 projections, we need to use the normals directions.
- Getting the absolute value of the Blue coordinate of the Normals, gives us a mask to blend in the Z projection.
- Using a power function we make the blending band smoother.

![Picture](./docs/8.png)
![Picture](./docs/9.png)

- Multiplying each mask to the corresponding projection, then blending them all together by adding the colors, gives us the triplanar projection.

![Picture](./docs/10.png)

### Material Function

- Extracting a set of nodes into a sub graph means creating a **Material Function**.
- This function has inputs and outputs.
- We can then reuse this in other materials.
- In the main **Triplanar Surface** **Material**, we now use the **Triplanar Projection** **Material Function** for each of the texures.

![Picture](./docs/11.png)
![Picture](./docs/12.png)

### Material Instances

- By making instances of a base material, we just change the parametrized textures and values.

![Picture](./docs/13.png)
![Picture](./docs/14.png)

