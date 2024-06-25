# Sunscreen
Sunscreen Automation in Home Assistant (YAW2A)

! A year later it was time for an update; with the introduction of two group binary sensors the automation is much simpler. 
You only need to put ALL criteria in binary_sensor.sunscreen_criteria.
And those criteria who need instant action in binary_sensor.sunscreen_criteria_critical.

I als introduced a (local) rain sensor, there are topics inside the HA-forum but also this post looks very much like mine:
https://en.number13.de/homeassistant-zha-zigbee-rain-sensor-hack-build-yourself/


There are many type of sunscreen / blinds and perhaps after lights your next thing to automate.
With fresh or read zero experience in Home Assistant and YAML's, I tried to RTFM and find proper examples.
The example suitable for my sunscreen I found in this topic published by Remco:

see: https://community.home-assistant.io/t/how-to-automate-my-awning-sunscreen/40813/91

stubborn as i am changed things, (likely to understand better the proces), hence the reason I did not fork his repository.

see: https://github.com/remb0/Sunscreen

So this automation is suitable for a (outside the house) sunscreen with only two options:
- CLOSED (for me inside container)
- OPEN (for me sunscreen fully opened)
It is possible you would like to see it working differently, this only requires a change at two places in the automation.

The tricky part remains the amount of sun; presently with sensor.buienradar_irradiance but I am thinking to change this part in more locally option with a lux sensor. I already a weather station 

To avoid frequent roll-in and roll-out's a waiting time is introduced for the next movement but off course in case of a critical situation there will be a direct action.

This is done by the following conditions (subject to change):

For this process the folowing sensors are used:
- rain (using Buienrader.nl but feel free to change)
- wind (using Buienrader.nl but feel free to change)
- irradiance (using Buienrader.nl but feel free to change)
- Azimuth (from sun.sun)
- Elavation (from sun.sun)
- in which months to operate
- anything else what better suits your needs like time, temperature, lumination, solar values, etc

These are compared against the valid values (just create them manual in helpers):
- input_number.sunscreen_valid_rain
- input_number.sunscreen_valid_wind
- input_number.sunscreen_valid_irradiance a _low and a _high value 
- input_number.sunscreen_valid_azimuth
- input_number.sunscreen_valid_elevation

Resulting in the following binary sensors (see template file: https://github.com/WJ4IoT/Sunscreen/blob/main/sunscreen_template.yaml)
- binary_sensor.sunscreen_criteria_rain
- binary_sensor.sunscreen_criteria_wind
- binary_sensor.sunscreen_criteria_irradiance
- binary_sensor.sunscreen_criteria_azimuth
- binary_sensor.sunscreen_criteria_elevation

These binary sensors are grouped together in the group binay sensors:
- binary_sensor.sunscreen_criteria          # your collection of ALL binary sensors which are criteria for open/close sunscreen
- binary_sensor.sunscreen_criteria_critical # your collection of CRITICAL binary sensors which are criteria for open/close sunscreen

Above two sensors plus the next three sensors are used in display and automation: 
- timer.sunscreen_delay                     # the time next sunscreen action will be dome if delayed
- input_boolean.sunscreen_manual            # manual overide, however critical criteria will close the sunscreen
- input_boolean.sunscreen_status            # present status of the sunscreen

Steps in the automation are (see: https://github.com/WJ4IoT/Sunscreen/blob/main/sunscreen_automation.yaml)

- Triggers: timer not idle or sunscreen criteria sensor changed
- Conditions: only continue when there is a change in status and not in manual modus (unless it is critical)
- Perform sunscreen actions and (re)set timer


How does it look like?
https://github.com/WJ4IoT/Sunscreen/blob/main/sunscreen.png
 

BTW = By The Way YAW2A = Yet Another Way to Automate.
TECP = Trial & Error, Cut & Paste
