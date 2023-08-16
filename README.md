# Omega_Mouse

### Shout Outs:
Shout out to Aegis, chaossparrot, Andreas Arvidsson, TimoTimo, Tommy Pensyl, Ziemowit Musiatowicz, and everyone at the Talon Slack channel for being so helpful and answering my rookie questions. Talon Slack channel can be found here: (https://talonvoice.com/chat)

## What is Omega Mouse:
Omega Mouse is an alternative way to interact with Control Mouse in Talon Voice. It’s basically just turning Gaze and Head tracking on and off at different times to change the user experience working hands-free. So, it’s a simple user set that’s not very complex or robust.

## Design Purpose:
The purpose was to get away from Control Mouse having the Gaze Tracking on all the time, as a few factors made this feature uncomfortable for me. First, I found the cursor constantly following my Gaze to be difficult on my eyes. Second, I have keratoconus in one eye, meaning I have to use single-eye tracking, which introduces more jitter to the cursor. And third, I found blinking to heavily displace the cursor by shooting it to the bottom of the screen. This later issue did not seem to be prevented by Control Mouse’s internal Head Tracking mode consistently enough, which led to unsustainable eye straining. These issues could very well be personal biological issues I have with my eyes, or my setup at home. Although something like toggling the eye tracking with a shortcut could help with my first issue, I found these discomforts problematic enough to try and code my own preferred method of solving these obstacles.

## Control Mouse vs Omega Mouse:
That said, I am not a coder, so I’m sure the code is not very advanced. In general, I do recommend you use the default Control Mouse in Talon Voice if possible. It will give you the most efficient and fluid experience of working hands-free. Omega Mouse is clunky, and was made for the niche audience of myself. If I could use Control Mouse effectively, I would. But my personal preferences and situation made Control Mouse uncomfortable to use. So, if you cannot use Control Mouse very well for whatever reason, like me, you can give Omega Mouse a try.

There is also a “hidden” eye tracking mode in Talon Voice since the 0.4 release you can try first. Set Control Mouse on, Gaze Tracking off, Head Tracking on. When you look away from your cursor and move your head, the cursor will warp to your gaze. So even if the default Control mouse functionality doesn’t work for you, this might. However, I have found it doesn’t really work with single eye tracking about 95% of the time. But once in a while it picks up your eye gaze and shoots the cursor there, even if that was not your intent. With 2 eye tracking, it’s very consistent. There is no way to disable this feature at the moment. This means it may interfere with you ruse of Omega Mouse. If you want to use Omega Mouse, I recommend you use single Eye tracking.

## Who is Omega Mouse For:
Omega Mouse may be for you if:
-	You have to use one eye tracking instead of both eye tracking (this results in a jittery mouse)
-	Your blinking normally throws your cursor off to an unsatisfactory degree
-	You don’t want the cursor constantly following your Gaze.
-	You like Zoom Mouse but wish it had head tracking functionality.

## Requirements:
-	Talon Voice 0.4 public release (I’m subscribed to the beta, but the 0.4 release should include the required functionality.)
-	Talon Community Repo Installed: 
-	Tobii Eye Tracker 5
-	A Microphone.
o	Recommended mics for Talon Voice here: https://talon.wiki/hardware/
o	I’m using a cheap usb Koss CS-100. It works okay, but is prone to misidentifying commands.

## Set-Up:
To use Omega Mouse with unintended cursor warping, use single eye tracking. The hidden feature (mentioned in “Control Mouse vs Omega Mouse”) cannot be disabled at the moment. But usually doesn’t work with single eye tracking, making it the most reliable way to “disable” it so Omega Mouse can work as intended.

In order for Omega Mouse to maintain its proper functionality as error free as possible, mouse.talon must be edited in the community repo. The functions in Eye_Tracking_Switches.py ("control_mouse_switch()" & "zoom_mouse_switch()") must be placed into mouse.talon for the rules Control Mouse and Zoom Mouse respectively. If you want to keep the original commands intact to revert to them easily later if you ever remove Omega Mouse, add a "#" before the commands that were
there before to deactivate them. It should look like this in mouse.py:

control mouse: 
	# tracking.control_toggle()
	user.control_mouse_switch()
zoom mouse:
	# tracking.control_zoom_toggle()
	user.zoom_mouse_switch()

Note: Updating the community repo in the future will require you to either re-edit the mouse.py file, or resolve these merge conflicts if using Git. These edits allow Omega Mouse to turn off it’s behavior when switching to default Control Mouse or Zoom Mouse. Without these edits, if you were to switch to one of these modes from Omega Mouse, remnant behavior may continue and interfere.

Also Note: Omega Mouse must be switched to-and-from verbally. Using the menu with your mouse will bypass the switches in the code, leading to unintended behavior.

### For Mac Users:
I don't have a Mac, so I have not tested this code on a Mac. But I assume it will work all the same. However, you will at least need to replace the "alt" key with "cmd" key in the function "omega_mouse_modifiers_release_function" found in the Omega_Mouse.py file.


## 3 Mode Summary:
There are three “modes” in Omega Mouse (3 different contexts) that behave slightly differently: Full, Lite, and Basic. But they all follow the same idea: The cursor does not follow your eye gaze, but will warp to your eye gaze when popping, with fine-tuned movement reserved for Head tracking. FULL mode uses popping to both move the cursor and left click in a 2-phase process (like zoom mouse). LITE uses popping to warp the cursor to your gaze, but requires a separate command to click. BASIC is the same as LITE, except instead of Head tracking turning off to keep the cursor still when not in use, Head tracking remains on all the time. I found these three modes can each serve a purpose, but one may be good enough for you.

FULL MODE
-	Convenient use of popping for move-and-click
-	Clunkier to maneuver due to start-stop dynamic

LITE MODE
-	Maneuverable due to popping solely used for cursor movement.
-	Less convenient due to two commands for move-and-click

BASIC MODE
-	Most maneuverable due to popping solely used for cursor movement, and Head tracking always being on for small adjustments.
-	Less convenient due to two commands for move-and-click

The decision to make popping the primary cursor movement command across all three modes (instead of the primary clicking command) came from noticing that moving the mouse needs to be an immediate response to feel satisfying. Clicking is easier to have patience for since you’re hovering over your target already while you wait for Talon to parse your voice command.

### Changing modes:
To choose which mode to use: Open the Omega_Mouse.talon file -> change the “user.omega_mouse_mode” setting number to 0 (Full mode), 1 (Lite mode), or 2 (basic mode) -> save the file -> If Omega Mouse was active when you made these changes, you must restart Omega Mouse to pick up the change.

### 3 Modes Explained:
FULL MODE: Moving the cursor and left clicking are done in a 2-phase process (like Zoom Mouse) with a popping sound. The first pop moves the cursor to your gaze (and enables Head tracking). The second pop left clicks (and disables Head tracking). Actions like clicking or starting a drag will leave your cursor still.

This process changes if you are dragging. While dragging with the mouse, popping merely moves the cursor, but does not click. To release dragging, the drop command (drop / drag end / end drag) must be invoked. This was done to simplify undoing a decision to drag by simply popping back to where you started and releasing. Otherwise, you could get caught in the 2-phase process.

LITE MODE: Moving the cursor and left click are separated into two separate commands. Moving the cursor is done by a pop. Left click is done by saying “yum” or “gum”. Actions like clicking or starting a drag will leave your cursor still.

The decision to settle on yum/gum came from observing the position of your tongue pre and post popping sound. The combination of popping and saying “yum” created a natural loop of commands that I found comfortable to run back-to-back. In order to pop, the back of your tongue has to block the passage of your throat to create a “cave” or “vacuum” for the pop sound to produce. After you make a popping sound, your mouth remains open, but your tongue is still at the back of your throat. Having a sound that starts from the back of your throat is the most natural next sound to make (which I find “yum” to satisfy), and which most likely will need to be a left click. Likewise, to start another pop sound, you want your lips to start together, which the M ending in “yum” provides.

BASIC MODE: The same as Lite Mode except actions like clicking or starting a drag will not leave your cursor still, but instead keep using Head Tracking. Essentially Control Mouse without eye gaze on all the time.


## Omega Mouse Commands:
There are 13 commands associated with Omega Mouse, whose behavior changes based on the mode they are in. Images of command logic flow charts are provided at the bottom for a (messy) visual reference.

Note: By default, Omega Mouse requires you to use “yum” or “gum” for left click (lite/basic modes) and twill for double click (all modes). Omega Mouse overrides some community repo functions to work with the community voice commands you might be familiar with. But the community voice commands for left click and double click do not use functions easily overridden. To minimize editing the community repo on the user end, new functions had to be created to maintain Omega Mouse behavior, hence “yum/gum” and “twill”. If you have custom voice commands, they will need to be reconciled with the Omega Mouse functions.

### Voice Commands
- Omega Mouse: Toggles Omega Mouse on/off. Accesses mode value when turning on.
- Omega Restart: Sets Omega Mouse to initial states. Re-captures mode value.
- Control Mouse: Turns Omega Mouse off and switches to default Control Mouse
  - See “Set-Up” section to make sure mouse.py edits are done correctly
- Zoom Mouse: Turns Omega Mouse off and switches to default Zoom Mouse
  - See “Set-Up” section to make sure mouse.py edits are done correctly

- *Popping sound*:
  - Full Mode:
    - Phase 1: Moves cursor.
    - Phase 2: Left Click. (Freeze cursor.)
    - While dragging: only moves cursor.
  - Lite Mode:
    - Moves cursor. (Freeze cursor)
  - Basic Mode:
    - Moves cursor
  - Omega Mouse Off:
    - Community default left click
- Yum / Gum:
  - Full Mode:
    - Left click. (Freeze cursor)
  - Lite Mode:
    - Left click. (Freeze cursor)
  - Basic Mode:
    - Left click.
  - Omega Mouse Off:
    - Left Click.
- Yummer / Gummer:
  - Full Mode:
    - Left click + release modifier keys (Freeze Cursor)
  - Lite Mode:
    - Left click + release modifier keys (Freeze Cursor)
  - Basic Mode:
    - Left click + release modifier keys
  - Omega Mouse Off:
    - Left click + release modifier keys (Freeze Cursor)
- Twill
  - Full Mode:
    - Double click (Freeze cursor)
  - Lite Mode:
    - Double click (Freeze cursor)
  - Basic Mode:
    - Double click
  - Omega Mouse Off:
    - Double click
- Nudge:
  - Full Mode:
    - Skips Gaze tracking to immediately use Head tracking for small adjustments.
  - Lite Mode:
    - Does nothing
  - Basic Mode:
    - Does nothing
  - Omega Mouse Off:
    - Does nothing
- Wait:
  - Full Mode:
    - Freezes cursor. (Does not release drag or modifier keys)
  - Lite Mode:
    - Freezes cursor. (Does not release drag or modifier keys)
  - Basic Mode:
    - Freezes cursor. (Does not release drag or modifier keys)
  - Omega Mouse Off:
    - Does nothing
- Drag:
  - Full Mode:
    - Starts a left mouse button hold. (Freezes cursor) (Alters 2-phase behavior)
  - Lite Mode:
    - Starts a left mouse button hold. (Freezes cursor)
  - Basic Mode:
    - Starts a left mouse button hold.
  - Omega Mouse Off:
    - Community default drag
- Drop / Drag End / End Drag:
  - Full Mode:
    - Releases all mouse buttons + Modifier keys. (Freezes cursor)
  - Lite Mode:
    - Releases all mouse buttons + Modifier keys. (Freezes cursor)
  - Basic Mode:
    - Releases all mouse buttons + Modifier keys.
  - Omega Mouse Off:
    - Community default drag end
- Omega Check State:
  - Full Mode:
    - Prints state of variables and tags in Talon log viewer (for troubleshooting)
  - Lite Mode:
    - Prints state of variables and tags in Talon log viewer (for troubleshooting)
  - Basic Mode:
    - Prints state of variables and tags in Talon log viewer (for troubleshooting)
  - Omega Mouse Off:
    - Does nothing

