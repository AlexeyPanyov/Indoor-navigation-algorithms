#Indoor-navigation-algorithms
============================
<a href="http://navigine.com"><img src="http://navigine.com//images/main_logo.png" align="left" hspace="10" vspace="6"></a>

This is a public repository of a Navigine company that develops different kinds of a 
navigation algorithms with the main focus on indoor navigation. Here we will step by step
publish source code of our algorithms getting started from trilateration.


##Trilateration
This is a method of determing coordinates of a device using measurements of pseudo distances
to all visible beacons or signal transmitters. For navigation purposes you may also operate with
RSSI readings. We use this algorithm for indoor navigation along with the distance/RSSI measurements
from Bluetooth LE 4.0 Beacons.

Algorithm with all required functions and data structures is presented as class in
trilateration.h, beacon.h files.

Example of the algorithm usage and filling of data structures you can find in a unit test for
this source code in the test_trilateration.cpp file.
