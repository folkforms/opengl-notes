# Errors

Severity levels are:

- 37190 high
- 37191 medium
- 37192 low
- 33387 info

----

```
GL DEBUG MESSAGE:
  Source:   33350
  Type:     33360
  ID:       131218
  Severity: 37191
  Message:  Program/shader state performance warning: Vertex shader in program 22 is being recompiled based on GL state.
```  

This was happening because I was not calling `glUseProgram(0)` at the end of every frame. I don't know much more than that.
