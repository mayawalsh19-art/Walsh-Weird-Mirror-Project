THIS IS MY BUILD LOG 

09/09/2026: I started this project. 

09/15/2026: Today is the day I started connecting Claude MCP to Touch Designer and let me tell you it has not been easy. I downloaded Touch Designer last week and thought it was fairly easy to download and sign in to. However, I have spent hours trying to connect the MCP to TD and I do not know if it is user error or not. I have spent almost two hours trying to connect the two servers. It has been a lot of trial and error tactics, but I finally got it to work. 

The next step I did was ask Claude to implement a webcam within touch designer, which it seemed to get right away. Here is what some of my prompts were, what broke, and what happened next. 

1. MCP server wouldn't connect to Claude Code
  - What broke: td-mcp server crashed on startup — venv had mcp 2.x installed, but the script used the old v1 FastMCP API.
  - What I asked Claude: to troubleshoot why the touchdesigner MCP server showed "Connection closed." 
  - What happened: pinned mcp<2 in the venv, then hit a second break —
    FastMCP()'s description kwarg was renamed to instructions in this version. Fixed both, connection worked.

  2. TD's Web Server DAT wouldn't run Claude's commands
  - What broke: chain of GUI mixups — wrong file path, wrong DAT edited (text1 vs the actual wired-in webserver1_callbacks1), a typo in the Callbacks DAT reference (webserver1_callbacks vs ...1), and TextEdit silently corrupting whitespace on paste (caused a stray IndentationError). 
  - What I asked Claude: to keep diagnosing each dead end via the TD textport error logs.
  - What happened: eventually had Claude write the callback file directly to disk (bypassing TextEdit entirely) and point the DAT's File field at it —that's what finally stuck. Also hit one more runtime bug (KeyError: 'headers' — this TD version's response object has no headers dict) and just removed that line.

  3. Webcam feed was black
  - What broke: Video Device In TOP stuck at 128×128 "Initializing" — a stale macOS camera permission from before TouchDesigner was granted access.
  - What I asked Claude: why the feed was black.
  - What happened: fully quit and relaunched TouchDesigner (not just toggling the node) — that fixed it.

  4. No built-in face tracking on Mac
  - What broke: TD's native facetrackCHOP is Windows-only — errored with "not supported on this operating system."
  - What I asked Claude: to add the face → fruit effect anyway.
  - What happened: pivoted to a custom Script TOP using OpenCV Haar-cascade face detection. Then that broke too — TD's bundled OpenCV was stripped of its data files (cv2.data didn't exist) — so Claude downloaded the standard cascade XML directly and pointed the script at it.

  9/17/2026: Today I worked on researching three designers and finding responsive videos/artwork for reference and logged it in a GitHub file named designerworks.md. Next I came up with 10 concepts for potential Weird Mirror ideas. 

  This process was hard because it came down to trying to find different ideas, which is something that I struggled with. I feel like towards the end some of my ideas were combining, so it will be helpful to get some feedback this coming Monday. Next after coming up with the ideas, the inputs, the changes, and how the viewer knows, i used AI to help come up with some sketches. I knew I would not be able to correctly sketch out my ideas, so AI helped that process be able to tell the story a little bit better. I uploaded the concepts.md file and the sketches to GitHub. 

  Lastly, now I am logging what I did for homework today. I am excited to finalize an idea and start working on the project. 

  09/22/2026: Today I attempted to build a small version of a weird mirror with Claude and Touch Designer. The goal for the day was to build a version version that demonstrates the affordance/action/feedback loop closes. 

  I prompted Claude many of times to pixelate my camera, and it would not listen. Rather instead, it created a distortion with wavy lines across the screen when a person goes into view. 

  After awhile, I finally got it to pixelate, then the next step was to get the pixels to seperate when the person moves. I decided on Concept #7: Pixel Escape. 
  
  Input: Webcam/movement
  
  Changes: Pixels around the viewer scatter away from parts of their body as they move.
  
  How do they know?: Waving their hand appears to physically push pieces of the image away.

  Once the pixels were finally formed, I prompted Claude to have the pixels seperate when a body part moves and come back together when I am still again. It took a few prompts for it to understand what I was saying and is still not there yet, however it did get the gist of what I wanted it to do. I want to add more thing and revise it so that it all makes sense and has a bit of a narrative to it. 

  My hope is this: For Pixel Escape, I wanted to develop the interaction into more of a narrative instead of having pixels scatter just because someone moves. The experience begins as a normal webcam mirror, but once the viewer starts moving, pixels around their body begin to scatter away. Small movements create subtle changes, while larger and faster movements cause more of the image to break apart. The idea is that the pixels are almost afraid of the viewer and are trying to escape from them. If the viewer moves enough, their reflection eventually breaks apart completely. When they stop moving, the pixels slowly return and rebuild the image. This creates a simple rule for the viewer to discover: movement destroys the reflection, while stillness restores it. It is still a work in progress but it is a start. 