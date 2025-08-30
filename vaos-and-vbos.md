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

## Divisors

Instace positions have a vertex attribute divisor of `1`.

```
GL33.glVertexAttribDivisor(positionLocation, 1);
```

This means "advance this data for every instance."

Vertex positions on the other hand implicitly have a vertex attribute divisor of `0`.

```
GL33.glVertexAttribDivisor(positionLocation, 0);
```

This means "advance this data for every vertex."

The above code is never called with a `0` as it is implicit, but I think it's useful to know.
