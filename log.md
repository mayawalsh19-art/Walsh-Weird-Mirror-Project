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

  09/27/2026: Today our task was to get the whole flow working from start to finish even if it may appear to look bad. Originally my idea was a pixelated camera view where when you moved the pixels broke a part and the movement was delayed. 

  Since then, I altered my idea a little bit after recieving a critique from the class. Now my idea focusses more on the idea of a sequins shirt where if you swipe one way it changes to a photo and if you swipe back the other way then it changes back to the pixelated image of yourself. 

  I actually did not struggle making the actual output as Claude listened pretty well. I told Claude to use an image of a Jayhawk when swiping my hand and make it pixelated and it did that instantly. The only thing it struggled with was the swiping motion, which we are still working on. I prompted it several times, however it still has a few bugs to fix in later sessions. 

  09/28/2026: Today's task was to polish and fully create a full interaction flow of our projects. I started by brainstorming what more I could add and what journey I could go down. 

  I came to this conclusion of my journey. Ideally you would step in front of the camera and it would be a pixelated image of you and the background. My idea was with the swip of your hand in one direction it would depict a pixelated jayhawk and with the swipe of your hand the opposite direction it would depict your face again. I just added in the aspect of when the jayhawk is fully shown then the "I'm a Jayhawk" song starts playing. 

  Here are the steps we took today to make a lot of progress: 

  1. Tried skin-based swipe detection. Only moving skin counted, to stop stray
     Jayhawk pixels and leftover patches. Your body still triggered it in a tank
     top, so we moved on.
  2. Tried MediaPipe hand tracking inside TouchDesigner. It froze TouchDesigner,
     and you had to force-quit (nothing was lost).
  3. Set up real hand tracking as a separate helper program. MediaPipe 0.10.14
     runs in ~/Desktop/td-mcp/handtracker/, and TouchDesigner sends it camera
     frames and gets back hand positions. The Jayhawk now follows only your
     hands' paths, and TouchDesigner starts the helper automatically.
  4. Added body delay and break-apart. Moving your body makes your pixels drag
     and blocks scatter, while hands keep swiping the Jayhawk. This also
     replaced the old always-wobbling scatter.
  5. Kept the Jayhawk still once it's revealed. No drag or scatter on or near
     the picture.
  6. Made your pixels come back faster. The drag recovers in about 0.13 s (was
     0.5 s), and the break-apart in about 0.25 s (was 0.6 s).
  7. Built, then removed, the Lawrence journey. We tried your KU prints as a
     journey (poster payoffs, then posters as the reveal), then went back to
     your original Jayhawk design at 40×30 pixels.
  8. Added a face guard. Your face can never create the Jayhawk, only your
     hands, and the tracker was made stricter.
  9. Added the KU fight song. "I'm a Jayhawk" (ku_song.mp3) plays when the
     Jayhawk covers 90% of the screen, and fades out when you swipe back to your
     face.
  10. Polish step 1: reset for the next stranger. After 20 seconds with nobody
      there, every sequin flips back to the mirror and the song stops.
  11. Polish step 2: run-through test. Your recorded run confirmed steady 60
      fps, no errors, swipes only from hands, and the song starting and stopping
      correctly. You skipped the walk-away and 3-minute unattended tests, which
      are still worth doing before the demo.
  12. Polish step 3: visual payoff. When the song starts, light sweeps across
      the Jayhawk's sequins, then they twinkle while it plays.

Overall it was a very successful day and progress photos will be within the TDprogress folder along with a screen recording of the improvements!

10/04/2026: Today's main focus was on guerilla testing and user testing in general. First before testing I wanted to work out the kinks and make sure that TD understood I needed my weird mirror to be a mirrored view and that I only wanted the mirror to pick up HAND movements to create my image of a jayhawk. I have had a lot of practice with prompting Claude by now, which has definitely helped in the final product. I have not had a lot of chances where things have broke. Within this session it was very straight forward: edit how the mirror percieves a person and user test. 

User testing: I set up my computer in front of two people who were visiting for the weekend and they walked in front of it. 