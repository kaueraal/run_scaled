Xpra directly includes an equivalent tool since [version 4.1](https://github.com/Xpra-org/xpra/blob/4d73c3644d6692bd51376296ed913f18a809f1a9/docs/CHANGELOG.md#41-2021-02-26). It is also called `run_scaled` and is part of [the default Xpra installation](https://github.com/Xpra-org/xpra/blob/master/fs/bin/run_scaled).

Hence, this script is deprecated and no longer maintained.

# run_scaled

Run an X application scaled via xpra.

```$ ./run_scaled --help
Usage: run_scaled [--scale=scaling_factor] [--opengl=auto|yes|no] [--sleep=sleeptime] application

--scale  Sets the factor the application is scaled by. Fractional scales are
         supported. It is set to 2 by default.
--opengl Sets whether xpra should use opengl for rendering. If you get rendering
         errors, especially when the window is resized, try setting it to no. It
         is set to auto by default.
--sleep  Sets how many seconds to wait after starting xpra before attaching to
         the xpra session. It is set to 1 by default.
         You might need to increase the value if your machine is particularly
         slow and attaching fails with a message like

             InitException: cannot find any live servers to connect to
             xpra initialization error:
              cannot find any live servers to connect to

```

Dependencies:
* xvfb
* xpra >= 2 https://xpra.org/
* xrandr