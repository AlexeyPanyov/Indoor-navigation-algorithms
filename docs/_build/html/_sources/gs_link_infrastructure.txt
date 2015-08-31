Link Infrastructure Components to the Map
=========================================

Once your `infrastructure is deployed <gs_setup_infrastructure.html>`__ and the `maps (locations) are implemented <gs_create_map.html>`__, you need to interconnect them.

Two methods of linking the actual infrastructure to the map exist:

- Trilateration - a method of determining coordinates of a device using measurements of pseudo distances to all visible beacons or signal transmitters.
- Pedometer - implements a method of manual radiomap measurement. 

Trilateration Algorithm
-----------------------

In case of using the Trilateration algorithm, you use an iBeacon infrastructure deployed at the target location. To use the algorithm, you need to specify the beacons' locations at the map in the `Navigine CMS's Locations tab <http://client.navigine.com/maps>`__.

For navigation purposes you may also operate with RSSI readings. We use this algorithm for indoor navigation along with the distance/RSSI measurements from Bluetooth LE 4.0 Beacons.

Find the algorith source code and unit tests at `sub-folder in the Navigine's GitHub <https://github.com/AlexeyPanyov/Indoor-navigation-algorithms/tree/master/navigation/trilateteration>`__.

Pedometer Algorithm (Radiomap Measurement)
------------------------------------------

In case of using the Pedometer algorithm, you can use both the Wi-Fi and iBeacon infrastructure setups at the target location. In case of using this algorithm, you need to walk inside the target location and add marks to the map via an Android device with the Navigine's demo app installed.

Find the algorith source code and unit tests at `sub-folder in the Navigine's GitHub <https://github.com/AlexeyPanyov/Indoor-navigation-algorithms/tree/master/navigation/pedometer>`__.