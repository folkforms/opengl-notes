# Shaders

## Types of shaders

A shader is one of GL_VERTEX_SHADER, GL_FRAGMENT_SHADER or GL_GEOMETRY_SHADER

OpenGL 3.3:

- A vertex shader's sole responsibility is to set `gl_Position` to a clip space position.
- A fragment shader's sole responsibility is to set `gl_FragColor` to a colour.
- A geometry shader's sole responsibility is to take primitives (points, lines, triangles) as input and generate zero or more new primitives as output.

OpenGL 4.5 adds GL_TESS_CONTROL_SHADER, GL_TESS_EVALUATION_SHADER and GL_COMPUTE_SHADER.

- A tessellation control shader's sole responsibility is to determine how finely you want to subdivide a patch (could be a quad, triangle, or line) into smaller pieces.
    - The tessellation control shader sets these subdivision levels (called tessellation levels) based on factors like distance from camera, surface curvature, or desired detail level. Higher tessellation levels create smoother surfaces but cost more performance.
- A tessellation evaluation shader's sole responsibility is to compute vertex positions from tessellation coordinates and patch data.
- A compute shader's sole responsibility is to perform arbitrary computations on data without rendering geometry.

During setup, you create your shaders and attach them to a Shader Program.

## Shader Program

A shader program is a combination of a shaders linked together. We activate it with `glUseProgram(id)` during the rendering phase.

## Locations and layouts

Input data to a vertex shader is specified using layout locations:

```
layout (location = 0) in vec3 vertexPosition;
layout (location = 1) in vec3 instancePosition;
```

You can also just use the short form below, but this gives you less control:

```
in vec3 vertexPosition;
in vec3 instancePosition;
```

Note:

- `glBindAttribLocation` and `layout(location = X)` both do the same thing. Pick one and stick to it.
- `glGetAttribLocation` only works on vertex shaders.

## Uniforms

A uniform is a global variable for vertex, fragment or other shaders.

For example, the camera view matrix and projection matrix are likely to be constant for every vertex in the frame, so they can be passed in as a uniform:

```
uniform mat4 cameraViewMatrix;
uniform mat4 projectionMatrix;
```
