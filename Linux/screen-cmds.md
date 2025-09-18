# Screen commands

A terminal multiplexer: allows you to manage multiple terminal sessions within a single terminal window.

## Installing Screen

`sudo apt install screen` -> for ubuntu.

## Starting Screen

`screen` or `screen -S <any name to screen>`

## Detach & Reattach Sessions

+ `Ctrl + A then D` -> Detach (keep running in background).
+ `screen -ls` -> List running sessions.
+ `screen -r <session_id or name>` -> Reattach session.
+ `screen -D -r <session_id or name>` -> Force reattach.

## Killing/Exiting Sessions

+ `exit` or `Ctrl + D` -> used Inside screen
+ `Ctrl + A + \ # confirm with y` -> Kill entire screen session (used Inside screen)
+ `screen -X -S <session_id> quit` -> From outside
+ `screen -ls | awk '/[0-9]+\./ {print $1}' | xargs -r -n1 screen -X -S quit` or `killall screen` -> killall screens (from outside)