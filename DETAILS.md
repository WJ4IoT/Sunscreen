# Noob way of programming

I still consider myself as a Newbie/NOOB and really cannot write YAML from scratch. So I need to rely on the RTFM, the topics in the community for good examples (which are sometime hard to find) and a lot of Cut & Paste and more Trial & Error and still despite my good intentions the devil is in the details. I know/knew the concept (grouping) was good but it was not yet working perfectly. At the moment I think it does, time will tell.

# Binary Sensor Template

Although you can use the textual template yaml file it might be better to in “create  helper” these sensors.
Why because each time (textual) template entities are reloaded (so even if you are working on completely something else) it will cause these sensors to be temporarily unavailable. This is not a real problem but if these sensors are grouped it can result in an unwanted SAFE/OK of that group sensor what will trigger the automation. This is due to, imho, a glitch in the “All entities” option were an unavailable sensor at that moment is not “ON” and can result in a SAFE/OK group sensor. Setting up a binary sensor template in the Ui is easy just use the details in the template yaml file.

# In between low – high do not change

The following sensor caused me some headache and here it was really the devil in  the details.
For irradiance/illuminance I was using first a “buienrader” sensor but I localised this when I used a equivalent sensor from a own weather station.
I already had a two values to compare with (high/low), and thought it was working perfectly.
So the objective is that the binary sensor does not change when the irradiance/illuminance value is between low and high. This will avoid that the sunscreen is triggered to often on a sunny but cloudy day were this value will fluctuate.
Thanks to the community I now know the value of {{ this.state }} 

```
{% if states('sensor.weather_station_illuminance')|int >= states('input_number.sunscreen_valid_irradiance_high')|int %} False
{% elif states('sensor.weather_station_illuminance')|int <= states('input_number.sunscreen_valid_irradiance_low')|int %} True
{% else %} {{ this.state | default(0, true) }}
{% endif %}
```


# Group Binary Sensor

All relevant binary sensors are grouped together in group binary sensor “sunscreen critical”
The objective is that this will simplify the automation and how often this automation will be triggered.
It does not really care how they are called as long as they act in the say way; ON equals UNSAFE/Problem Detected/Wet and OFF equals SAFE/OK/Dry. 
If you have a standard sensor which act the other way round you need to create an extra inverted sensor. 
You can replace them by any sunscreen relevant condition/criteria which you have already available in principle irradiance, illuminance, solar panels result are the same but inside temperature can be a bit tricky. The group binary sensor “Sunscreen Criteria” will act as which action is required; ON=Open and Off=Close and the group binary sensor “Sunscreen Criteria Critical” will act if the action should be done straight away (when value is UNSAFE) 

# Automation

In principle the automation does not require further maintenance (or will be much easier to maintain)
In the automation I had a problem with the syntax “ not ‘ídle’ ” but on a recent rainy I was able to tackle this

My first tip for all automation you create would be avoid the use of a device but use the entity, this will improve readability of your automation!

The most Important in the automation is the CHOOSE part; the default option is to continue and Open or Close the Sunscreen. All other options are situation where these is no change, manual is ON or the delay timer is not idle.
