# Sunscreen
Sunscreen Automation in Home Assistant (YAW2A)

There are many type of sunscreen / blinds and perhaps after lights your next thing to automate.
With fresh or read zero experience in Home Assistant and YAML's, I ried to RTFM and find proper examples.
The example suitable for my sunscreen I found in this topic from Remco:

see: https://community.home-assistant.io/t/how-to-automate-my-awning-sunscreen/40813/91

stubborn as i am changed things, (likely to understand better the proces), hence the reason I did not fork his repository.

see: https://github.com/remb0/Sunscreen

So this automation is suitable for a (outside the house) sunscreen with only two options:
- fully open (rolled inside the cassette)
- fully close (rolled-out)

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
- input_number.sunscreen_valid_irradiance
- input_number.sunscreen_valid_azimuth
- input_number.sunscreen_valid_elevation

Resulting in the boolean vaLues (created in the template file)
- input_boolean.sunscreen_valid_raid
- input_boolean.sunscreen_valid_wind
- input_boolean..sunscreen_valid_irradiance
- input_boolean..sunscreen_valid_azimuth
- input_boolean.sunscreen_valid_elevation

These boolean values are used in the automation with the additional booleans:
- timer.sunscreen_delay
- input_boolean.sunscreen_set_manual
- input_boolean.sunscreen_conditions_close
- input_boolean.sunscreen_conditions_close_request
- input_boolean.sunscreen_fake # can be used for sandboxing

Steps in the automation are:

1 Triggers to start the automation are the criteria of a potentitial change
  These are devided in three trigger-ID's
  - direct (critical wind/rain starting)
  - others; these can be delayed if a timer is still running or direct.
  - delayed action based upon the timer becomes idle

2 check if there is a change, if not stop

3 check if we need to delay the change to avoid too frequent changes

4 change and (re)set timer

BTW = By The Way

YAW2A = Yet Another Way to Automate.

TECP = Trial & Error, Cut & Paste

