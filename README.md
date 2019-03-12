# Documentation

This is a file of some self-made documentation.  Pimoroni don't seem to have documented this board at all (!), and I'm considering adding a few bits to the library, so I figured that documenting the features which are already there is the smart place to start.

##### This first section will focus on functions which the user is likely to use, and ignore "backend" functions which a user shouldn't need to interact with.  Some additional stuff for my own information will follow later.

Using the library is quite straightforward.  First, you import the library and create an instance of the device:

```
from rgbmatrix5x5 import RGBMatrix5x5
rgb5x5 = RGBMatrix5x5()
```

The instance contains a buffer to hold values for the LEDs and also a few functions which control the breakout.  You then set the red, green and blue values of the LEDs in the buffer and finally transmit those values to the board using the `show()` function.

# Functions and attributes

### rgb5x5.set_clear_on_exit(value=True):

This controls whether or not the LEDs are switched off after the script is complete.  By default this is set to `True` so if you want the LEDs to automatically turn off you don't have to do anything.  If you want the LEDs to maintain their state after the script ends you need to use this line and pass `False` as the argument (i.e., `rgb5x5.set_clear_on_exit(value=False)`).

### rgb5x5.clear()

The 5x5 breakout keeps a buffer of values which the LEDs are currently set to.  If you want to set them all to zero you can avoid doing this for each LED individually by using the `clear()` function to zero them all in one line.  **Note that the display won't change until you use the `show()` function**, the `clear()` function simply zeros the buffer.

### rgb5x5.set_brightness(brightness)

This sets a global brightness across the breakout.  It requires a value from 0.0 to 1.0.  It seems to be happy to accept floats with a number of decimal places, but this looks like it gets scaled effectively from 0-255.

### rgb5x5.set_all(r, g, b, brightness=1.0):

This function fills the whole matrix with a single colour.  It should be passed separate values for red, green and blue between 0 and 255, and also takes an optional brightness value between 0.0 and 1.0.  Like most functions in the library, the changes won't show on the breakout until `show()` is called.

### rgb5x5.set_pixel(x, y, r, g, b, brightness=1.0)

Sets a pixel with a specified `x` and `y` coordinate to red, green and blue values of `r`, `g` and `b` respectively, with the option to also set the brightness of that individual pixel (defaults to a brightness of 1.0 of 1.0).  The `x` and `y` coordinates should be from 0-4, and start from the top left of the board.  Confusingly, the `x` coordinate specifies how far down the column the LED is, and the `y` coordinate specifies how far along the row the LED is.

### set_multiple_pixels(self, indexes, from_colour, to_colour=None)

This function fails because of a bug in the library.  I've requested a fix be added to it, so let's see how busy Phil from Pimoroni is.

When it gets fixed, this will be used to set a group of user-selected pixels to either a single colour (similar to `set_all()` but with a user-selectable group of pixels rather than all) or to a spectrum of colours between user-defined starting and ending colours.  `indexes` should be a list of pixels from 0-24, where 0 is the pixel in the top left of the board, 24 is the pixel in the bottom right, and the numbering scheme moves down columns, not across rows.  `from_colour` should be a tuple of three values from 0-255 (e.g. `(0,128,255)`) which represent the red, green and blue values of the LED respectively.  If no `to_colour` is provided then all LEDs will be set to this colour.  If a `to_colour` is provided (same format as `from_colour`) then the LEDs will have a gradient of colour, starting at the `from_colour`, ending at the `to_colour`, and blending in a linear way from one to the other.

### show()

When you use `set_pixel()` to define the colour of a pixel, those values are simply stored in a buffer.  It isn't until you call `show()` that those values are actually pushed to the breakout board and are then displayed.  

# Backend stuff

This is documentation which digs a little deeper into how the breakout is controlled.  If all you want to do is show LEDs on the board then you don't need to read this, I'm mainly putting it here to keep a note of things I've found whilst figuring out what else I can do to the library.

### \_bank(bank=None)

This points the Command Register to a particular Response/Frame register.  By default it triggers a read of the Command Register (0xFD) which returns the last Response/Page register accessed.  If a value of `bank` is provided it then points the Command Register to that Response/Page register.

By default when `show()` is called it sets this to `0` (there's an odd check to see if `_current_frame` is `1` before it sets `next_frame` to `0` anyway, and then sets the current Frame/Page register to `0`.  Is this a hangover from other code?).  Every time you clear the buffer `_current_frame` is set to `0`.  Alternatively, you appear to be able to use `_frame` to set the frame.  By default this should set the board to display the values saved for that frame register.
