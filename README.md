# ESPHome Weather Dashboard
This Uses Home Assistant paired with ESPHome to make a weather dashboard. Others have similar dashboards, but due to my lack of knowledge or skill I was not always able to replicate. This readme is as much documentation for myself as it may be for someone else. I personally used the Pirate Weather Integration for my weather information. I found it to be more accurate than many other popular options including OpenWeather. 

## Contents
- [Hardware](#hardware)
- [Wiring](#wiring)
- [Home Assistant Prep](#HomeAssistantPrepWork)
- [ESPHome Setup](#ESPHome-Setup)
  -   [YAML](#YAML)

## Hardware

- [Waveshare Universal e-Paper Driver Board - ESP32](https://amzn.to/42aUrFG)
- [Waveshare 7.5" e-Paper Screen 800x480](https://amzn.to/40AmZ9p)
- [3d Printed Case (thanks Spanholz)](https://www.printables.com/model/657756-simple-case-for-esp32-weather-station/files)

## Wiring
take pictures and include here

## Home Assistant Prep Work
I found the data in Pirate Weather to be the most accurate for my location and it had the benefit of having a configuration option to make sensors for my various forecast choices. I now understand how to handle it without creating all the sensor entities but it's working so why change it now. 

- <b>Step 1:</b> Install the Pirate Weather Integration. I installed through HACS. Find more information here [Pirate Weather Integration](https://github.com/Pirate-Weather/pirate-weather-ha)

- <b>Step 2:</b> Configure Pirate Weather integration. If you are like me remember to enter in the comma separated days and hours (ex: "0,1,2,3,4,5"). I did not round values. 

  <i>Important! I configured mine to check both the "Sensor" and "Weather" options. This will make a bunch of entities in your home assistant. If that is a problem for you then you will have heavier edits to my setup. My setup utilizes the sensors, this will likely make the most difference in the Automation setup in Home Assistant. </i>

- <b>Step 3:</b> Create Helpers. This will make life 1,000x easier in ESPHome. These are to format text like days ("Mon", "Tues", etc) and times ("1PM", "10AM", etc) in Home Assistant that you can pull into ESPHome already formatted. We will make an automation to fill these in on a schedule. You need to make one for each day you are forecasting in the middle daily section and each hour you have in the hourly section. 

  - Go to your Home Assistant integration go to "Settings", "Helpers", "Create Helper". 

  - Select "Text" and enter a name. My advice is be consistent. 

      <i>I followed a format of "Weather: PW: Day X Name" and "Weather: PW: Time XH Name". Additionally, be consistent in your entity IDs (ex: "input_text.pw_day_x_name" and "input_text.pw_time_xh_name")</i>

- <b>Step 4:</b> Create an Automation to set the values in the Helpers we created. 
