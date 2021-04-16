# Syringe Pump Assembly and Automation

![Image of full syringe assembly for maximum wow factor.](/SyringePumpDemo/assets/syringepump.jpg)

See mechanical page [here](/SyringePumpDemo/mechanical.html).

For electrics, check [here](/SyringePumpDemo/electrical.html).

Code is [here](/SyringePumpDemo/code.html). It's a little different from what you might expect. 

## Specifications

This syringe pump is capable of a resolution of 0.111 microliters/step if microstepping is employed to the controller's full capacity, and 17.7 microliters/step if full steps are taken. My code allows little loss of speed at minimum microstep size, but the workaround would likely have issues with Arduino transmission speed at faster rates. The syringe itself is 20mL and can easily be changed out, allowing for ideal solution addition to the device. A few parts are 3D printed, and the rest can easily be obtained from a dealer like McMaster-Carr. Links and 3D models are in the mechanical page, above. 

The pump is driven by a standard Arduino Uno or MEGA microcontroller, and the circuitry description can be found on the electric page. Code with microstepping compensation is on the code page.

The maximum flow rate is limited primarily by the stepper motor's maximum step speed, and those vary between NEMA 17 motors. With a sufficiently good motor, the limiting factor--particularly with the aforementioned microstepping compensation--would be the Arduino's ability to send step pulses. 

## Why you should care and build your own:

This pump is criminally easy to build, and can form the core of a fluid delivery or bioplotter system. All of my files are available to you for printing and construction, and all parts are easily available from dealers. By building one yourself, you can gain valuable expertise and insight that will allow you to customize and alter my design to your specifications with a far higher amount of flexibility than buying a premade one, all while saving yourself a decent amount of money. It's worth it for the construction experience alone.
