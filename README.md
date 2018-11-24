# A guide to the GFX HAT


This reference is mostly for reminding me of what I've learnt about the GFX HAT when I inevitably come back to it and have no idea how I got it to work.

# Introduction

The GFX HAT is Pimoroni's updated version of the venerable Display-O-Tron 3000, and brings a 128x64 graphic LCD, 6-section RGB backlight and 6 capacitive touch buttons with their own individual white LEDs.  It's more complex to use than the DoT3K HAT, but is also significantly more flexible.  

Controlling the GFX HAT is split into three different sections:  one module for the LCD, one for the RGB backlight and one for the capacitive touch buttons and their LEDs.  The Pimoroni function reference can be found [here](http://docs.pimoroni.com/gfxhat/), and this is mostly an explaination/worked example of this, plus some information on how to design images to show on the matrix.

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

### Getting the buttons to work

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

The function _example_function_ first checks which button (or channel) was pressed.  If this is `0`, corresponding to the "up" button, it then checks what the event is.  If the event is `press` it prints that the button was pressed, and if the event is `release` it prints that the button was released.  Finally, the line at the end tells the script that touching channel `0`, which is the "up" button, should trigger _example_function_.

One of the first things that _example_function_ does is check which channel/button was pressed, and so you can actually assign all button presses to the same function if there is an `if`/`else if` statement checking which button was pressed:

```
for x in range (6):
    touch.on(x, example_function)
```

To use this you'll need _example_function_ to handle presses from all of the buttons:

```
def touched(channel, event):
        if channel == 0:
                if event == "press":
                        print("UP Touched")
                elif event == "release":
                        print("UP Released")
        if channel == 1:
                if event == "press":
                        print("DOWN Touched")
                elif event == "release":
                        print("DOWN Released")
        if channel == 2:
                if event == "press":
                        print("BACK Touched")
                elif event == "release":
                        print("BACK Released")
        if channel == 3:
                if event == "press":
                        print("MINUS Touched")
                elif event == "release":
                        print("MINUS Released")
        if channel == 4:
                if event == "press":
                        print("ENTER Touched")
                elif event == "release":
                        print("ENTER Released")
        if channel == 5:
                if event == "press":
                        print("PLUS Touched")
                elif event == "release":
                        print("PLUS Released")


for x in range (6):
        touch.on(x, touched)
```
This script defines a function called _touched_, which first checks the _channel_ to see which button has triggered an alert, then checks the _event_ to see whether the button was pressed or released, and finally prints this information.  At the very bottom, all six buttons are assigned to trigger the _touched_ function when they trigger an alert.

### Using the button LEDs

The LED backlight isn't the only source of shiny on the GFX HAT, each touch-button also has its own white LED.

* `touch.set_led(led, state)`

This line tells one of the LEDs to adopt _state_, where state is either 1 (on) or 0 (off).  The LEDs are identified in the same way as the touch buttons, with a number from 0-5, where 0 is the "up" button and 5 is the "plus" button.  It's really simple to use, just pick an LED and turn it on or off with `1` or `0`!

### Additional touch functions

The `touch` module comes with a few extra functions which tweak _how_ the buttons respond to being pressed.  

* `touch.enable_repeat(enable)`

Whenever you touch a button it triggers the attached event just once.  It then doesn't do anything until your finger is lifted off the button.  That isn't what peple always want, and so there's a way to tell the HAT to continuously trigger an event while your finger is on the button.  Unfortunately you can't apply this to only some of the buttons, so either all of the buttons trigger continuously or none of them do.  This function supposedly allows you to cause the function to repeat at set intervals for as long as your finger is on the touchpad, but for the life of me I can't get it to work.

* `touch.set_repeat_rate(interval)`

This function sets how often a trigger signal is sent while your finger is on the touchpad.  It takes values from 35 to 560 millisections, which are rounded to the nearest 35 milliseconds.  Unfortunately not useful if you can't get the `touch.enable_repeat()` function to work.

* `touch.high_sensitivity()`

This very simple line increases the sensitivity of the touch buttons.  Without anything on top of them they will now trigger from 1-1.5 cm away (so you don't need to physically touch the button to activate it).  Pimoroni's notes state that this allows the touch buttons to function through 3mm Perspex plastic or similar materials which you might cover the PCB with in a case.

* `touch.get_name(button)`

This final function returns the built-in name of a button when given the ID number of a button (again, they're numbered from 0 at the upper left to  5 at the lower right).  If you ever forget the names you can use this to remind yourself:

```
for x in range (6):
    print("Channel " + str(x) = " is called " + str(touch.get_name(x)))
```

This function will fetch the names of all of the buttons and print them on the terminal as a handy reference of their official names.  This might be useful for getting the buttons to work (see above).        
