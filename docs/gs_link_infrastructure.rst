Link Infrastructure Components to the Map
=========================================

Once your `infrastructure is deployed <1_infrastructure_setup.html>`__ and the `maps (locations) are implemented <1_creating_maps.html>`__, you need to interconnect them.

Two methods of linking the actual infrastructure to the map exist:

- Trilateration - a method of determining coordinates of a device using measurements of pseudo distances to all visible beacons or signal transmitters. For navigation purposes you may also operate with RSSI readings. We use this algorithm for indoor navigation along with the distance/RSSI measurements from Bluetooth LE 4.0 Beacons.
- Pedometer - an algorithm that also supports the Wi-Fi-only setup

Trilateration Algorithm
-----------------------

In case of using the trilateration algorithm, you need an iBeacon infrastructure deployed at the target location. To implement the algorithm, you need to

1. Specify the beacons' locations at the map in the `Navigine CMS's Locations tab <http://client.navigine.com/maps>`__.
2. 

Pedometer Algorithm (Radiomap Measurement)
------------------------------------------
