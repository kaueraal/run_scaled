#!/usr/bin/env bash

# Default settings
SCALING_FACTOR=2
USE_OPENGL="auto"
SLEEPTIME=1
PERFORMANCE_OPTIONS="--encoding=rgb --mmap=yes --compress=0"

print_help() {
    echo "Usage: run_scaled [--scale=scaling_factor] [--opengl=auto|yes|no] [--sleep=sleeptime] application"
    echo ""
    echo "--scale  Sets the factor the application is scaled by. Fractional scales are"
    echo "         supported. It is set to 2 by default."
    echo "--opengl Sets whether xpra should use opengl for rendering. If you get rendering"
    echo "         errors, especially when the window is resized, try setting it to no. It"
    echo "         is set to auto by default."
    echo "--sleep  Sets how many seconds to wait after starting xpra before attaching to"
    echo "         the xpra session. It is set to 1 by default."
    echo "         You might need to increase the value if your machine is particularly"
    echo "         slow and attaching fails with a message like"
    echo ""
    echo "             InitException: cannot find any live servers to connect to"
    echo "             xpra initialization error:"
    echo "              cannot find any live servers to connect to"
    echo ""
}

if [ $# = 0 ]
    then print_help
        exit 1
fi

while true
    do if [ $# -gt 0 ]
        then case $1 in
            --scale=*)
                SCALING_FACTOR="${1#*=}"
                shift
            ;;
            --opengl=*)
                USE_OPENGL="${1#*=}"
                shift
            ;;
            --sleep=*)
                SLEEPTIME="${1#*=}"
                shift
            ;;
            --)
                shift
                break
            ;;
            --*)
                print_help
                exit 1
            ;;
            *)
                break
            ;;
        esac
    else
        echo "No application given!"
        exit 2
    fi
done

declare -i UNSCALED_RESOLUTION_X
declare -i UNSCALED_RESOLUTION_Y
UNSCALED_RESOLUTION_X=`xrandr | sed -n -e 's/Screen 0:.*current \([0-9]\+\) x \([0-9]\+\).*/\1/p'`
UNSCALED_RESOLUTION_Y=`xrandr | sed -n -e 's/Screen 0:.*current \([0-9]\+\) x \([0-9]\+\).*/\2/p'`

UNSCALED_RESOLUTION="$( bc <<<"$UNSCALED_RESOLUTION_X / $SCALING_FACTOR" )x$( bc <<<"$UNSCALED_RESOLUTION_Y / $SCALING_FACTOR" )"

DISPLAYNUM=:`shuf -i 10000-99999999 -n 1`
ESCAPED_PARAMS=`printf '%q ' "$@"`
xpra start "$DISPLAYNUM" --xvfb="Xvfb +extension Composite -screen 0 ${UNSCALED_RESOLUTION}x24+32 -nolisten tcp -noreset  -auth \$XAUTHORITY" --env=GDK_SCALE=1 --env=GDK_DPI_SCALE=1 --start-child="$ESCAPED_PARAMS" --exit-with-children
sleep "$SLEEPTIME"
xpra attach "$DISPLAYNUM" "--desktop-scaling=$SCALING_FACTOR" "--opengl=$USE_OPENGL" $PERFORMANCE_OPTIONS || xpra stop "$DISPLAYNUM"
