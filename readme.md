Pi Weather Station
==================

`STILL very much A WORK IN PROGRESS`

This is a Raspberry Pi project that measures temperature and humidity and pressure then uploads the data to a Weather Underground weather station. This is a complementary project to [https://github.com/johnwargo/pi_weather_station](https://github.com/johnwargo/pi_weather_station) that performs the same function, only uses the Astro Pi Sense HAT for measurement instead.  I added this project in case Makers wanted to implement the project, but didn't want the added expense of the Sense HAT.   

https://learn.adafruit.com/dht/overview
https://github.com/adafruit/Adafruit_Python_DHT
https://learn.adafruit.com/dht-humidity-sensing-on-raspberry-pi-with-gdocs-logging/overview


 
Required Components
-----------------------------

This project is very easy to assemble, all you need is the following 4 parts, and they all connect together:

+ [Raspberry Pi 3](https://www.raspberrypi.org/products/raspberry-pi-3-model-b/) - I selected this model because it has built in Wi-Fi capabilities. You can use one of the other models, but you'll need to also purchase a Wi-Fi module or run the device on a wired connection.
+ DHT22 temperature-humidity sensor - Since I used the Adafruit drivers for this project, it's probably best if you acquire one from [Adafruit](https://www.adafruit.com/products/385). 
+ Raspberry Pi Power Adapter. I used this one from [Amazon](http://amzn.to/29VVzT4). 

Project Files
-----------------------------

The project folder contains several files and one folder: 

+ `config.py` - This is the project's external service configuration file, it provides the weather station with details about your Weather Underground station.
+ `LICENSE` - The license file for this project
+ `readme.md` - This file. 
+ `start-station.sh` - Shell script to start the weather station process. 
+ `weather_station.py` - The main data collection application for this project. You'll run this application to read the DHT22 and post the collected data.   

Hardware Assembly
-----------------------------



Weather Underground Setup
-----------------------------

Weather Underground (WU) is a public weather service now owned by the Weather Channel; it's most well-known for enabling everyday people to setup weather stations and upload local weather data into the WU weatherbase for public consumption. Point your browser of choice to [https://www.wunderground.com/weatherstation/overview.asp](https://www.wunderground.com/weatherstation/overview.asp) to setup your weather station. Once you complete the setup, WU will generate a station ID and access key you'll need to access the service from the project. Be sure to capture those values, you'll need them later.

Software Installation
-----------------------------

Download the Raspbian image from [raspberrypi.org](https://www.raspberrypi.org/downloads/raspbian/) then burn it to an SD card using the instructions found at [Installing Operating System Images](https://www.raspberrypi.org/documentation/installation/installing-images/README.md).

Power up the Raspberry Pi. If you'll be using a Wi-Fi connection for your Pi, configure Wi-Fi access using [Wi-Fi](https://www.raspberrypi.org/documentation/configuration/wireless/).

Next, open a terminal window and execute the following commands:

	sudo apt-get update
	sudo apt-get upgrade

Those two commands will update the Pi's software repository with the latest information then upgrade the existing code in the Raspbian image to their latest versions.

Next, you'll need to install some support libraries used to install the Adafruit's DHT22 library. In the same terminal window, execute the following command:

	sudo apt-get install build-essential python-dev

Now, download and install the Adafruit DHT Python library. Point the Pi's web browser to [https://github.com/adafruit/Adafruit_Python_DHT](https://github.com/adafruit/Adafruit_Python_DHT) and click the download link on the right side of the page. Unzip the archive to the folder where you downloaded it then execute the following command in a terminal window:

	python setup.py install


Project Software Installation
-----------------------------

Make a folder in the pi user's home folder. to do this, open a terminal window and enter the following command:
 
	mkdir folder_name

For example: 

	mkdir pi_weather_station

Copy the project files into the folder that was just created.

Configuration
=============

In order to upload weather data to the Weather Underground service, the application needs the station ID and station access key you created earlier in this setup process. Open the project's `config.py` in your editor of choice and populate the `STATION_ID` and `STATION_KEY` fields with the appropriate values from your Weather Underground Weather Station: 

	class Config:
    	# Weather Underground
    	STATION_ID = ""
    	STATION_KEY = ""

Refer to the Weather Underground [Personal Weather Station Network](https://www.wunderground.com/personal-weather-station/mypws) to access these values.

The main application file, `weather_station.py` has two configuration settings that control how the program operates. Open the file in your favorite text editor and look for the following line near the beginning of the file:

	# specifies how often to measure values from the DHT22 (in minutes)
	MEASUREMENT_INTERVAL = 10  # minutes

The `MEASUREMENT_INTERVAL` variable controls how often the application reads temperature measurements from the sensor. To change how often the application checks temperature values, change the value on the right of the equals sign on the second line.

If you’re testing the application and don’t want the weather data uploaded to Weather Underground until you're ready, change the value for `WEATHER_UPLOAD` to `True` (case matters, so it has to be `True`, not `true`):

	# Set to False when testing the code and/or hardware
	# Set to True to enable upload of weather data to Weather Underground
	WEATHER_UPLOAD = False

Testing the Application
=======================
  
To execute the data collection application, open a terminal window, navigate to the folder where you copied the project files and execute the following command: 

	python ./weather_station.py

The terminal window should quickly sprout the following output:

	########################################
	# Pi Weather Station (Simple Sensor)   #
	# By John M. Wargo (www.johnwargo.com) #
	########################################
	
	Initializing Weather Underground configuration
	Successfully read Weather Underground configuration values
	Station ID: YOUR_ID		
	Initialization complete!
 
If you see something like that, you're golden. If not, figure out what any error messages mean, fix things, then try again. At this point, the application will start collecting data and uploading it to the Weather Underground every 10 minutes on the 10 minute mark (unless you changed the app's configuration to make the application work differently).

Starting The Project's Application's Automatically
--------------------------------------------------

There are a few steps you must complete to configure the Raspberry Pi so it executes the project's python application on startup. If you don't already have a terminal window open, open one then navigate to the folder where you extracted the project files. Next, you need to make the project's bash script files executable by executing the following command:

    chmod +x start-station.sh
    
Next, you'll need to open the pi user's session autostart file using the following command:  

	sudo nano ~/.config/lxsession/LXDE-pi/autostart    

Add the following lines to the end (bottom) of the file:

	@lxterminal -e /home/pi/pi_weather/start-station.sh	

To save your changes, press ctrl-o then press the Enter key. Next, press ctrl-x to exit the nano application.
  
Reboot the Raspberry Pi. When it restarts, both python processes should execute in a terminal window as shown in Figure 4. 

![Weather Monitor Home Page](http://johnwargo.com/files/pi-weather-station-800.png)

Figure 1 - Raspberry Pi Weather Station

Revision History
================

None yet!

***

You can find information on many different topics on my [personal blog](http://www.johnwargo.com). Learn about all of my publications at [John Wargo Books](http://www.johnwargobooks.com). 
