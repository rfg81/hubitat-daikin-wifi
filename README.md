# Hubitat driver - Daikin WiFi Split System

Add driver code from daikin-wifi-split-system-hubitat.groovy to hub and add new device type Daikin WiFi Split System Hubitat.

Settings should be self-explanatory. Be careful using refresh interval 1 minute, its barely supported.

To create a temp sensor for outside temp, add a virtual temp sensor and update its value in rulemachine using the "outsideTemp" attribute value *changed* event and custom action setTemperature to string %value%.

See daikinapi.md for info about the AC api calls you can make if you want to modify the driver.
