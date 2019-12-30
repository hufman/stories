---
layout: page
title: Chevy Spark EV Theme
---

The Chevy Spark EV is Chevrolet's first fully battery-electric vehicle, and it's an absolute hoot! 400ftlbs of torque can reliably skip the tires off the pavement, the accelerator pedal is instantly responsive, and it feels hilariously lightweight and fun to throw around.

I found [some ](https://sourceforge.net/p/mylink-unlock-project/wiki/Home/)[instructions](https://lu7adw.blogspot.com/2016/12/chevrilet-mylink-development-mode.html) for getting into the car's Development Mode, and from there you can get right into "Explorer Mode", which is just the Windows CE Desktop.

Startup Video
-------------

Poking around, I found the startup video easily, so I set out to see if I could replace it! The standard interface for a futuristic touch screen is, of course, LCARS, so I snipped out a bit of the __Star Trek: The Next Generation__ opening and slipped it into place. Then I discovered that the car skips out of the video after about 5 seconds to show the main menu, so I sped up the video a lot, and it looks great.

Theme Files Discovery
---------------------

While digging around in a backup of the car's files, I discovered thousands of BMP files. I didn't notice them in the car itself since Windows didn't set the normal icon for them, because there's no application installed in the car to view BMPs. Funnily enough, there is an AMD utility installed to play videos.

These BMP (with a few PNGs scattered around) turned out to be all the menu graphics for the car. Of course, that means I have to replace them with LCARS graphics!!

Theme Approach
--------------

My approach was pretty simple: I'd generate SVG files that embed a neighboring BMP file as a layer, for easy tracing and positioning, and then I'd write a script to render out any edited SVG files to new BMPs to transfer to the car. Nice and simple!

So then I make a test file and render it out, and the car ignores it. I look a little more closely at the original files, and they are all 256-color BMPs, so now I have to do some fancy work in the render script to generate palettes and render any transparent colors with cyan, and that was fun.

The car has some graphics that are correlated, and have the same filename except for a short suffix. For example, a button's normal graphic might be `btn_n.bmp`, the pressed state is `btn_p.bmp`, and the disabled state is `btn_d.bmp`. My script groups those all as layers within the same SVG file.

This logically extends to animation frames, such as `ani_right_01.bmp`, `ani_right_02.bmp`, and so on. This means that any internal animations, except for the opening video, would be edited as layers of a single SVG file.

Because it's all SVG and XML, I wrote [some](https://github.com/hufman/chevy_mylink_theming/blob/master/lcars/Bev/Battery%20Information/ani_left_particles.py) Python [scripts](https://github.com/hufman/chevy_mylink_theming/blob/master/lcars/Bev/Battery%20Information/ani_right_particles.py) to generate particle affects for the Battery Info screen, by moving some SVG `<use>` elements around the viewport for each frame.

Creativity
----------

It used up all of my graphical creativity, I have no more left :) The Battery Information entry video was the hardest, having to come up with ideas for all the graphical elements.

I found an example wireframe of __The Original Series__ USS *Constitution* and traced that, and then realized that LCARS demands that I use the USS *Enterprise*, so I had to redraw it all.

I knew I wanted to zoom in on a wireframe of the nacelle to show the battery state. However, I didn't know what I wanted to put along the sides of the screen: Just having a battery gauge in the middle of the screen is not very interesting. I couldn't make a moving starfield in the background, because the only animated parts of the screen are on the left and right, where the wheels normally spin in the original theme.

The idea for depicting the Bussard ramjet scooping up hydrogen, and some mystical propulsion particles out the back, was exciting but daunting, because I had been placing everything by hand. Then I came up with the idea for programmatically placing the particles, and that made it fun!

Then I had the idea for the LCARS interface decorating the side, which was perfect. I even did a neat flicker effect during the entry video, which adds some extra interesting visuals.

Results
-------

[Repo](https://github.com/hufman/chevy_mylink_theming)

Themed Screens:

{% include image.html src="images/sparktheme/consumption.jpg" title="Consumption" %}
{% include image.html src="images/sparktheme/chargemode.jpg" title="Charge Mode" %}
{% include image.html src="images/sparktheme/menu1.jpg" title="Menu 1" %}
{% include image.html src="images/sparktheme/menu2.jpg" title="Menu Done" %}
{% include image.html src="images/sparktheme/charge.gif" title="Battery Info" %}

Wireframes:

{% include image.html src="images/sparktheme/constitution.png" title="USS Constitution" %}
{% include image.html src="images/sparktheme/enterprise.png" title="USS Enterprise" %}
