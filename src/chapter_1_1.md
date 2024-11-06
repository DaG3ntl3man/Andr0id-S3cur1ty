## Android Architecture 
> Android is a Linux based OS

### Noteworthy API versions:<br>
* Android 4.2 (API level 16) in November 2012 (introduction of SELinux)
* Android 4.3 (API level 18) in July 2013 (SELinux became enabled by default)
* Android 4.4 (API level 19) in October 2013 (several new APIs and ART introduced)
* Android 5.0 (API level 21) in November 2014 (ART used by default and many other features added)
* Android 6.0 (API level 23) in October 2015 (many new features and improvements, including granting;
detailed permissions setup at runtime rather than all or nothing during installation)
* Android 7.0 (API level 24-25) in August 2016 (new JIT compiler on ART)
* Android 8.0 (API level 26-27) in August 2017 (a lot of security improvements) 
* Android 9 (API level 28) in August 2018 (restriction of background usage of mic or camera,
introduction of lockdown mode, default HTTPS for all apps)
* Android 10 (API level 29) in September 2019 (notification bubbles, project Mainline)

### SELinux

Security-Enhanced Linux (SELinux) uses a Mandatory Access Control (MAC) system to further lock down which processes should have access to which resources. Each resource is given a label in the form of user:role:type:mls_level which defines which users are able to execute which types of actions on it. 

For example, one process may only be able to read a file, while another process may be able to edit or delete the file. This way, by working on a least-privilege principle, vulnerable processes are more difficult to exploit via privilege escalation or lateral movement.

> A well know attack path we will follow in general when attacking or researching android security is running the exploits on emulators which are API level 29 or on API level 22, to see the differences in terms of security.

<br>

---

### The Android software stack
![The Android software stack](./The%20Android%20software%20stack.png)

* ### <span style="color:#FF5607">Linux Kernel</span>
  * Android devices have to support multiple CPU types (ARM, SoC(System on Chips))- 32 and 64-bit
  
  * Applications are explicitly told which version of the Android Runtime/API version to run on

    * Android Manifest - minSDK Version *(SDK versions determine what versions of the Android OS or the API, our application can run on)*
  
    * The higher, the better, BUT developers want to include the most customers possible!
  
  * Lower SDK/Android Versions can have more security vulnerabilities and can allow for different types of attacks on only some devices/emulators
  
  * Access to the physical components of the Linux device are controlled by drivers
    * Bluetooth, Camera, Microphone, Wifi, LTE, Display, etc.

* ###  <span style="color:#00C9CC">Hardware Abstraction Layer (HAL) →</span> 
  > This layer allows an application irrespective of the device manufacturer, irrespective of components inside of it, to access things in generic categories like the camera, microphone, bluetooth, location, touch drivers, etc,.

  * Abstraction layer that allows applications to access hardware components irrespective of the device manufacturer or type
    * Allows applications to simply access "the camera", "the microphone", "the location (GPS)", Bluetooth, the touch drivers without needing specific built-in drivers or manufacturer details
  
  * New/Upcoming HAL Types:
    * Automotive - Android Auto, Apple Car Play
  
    * IoT
      * Fitness Watches
      * Fitness Devices (peloton, etc.)
      * IoT Home devices (Alexa, Google Home, etc.)
  
    * Gaming Peripherals
  
* ### <span style="color:#A87BD9">Native C</span> vs <span style="color:#FDBE00">Android Runtime</span>

* C and C++ is the device's native language
  * Does not require a VM
  * Webkit - built in web browser for the app (iFrame)
  * OpenMAX AL, OpenGL ES - UI frameworks for 2D/3D models
  
* Java is often easier to code in, meaning that most developers prefer this
  * Older apps built in Java, newer ones often built in Kotlin
  * In 2019 Google announced Kotlin will be the preferred coding standard
  * Kotlin is utilized by approximately 60% of app developers for Android
  * https://developer.android.com/kotlin

* ### <span style="color:#8BC34A">Java API Framework</span>

  * This is often what allows you app to interact with other apps, and also the device itself as defined in the Android app
  
  * Content Providers - a way of sharing data to other applications via a specific directory (if exported)
    * content://<app-URI>/directory → `This is looked for in our Static analysis to see if our application is sharing any data`
  
  * View System - utilized for making the App's UI and normalizing it
  
  * Managers:
    * Manages and runs things like:
      * Notifications - app pop-ups for reminders
      * Telephony - receiving/making calls, and opening contacts
      * Package - managing the application package, looking for updates, ensuring integrity
      * Location - manager of location services
  
* ### <span style="color:#00BCD4">System Applications Layer</span>
  * The pre-installed applications on the Android Phone
    * Contacts, Phone, System Settings, Text Message Apps, Camera, System Monitor, Calendar
    * Google Applications
    * Vendor Specific Apps
    * Great thing about Android is you can always set a new default app to replace the vendor-supplied or system app 

