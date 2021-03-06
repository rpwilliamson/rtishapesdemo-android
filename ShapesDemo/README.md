# RTI Shapes Demo For Android

Installation and Setup
-----------------------
These instructions are for a Windows development host.

1. You need to have Eclipse and the Android ADT plugin installed. The Android ADT plugin depends on the Android SDK. You can:

    1. Install the Eclipse ADT Bundle, which has Eclipse with ADT in a single installer.
     
      Use this if you don't have Eclipse on your computer yet. Look for the Download the [SDK ADT Bundle for Windows link](http://developer.android.com/sdk/index.html) and follow [these instructions](http://developer.android.com/sdk/installing/bundle.html).
    
    2. Install the ADT plugin and Android SDK separately.
    
      Use this if you already have an Eclipse installation that you want to use. Look for the ["Use an existing IDE" link](http://developer.android.com/sdk/index.html) and follow [these instructions](http://developer.android.com/sdk/installing/index.html).


2. Use the Android SDK Manager to install the Android platforms for which you want to build (e.g., Android 4.1.2, Android 4.2.2, Android 4.3, etc.).  Make use that you run the Android SDK Manager with Adminstrator Priviledges if on Windows 7.

3. Install the Android NDK (native compiler for JNI).

  Go to http://developer.android.com/tools/sdk/ndk/index.html and follow [these instructions](http://tools.android.com/recent/usingthendkplugin). Note that you don't need the legacy NDK toolchains.

4. Install RTI Connext DDS Micro
   
   RTI Connext DDS Micro provides a small-footprint modular messaging solution for resource-limited devices that have   
   minimal memory, Flash, CPU power or no operating system. For detail http://www.rti.com/products/micro/.

   To get a copy of RTI Connext DDS Micro, contact your local RTI representative   
   (http://www.rti.com/company/contact/index.html) or email at info@rti.com to find out your local representative.
  
   To install RTI Connext DDS Micro, you need to get a set of Host support source files and target ARCH libraries. 

5. Open ShapesDemo project in Eclipse
   
    1. Start Eclipse
    2. Use File -> Import -> Existing Android Code into Workspace
    3. Select the directory where you have installed the ShapesDemo project

6. Edit `jni/Android.mk`
  You should modify the file to reflect the directory where you installed RTI Connext DDS Micro.  
  You may also want to use a different architecture for the Connext DDS Micro libraries.
  
   For example on Windows platform,

        RTI_CONNEXT_MICRO_HOME := C:\RTI\rti_connext_micro.2.3
        PLATFORM_ARCH := armv7leAndroidR8Dgcc4.7

    If you have NDK8 or earlier, please upgrade to NDK9 if you can.  Otherwise, look in the 
    Troubleshooting section below for alternate instructions.


7. Build Project
Use Project -> Build Project. The Output in the (CDT Build) Console should look like:

        11:59:30 **** Build of configuration Default for project ShapesDemo ****
        "C:\\Apps\\android-ndk-r9\\ndk-build.cmd" NDK_DEBUG=1 all 
        Android NDK: WARNING: APP_PLATFORM android-18 is larger than android:minSdkVersion 14 in ./AndroidManifest.xml    
        Gdbserver      : [arm-linux-androideabi-4.6] libs/armeabi/gdbserver
        Gdbsetup       : libs/armeabi/gdb.setup
        "Compile thumb : rti_android_shapesdemo <= RTIDDSWrapper.c
        "Compile thumb : rti_android_shapesdemo <= ShapeType.c
        "Compile thumb : rti_android_shapesdemo <= ShapeTypePlugin.c
        "Compile thumb : rti_android_shapesdemo <= ShapeTypeSupport.c
        SharedLibrary  : librti_android_shapesdemo.so
        Install        : librti_android_shapesdemo.so => libs/armeabi/librti_android_shapesdemo.so


## Getting Started

1. Installing `ShapesDemo.apk` on a real Android Device
    1. Attach your Device via USB
    
    2. Enable your Device to install apps from unknown sources. 
      On Device, use Settings -> Security -> Unknown Sources, check the checkbox
        
    3. Optional, enable your Device to allow debugging over USB
      On Device, use Settings -> Developer options -> USB debugging, check the checkbox
      NOTE: You may need to enable the Developer options menu option on some devices.
      e.g., on Nexus 7, use Settings -> About tablet -> Build number, select Build number 10 times (or so)
        
    4. Installing the .apk
    
        1. Using Eclipse
          
          Use Run -> Run or Debug. This will automatically build the project (so project must successfully build).
     
          The dialog, Android Device Chooser, should popup and show your device. 
          If your device is not shown as "online", you probably need to go to the Device itself 
          which has popped up a dialog asking you for permission to allow your computer to connect 
          via USB for debugging.  Confirm (you may want to select the checkbox so that you never 
          have to confirm again for your computer).
          Select your device.
          The `ShapesDemo.apk` will automatically be installed and started.

        2. Using the ShapesDemo.apk directly
        
          If you have built the ShapesDemo via Eclipse or you want use the .apk that came prebuilt with 
          the installation located in `bin/ShapesDemo.apk`. You install the `ShapesDemo.apk` by either
          * Copying  `ShapesDemo.apk` to your Device's filesystem mounted via USB.
           Use a file manager app (which you'll need to install from Google Play, many free ones available)
           to select and install the .apk on your Device.
           Select the installed ShapesDemo icon to run.
          * or from a command line, running: 
        
                  adb install ShapesDemo.apk
        
            See http://developer.android.com/tools/help/adb.html
 
2. Using a Device Emulator
 
    1. Create a Virtual Device via Eclipse
    Use Window -> Android Virtual Device Manager -> New
    
    2. In Eclipse, use Run -> Run or Debug
    This will automatically build the project (so project must successfully build).
    You may get dialog, Android Device Chooser, if you have a real Device connected via USB.
    Select your virtual device, or use Launch a new Android Virtual Device.
    The ShapesDemo.apk will automatically be installed and started.

## Running

1. ShapesDemo is configured to work using Android's WiFi network interface, 
so your device should be connected to a WiFi network.
    
    
2. Run ShapesDemo.exe on your computer. Your computer should be on the same 
WiFi network (on the same subnet).
   
   
3. By default, multicast discovery is used.

    1. Windows 7 users should make sure that their WiFi's network location is set to either
    Home, Work, or Domain.  If the network location is set to Public, the Windows 7 firewall may
    prevent full discovery...so in that case, either change the network location or disable the
    firewall.
    
    2. If the computer and Android device on are different subnets or multicast is not
    supported on the WiFi network, then you should use `NDDS_DISCOVERY_PEERS` environment
    variable to set the IP addresses of the computer and/or device as the initial peers for
    discovery.
    The Android device's IP address on WiFi can be found using Settings -> WiFi -> `< whatever WiFi network is connected >`.
    You can set the computer's IP address in the Peer List for the ShapesDemo application on the
    Android device using the "..." -> Settings -> DDS Peer List preferences menu option.


4. In ShapesDemo.exe (on your computer), you MUST change the Liveliness Kind of your 
shape publishers (datawriters) to `MANUAL_BY_TOPIC` for the Android subscriber to receive the data.

5. Notes:
ShapesDemo on Android is configured by default to publish/subscribe using `BEST_EFFORT_RELIABILITY` 
and `MANUAL_BY_TOPIC` Liveliness kind.

6. See Troubleshooting section below if you can't get the two ShapesDemos to exchange data.
    
File Description
-----------------
### Java ###
 
* `src/com/rti/android/shapesdemo/ShapesDemo.java` — The main Activity for ShapesDemo. Handles all of the button actions and has a periodic event to redraw the canvas.
 
* `src/com/rti/android/shapesdemo/DrawView.java` — Class that manages shapes that have been published and subscribed to as well as the canvas on which the shapes are drawn.
 
* `src/com/rti/android/shapesdemo/ShapesObject.java` — Class that implements the behavior of an shape.

* `src/com/rti/android/shapesdemo/ShapesDDS.java` — Class that manages how RTI Connext DDS is used for the ShapesDemo.
 
* `src/com/rti/android/shapesdemo/ShapesSetting.java` — Android Activity used to create "Settings" option.

* `src/com/rti/android/shapesdemo/util/Vector2d.java` — A mimimalistic implementation of a 2-D vector class with some helpful functions.
 
* `src/com/rti/android/shapesdemo/util/EditTextPreferenceWithValue.java` — Useful class so that when a preference (setting/option) is changed by the user, the new value is automatically reflected in the preferences menu.
 
 
### JNI - C ###

* `jni/RTIDDSWrapper.c` — JNI interface using RTI Connext DDS Micro's C API.

* `jni/ShapeType.xxx` — Support files for data type generate with "rtiddsgen -language microC ShapeType.idl" using rtiddsgen distributed with RTI Connext DDS Micro.
 
 
### Android Application Files ###

* `AndroidManifest.xml` — Main definition file for ShapesDemo app including which Activity to start on startup

* `res/layout/activity_shapes_demo.xml` — main layout of the graphics elements of the ShapesDemo application

* `res/layout/preferences_with_value.xml` — utility used to define a layout where the current value of a preference are displayed on the same line as the preference.

* `res/menu/menu.xml` — defines the layout of the Android options menu

* `res/xml/settings.xml` — defines the preferences that can be set by the Settings option submen


## Troubleshooting
  
1. ShapesDemo can't publish or subscribe to host?

    1. Make sure that the Android device and your host are on the same WiFi subnet.  If not, 
    you'll have to configure the `NDDS_DISCOVERY_PEERS` on your host or DDS Peer List in the settings
    menu of ShapesDemo to the actual IP addresses.
    
    2. On Windows 7 (and Linux) check your firewall, you may need to turn it off.
    
    3. Get data to Android but not from Android, probably because your host computer has too
    many active NIFs.  Disable all but your WiFi NIF and rerun RTI Shapes Demo.exe.


2. Build problems?

    1. Make sure that Eclipse knows where your NDK is located
    Set Eclipse -> Window -> Preferences -> Android -> NDK to the location of your Android NDK installation
    
    2. NDK R8 users
    If you can't upgrade to NDK 9...then unfortunately, NDK 8 has a problem in which the paths set in _jni/Android.mk_ must be relative to the location of the _Android.mk_ file. 
    So, in _jni/Android.mk_, you will need to set `RTI_CONNEXT_MICRO_HOME` differently...
    For example, you have the following installation directories
    
            C:\Rti\rti_me.2.2.3
            C:\Users\demo\workspace\ShapesDemo\jni\Android.mk
            
      In this case, you would have to use
      
          RTI_CONNEXT_MICRO_HOME := ..\..\..\..\..\Rti\rti_me.2.2.3
        
      AND, unfortunately, you have to modify
    
          LOCAL_C_INCLUDES := $(RTI_CONNEXT_MICRO_HOME)\include\rti_me $(RTI_CONNEXT_MICRO_HOME)\include
    
      to be
   
          LOCAL_C_INCLUDES := C:\Rti\rti_me.2.2.3\include\rti_me C:\Rti\rti_me.2.2.3\include





 


