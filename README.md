# A guide to the GFX HAT


This reference is mostly for reminding me of what I've learnt about the GFX HAT when I inevitably come back to it and have no idea how I got it to work.

# Introduction

The GFX HAT is Pimoroni's updated version of the venerable Display-O-Trom 3000, and brings a 128x64 graphic LCD, 6-section RGB backlight and 6 capacitive touch buttons with their own individual white LEDs.  It's more complex to use than the DoT3K HAT, but is also significantly more flexible.  

Controlling the GFX HAT is split into three different sections:  one module for the LCD, one for the RGB backlight and one for the capacitive touch buttons and their LEDs.  The Pimoroni function reference can be found [here](http://docs.pimoroni.com/gfxhat/).

# RGB Backlight

This is the most simple aspect of the GFX HAT to control.  The LCD has six backlight areas moving from left to right across the screen, designated 0 - 5.  First of all, begin by importing this part of the module:

* `from gfxhat import backlight`

Before you can show anything on the LEDs you need to tell them what they're showing, and there are two ways to do that.

* `backlight.set_pixel(pixel, red, green, blue)`

This simple command sets a single pixel identified using its designation number to the values _red_, _green_ and _blue_.  Like a Neopixel valid values are 0-255 levels of red, green or blue.

* `backlight.set_all(red, green, blue)`

This equally simple command sets all of the backlight zones to the same _red_, _green_ and _blue_ values in a single command.  Again, valid values are 0-255 with larger numbers being brighter.

* `backlight.show()`

Once you've done one of these you must tell the LEDs to actually activate using the `backlight.show()` command.  And that's all there is to the LED backlight!

# Capacitive touch

The captouch buttons are numbered from 0-5, starting with the "up" button on the left of the HAT, moving down to the "back" button at the bottom left and then running left to right along the bottom.  The buttons are usually referred to by their number in code, so it's worth remembering which way the numbers are assigned.

To use the captouch buttons, begin by importing the module:

* `from gfxhat import touch`

Before you can use the captouch buttons you have to tell them what to do when a button is pressed.  You do this by assigning a function to a button number:

* `touch.on(button_number, function)`

This tells the script that when button _button_number_ is pressed it should run _function_. _function_ must take two arguments, _channel_ and _event_.  _channel_ tells the function which button was pressed, and _event_ tells the function whether the button was pressed (by passing the string "press") or released (with the string "release").  This might sound a bit complex, but it's easier to grasp with an example:

```
def example_function(channel, event):
    if channel == 0:
        if event == "press":
            print("Button UP was PRESSED")
        elif event == "release":
            print("Button UP was RELEASED")
    
touch.on(0, example_function)
```

The function _example_function_ first checks which button (or channel) was pressed.  If this is `0`, corresponding to the "up" button, it then checks what the event is.  If the event is `press` it prints that the button was pressed, and if the event is `release` it prints that the button was released.  Finally, the line at the end tells the scripts that touching channel `0`, which is the "up" button, should trigger _example_function_.

One of the first things that _example_function_ does is check which channel/button was pressed, and so you can actually assign all button presses to the same function if there is an `if`/`else if` statement checking which button was pressed:

```
for x in range (6):
    touch.on(x, example_function)
```
