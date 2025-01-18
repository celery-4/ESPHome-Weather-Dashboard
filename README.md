# ESPHome Weather Dashboard
This Uses Home Assistant paired with ESPHome to make a weather dashboard. Others have similar dashboards, but due to my lack of knowledge or skill I was not always able to replicate. This readme is as much documentation for myself as it may be for someone else. I personally used the Pirate Weather Integration for my weather information. I found it to be more accurate than many other popular options including OpenWeather. 

## Contents
- [Hardware](#hardware)
- [Wiring](#wiring)
- [Home Assistant Prep Work](#home-assistant-prep-work)
- [ESPHome Setup](#ESPHome-Setup)
  -   [YAML](#yaml)
- [Case](#case)

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

- <b>Step 3:</b> Create Helpers. This will make life 1,000x easier in ESPHome. We will make an automation to fill these in on a schedule.

    These are to format text like days ("Mon", "Tues", etc) and times ("1PM", "10AM", etc) in Home Assistant that you can pull into ESPHome already formatted.  You need to make one for each day you      are forecasting in the middle daily section and each hour you have in the hourly section. If you have other text that you want to dynamically change its a good idea to make them now. 

  - Go to your Home Assistant integration go to "Settings", "Helpers", "Create Helper". 

  - Select "Text" and enter a name. My advice is be consistent. 

      <i>I followed a format of "Weather: PW: Day X Name" and "Weather: PW: Time XH Name". Additionally, be consistent in your entity IDs (ex: "input_text.pw_day_x_name" and "input_text.pw_time_xh_name")</i>

- <b>Step 4:</b> Create an Automation to set the values in the Helpers we created. The idea behind this automation is to set a trigger of something like every hour on the 2nd minute, set the helpers you made before to the formatted value of text you would like ("Mon", "2pm", etc). The speediest way to is to create an automation and copy my yaml but change as you need. However, I laid out the steps below. 

  1.  Go to the Automations page in your Home Assistant setup.
  2. Under "When", Click "+ Add Trigger"
  3. Select "Time and Location"
  4. Select "Time Pattern"
  5. In the "Minutes" section type in the minute you want it to run. I suggest a couple mins into an hour to make sure your sensors have updated. For example if you type in "2" then it will run on the 2nd minute of every hour.
  6. Now you will have to go to the YAML view. Click the top right 3 button menu. and select "Edit in YAML"
  7. Under "actions:" you will need to put in a block similar to the below for each of your days/hours". You can use the Developer Tools, Template to play with the formatting you want. The "value" below is to change the datetime of the priate weather sensor to an abbreviated day name. See the guide here (https://esphome.io/components/time/). It then sets the helper that was created in step 3 to that text. 
     ```
     actions:
      - action: input_text.set_value
        metadata: {}
        data:
         value: "{{ as_datetime(states('sensor.pirateweather_time_0d')).strftime('%a') }}"
        target:
         entity_id: input_text.pw_day_0_name
  8. Name and Save the Automation
 
## Case
Lilifee made a beautiful case here (https://www.printables.com/model/47605-e-paper-display-stand-esp32)

Spanholz made another great case that has lots of room for batteries or whatever you may want to store (https://www.printables.com/model/657756-simple-case-for-esp32-weather-station)
