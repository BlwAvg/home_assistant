# Configs
- Air Quality - Turn on AC Fan
  - Checks C02 from 3 sensors and VOCs from one sensor. If they reach a defined threshold, it will turn on the AC fan. 
  - The sensors are an Awair, Apollo MSR-2 and MTR-1. The thermostat is an EcoBee3.
  - This automation will kick off if any sensor sets off the automation but won’t set the fan back to "auto" until all sensors are below the defined threshold.
  - There is a dead zone between turning on and off. That is intentional because I do not want the fan to keep flapping between "ON" and "Auto".
  - I should also add timer that it has to be in a specific state for at least 5-10min. 
- Auto Close Garage
  - If a person leaves the house and the garage door is left open for more than 15min this will auto close the garage door.
  - Presence is detected using cell phones. The garage door controller is a RATGDO.
- Enable Plug - Occupancy
  - Enables a plug upon entering a room.
  - You may need to add a timer, so it stays on or off for a specified period of time. Although I have not needed to because the radar sensor works awesomely.
- Moist Master
  - This turns on a plug, to a humidifier if the humidity goes below a specific threshold.

