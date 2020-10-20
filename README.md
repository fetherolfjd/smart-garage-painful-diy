# A Smart Garage the Stupid Way

## Background

I've recently moved to a new home, which happened to come with a [Chamberlain MyQ](https://www.chamberlain.com/myq) garage door opener.
I've previously been using the [GoControl Z-Wave Garage Door Controller](https://www.gocontrol.com/detail.php?productId=4) in conjunction
with [Samsung SmartThings Hub](https://www.smartthings.com/products/smartthings-hub) for a number of years with great success. Well, as it
turns out, the Chamberlain MyQ opener does not work the same way that most garage door openers do. The wall opener is treated more or less
like one of the mobile garage door remote controls, rather than just a switch that connects two wires it sends a wireless signal to the opener.

Of course, Chamberlain sells their own "gateway", but it does not natively support integration with SmartThings.
Instead someone clever has created a [third party integration](https://github.com/brbeaird/SmartThings_MyQ) that may be shut down at any point.
I don't particularly like being at the whim of a company that probably doesn't care about the few customers that want this type of integration,
so I set out to make my GoControl controller work in this setup.

The usual internet research reveals that some other people have tried to tackle this as well, and the generally accepted
consensus was to use one of the wireless remotes, and connec the GoControl controller across one of the buttons so that
it would basically act as though a person were pressing it. It sounds like some people have very good success with this,
but I did not. The garage door would open and close properly for the first little while (a few minutes to a few hours)
before it would start erratically and constantly trying to open and close.

Looking at the wireless remote control that I had connected the GoControl controller to, sure enough the light was constantly flashing
as if someone was holding the open/close button down and never releasing it.

Clearly, this simply will not do....

## Hypothesis

After some poking around with my digital multimeter, I think I have found the culprit!

The wireless remote control uses a CR2032 coin battery, which outputs 3V. The voltage coming out on the other side
of the GoControl controller was only ~1.5V! This is almost certainly in the center of the expected voltage extremes that
the little microcontroller on the wireless remote control expects. This is reinforced by the erratic behavior that the opener
exhibits, constantly turning on and off, unsure if it should be acting as though the button is pressed or not.

## Solution

To solve this problem, clearly the best solution is a [voltage booster](https://www.electroschematics.com/one-battery-3-volts-step-boost-converter/)!
Searching the internet again, I quickly found several options, with one of the simplest being [this circuit](assets/1.5v-to-3v-boost-converter.png).
It does not require much in the way components, and is relatively cheap. [Here](assets/mouser_parts_list.pdf) is my purchase order from [Mouser Electronics](https://www.mouser.com/).

Eventually, I got around to poorly soldering this circuit together and wiring it up, and behold! Success! At least for a little bit....

It turns out that the combination of the voltage booster circuit and the GoControl controller was causing the CR2032 battery to drain
_very_ quickly. I would only get a few minutes of use out of the opener before the battery was so low nothing responded at all.

Fortunately, I had an extra wall socket available that I could purchase a simple [3V power brick](https://www.amazon.com/gp/product/B07663HSRN/), and solder that to the wireless
remote in place of the battery. Once this was accomplished everything has been working flawlessly, just like the old days!

[Completed circuits](assets/completed_1.jpg)

[Wider environment setup](assets/completed_2.jpg)

## Remarks

### Possible improvements

 - Circuit could be made smaller
 - More efficient circuit that could continue using the CR2032 battery (though the GoControl controller may have a role in this)
 - Larger battery pack that could stand up to the power draw better and last for months or a year
 - An enclosure
 - Cheaper components could be used

### Drawbacks

Not many drawbacks to this that I've found yet. The biggest being finding the time to just troubleshoot the problem and implement
the solution. From first experiencing problems (shortly after moving in) to having this solution complete about 6 months have
passed.

I also probably ended up spending a little bit more money on this solution, but probably not much.
