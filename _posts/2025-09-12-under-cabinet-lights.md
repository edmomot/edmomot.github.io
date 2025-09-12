During our major kitchen remodel, my wife and I wanted to add bright under cabinet lights. We paid for a majority of the remodel, but there were some parts I wanted to do myself, like these lights, flooring, and floating shelves. The install took some research and planning, but we are happy with the results. This post describes the process and technical install details.

![Under cabinet lights from below](images/under-cabinet-lights/12-20250212-200636-23.jpg)

# Priorities

1. Dimmable
1. Control dimming and on/off with wall switch. I want easy access while in the kitchen without pulling the phone out.
1. Control dimming and on/off with home assistant. I want to turn the lights down from the living room when playing a movie.
1. Bright. I want light that's useful for working on the counter, not just an accent.
1. Even, soft light, minimal "dots", color matched.

# Dimmable LED configuration options

## RGB LEDs

DIAGRAM HERE


A/C power -> control (wifi module here) -> DC power / individually addressable DC

Pros
* RGB control

Cons
* Dimming must be done through an app
* A simple wall switch doesn't work well. When switched off, the lights can't be turned on via app. Need a smarter switch that works with the LED controller.

## Constant color LEDs

DIAGRAM HERE


A/C power -> smart dimmer switch (wifi module here) -> dimmable power supply -> DC power

Pros:
* modular components
* brigher light

Cons:
* No RGB control, single color

## My decision

Constant color LEDs. I care more about the wall switch control than RGB LEDs. The custom colors are probably fun to play with for the first week, then get left alone. The modularity is a plus. If anything fails, I can swap that part independently, and I'm not tied to a company that could be out of business.

# Running wires

Before the drywall was patched and cabinets were installed, I needed to run wires to every cabinet from one central location. To do this, I used 16 gauge landscape wire. This was a cheaper option than other wires I found, and the thick sheath is a positive in this application. Copper is copper.

[Menards 16 Gauge Landscape Wire](https://www.menards.com/main/electrical/electrical-wire-cable/outdoor-electrical-cable/southwire-reg-black-low-voltage-landscape-lighting-cable/55213143/p-1479453570318-c-1525874617504.htm)

Some drywall was open for other electrical, but I had to cut a few holes to get everywhere.

Getting from the wall into the ceiling was a mystery to me at the start, but notching out the drywall worked in this case since this is low voltage. I had to do this twice. Two wires from the power supply, with one of them coming down at another notch, and another through an already open hole where the pantry was removed. Two of these thick sheath wires were pushing the capacity of the notch, but it worked.

DIAGRAM HERE

Luckily I already had flat can lights installed, so I popped those out and used the holes to help pull the wire through. Also working in my favor were the open joists that saved a lot of hassle running perpendicular.

At this point, some [glow in the dare fish tape](https://www.homedepot.com/p/Milwaukee-20-ft-Glow-Fish-Tape-48-22-4182/327567743) would have helped a ton, but I didn't have the tool for this project. In hindsight, the fish tape would have made this process smoother.

I measured and marked the exit holes carefully to make sure they aligned with the narrow recess at the bottom of the cabinets.

# Drywall and cabinets

I didn't do this part, but I did have to work around schedules, so I was up late pulling wire through before the drywall was patched the next day.

For the large open hole where the pantry was removed, I marked the wire exit locations, taped the wire into a loop and marked it, and talked to the drywall crew to make sure the wires popped out in the right place.

Cabinets were drilled on the bottom, wires passed through, then mounted.

# Power supply install

Having learned from prior attempts at powering LEDs with underpowered power supplies, I went ahead and bought a proper power supply from the start.

[Super Bright LEDs 24V 150W Magnitude Lighting power supply](https://www.superbrightleds.com/magnitude-dimmable-led-power-supply-60-200w-24v-dc+wattconsume-150~Watts+powercordyn-Without~Power~Cord)

This was by far the most expensive part of the project at ~$130

I used a terminal block with connecting strips to make the wiring neater from the power supply to the individual cabinets.

[Amazon Terminal Block](https://www.amazon.com/dp/B08ZXH6768)

PICTURE HERE

# LED strip purchase

I bought some 24V LED strips from Aliexpress. Full description from my purchase:

High CRI 95 RGB+CCT 24V RGBW RGBWW White Warm White LED Strips

Similar items from the same store
* [https://www.aliexpress.us/item/3256807401475378.html](https://www.aliexpress.us/item/3256807401475378.html)
* [https://www.aliexpress.us/item/2255800326752382.html](https://www.aliexpress.us/item/2255800326752382.html)

I mostly used Aliexpress because it's hard to find LED strips at a reasonable price without a cheap built in controller.

# LED strip install

Aluminum channels solve a few problems
* Adhesion. Sticking LED strips directly to wood does not last very long.
* Clean mounting. Looks nicer than exposed strips. Hides wires.
* Softer light. The plastic cover diffuses the points of light slightly.

I went with the hunhun channels here. They had *almost* enough hardware for the entire install. If I didn't use the channels to hide wires all the way to the back of cabinets, the included harware would have been plenty. When I ran out, I cut some of the extension connectors with mixed success.

[Amazon hunhun LED Aluminum Channels](https://www.amazon.com/dp/B071FRFQVZ)

I used a miter saw to cut the channels and plastic covers to length. I needed some notches on the sides for the pieces I used to hide the wires. I also used the miter saw for the aluminum channels. The plastic covers had a tendency to explode, so I used a dremel there.

To connect the strips to the power source, I used a mix of soldering and heat shrink crimp connectors. Soldering is needed directly to the strips, but the crimp connectors are easier to connect two wires.

I measured and cut the LED strips to fit in the channel, soldered some wire tails on, adhered to the LED channels, installed the LED channels in place, cut the wires to length, and connected to source with the heat shrink connectors.

# Color Matching

After installing everything, I noticed a different color hue between the kitchen lights and the under cabinet LEDs. The cabinet lights were more magenta than the rest of the kitchen.

I have already solved this problem  when installing different model can lights in the living room and kitchen.

Ths solution is to add some green to the lights. Photography and videography use gels to shift color balance. In my case, I needed some light green.

[BHPhotoVideo green filter gel](https://www.bhphotovideo.com/c/product/166314-REG/Gam_GC1589_1_8_Plus_Green_Cine.html)

I cut this into 1/8" strips after experimenting on the amount of green I needed to add. Then attached these to the plastic covers of the LED channels with a bit of tape, and I got consistent color throughout the kitchen.

# End result
