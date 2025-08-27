# VAOs and VBOs

During mesh setup:

- Create the VAO
- Bind the VAO
- Create and populate VBOs (position, texture coordinates, normals, indices, etc.)
- Unbind the VAO

During mesh rendering:

- Bind the VAO
- All VBOs will be remembered
- Call OpenGL
- Unbind the VAO
