# Sunscreen
Sunscreen Automation in Home Assistant (YAW2A)

There are many type of sunscreen / blinds and perhaps after lights your next thing to automate.
With fresh or read zero experience in Home Assistant and YAML's, I tried to RTFM and find proper examples.
The example suitable for my sunscreen I found in this topic published by Remco:

see: https://community.home-assistant.io/t/how-to-automate-my-awning-sunscreen/40813/91

stubborn as i am changed things, (likely to understand better the proces), hence the reason I did not fork his repository.

see: https://github.com/remb0/Sunscreen

So this automation is suitable for a (outside the house) sunscreen with only two options:
- fully open = rolled in(side the cassette)
- fully close = rolled-out

To be honest this is not yet fool proof; if you manual operrate your sunscreen during heavy rain fall you just can do that.

As you can see in the automation I consider wind and rain as critical in other words roll-in (to speak of close or open can be a bit confusing for me)

I also use the orientation of my house (translated in sun-azimuth) and for an earlier sunset due nearby trees the sun- elevation (over time or sun set/rise).

The tricky part remains the amount of sun; presently with sensor.buienradar_irradiance but I am thinking to change this part in more locally option with a lux sensor. Situated inside the house will give another problem due to likely the dimming factor of ther sunscreen.

To avoid frequent roll-in and roll-out's a waiting time is introduced for the next movement but off course in case of a critical situation there will be a direct action.


This is done by the following conditions (subject to change):

For this process the folowing sensors are used:
- rain (using Buienrader.nl but feel free to change)
- wind (using Buienrader.nl but feel free to change)
- irradiance (using Buienrader.nl but feel free to change)
- Azimuth (from sun.sun)
- Elavation (from sun.sun)
- in which months to operate
- anything else what better suits your needs like temperature, lumination, solar values, etc (you only need to add them)

These are compared against the valid values (just create them manual in helpers):
- input_number.sunscreen_valid_raid
- input_number.sunscreen_valid_wind
- input_number.sunscreen_valid_irradiance a _low and a _high value 
- input_number.sunscreen_valid_azimuth
- input_number.sunscreen_valid_elevation

Resulting in the boolean vaLues (created in the template file)
- input_boolean.sunscreen_valid_raid
- input_boolean.sunscreen_valid_wind
- input_boolean..sunscreen_valid_irradiance
- input_boolean..sunscreen_valid_azimuth
- input_boolean.sunscreen_valid_elevation

These boolean values are used in the automation with the additional booleans (BTW you also need them to add in helpers):
- timer.sunscreen_delay
- input_boolean.sunscreen_set_manual
- input_boolean.sunscreen_conditions_close
- input_boolean.sunscreen_conditions_close_request
- input_boolean.sunscreen_fake # can be used for sandboxing

Steps in the automation are:

1 Triggers to start the automation are the criteria of a potentitial change
  These are devided in three trigger-ID's
  - critical: when there is to much wind or rain the sunscreen will roll-in
  - normal; these can be delayed if a timer is still running or direct.
  - delayed action based upon the timer becomes idle

2 check if there is a change, if not stop

3 check if we need to delay the change to avoid too frequent changes

4 change and (re)set timer

For the moment I am not using packages (I wish I did), but in principle you can C&P this automation into automation for the exception of the change of "YOUR_SUNSCREEN_ID", the template file you can save according instuctions or what you use to do.

BTW = By The Way YAW2A = Yet Another Way to Automate.

TECP = Trial & Error, Cut & Paste

