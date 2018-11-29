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
