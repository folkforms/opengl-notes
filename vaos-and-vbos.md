# VAOs and VBOs

## What are they?

VAO: FIXME Description ???

VBO: FIXME Description ???

## How to use them (part 1)

- You create bind a VAO. It is now the "active" VAO, and any VBOs that are bound will be linked with this VAO.
- You do this for each mesh, so each mesh has its own VAO.
- You create bind a VBO. It is now the "active" VBO, and you can configure it.
- When you later want to render a mesh, you bind its VAO and all of the VBOs will automatically be bound as well

## How to use them (part 2)

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

(FIXME Isn't there one type of VBO you shouldn't unbind, or something similar?)

## Divisors

Instance positions have a vertex attribute divisor of `1`.

```
GL33.glVertexAttribDivisor(positionLocation, 1);
```

A value of `1` means "advance this data for every instance."

A value of `0` (the default) means "advance this data for every vertex".

```
GL33.glVertexAttribDivisor(positionLocation, 0);
```
