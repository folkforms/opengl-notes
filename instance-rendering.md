# Instance rendering

Typically meshes use instances for performance. This means a mesh will usually have an array of positions, rotations, and scales for each instance of that mesh.

A mesh might have a buffer for 100 instances but only be using 20. It can repalce the buffer with a larger one if needed, at a small cost to performance.

Rendering is almost identical to the above. The only difference is we call `GL33.glDrawElementsInstanced` and tell it how many instances we want to render. It will loop over the data for each instance in sequence.
