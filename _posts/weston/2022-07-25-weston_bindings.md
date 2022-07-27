```
weston-bindings(7)                                                               Miscellaneous Information Manual                                                               weston-bindings(7)

NAME
       weston-bindings - a list of keyboard bindings for Weston - the reference Wayland compositor

INTRODUCTION
       The Weston desktop shell has a number of keyboard shortcuts. They are listed here.

DESCRIPTION
       Almost all keyboard shortcuts for Weston include a specified modifier mod which is determined in the file weston.ini(5).

       mod + Shift + F
           Make active window fullscreen

       mod + K
           Kill active window

       mod + Shift + M
           Maximize active window

       mod + PageUp, mod + PageDown
           Zoom desktop in (or out)

       mod + Tab
           Switch active window

       mod + Up, mod + Down
           Increment/decrement active workspace number, if there are multiple

       mod + Shift + Up, mod + Shift + Down
           Move active window to the succeeding/preceding workspace, if possible

       mod + F1/F2/F3/F4/F5/F6
           Jump to the numbered workspace, if it exists

       Ctrl + Alt + Backspace
           If supported, terminate Weston. (Note this combination often is used to hard restart Xorg.)

       Ctrl + Alt + F
           Toggle if Weston is fullscreen; only works when nested under a Wayland compositor

       Ctrl + Alt + S
           Share output screen, if possible

       Ctrl + Alt + F1/F2/F3/F4/F5/F6/F7/F8
           Switch virtual terminal, if possible

       Super + S
           Make a screenshot of the desktop

       Super + R
           Start or stop recording video of the desktop

   TOUCH / MOUSE BINDINGS
       There are also a number of bindings involving a mouse:

       <Touch>, <Left button>, <Right button>
           Activate clicked window

       Super + Alt + <Vertical Scroll>
           Change the opacity of a window

       mod + <Vertical Scroll>
           Zoom/magnify the visible desktop

       mod + <Left button>
           Click and drag to move a window

       mod + Shift + <Left button>, mod + <Right button>, mod + <Touch>
           Click and drag to resize a window

       mod + <Middle button>
           Rotate the window (if supported)

   DEBUG BINDINGS
       The combination mod + Shift + Space begins a debug binding. Debug bindings are completed by pressing an additional key. For example, pressing F may toggle texture mesh wireframes with the
       GL renderer.  (In fact, most debug effects can be disabled again by repeating the command.)  Debug bindings are often tied to specific backends.

SEE ALSO
       weston(1), weston-launch(1), weston-drm(7), weston.ini(5), xkeyboard-config(7)

Weston 8.0.0                                                                                2019-03-27                                                                          weston-bindings(7)
```
