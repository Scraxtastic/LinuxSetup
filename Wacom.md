# Wacom
How to install tablet drivers for wacom


Basic Options: https://linuxwacom.github.io/


# Xorg
Install xf86-input-wacom as mentioned [here](https://github.com/linuxwacom/xf86-input-wacom/wiki/Building-The-Driver)




# Setup Wacom

## Bind SCreen
use `xsetwacom list devices` to list your devices.

You should see a `Pen stylus`. I'll mention the output id of it as <id>.

`xsetwacom set <id> MapToOutput <ScreenX>x<ScreenY>+<OffsetX>+<OffsetY>`


I have 2x wqhd screens (2560x1440) and want to bind the tablet to the right one. The `id` of my stylus pen is `10`.

So I use `xsetwacom set 10 MapToOutput 2560x1440+2560+0`.


## Set Tablet Area
Get your current (default) Area with`xsetwacom get <id> Area`.

For my tablet it is `0 0 21600 13500` (Wacom intuos M).


## Set the ratio (full size)
Calculation: <tabletWidth> * (<screenHeight>/<screenWidth>)

`xsetwacom set <id> Area 0 0 21600 <calculated>`


## Osu config

Ye, I used to play osu and want to play a bit on Linux as well. Therefore I add my configs here :P

I set my settings in the wacom UI in windows, so i got the following values: 
- top: 0
- bottom: 6088
- left: 0
- right: 8126

since top and left are 0, i can simple subtract the bottom and right value from the height and width:

width: 21600-8126 = 13474
height: 13500-6088 = 7412

so i call

`xsetwacom set 10 Area 0 0 13474 7412`

after several tries the following fit better for me
`xsetwacom set 10 Area 0 0 10000 6000`


# More info
I actually used that video to get an easy insight into the things
https://www.youtube.com/watch?v=dzplf-0RJDE