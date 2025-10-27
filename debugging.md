# Debugging

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
