# Documentation

This is a file of some self-made documentation.  Pimoroni don't seem to have documented this board at all (!), and I'm considering adding a few bits to the library, so I figured that documenting the features which are already there is the smart place to start.

##### This will focus on functions which the user is likely to use, and ignore "backend" functions which a user shouldn't need to interact with.

The Pimoroni library is designed in such a way that you create an object for the breakout, and then either call the object's functions or modify it's attributes.  Normally you set values for individual LEDs, but the LEDs will not actually change until you use the `show()` function to actually push those values to the breakout.

# Functions and attributes

### set_clear_on_exit(value=True):

This attribute controls whether or not the LEDs are switched off after the script is complete.  By default this is set to `True` so if you want this functionality you don't have to do anything.  If you want the LEDs to maintain their state after the script ends you need to use this line and pass `False` as the argument (i.e., `set_clear_on_exit(value=False)`).

### clear()

The 5x5 breakout keeps a buffer of values which the LEDs are currently set to.  If you want to set them all to zero you can avoid setting the values one-by-one and use the `clear()` function to zero them all in one line.  **Note that the display won't change until you use the `show()` function**, the `clear()` function simply empties the buffer.

### set_brightness(brightness)

This sets a global brightness across the breakout.  It requires a value from 0.0 to 1.0.  It seems to be happy to accept floats with a number of decimal places, but this looks like it gets scaled effectively from 0-255.

### set_all(r, g, b, brightness=1.0):

This appears to be broken.  Presumably it is meant to fill the entire LED matrix with a single colour and set the brightness value (default 1.0 of 1.0), but it fails and complains about missing X and Y values.

### set_pixel(x, y, r, g, b, brightness=1.0)

Sets a pixel with a specified `x` and `y` coordinate to red, green and blue values of `r`, `g` and `b` respectively, with the option to also set the brightness of that individual pixel (defaults to a brightness of 1.0 of 1.0).  The `x` and `y` coordinates should be from 0-4, and start from the top left of the board.  Confusingly, the `x` coordinate specifies how far down the column the LED is, and the `y` coordinate specifies how far along the row the LED is.

### set_multiple_pixels(self, indexes, from_colour, to_colour=None)

I can't ge tthis one to work either.  I can't tell if it's because it's broken or because I'm an idiot.  To be reviewed in future.

### show()

When you use `set_pixel()` to define the colour of a pixel, those values are simply stored in a buffer.  It isn't until you call `show()` that those values are actually pushed to the breakout board and are then displayed.  
