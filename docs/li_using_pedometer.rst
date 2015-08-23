Using Pedometer Algorithm
=========================

The Pedometer algorithm is actually measuring the target location's radiomap on foot via an Android device with Navigine's Demo app installed.

By this moment you should have `infrastructure deployed <gs_setup_infrastructure.html>`__ and `maps (locations) implemented <gs_create_map.html>`__,.

Installing the Demo App
-----------------------

You need the Android Demo App installed on your Android device first. To get it,

#. Install the `Navigine's Demo App for Android<http://client.navigine.com/manual/Navigine-debug.apk(v.).apk>`__ from the Navigine's website first. You can also get the application and app sources from the `Navigine's GitHub repository <https://github.com/AlexeyPanyov/Indoor-navigation-algorithms/tree/master/demo%20apps/Android>`__.
#. Switch on the Android device's Wi-Fi and Bluetooth modules.

Installing the Map
------------------

Prior to measuring the radio map, you need to download and install the map you've `created in the Navigine's CMS beforehand <gs_create_map.html>`__.

#. Run the Demo App.
#. Tab on the **Measuring mode**. 

|image0|

#. Select **Load Content**.

|image3|

#. In the **Load Content** dialog, put checkmark next to **Forced load**, enter name of your `location <cm_creating_location.html>`__,and wait for the download to complete.

|image2|

#. Go back to the **Settings** menu and select **Load map**. 

|image1|

#. After your file manager opens, find the folder with your location's name, and open the map ZIP archive. Your location's map should appear.

|image5|

Measuring the Radio Map 
-----------------------

To proceed with the Pedometer algorithm or in different words to measure the radio map of the target location, you need to do the following:

#. In the Demo App, specify your current physical location via the aim mark symbol on the map.

|image6|

#. Tap the **Function** button, and tab **Create point**. In the **Set measuring object** dialog tap **Ok**.

|image7|

#. Wait for the % indicator in the top part of the screen to reach 100% for the currently measured reference point, which should take around one minute. 100% means that the application gathered enough information for effective navigation.

#. Repeat the measurement process until you create enough reference points <TBD> how much is enough?

Measurement Recommendations
---------------------------

<TBD> Need to take a closer look at the app. The RU version of the reference doc contains confusing screenshots.
 

.. |image0| image:: _static/android_app_measuring_mode.png
.. |image1| image:: _static/android_app_map.png
.. |image2| image:: _static/android_app_forced.png
.. |image3| image:: _static/android_app_content.png
.. |image4| image:: _static/navigine_apk_menu.png
.. |image5| image:: _static/android_app_map_loaded.png
.. |image6| image:: _static/android_app_aim_mark.png
.. |image7| image:: _static/android_app_set_object.png