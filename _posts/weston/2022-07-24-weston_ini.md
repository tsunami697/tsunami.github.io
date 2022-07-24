```
weston.ini(5)                                                                         File Formats Manual                                                                         weston.ini(5)

NAME
       weston.ini - configuration file for Weston - the reference Wayland compositor

INTRODUCTION
       Weston obtains configuration from its command line parameters and the configuration file described here.
	   [weston从其命令行参数和此处描述的配置文件中获取配置。]

DESCRIPTION
       Weston uses a configuration file called weston.ini for its setup.  The weston.ini configuration file is searched for in one of the following places when the server is started:
	   [weston使用称为weston.ini的配置文件进行设置。启动服务器时，将在以下一个位置之一中搜索Weston.ini配置文件：]

           $XDG_CONFIG_HOME/weston.ini   (if $XDG_CONFIG_HOME is set)
           $HOME/.config/weston.ini      (if $HOME is set)
           weston/weston.ini in each
               $XDG_CONFIG_DIR           (if $XDG_CONFIG_DIRS is set)
           /etc/xdg/weston/weston.ini    (if $XDG_CONFIG_DIRS is not set)

       where  environment  variable $HOME is the user's home directory, and $XDG_CONFIG_HOME is the user specific configuration directory, and 
	   $XDG_CONFIG_DIRS is a colon ':' delimited liste dof configuration base directories, such as /etc/xdg-foo:/etc/xdg.
	   [环境变量$ home是用户的家庭目录]
	   [$xdg_config_home是用户制定的配置目录]
	   [$xdg_config_dirs是一个以'：'界定列表dof配置基本目录，例如/etc/xdg-foo：/etc/xdg。]

       The weston.ini file is composed of a number of sections which may be present in any order, or omitted to use default configuration values. Each section has the form:
	   [Weston.ini文件由许多部分组成，这些部分可能以任何顺序存在，或省略用于使用默认配置值。每个部分的格式：]

           [SectionHeader]
           Key1=Value1
           Key2=Value2
               ...

       The spaces are significant.  Comment lines are ignored:
	   [注释格式]

           #comment

       The section headers are:

           core           The core modules and options
           libinput       Input device configuration
           shell          Desktop customization
           launcher       Add launcher to the panel
           output         Output configuration
           input-method   Onscreen keyboard input
           keyboard       Keyboard layouts
           terminal       Terminal application options
           xwayland       XWayland options
           screen-share   Screen sharing options

       Possible value types are string, signed and unsigned 32-bit integer, and boolean. Strings must not be quoted, do not support any escape sequences, and run till the end of the line. In‐
       tegers can be given in decimal (e.g. 123), octal (e.g. 0173), and hexadecimal (e.g. 0x7b) form. Boolean values can be only 'true' or 'false'.

	   [可以填充能的value类型是string，signed和unsigned int 和 bool]
	   [字符串不能上引号，不支持任何转义序列]

CORE SECTION
       The core section is used to select the startup compositor modules and general options.
	   [核心部分用于选择启动复合器模块和常规选项。]

       shell=desktop-shell.so
              specifies  a  shell to load (string). This can be used to load your own implemented shell or one with Weston as default. Available shells in the /usr/lib/x86_64-linux-gnu/weston
              directory are:
			  [指定要加载的外壳（字符串）,这可以用来加载您自己的实现的外壳，也可以用weston默认的shell]

                 desktop-shell.so

       xwayland=true
              ask Weston to load the XWayland module (boolean).

       modules=cms-colord.so,screen-share.so
              specifies the modules to load (string). Available modules in the /usr/lib/x86_64-linux-gnu/weston directory are:
			  [指定要加载的模块（字符串): 路径：/usr/lib/x86_64-linux-gnu/weston]

                 cms-colord.so
                 screen-share.so

       backend=headless-backend.so
              overrides defaults backend. Available backend modules in the /usr/lib/x86_64-linux-gnu/libweston-8 directory are:
			  [默认后端,路径：/usr/lib/x86_64-linux-gnu/libweston-8]

                 drm-backend.so
                 fbdev-backend.so
                 headless-backend.so
                 rdp-backend.so
                 wayland-backend.so
                 x11-backend.so

       repaint-window=N
              Set the approximate length of the repaint window in milliseconds. The repaint window is used to control and reduce the output latency for clients. If the window is  longer  than
              the  output  refresh  period,  the  repaint  will be done immediately when the previous repaint finishes, not processing client requests in between. If the repaint window is too
              short, the compositor may miss the target vertical blank, increasing output latency. The default value is 7 milliseconds. The allowed range is from -10 to 1000 milliseconds. Us‐
              ing a negative value will force the compositor to always miss the target vblank.
			  [以毫秒为单位设置重新启动窗口的大致长度。重新启动窗口用于控制和减少客户的输出延迟]

       gbm-format=format
              sets the GBM format used for the framebuffer for the GBM backend. Can be xrgb8888, xrgb2101010, rgb565.  By default, xrgb8888 is used.
			  [设置用于GBM后端的Framebuffer的GBM格式。可以是XRGB8888，XRGB2101010，RGB565。默认情况下，使用XRGB8888。]

       idle-time=seconds
              sets  Weston's  idle  timeout in seconds. This idle timeout is the time after which Weston will enter an "inactive" mode and screen will fade to black. A value of 0 disables the
              timeout.
			  [将Weston的闲置超时设置为几秒钟。此闲置超时是weston进入“不活动”模式的时间，并且屏幕将淡入黑色或者锁屏。值0禁用这个延时]

              Important : This option may also be set via Weston's '-i' command line option and will take precedence over the current .ini option. This means that if both weston.ini and  com‐
              mand line define this idle-timeout time, the one specified in the command-line will be used. On the other hand, if none of these sets the value, default idle timeout will be set
              to 300 seconds.
			  [此选项也可以通过Weston的“-i”命令行选项设置，并且将优先于当前.INI选项]

       require-input=true
              require an input device for launch

       pageflip-timeout=milliseconds
              sets Weston's pageflip timeout in milliseconds.  This sets a timer to exit gracefully with a log message and an exit code of 1 in case the DRM driver is non-responsive.  Setting
              it to 0 disables this feature.
			  [将Weston的PageFlip超时设置为毫秒]

       wait-for-debugger=true
              Raises SIGSTOP before initializing the compositor. This allows the user to attach with a debugger and continue execution by sending SIGCONT. This is useful for debugging a crash
              on start-up when it would be inconvenient to launch weston directly from a debugger. Boolean, defaults to false.  There is also a command line option to do the same.

       remoting=remoting-plugin.so
              specifies a plugin for remote output to load (string). This can be used to load your own implemented remoting plugin or one with Weston as default. Available remoting plugins in
              the __libweston_modules_dir__ directory are:

                 remoting-plugin.so

       use-pixman=true
              Enables pixman-based rendering for all outputs on backends that support it.  Boolean, defaults to false.  There is also a command line option to do the same.

LIBINPUT SECTION
       The  libinput section is used to configure input devices when using the libinput input device backend. The defaults are determined by libinput and vary according to what is most sensi‐
       ble for any given device.

       Available configuration are:

       enable-tap=false
              Enables tap to click on touchpad devices.

       tap-and-drag=false
              For touchpad devices with enable-tap enabled. If the user taps, then taps a second time, this time holding, the virtual mouse button stays down for as long  as  the  user  keeps
              their finger on the touchpad, allowing the user to click and drag with taps alone.

       tap-and-drag-lock=false
              For  touchpad  devices  with enable-tap and tap-and-drag enabled.  In the middle of a tap-and-drag, if the user releases the touchpad for less than a certain number of millisec‐
              onds, then touches it again, the virtual mouse button will remain pressed and the drag can continue.

       disable-while-typing=true
              For devices that may be accidentally triggered while typing on the keyboard, causing a disruption of the typing.  Disables them while the keyboard is in use.

       middle-button-emulation=false
              For pointer devices with left and right buttons, but no middle button.  When enabled, a middle button event is emitted when the left and right  buttons  are  pressed  simultane‐
              ously.

       left-handed=false
              Configures  the  device for use by left-handed people. Exactly what this option does depends on the device. For pointers with left and right buttons, the buttons are swapped. On
              tablets, the tablet is logically turned upside down, because it will be physically turned upside down.

       rotation=n
              Changes the direction of the logical north, rotating it n degrees clockwise away from the default orientation, where n is a whole number between 0 and 359 inclusive. Needed  for
              trackballs, mainly. Allows the user to orient the trackball sideways, for example.

       accel-profile={flat,adaptive}
              Set  the  pointer  acceleration  profile. The pointer's screen speed is proportional to the physical speed with a certain constant of proportionality.  Call that constant alpha.
              flat keeps alpha fixed. See accel-speed.  adaptive causes alpha to increase with physical speed, giving the user more control when the speed is slow, and  more  reach  when  the
              speed is high.  adaptive is the default.

       accel-speed=v
              If  accel-profile is set to flat, it simply sets the value of alpha.  If accel-profile is set to adaptive, the effect is more complicated, but generally speaking, it will change
              the pointer's speed.  v is normalised and must lie in the range [-1, 1]. The exact mapping between v and alpha is hardware-dependent,  but  higher  values  cause  higher  cursor
              speeds.

       natural-scroll=false
              Enables  natural  scrolling,  mimicking  the behaviour of touchscreen scrolling.  That is, if the wheel, finger, or fingers are moved down, the surface is scrolled up instead of
              down, as if the finger, or fingers were in contact with the surface being scrolled.

       scroll-method={two-finger,edge,button,none}
              Sets the scroll method. two-finger scrolls with two fingers on a touchpad. edge scrolls with one finger on the right edge of a touchpad.  button  scrolls  when  the  pointer  is
              moved while a certain button is pressed. See scroll-button. none disables scrolling altogether.

       scroll-button={BTN_LEFT,BTN_RIGHT,BTN_MIDDLE,...}
              For devices with scroll-method set to button. Specifies the button that will trigger scrolling. See /usr/include/linux/input-event-codes.h for the complete list of possible val‐
              ues.

       touchscreen_calibrator=true
              Advertise the touchscreen calibrator interface to all clients. This is a potential denial-of-service attack vector, so it should only be enabled on trusted  userspace.  Boolean,
              defaults to false.

              The interface is required for running touchscreen calibrator applications. It provides the application raw touch events, bypassing the normal touch handling.  It also allows the
              application to upload a new calibration into the compositor.

              Even though this option is listed in the libinput section, it does affect all Weston configurations regardless of the used backend. If the backend does not use libinput, the in‐
              terface can still be advertised, but it will not list any devices.

       calibration_helper=/bin/echo
              An optional calibration helper program to permanently save a new touchscreen calibration. String, defaults to unset.

              The  given  program  will  be executed with seven arguments when a calibrator application requests the server to take a new calibration matrix into use.  The program is executed
              synchronously and will therefore block Weston for its duration. If the program exit status is non-zero, Weston will not apply the new calibration. If the helper is unset or  the
              program exit status is zero, Weston will use the new calibration immediately.

              The program is invoked as:

                 calibration_helper syspath m1 m2 m3 m4 m5 m6

              where syspath is the udev sys path for the device and m1  through m6 are the calibration matrix elements in libinput's LIBINPUT_CALIBRATION_MATRIX udev property format.  The sys
              path is an absolute path and starts with the sys mount point.

SHELL SECTION
       The shell section is used to customize the compositor. Some keys may not be handled by different shell plugins.

       The entries that can appear in this section are:

       client=file
              sets the path for the shell client to run. If not specified weston-desktop-shell is launched (string).

       background-image=file
              sets the path for the background image file (string).

       background-type=tile
              determines how the background image is drawn (string). Can be centered, scale, scale-crop or tile (default).  Centered shows the image once centered. If  the  image  is  smaller
              than  the  output,  the rest of the surface will be in background color. If the image size does fit the output it will be cropped left and right, or top and bottom.  Scale means
              scaled to fit the output precisely, not preserving aspect ratio.  Scale-crop preserves aspect ratio, scales the background image just big enough to cover the output, and centers
              it. The image ends up cropped from left and right, or top and bottom, if the aspect ratio does not match the output. Tile repeats the background image to fill the output.

       background-color=0xAARRGGBB
              sets the color of the background (unsigned integer). The hexadecimal digit pairs are in order alpha, red, green, and blue.

       clock-format=format
              sets the panel clock format (string). Can be none, minutes, seconds.  By default, minutes format is used.

       panel-color=0xAARRGGBB
              sets the color of the panel (unsigned integer). The hexadecimal digit pairs are in order transparency, red, green, and blue. Examples:

                 0xffff0000    Red
                 0xff00ff00    Green
                 0xff0000ff    Blue
                 0x00ffffff    Fully transparent

       panel-position=top
              sets the position of the panel (string). Can be top, bottom, left, right, none.

       locking=true
              enables screen locking (boolean).

       animation=zoom
              sets the effect used for opening new windows (string). Can be zoom, fade, none.  By default, no animation is used.

       close-animation=fade
              sets the effect used when closing windows (string). Can be fade, none.  By default, the fade animation is used.

       startup-animation=fade
              sets the effect used for opening new windows (string). Can be fade, none.  By default, the fade animation is used.

       focus-animation=dim-layer
              sets the effect used with the focused and unfocused windows. Can be dim-layer, none.  By default, no animation is used.

       allow-zap=true
              whether the shell should quit when the Ctrl-Alt-Backspace key combination is pressed

       binding-modifier=ctrl
              sets  the  modifier  key used for common bindings (string), such as moving surfaces, resizing, rotating, switching, closing and setting the transparency for windows, controlling
              the backlight and zooming the desktop. See weston-bindings(7).  Possible values: none, ctrl, alt, super (default)

       num-workspaces=6
              defines the number of workspaces (unsigned integer). The user can switch workspaces by using the binding+F1, F2 keys. If this key is not set, fall back to one workspace.

       cursor-theme=theme
              sets the cursor theme (string).

       cursor-size=24
              sets the cursor size (unsigned integer).

       lockscreen-icon=path
              sets the path to lock screen icon image (string). (tablet shell only)

       lockscreen=path
              sets the path to lock screen background image (string). (tablet shell only)

       homescreen=path
              sets the path to home screen background image (string). (tablet shell only)

LAUNCHER SECTION
       There can be multiple launcher sections, one for each launcher.

       icon=icon
              sets the path to icon image (string). Svg images are not currently supported.

       path=program
              sets the path to the program that is run by clicking on this launcher (string).  It is possible to pass arguments and environment variables to the program. For example:

                  path=GDK_BACKEND=wayland gnome-terminal --full-screen

OUTPUT SECTION
       There can be multiple output sections, each corresponding to one output. It is currently only recognized by the drm and x11 backends.

       name=name
              sets a name for the output (string). The backend uses the name to identify the output. All X11 output names start with a letter X.  All Wayland output names start with the  let‐
              ters WL.  The available output names for DRM backend are listed in the weston-launch(1) output.  Examples of usage:

                 LVDS1    DRM backend, Laptop internal panel no.1
                 VGA1     DRM backend, VGA connector no.1
                 X1       X11 backend, X window no.1
                 WL1      Wayland backend, Wayland window no.1

              See weston-drm(7) for more details.

       mode=mode
              sets  the  output mode (string). The mode parameter is handled differently depending on the backend. On the X11 backend, it just sets the WIDTHxHEIGHT of the weston window.  The
              DRM backend accepts different modes, along with an option of a modeline string.

              See weston-drm(7) for examples of modes-formats supported by DRM backend.

       transform=normal
              The transformation applied to screen output (string). The transform key can be one of the following 8 strings:

                 normal        Normal output.
                 90            90 degrees clockwise.
                 180           Upside down.
                 270           90 degrees counter clockwise.
                 flipped       Horizontally flipped
                 flipped-90    Flipped and 90 degrees clockwise
                 flipped-180   Flipped upside down
                 flipped-270   Flipped and 90 degrees counter clockwise

       scale=factor
              The scaling multiplier applied to the entire output, in support of high resolution ("HiDPI" or "retina") displays, that roughly corresponds to the pixel ratio of  the  display's
              physical  resolution  to the logical resolution.  Applications that do not support high resolution displays typically appear tiny and unreadable. Weston will scale the output of
              such applications by this multiplier, to make them readable. Applications that do support their own output scaling can draw their content in high resolution, in which case  they
              avoid compositor scaling. Weston will not scale the output of such applications, and they are not affected by this multiplier.

              An integer, 1 by default, typically configured as 2 or higher when needed, denoting the scaling multiplier for the output.

       seat=name
              The  logical  seat name that this output should be associated with. If this is set then the seat's input will be confined to the output that has the seat set on it. The expecta‐
              tion is that this functionality will be used in a multiheaded environment with a single compositor for multiple output and input configurations. The default seat is called  "de‐
              fault" and will always be present. This seat can be constrained like any other.

       allow_hdcp=true
              Allows  HDCP  support for this output. If set to true, HDCP can be tried for the content-protection, provided by the backends, on this output. By default, HDCP support is always
              allowed for an output. The content-protection can actually be realized, only if the hardware (source and sink) support HDCP, and the backend has the implementation  of  content-
              protection protocol. Currently, HDCP is supported by drm-backend.

INPUT-METHOD SECTION
       path=/usr/libexec/weston-keyboard
              sets the path of the on screen keyboard input method (string).

KEYBOARD SECTION
       This section contains the following keys:

       keymap_rules=evdev
              sets the keymap rules file (string). Used to map layout and model to input device.

       keymap_model=pc105
              sets the keymap model (string). See the Models section in xkeyboard-config(7).

       keymap_layout=us,de,gb
              sets the comma separated list of keyboard layout codes (string). See the Layouts section in xkeyboard-config(7).

       keymap_variant=euro,,intl
              sets the comma separated list of keyboard layout variants (string). The number of variants must be the same as the number of layouts above. See the Layouts section in xkeyboard-
              config(7).

       keymap_options=grp:alt_shift_toggle,grp_led:scroll
              sets the keymap options (string). See the Options section in xkeyboard-config(7).

       repeat-rate=40
              sets the rate of repeating keys in characters per second (unsigned integer)

       repeat-delay=400
              sets the delay in milliseconds since key down until repeating starts (unsigned integer)

       numlock-on=false
              sets the default state of the numlock on weston startup for the backends which support it.

       vt-switching=true
              Whether to allow the use of Ctrl+Alt+Fn key combinations to switch away from the compositor's virtual console.

TERMINAL SECTION
       Contains settings for the weston terminal application (weston-terminal). It allows to customize the font and shell of the command line interface.

       font=DejaVu Sans Mono
              sets the font of the terminal (string). For a good experience it is recommended to use monospace fonts. In case the font is not found, the default one is used.

       font-size=14
              sets the size of the terminal font (unsigned integer).

       term=xterm-256color
              The terminal shell (string). Sets the $TERM variable.

XWAYLAND SECTION
       path=/usr/bin/Xwayland
              sets the path to the xserver to run (string).

SCREEN-SHARE SECTION
       command=/usr/bin/weston --backend=rdp-backend.so --shell=fullscreen-shell.so --no-clients-resize
              sets the command to start a fullscreen-shell server for screen sharing (string).

SEE ALSO
       weston(1), weston-bindings(7), weston-drm(7), xkeyboard-config(7)

Weston 8.0.0                                                                               2019-03-26                                                                             weston.ini(5)
```
