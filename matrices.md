# Matrices

## The basics of matrices

Matrices are useful because we can represent various types of transformations:

Scaling:

```
| S1  0   0   0 |   | x |   | S1 . x |
| 0   S2  0   0 | . | y | = | S2 . y |
| 0   0   S3  0 |   | z |   | S3 . z |
| 0   0   0   1 |   | 1 |   |    1   |
```

Translating:

```
| 1  0  0  Tx |   | x |   | x + Tx |
| 0  1  0  Ty | . | y | = | y + Ty |
| 0  0  1  Tz |   | z |   | z + Tz |
| 0  0  0  1  |   | 1 |   |    1   |
```

Rotation about the X-axis:

```
| 1  0     0      0 |   | x |   |          x          |
| 0  cosT  -sinT  0 | . | y | = | cosT . y - sinT . z |
| 0  sinT  cosT   0 |   | z |   | sinT . y + cosT . z |
| 0  0     0      1 |   | 1 |   |          1          |
```

Rotation about the Y-axis:

```
| cosT   0  sinT  0 |   | x |   |  cosT . x + sinT . z |
| 0      1  0     0 | . | y | = |           y          |
| -sinT  0  cosT  0 |   | z |   | -sinT . x + cosT . z |
| 0      0  0     1 |   | 1 |   |           1          |
```

Rotation about the Z-axis:

```
| cosT  -sinT  0  0 |   | x |   | cosT . x - sinT . y |
| sinT  cosT   0  0 |   | z |   | sinT . x + cosT . y |
| 0     0      1  0 | . | y | = |          y          |
| 0     0      0  1 |   | 1 |   |          1          |
```

For a given scene with thousands of points, we first combine all three transformations into a **model matrix**.

Then we multiply the model matrix by a **view matrix** (camera) and a **projection matrix** (for 3D or 2D.)

Finally, we apply this **ModelViewProjection** matrix to all points in the mesh.

### Gimbal lock

When you rotate into certain positions, you can experience a scenario known as "gimbal lock". For example, if you rotate an object directly downwards (Y-axis), then rotation about the X and Z axis will do the same thing. You can free yourself from gimbal lock by undoing the Y-axis rotation. But the main issue is that when tracking an object, the camera rotation becomes unpredictable as the object flies though one of these "zones". The camera might execute a series of wild turns in order to keep tracking an object as it flies overhead.

The solution is to add a fourth dimension to the rotation, i.e. to use a quaternion for rotation.

FIXME Add a chapter on quaternions

## Matrices as objects

There are several types of matrix used:

- Camera view matrix: Represents the camera's point of view
    - This matrix is a way to transform other object's so they are relative to the camera
- Light view matrix: Represents a light's point of view
    - This matrix is a way to transform other object's so they are relative to the light
- Model: Represent's the model instance's pos/rot/scale in world space
    - This is a matrix purely for convenience. We could pass around a Vector3f/Vector3f/float.
- Projection matrix: Represents the screen.
    - It's not a view, per se. It's a way of manipulating data. For example, a perspective projection would be used in 3D games, whereas an orthographic projection would be used in 2D games.

## Projection matrix in 3D games

### What type of projection matrix?

A "frustum" is a pyramix with the top cut off.

For 3D games, we use a projection matrix which is a frustum. This is simple enough to calculate.

    public static void setProjectionMatrix(float windowWidth, float windowHeight, float fov) {
      float aspectRatio = windowWidth / windowHeight;
      projectionMatrix.setPerspective(fov, aspectRatio, Z_NEAR, Z_FAR);
    }

We use the FOV, the window's aspect ratio, and arbitrary Z_NEAR and Z_FAR coordinates. These might be 0.01f and 1000f, respectively. Anything with a z-value > Z_FAR is too far away to bother rendering, and anything with a z-value < 0.01f is inside or behind the camera.

- Note: `setPerspective` documentation says "Set this matrix to be a symmetric perspective projection frustum transformation for a right-handed coordinate systemusing OpenGL's NDC z range of [-1..+1]." So it's using a well-known calculation.
- We need to update `projectionMatrix` any time the FOV or window size changes.

### How is it used?

We combine the model data and the cameraViewMatrix into a modelViewMatrix

This is combined with a projection matrix (typically in the shader.)

Vertex shader will multiply a vertex by the modelViewMatrix to get the 3D position in camera space. This is equivalent to `vertex * model instance pos/rot/scale * camera view`. This gives us the vertex position in camera space (i.e. relative to the camera position and facing.)

We then multiply this value by the projection matrix to get the final vertex position. So the final equation for each vertex is:

`vertex * model instance pos/rot/scale * camera view * projection matrix`

This gives us the final vertex position in clip space (which is -1 to +1 coordinate space for x,y,z.) The GPU then automatically converts clip space into screen pixels.

## Projection matrix in 2D games

### What type of projection matrix?

In 2D games we use an orthographic projection.

    // left/right/bottom/top are 0/windowWidth/0/windowHeight
    public final void setOrtho2DProjectionMatrix(float left, float right, float bottom, float top) {
      ortho2DMatrix.setOrtho2D(left, right, bottom, top);
    }

- Note: `setOrtho2D` documentation says "Set this matrix to be an orthographic projection transformation for a right-handed coordinate system." Again, it's using a well-known calculation.
- We need to update `ortho2DMatrix` any time the window size changes.

### How is it used?

The same equation as above is used: `vertex * model instance pos/rot/scale * camera view * projection matrix`. The only difference is the projection matrix has a different value.

