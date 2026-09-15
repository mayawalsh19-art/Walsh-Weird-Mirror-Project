THIS IS MY BUILD LOG 

09/09/2026: I started this project. 

09/15/2026: Today is the day I started connecting Claude MCP to Touch Designer and let me tell you it has not been easy. I downloaded Touch Designer last week and thought it was fairly easy to download and sign in to. However, I have spent hours trying to connect the MCP to TD and I do not know if it is user error or not. I have spent almost two hours trying to connect the two servers. It has been a lot of trial and error tactics, but I finally got it to work. 

The next step I did was ask Claude to implement a webcam within touch designer, which it seemed to get right away. Here is what some of my prompts were, what broke, and what happened next. 

1. MCP server wouldn't connect to Claude Code
  - What broke: td-mcp server crashed on startup — venv had mcp 2.x installed,
    but the script used the old v1 FastMCP API.
  - What I asked Claude: to troubleshoot why the touchdesigner MCP server showed
    "Connection closed." 
  - What happened: pinned mcp<2 in the venv, then hit a second break —
    FastMCP()'s description kwarg was renamed to instructions in this version.
    Fixed both, connection worked.

  2. TD's Web Server DAT wouldn't run Claude's commands
  - What broke: chain of GUI mixups — wrong file path, wrong DAT edited (text1
    vs the actual wired-in webserver1_callbacks1), a typo in the Callbacks DAT
    reference (webserver1_callbacks vs ...1), and TextEdit silently corrupting
    whitespace on paste (caused a stray IndentationError). 
  - What I asked Claude: to keep diagnosing each dead end via the TD textport
    error logs.
  - What happened: eventually had Claude write the callback file directly to
    disk (bypassing TextEdit entirely) and point the DAT's File field at it —
    that's what finally stuck. Also hit one more runtime bug (KeyError:  
    'headers' — this TD version's response object has no headers dict) and just
    removed that line.

  3. Webcam feed was black
  - What broke: Video Device In TOP stuck at 128×128 "Initializing" — a stale
    macOS camera permission from before TouchDesigner was granted access.
  - What I asked Claude: why the feed was black.
  - What happened: fully quit and relaunched TouchDesigner (not just toggling
    the node) — that fixed it.

  4. No built-in face tracking on Mac
  - What broke: TD's native facetrackCHOP is Windows-only — errored with "not
    supported on this operating system."
  - What I asked Claude: to add the face → fruit effect anyway.
  - What happened: pivoted to a custom Script TOP using OpenCV Haar-cascade face
    detection. Then that broke too — TD's bundled OpenCV was stripped of its
    data files (cv2.data didn't exist) — so Claude downloaded the standard
    cascade XML directly and pointed the script at it.

09/15/2026: I also got the webcam face effect working visually. The camera feed is
masked into a peach-shaped face with a green stem and leaf, which gives the mirror
an uncanny, playful fruit-person look. The soft pink and peach shapes make the
whole image feel distorted, strange, and a little surreal.

![Weird Mirror Photo](Screenshot%202026-09-15%20at%203.47.44%20PM.png)


