# VAOs and VBOs

## What are they?

VAO ???

VBO ???

## How to use it (part 1)

- You create bind a VAO. It is now the "active" VAO, and any VBOs that are bound will be linked with this VAO.
- You do this for each mesh, so each mesh has its own VAO.
- You create bind a VBO. It is now the "active" VBO, and ???
- When you later want to render a mesh, you bind its VAO and all of the VBOs will automatically be bound as well

## How to use it (part 2)

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

Instance positions have a vertex attribute divisor of `1`.

```
GL33.glVertexAttribDivisor(positionLocation, 1);
```

This means "advance this data for every instance."

Vertex positions implicitly have a vertex attribute divisor of `0`.

```
GL33.glVertexAttribDivisor(positionLocation, 0);
```

This means "advance this data for every vertex."

The above code with a `0` is never called as it is the default, but I think it's useful to know.
