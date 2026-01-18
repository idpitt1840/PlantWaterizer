An ESPHome / Home Assistant Indoor plant irrigation system

This unit monitors the moisture content of up to 5 plant pots and delivers water to them as needed.  It uses low cost moisture sensors and pumps. 

<img width="1734" height="976" alt="image" src="https://github.com/user-attachments/assets/e61fa546-39ee-4300-9bb3-7b0f0fd9d52b" />

( The controller is the top box.  The bottom box is an overengineered grow light power switch that's repurposed from a different project ).

<img width="1734" height="976" alt="image" src="https://github.com/user-attachments/assets/b612c39a-7bfa-42a2-b82a-88a61819572b" />

The pots are fed via a 3D printed water distribution ring ( STLs available from https://www.printables.com/model/103003-drip-irrigation-ring ). I modified the design a little to take a bigger hose.

<img width="1734" height="976" alt="image" src="https://github.com/user-attachments/assets/53b1d9de-0f58-4fbb-9837-d463f7509223" />

The pumps are held in a 5 gallon bucket, lid cutout is to prevent the cats from trying to get a drink and knocking the whole thing over.  The hoses are held in place with 3D printed mounts, STL at https://www.thingiverse.com/thing:5642735/files. The pumps are held at the bottom of the bucket simply by the friction of the hose clamps and the hose itself. I did create a 3D printed mount for all of this then decided not to stick more microplastics into the mix. 

Soil sensors are enclosed in a 3D printed housing, STL at: https://www.printables.com/model/247398-capacitive-soil-moisture-sensor-v20-cover

The project case is generated from the follow parametric OpenSCAD project: https://www.printables.com/model/72839-customizable-parametric-stable-and-waterproof-elec.    The hardware is probably overkill but it adds a nice industrial look to the finished project: 

<img width="1734" height="976" alt="image" src="https://github.com/user-attachments/assets/aaf9fb6e-4158-460a-9fde-1bdf01c7b3d8" />

( Yes, I know the Float and Water sensor labels don't match  the connectors ).  This unit is currently driving 4 zones. 

Assembled unit: 

<img width="549" height="976" alt="image" src="https://github.com/user-attachments/assets/f6c17722-7621-4bf2-8c3c-1152e0862de1" />

Plenty of hot glue to keep those connectors under control. The aviation plugs should be fine if tightened up sufficiently. The USB sockets were a little wobbly so they need the glue treatment.  The wires from the LEDs and connectors are soldered directly to the header pins which isn't great and makes for fiddly assembly. I really couldn't be bothered with creating the dupont connectors enmass.   The moisture sensor inputs require 3v3, input and ground connectivity. The onboard 3v3 regulator on the ESP32 devkit board has enough capacity for these.   The pumps are fed from 5V via the MOSFETs.  Are MOSFETS overkill? Probably but they work and I had them and there's zero heat given off due to the low voltages, currents and the fact they're not rapidly switching on and off plus if I choose to upgrade the pumps to high current units, I won't need to change the design. A common NPN transistor might do the job. 

While this design uses HomeAssistant for status reporting and dashboarding,  it will run standalone once flashed since the logic controlling the moisture is in the ESP32.  You'll need to set the timeouts on the network and Home Assistant API to 0 in the code if you wish to run standalone.
I use Home Assistant for emailing me when the water is low and manual control of the pumps, and the dashboarding of course.   HA also controls the grow lights, though as a simple timer. 

When I added up all the components, it came to a somewhat shocking $100+ USD though a lot of that is due to the connecting hardware.  That's still way cheaper than having a 5 zone commercial unit though.  

The pumps aren't great. They are only $2 each so I'm not surprised. They have just enough water column head capacity to do what I want though one of the pots is right on the edge given it's height.  If you look in the code, you'll see one zone waters on a longer on cycle than the others. That's for this pot. I didn't do any noise or spike supression of the motor power circuits since I figured they were USB only and that would be within the motor.  I did find that the float switch GPIO would occasionally trigger when a pump turned on but I solved that through software. 

A note on the ESP32-S3 devboard: You could get away with a simpler ESP32 device since the S3 is overkill for the needs of this unit.  Note that the footprint in the schematic isn't quite the same as the board I used with the only difference of note being the ADC inputs. The ADC inputs you use isn't that important, though look to keep within the ADC-1 range rather than ADC-2 since that gets used for the chip's wifi apparently.  Just update the YAML for ESPHome accordingly. While this board has PSRAM, the program doesn't use it.  I really should have just used a cheaper board :) 

The board layout was done in KiCad.  I used Gemini to create some python to generate the strip board layout since that then allowed me to print the cutting mask to stick on the underside of the strip board for guiding my track cutting hand. It also allowed for a 3D representation to be rotated and zoomed to check that it was buildable. I had tried Fritzing which, while very capable, didn't have the fabrication tools that KiCad with python allowed me to have. I've uploaded the KiCad folder should you wish to go this route.  I added a screen shot of the strip board layout anyway. 

Enjoy
