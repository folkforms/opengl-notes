# Matrices

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

    public static Matrix4f updateProjectionMatrix() {
      float aspectRatio = (float) windowWidth / (float) windowHeight;
      return projectionMatrix.setPerspective(FOV, aspectRatio, Z_NEAR, Z_FAR);
    }

Note: `setPerspective` documentation says "Set this matrix to be a symmetric perspective projection frustum transformation for a right-handed coordinate systemusing OpenGL's NDC z range of [-1..+1]." So it's using a well-known calculation.

We use the FOV, the window's aspect ratio, and arbitrary Z_NEAR and Z_FAR coordinates. These might be 0.01f and 1000f, respectively. Anything with a z-value > Z_FAR is too far away to bother rendering, and anything with a z-value < 0.01f is inside or behind the camera.

### How is it used?

We combine the model data and the cameraViewMatrix into a modelViewMatrix

This is combined with a projection matrix (typically in the shader.)

Vertex shader will multiply a vertex by the modelViewMatrix to get the 3D position in camera space. This is equivalent to `vertex * model instance pos/rot/scale * camera view`. This gives us the vertex position in camera space (i.e. relative to the camera position and facing.)

We then multiply this value by the projection matrix to get the final vertex position. So the final equation for each vertex is:

`vertex * model instance pos/rot/scale * camera view * projection matrix`

This gives us the final position in clip space (which is -1 to +1 coordinate space for x,y,z.) The GPU then automatically converts clip space into screen pixels.

## Projection matrix in 2D games

### What type of projection matrix?

In 2D games we use an orthographic projection.

    // left/right/bottom/top are 0/windowWidth/0/windowHeight
    public final Matrix4f getOrtho2DProjectionMatrix(float left, float right, float bottom, float top) {
      return ortho2DMatrix.setOrtho2D(left, right, bottom, top);
    }

- "ortho2DMatrix" is a blank object and we give it values.
- Note: `setOrtho2D` documentation says "Set this matrix to be an orthographic projection transformation for a right-handed coordinate system." Again, it's using a well-known calculation.

### How is it used?

The exact same equation as above is used: `vertex * model instance pos/rot/scale * camera view * projection matrix`. The only difference is the projection matrix has different contents.

