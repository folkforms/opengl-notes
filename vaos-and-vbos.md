# VAOs and VBOs

## What it does

- You bind a VAO. It is now the "active" VAO, and any VBOs that are bound will be linked with this VAO.
- You do this for each mesh, so each mesh has its own VAO number.
- When you later want to render a mesh, you bind its VAO and all of the VBOs are automatically bound as well.

## How to do it

During mesh setup:

- Create the VAO
- Bind the VAO
- Create and populate VBOs (position, texture coordinates, normals, indices, etc.)
- Unbind the VAO

During mesh rendering:

- Bind the VAO
- All linked VBOs will be bound as well
- Call OpenGL methods (e.g. draw methods)
- Unbind the VAO
