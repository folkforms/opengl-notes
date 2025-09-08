# Textures

Each vertex gets a texture coordinate that says what part of the texture it corresponds to. Fragment interpolation then does the
rest for the fragments between each vertex.

Texture coordinates range from 0 to 1 in the x and y axis, and 0,0 is at the bottom-left.

## Texture wrapping

If the texure is not big enough for the area, it can be set to repeat, mirror, or clamp in various ways:

- GL_REPEAT: Repeats the texture image. This is the default behavior.
- GL_MIRRORED_REPEAT: Same as GL_REPEAT but mirrors the image with each repeat.
- GL_CLAMP_TO_EDGE: Clamps the coordinates between 0 and 1. The result is that higher coordinates become clamped to the edge, resulting in a stretched edge pattern.
- GL_CLAMP_TO_BORDER: Coordinates outside the range are now given a user-specified border color.

## Texture filtering

There are several options available but the most common are GL_NEAREST and GL_LINEAR.

- GL_NEAREST (also known as "nearest neighbor" or "point filtering") is the default texture filtering method and selects the texel whose center is closest
to the texture coordinate.
- GL_LINEAR (also known as "(bi)linear filtering") takes an interpolated value from the texture coordinate's neighboring texels and approximates a color between the texels.

Texture filtering can be set for magnifying and minifying operations (when scaling up or downwards.) So you could use nearest neighbor filtering when textures are scaled downwards and linear filtering for upscaled textures. We thus have to specify the filtering method for both options via `glTexParameter`:

```
glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MIN_FILTER, GL_NEAREST);
glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MAG_FILTER, GL_LINEAR);
```

## Mipmaps

OpenGL can generate mipmaps for us using `glGenerateMipmaps` after we've created a texture.

## Mipmap filtering

You can also specify how mipmaps should be filtered, to get better results:

- GL_NEAREST_MIPMAP_NEAREST: Takes the nearest mipmap to match the pixel size and uses nearest neighbor interpolation for texture sampling.
- GL_LINEAR_MIPMAP_NEAREST: Takes the nearest mipmap level and samples that level using linear interpolation.
- GL_NEAREST_MIPMAP_LINEAR: Linearly interpolates between the two mipmaps that most closely match the size of a pixel and samples the interpolated level via nearest neighbor interpolation.
- GL_LINEAR_MIPMAP_LINEAR: Linearly interpolates between the two closest mipmaps and samples the interpolated level via linear interpolation.

You can specify the filtering method as above:

```
glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MIN_FILTER, GL_LINEAR_MIPMAP_LINEAR);
glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MAG_FILTER, GL_LINEAR);
```

Note that mipmap options can only be used for the `GL_TEXTURE_MIN_FILTER`. It does not make sense to use mipmap options for `GL_TEXTURE_MAG_FILTER` since mipmaps are only about downsized textures and those are expanded textures. Doing so will generate an `GL_INVALID_ENUM` error.

## Textures in the vertex shader

You put the texture coordinates for each vertex into a VBO that gets sent to the vertex shader. Think of it like stretching a canvas over a frame. The coordinates are the points you want to pin, and the rasterisation will figure out the rest.

That said, in the vertex shader we just pass the coordinates along to the fragment shader.

```
layout (location = 1) in vec2 texCoords;

out vec2 fragTexCoords;

void main() {
  fragTexCoords = texCoords;
  ...
}
```

## Textures in the fragment shader

OpenGL has a built-in data type for textures called a "sampler". You can use `sampler1D`, `sampler2D` or `sampler3D`.

In the fragment shader we have a uniform that uses the sampler to access whatever texture(s) are currently bound.

The `texture` method takes the texture data and the texture coords and calculates the pixel colour:

```
in vec2 fragTexCoords;
uniform sampler2D textureData;
out vec4 finalColour;

finalColour = texture(textureData, fragTexCoords);
```
