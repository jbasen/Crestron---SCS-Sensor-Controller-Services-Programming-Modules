# Crestron---SCS-Sensor-Controller-Services-Programming-Modules
A full description of this architecture is available on my blog : topicsinhomeautomation.blogspot.com  I have 3 posts that introduce this new programming architecture.  The posts are titled "A New Model for Residential Automation".  This is the fourth post and it introduces the code that I have posted here.

In the first part of this four part series of posts I discussed the differences between a smart home system that implements true automation vs. one that just implements control.  In the second part I discussed a software architecture where signals from sensors in a home are routed to software control modules that implement true automation.  In the third part of this series discussed how services, or more specifically value added services, can elevate an automation system to include functionality that isn’t available today.

In my own automation system I identified eleven devices that are integrated with my automation system and I have written code to automate.  These include:

1.	Air Cleaner
2.	Ceiling Fan
3.	Garage Overhead Door
4.	HVAC System
5.	Lighting
6.	Door Lock
7.	A/V System
8.	Shades
9.	Hot Water Heater
10.	Water Shutoff Valve
11.	Whole House Fan
12.	Motorized Window

As I described in my second post, I created a template software module with a standard set of inputs from the various sensors in my home along with data my system collects from the Internet.  This includes: 

•	Environmental Data (indoor/outdoor temperature and humidity data, air quality data, weather forecast data, etc.) 
•	Presence Data (whether the home is occupied and input from motion sensors)
•	Security Data (burglar alarm triggered, fire alarm triggered, CO alarm triggered, water alarm triggered, and Armed / Disarmed status of the alarm system)
•	Time Data (time of day, sunrise time, and sunset time)
•	Bedtime Status (whether  my family is in bed for the night; or not)
•	Scenes (house wide scenes that could include lighting, shades, and other devices)
•	Module Specific Data (inputs that are unique to each specific module and the device it controls)

I then created each of the above 11 modules and took all the automation logic out of my automation programming and encapsulated it in the modules.  So, for example, if I had eleven lights being controlled by the automation system then there would be eleven lighting modules in the system; one for each light.  Parameters on the module customize it for the individual light that it is connected to. For example, one parameter would tell the automation module whether the light being automated is an interior, or exterior, light.

This significantly simplified the overall programming of my automation system as the modules encapsulate a great deal of the more complicated programming.  Essentially, the programming of my automation system has been changed to that of a simpler control system with the exception of a few additional modules added to it.  

As an example, the HVAC module implements the following automation logic

•	The module will turn off the HVAC system in the event of a fire to minimize the spread of smoke
•	The module will turn off the HVAC system in the event that the CO alarm is triggered to minimize the spread of this poisonous gas.
•	The module will turn off the HVAC system in the event that the gas alarm is triggered to minimize the spread of combustible gas.
•	If the HVAC system is turned off and the temperature in the home drops below the value specified in the modules “Low Temperature Limit in Degrees” parameter, the module will turn on the heat and set the thermostats set point to the value specified by the “HVAC_Normal_Heat_Setpoint” input to keep pipes from freezing and potentially bursting.  
•	If the current outside temperature, or the forecasted high temperature, are above the value specified in the “Warm Day Outside Temp in Degrees” parameter and it is past 9am, then the module will set the thermostats set point to the value specified by the “HVAC_Warm_Day_Heat_Setpoint” input.  The idea being that this set point is lower than the normal heat set point and money will be saved because the temperature outside is high enough to heat the home up to a comfortable temperature. 
•	The module will hold the set point to the normal value if the “Party” input is high even if a scheduled timer triggers the system that bedtime mode should be enabled.  The system will be restored to the bedtime set point once the “Party” input goes low signifying that the party is over.
•	The module will set back the temperature at bedtime and restore the set point to the normal set point value when the bedtime signal goes low
•	The module will set back the temperature when the alarm is armed in vacation mode and restore the set point to the normal set point value when the “Security_Arm_Vacation” signal goes low
•	The module will set back the temperature when the “Occupied” signal goes low and restore the normal set point when the “Occupied” signal goes high
•	HVAC Health – The system will validate that the furnace or air conditioner is working properly by validating that it is keeping the temperature within “HVAC Health Temp Limit” degrees of the set point
•	Offline Protection – If the thermostat goes offline the module will pulse the “Notification_Thermostat_Offline” module output so notification can be sent to the homeowners

To help other people envision this I’ve placed all of my modules into a demo program with an extensive help file and published them on my github.  All of the above modules are free for other people to use.  The only exception is that, in the demo program, I’m also using the module I wrote that tracks that angle and elevation of the sun.  This module provides input to the shade automation module.  It is released as shareware and the licensing terms are specified in the help file for that specific module.

I expect that other people may not want to automate their systems in exactly the same way I have automated mine.  In this case the modules can be a starting point and you can modify the logic inside as you see fit.  The code of the modules is written with Crestron’s Simplwindows tool; plus some code written in Simpl+.

I obviously couldn’t implement the value added services portion of the architecture on my own as that requires an investment by product manufacturers. 

Hopefully, other people will see the value of this architecture and use it to take the systems the systems they develop for customers to the next level.

