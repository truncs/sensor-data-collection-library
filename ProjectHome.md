We recently added indoor features to Google Maps on Android. To determine a user’s location when they are indoors, we use a process similar to that of My Location for outdoor spaces but fine tuned for indoors. This process included working with business owners to collect publicly broadcast location information around their building, such as WiFi signal strength, GPS, and cell tower information.

This project contains code that was used to collect sensor data and wifi signal strengths for these indoor locations. In the spirit of transparency, we've posted the code here so that anyone can review it.  Some highlights:

**Sensors used:**
In SensorCollector.java, the method "getSensorList()" shows the phone sensors that we log, if equipped: the gyroscope, the accelerometer, the compass, and the raw magnetic sensor.  The calling method, "startCollectors()", additionally logs latitude/longitude inferences from the publicly available Android "network provider", and also the GPS radio.

**No SSID text logging:**
In TextFileSensorLog.logWifiScan(), you'll see that the ScanResult.SSID field is not referenced in the data string that is logged to the file.  That means that SSIDs such as "linksys", "netgear204", and "my house" are never logged by this application.

**SSID opt-out behavior:**
In TextFileSensorLog.shouldLog(), the SSID is compared to the opt-out suffix (underscore followed by "nomap"), so that wireless access point owners can block their APs from ever being logged. See http://googleblog.blogspot.com/2011/11/greater-choice-for-wireless-access.html for additional information.

**Scope of WiFi data collected:**
Because this package uses only the Android "ScanResult" object for gathering information about 802.11 access points, it should be clear from the code that network payload data is not collected or logged.  This means that even when signal strength data is logged, the contents of packets are not.