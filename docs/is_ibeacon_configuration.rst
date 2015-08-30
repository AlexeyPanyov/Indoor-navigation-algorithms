Setting Up iBeacon Infrastructure
=================================

The following section provides guidelines on setting up the iBeacon infrastructure for your indoor navigation app.

Prior to installing beacons in the target locations, you need to configure them. Use the NavigineSetting or kontact.io application for beacon configuration.

.. raw:: html

   <div class="note">

Make sure that the beacons are in iBeacon mode and the signal transmit power is set to\ `` -4dbm``.

.. raw:: html

   </div>

By default the kontact.io beacons are set to travel mode and have minimal transmit power for power saving purposes. In the beacon transmit power options choose the 5th value, which corresponds to ``-4dbm``.

Configuring with NavigineSetting
--------------------------------

One way to configure the beacons is via the NavigineSetting application on iOS devices. Consider using this approach in case you want to configure multiple beacons at once.

#. Launch the NavigineSetting app on your iOS device.
#. In the first screen of the applications, specify the following parameters:
	
	* **TxPower** - specify the power of a beacon's transmit signal. Values: from 1 to 7, where 1 corresponds to 5 meters, and 7 - to 70 meters. The higher the specified value is, the more power consumes the beacon.
	* **AdvertisingInterval** - set up the periodicity of a beacon's advertising package transmission. Values: from 20 ms to 1000 ms.
	* **Api Key** - specify Api Key for your iBeacons. The Key is available in your account at `http://panel.kontakt.io <http://panel.kontakt.io>`__. Take into account that if you specify an incorrect Api Key, the application locks beacons for 20 minutes.

#. Tap **CONFIGURE** to see the list of beacons that are visible or already prepared.
#. Tap **START** to run the automatic configuration procedure with the settings specified. You should see log in the console under the list of beacons. In case of failure, try again via tapping the **START** button again. Make sure no beacons are locked for configuration.

Consider configuring no more than 20 beacons at once.

Configuring with kontakt.io
---------------------------

One more way to configure your iBbeacons is via the kontakt.io application. Consider this approach if you want to configure beacons' advanced settings. This approach let's you configure each your beacons one-by-one.

With this approach you need Internet connection.

#. Launch the kontakt.io app on your iOS device.
#. Switch to the **Settings** tab and enter your kontakt.io login information.
#. In the **Settings** tab, scroll down to the **Administrator** section, and tap **Enter administrator mode**.
#. In the **Beacons** tab, find the ID of the beacon that you want configure, and tap it to connect. Beacon's ID is written on it's back.
#. Specify the following parameters for each of the beacons you need to configure:

	* **Proximity UUID** - the kontakt.io beacon identifier
	* **major** - beacon major identifier
	* **minor** - beacon minor identifier
	* **TxPower** - power of a beacon's transmit signal. Values: from 1 to 7, where 1 corresponds to 5 meters, and 7 - to 70 meters. The higher the specified value is, the more power consumes the beacon.
	* **Advertising Time Interval** - periodicity of a beacon's advertising package transmission. Values: from 20 ms to 1000 ms.
	* **Model Name** - beacon identifier as bluetooth device.

Deploying Beacons
-----------------

Take into account the following golden rules during the beacon
installation procedure:

-  Use the beacons only in the areas where navigation is required.
-  Install the beacons above the head level at the height between 2 and
   4 meters. The best practice is to fasten beacons on the ceiling.
-  In the case when the recommended beacon installation place is
   unavailable (for example, the ceiling is too high), you can attach
   the beacons to the walls.
-  Use 1 beacon for the locations smaller than 25 square meters.
-  Place the beacons evenly across the location, still do not put them
   on the same direct line.
-  The more beacons you use, the higher the accuracy level is. Consider
   using 8-15 beacons per 1000 square meters.
-  DO NOT put beacons behind metal objects and/or any other obstacles,
   otherwise the beacon's usefulness will be tending to zero.
-  Make sure the beacons are inaccessible so that they cannot be moved
   by unauthorized people.

The following figure demonstrates the optimal settlement of 18 beacons
for a single facility with multiple rooms inside.

|image0|

.. |image0| image:: _static/beacons_setup.png
.. |image1| image:: _static/

