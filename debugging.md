# Debugging

## Errors

Source: https://learnopengl.com/In-Practice/Debugging

The debug logging is only available in OpenGL 4.3, so you have to force it when running in OpenGL 3.3 mode (assume you have a graphics card capable of OpenGL 4.3.)

```
    boolean forceDebugMode = variables.getAsBoolean("game1.graphics.forceDebugMode");
    if (debugMode && (caps.OpenGL43 || forceDebugMode)) {
      LWJGL3Debug.setUpDebugMode();
    }
```

```
  public static void setUpDebugMode() {
    GL43.glEnable(GL43.GL_DEBUG_OUTPUT);
    if (variables.getAsBoolean("game1.graphics.syncDebugMode")) {
      GL43.glEnable(GL43.GL_DEBUG_OUTPUT_SYNCHRONOUS);
    }
    GL43.glDebugMessageControl(GL43.GL_DONT_CARE, GL43.GL_DONT_CARE,
        GL43.GL_DEBUG_SEVERITY_NOTIFICATION, (IntBuffer) null, false);
    GL43.glDebugMessageCallback((source, type, id, severity, length, message, userParam) -> {
      String msg = GLDebugMessageCallback.getMessage(length, message);
      System.err.println("GL DEBUG MESSAGE:");
      System.err.println(getErrorMessage("Severity", severity, severityMap));
      System.err.println(getErrorMessage("Source", source, sourceMap));
      System.err.println(getErrorMessage("Type", type, typeMap));
      System.err.println("  ID: " + id);
      System.err.println("  Message: " + msg);
    }, 0);
  }
```

`GL_DEBUG_OUTPUT_SYNCHRONOUS` means "log the error immediately". This means we can put a breakpoint in the above method and see where the error occurred!

FIXME I think adding `Thread.dumpStackTrace()` will work as well?

## Items not rendering

1. Turn off depth testing

If an items z-index is outside the range of 0 to 1 it will be discarded by depth testing.

This does not seem to apply to 3D rendering and I'm not sure why. But for 2D items they must have a z-index of 0 to 1 (in the old rendering pipeline this was 0 to 100,000 and normalised to be between 0 and 1.)

2. Check for clashing z-indices

If the z-index of two items is the same and they are rendered on top of each other, one will be drawn and one will not. It doesn't matter if there are transparent pixels. The solution is to have slightly different z-index values.
