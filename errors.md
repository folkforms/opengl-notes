# Errors

Severity levels are:

- 37190 high (GL43.GL_DEBUG_SEVERITY_HIGH)
- 37191 medium (GL43.GL_DEBUG_SEVERITY_MEDIUM)
- 37192 low (GL43.GL_DEBUG_SEVERITY_LOW)
- 33387 info (GL43.GL_DEBUG_SEVERITY_NOTIFICATION)

----

```
GL DEBUG MESSAGE:
  Severity: 37191
  Source:   33350
  Type:     33360
  ID:       131218
  Message:  Program/shader state performance warning: Vertex shader in program 22 is being recompiled based on GL state.
```  

This was happening because I was not calling `glUseProgram(0)` at the end of every frame. I don't know much more than that.

