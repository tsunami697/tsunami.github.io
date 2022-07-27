```
WESTON-DRM(7)                                                                   Miscellaneous Information Manual                                                                  WESTON-DRM(7)

NAME
       weston-drm - the DRM backend for Weston

SYNOPSIS
       weston-launch

       weston --backend=drm-backend.so

DESCRIPTION
       The DRM backend is the native Weston backend for systems that support the Linux kernel DRM, kernel mode setting (KMS), and evdev input devices.  It is the recommended backend for desk‐
       top PCs, and aims to provide the full Wayland experience with the "every frame is perfect" concept.  It also relies on the Mesa GBM interface.
	   [DRM 依赖Mesa GBM 接口][evdev]

       With the DRM backend, weston runs without any underlying windowing system. The backend uses the Linux KMS API to detect connected monitors. Monitor hot-plugging is supported. Input de‐
       vices  are found automatically by udev(7).  Compositing happens mainly in GL ES 2, initialized through EGL. It is also possible to take advantage of hardware cursors and overlays, when
       they exist and are functional. Full-screen surfaces will be scanned out directly without compositing, when possible.  Hardware accelerated clients are supported via EGL.
	   [借助DRM后端，weston在没有任何基础窗户系统的情况下运行]
	   [后端使用Linux KMS API检测连接的监视器, 支持监视热插拔]
	   [输入设备由udev自动发现]
	   [合成主要发生在GL ES 2中，通过EGL初始化]

       The backend chooses the DRM graphics device first based on seat id.  If seat identifiers are not set, it looks for the graphics device that was used in boot. If that is not  found,  it
       finally chooses the first DRM device returned by udev(7).  Combining multiple graphics devices are not supported yet.

       The  DRM  backend  relies on weston-launch for managing input device access and DRM master status, so that weston can be run without root privileges. On switching away from the virtual
       terminal (VT) hosting Weston, all input devices are closed and the DRM master capability is dropped, so that other servers, including Xorg(1), can run on other VTs. On  switching  back
       to Weston's VT, input devices and DRM master are re-acquired through the parent process weston-launch.
	   [DRM后端依靠Weston-Launch来管理输入设备访问和DRM主状态，因此可以在没有root特权的情况下运行weston]

       The  DRM backend also supports virtual outputs that are transmitted over an RTP session as a series of JPEG images (RTP payload type 26) to a remote client. Virtual outputs are config‐
       ured in the remote-output section of weston.ini.

CONFIGURATION
       The DRM backend uses the following entries from weston.ini.

   Section output
       name=connector
              The KMS connector name identifying the output, for instance LVDS1.

       mode=mode
              Specify the video mode for the output. The argument mode can be one of the words off to turn the output off, preferred to use the monitor's preferred video mode, or  current  to
              use the current video mode and avoid a mode switch.  It can also be a resolution as:

       mode=widthxheight

       mode=widthxheight@refresh_rate
              Specify a mode with a given refresh-rate measured in Hz.

       mode=widthxheight@refresh_rate ratio
              Here  ratio is Picture Aspect-Ratio which can have values as 4:3, 16:9, 64:27, and 256:135. This resolution-format helps to select a CEA mode, if such a video mode is present in
              the mode-list of the output.

              CEA defines the timing of a video mode, which is considered as a standard for HDMI spcification and compliance testing. It defines each and every parameter of a video mode, like
              hactive,  vactive, vfront, vback etc., including aspect-ratio information. For CEA modes, the drm layer, stores this aspect-ratio information in user-mode (drmModeModeInfo) flag
              bits 19-22. For the non-CEA modes a value of 0 is stored in the aspect-ratio flag bits.

              Each CEA-mode is identified by a unique, Video Identification Code (VIC).  For example, VIC=4 is 1280x720@60 aspect-ratio 16:9. This mode will be different than a  non-CEA  mode
              1280x720@60  0:0.  When  the  video  mode 1280x720@60 0:0 is applied, since its timing doesn't exactly match with the CEA information for VIC=4, it would be treated as a non-CEA
              mode. Also, while setting the HDMI-AVI-Inforframe, VIC parameter will be given as '0'. If video mode 1280x720@60 16:9 is applied, its CEA timimgs matches with that of video mode
              with VIC=4, so the VIC parameter in HDMI-AVI-Infoframe will be set to 4.

              Many a times, an output may have both CEA and non-CEA modes, which are similar in all resepct, differing only in the aspect-ratio. A user can select a CEA mode by giving the as‐
              pect-ratio, along with the other arguments for the mode.  By omitting the aspect-ratio, user can specify the non-CEA modes.  This helps when certification testing  is  done,  in
              tests like 7-27, the HDMI-analyzer applies a particular CEA mode, and expects the applied mode to be with exactly same timings, including the aspect-ratio and VIC field.

              The resolution can also be a detailed mode line as below.

       mode=dotclock hdisp hsyncstart hsyncend htotal vdisp vsyncstart vsyncend vtotal hflag vflag
              Use the given detailed mode line as the video mode for this output.  The definition is the same as in xorg.conf(5), and cvt(1) can generate detailed mode lines.

       transform=transform
              Transform  for  the  output,  which  can  be  rotated  in  90-degree  steps and possibly flipped. Possible values are normal, 90, 180, 270, flipped, flipped-90, flipped-180, and
              flipped-270.

       pixman-shadow=boolean
              If using the Pixman-renderer, use shadow framebuffers. Defaults to true.

       same-as=name
              Make this output (connector) a clone of another. The argument name is the name value of another output section. The referred to output section  must  exist.  When  this  key  is
              present in an output section, all other keys have no effect on the configuration.

              NOTE:  cms-colord  plugin  does not work correctly with this option. The plugin chooses an arbitrary monitor to load the color profile for, but the profile is applied equally to
              all cloned monitors regardless of their properties.

       force-on=true
              Force the output to be enabled even if the connector is disconnected.  Defaults to false. Note that mode=off will override force-on=true.   When  a  connector  is  disconnected,
              there is no EDID information to provide a list of video modes. Therefore a forced output should also have a detailed mode line specified.

   Section remote-output
       name=name
              Specify unique name for the output.

       mode=mode
              Specify the video mode for the output. The argument mode is a resolution setting, such as:

       mode=widthxheight

       mode=widthxheight@refresh_rate
              If refresh_rate is not specified it will default to a 60Hz.

       host=host
              Specify the host name or IP Address that the remote output will be transmitted to.

       port=port
              Specify the port number to transmit the remote output to. Usable port range is 1-65533.

       gst-pipeline=pipeline
              Specify  the  gstreamer  pipeline. It is necessary that source is appsrc, its name is "src", and sink name is "sink" in pipeline.  Ignore port and host configuration if the gst-
              pipeline is specified.

OPTIONS
       When the DRM backend is loaded, weston will understand the following additional command line options.

       --current-mode
              By default, use the current video mode of all outputs, instead of switching to the monitor preferred mode.

       --drm-device=cardN
              Use the DRM device cardN instead of the default heuristics based on seat assignments and boot VGA status. For example, use card0.

       --seat=seatid
              Use graphics and input devices designated for seat seatid instead of the seat defined in the environment variable XDG_SEAT. If neither is specified, seat0 will be assumed.

       --tty=x
              Launch Weston on tty x instead of using the current tty.

ENVIRONMENT
       WESTON_LIBINPUT_LOG_PRIORITY
              The minimum libinput verbosity level to be printed to Weston's log.  Valid values are debug, info, and error. Default is info.

       WESTON_TTY_FD
              The file descriptor (integer) of the opened tty where weston will run. Set by weston-launch.

       WESTON_LAUNCHER_SOCK
              The file descriptor (integer) where weston-launch is listening. Automatically set by weston-launch.

       XDG_SEAT
              The seat Weston will start on, unless overridden on the command line.

SEE ALSO
       weston(1)

Weston 8.0.0                                                                               2012-11-27                                                                             WESTON-DRM(7)
```
