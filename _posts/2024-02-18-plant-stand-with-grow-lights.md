My wife wanted a plant stand because our succulents had gone wild. She showed me an amazon item, and I thought, "$60? I can build that myself." So I did. It only cost $100 and took an entire weekend.

## Inspiration

Something like this: [https://www.amazon.com/ROSSNY-Bamboo-Outdoor-Multiple-Planter/dp/B08TH15BDH](https://www.amazon.com/ROSSNY-Bamboo-Outdoor-Multiple-Planter/dp/B08TH15BDH)

The original idea was to buy two and have them in the window corner at right angles. The gaps on the shelves were a bit too wide, and too much space between the shelves for succulents.

Drew up some rough plans on how to build this myself.

![Initial plans]({{site.baseurl}}images/plant-stand/20231003_174828.jpg)

## Materials

1x2 pine from menards. A mix of the cheaper and nicer boards. The nicer ones would be for the frame, and the cheaper would be ripped into 3 for shelf slats.

## Frame

The frame is made of four components: Base, back, left, and right. Shelves would fit on both sides of the left/right bars, and would be supported by the back for the outer shelves.

I cut the boards to length and joined with pocket holes and glue. Hid the pocket holes on faces that wouldn't be visible. I did a poor job aligning the faces, so I paid for that with aggressive sanding later. Learned why face clamps exist. Will probably buy those next time, or at least use my F clamps.

## Shelves

Using my 3d printed featherboard, I ripped some 1x2 pine into thirds. Dialing in the cut took some math that I quickly abandoned in favor of trial and error.

![3D printed featherboard]({{site.baseurl}}images/plant-stand/featherboard.jpg)

To make the shelves consistent, I created a box frame, cut spacers from scrap, and made a nail alignment board.

![Shelf jig]({{site.baseurl}}images/plant-stand/20231007_115919.jpg)

![Spacers]({{site.baseurl}}images/plant-stand/20231007_124850.jpg)

![Nail alignment jig]({{site.baseurl}}images/plant-stand/20231007_124901.jpg)

The shelf came together with wood glue and brad nails.

![Single shelf]({{site.baseurl}}images/plant-stand/20231007_125520.jpg)

Now to make 13 more.

Luckily this is a multi-core system and the IO-bound workload could be distributed across processors.

![Helper]({{site.baseurl}}images/plant-stand/20231007_144253.jpg)

After a few hours, we were ready for a forklift themed party.

![Forklift themed party]({{site.baseurl}}images/plant-stand/Snapchat-239340556.jpg)

## Finnish

I'm not Finnish, I'm Ukrainian.

## Finish

Used minwax paste wax, applied with a rag before assembly. Tough to get into all of the corners. Applying finish before gluing the shelves together might have been easier, but I don't think the glue would hold.

![Completed shelf]({{site.baseurl}}images/plant-stand/20231007_211957.jpg)

![Completed shelf close up]({{site.baseurl}}images/plant-stand/20231007_212004.jpg)

![Loaded with plants]({{site.baseurl}}images/plant-stand/20231009_102631.jpg)

# LED Lighting

Succulents typically want to be in full, hot desert sun for 10 hours per day. A window in the gloomy midwest doesn't come close. Lights are absolutely necessary to keep the plants from stretching and getting lanky.

Grow lights aren't anything special. Purple lights only emit red and blue. There are some claims that this is more efficient, and that's all plants need, but I don't believe that. They get the full spectrum outside, they should be fine with full spectrum inside.

I just wanted the brightest lights per dollar, and to make it look good on the shelf. Aliexpress LED strips are what I settled on.

[https://www.aliexpress.us/item/2251832514287343.html](https://www.aliexpress.us/item/2251832514287343.html)

5630 5M IP30 WW

$10 for 5 meters. I bought too few, so I had to place a second order halfway through the project. Ended up using almost exactly 15 meters, so a total cost of $30 for the LEDs, plus a month of shipping/customs.

## Attaching

I started the project by using included adhesive backing. This was a mistake. The adhesive was nowhere near strong enough, and about half of the strips started falling off within two weeks.

My next attempt was hot glue. I needed a hot glue gun for another project anyway. After a couple hours watching youtube reviews I bought the big one from harbor freight: [https://www.harborfreight.com/high-temperature-full-size-glue-gun-68334.html](https://www.harborfreight.com/high-temperature-full-size-glue-gun-68334.html)

If it breaks, then I'll consider the nicer ones.

The hot glue actually went pretty well. I ran a bead down the entire line and pushed the LED strips down quickly.

After a few months, I see one strip halfway off that I need to fix. Better success rate than the adhesive strips.

## Wiring

The bulk of the work. I couln't think of a better way than running strips down the thin shelf slats and soldering them together.

The process was a bit of assembly line work, cutting wires, cutting and de-soldering LED strips, stripping (the wire, not the clothes), tinning, and soldering together. I basically made a wiring harness for each shelf based on standard dimensions, then attached.

Positive to positive, negative to negative. Over and over. Tedious.

![Led strips attached, one shelf]({{site.baseurl}}images/plant-stand/20231217_140816.jpg)

Proof of concept, one shelf.

![One shelf LEDs done]({{site.baseurl}}images/plant-stand/20231217_140800.jpg)

I ran out of LED strips halfway through, had to order more and wait a month for shipping from China.

![Halfway done LEDs]({{site.baseurl}}images/plant-stand/20231217_201706.jpg)

To connect all of the shelves, I used heat shrink crimp butt connectors (hehe). By fitting two wires in each end, I created junctions of up to four directions: input, nearby shelf 1, nearby shelf 2, output.

The entire assembly is wired in parallel, meaning the voltage is equal acros all points. LED strips themselves are wired in parallel to a point. The solder connection points are all parallel, but the indiviual diodes are in series to drop the voltage to the correct amount. So the cut line is at 12V, but with 3 LEDs per cut section, each LED recieves 4V (looks like there's a resistor too).

![Wiring harness junction]({{site.baseurl}}images/plant-stand/20231229_142023.jpg)

Hot glue for cable management. Better than my first attempt with bent brad nails.

![Hot glue cable management]({{site.baseurl}}images/plant-stand/20231229_141842.jpg)

Overall, it was a lot of work.

![Wiring complete]({{site.baseurl}}images/plant-stand/20231229_141827.jpg)

![Wiring complete powered on]({{site.baseurl}}images/plant-stand/20231229_141911.jpg)

But I can't think of a better way to get the same clean look.

![Shelf in place with lights on]({{site.baseurl}}images/plant-stand/20231230_092932.jpg)

I had a spare mirror, so I put that behind the shelf to reflect even more light.

## Power supply

I figured, LEDs are super efficient, throw 12v of whatever at them, should be fine. I was very wrong.

This thing needs about 1.21 jigowatts to run.

I went through the five stages of cheapskate grief.

### Denial

I bought a cheap power supply on amazon rated for 5 amps. The package arrived empty so that was a whole separate ordeal. Once I actually got the item, I plugged it in, and it quickly overheated with only half the LEDs installed. Once overheated, the lights would flicker on and off about 2 times per second. Very annoying.

### Anger

Why is this happening to me? This power supply is garbage, Amazon has gone way downhill.

### Bargaining

I bought another cheap power supply, this time from eBay, rated for 10 amps. This would surely work. The multimeter says I'm only pulling 4 amps. Over double the capacity should be plenty.

Nope. Overheated, but a little slower this time.

### Depression

I may have cried.

### Acceptance

So the $10 range doesn't cut it. I need to go over the top. 

I bought this 350W (29A at 12V. P = V * I) power supply. For $50. It has a built in fan, which helps with the overheating problem.

[https://www.superbrightleds.com/mean-well-led-switching-power-supply-lrs-se-series-100-1000w-enclosed-12v-dc-se-x-12+powercordyn-Without~Power~Cord+wattconsume-350~Watts](https://www.superbrightleds.com/mean-well-led-switching-power-supply-lrs-se-series-100-1000w-enclosed-12v-dc-se-x-12+powercordyn-Without~Power~Cord+wattconsume-350~Watts)

![Power supply]({{site.baseurl}}images/plant-stand/20231229_141928.jpg)

So far, this has been flawless.

It's plugged into a cheap christmas light timer, so the lights automatically turn on and off.

# Pictures

Mirror behind the shelf for more light reflection.

![Mirror]({{site.baseurl}}images/plant-stand/20231229_142511.jpg)

Time to load this up with plants.

![Lights on]({{site.baseurl}}images/plant-stand/20231230_093003.jpg)

![Plants and lights on]({{site.baseurl}}images/plant-stand/20231230_121047.jpg)

![Plants and lights on angle]({{site.baseurl}}images/plant-stand/20231230_121100.jpg)

You can tell some of the succulents started to stretch for light while I worked on thisl.

![Plants close up]({{site.baseurl}}images/plant-stand/20231230_121116.jpg)
