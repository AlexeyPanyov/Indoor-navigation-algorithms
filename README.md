#Indoor-navigation-algorithms
============================
<a href="http://navigine.com"><img src="https://navigine.com/wp-content/themes/flat-theme/assets/images/img/w_menuLogo.svg" align="left" height="60" width="180" hspace="10" vspace="5"></a>

This is the public repository of the Navigine company. The company develops various navigation algorithms with main focus on the indoor navigation.

This repository contains the following source codes:

- algorithms developed by Navigine engineers
- demo applications for infrastructure development and testing
- Navigine's public documentation

[TOC]

##Algorithms

The following sections describe the Navigine's algorithms, residing in this repository.

###Trilateration

Trilateration algorithm implements a method of determing coordinates of a device using measurements of pseudo distances to all visible beacons or other signal transmitters such as Wi-Fi routers. 

For navigation purposes you may also operate with RSSI readings. 

We use this algorithm for indoor navigation along with the distance/RSSI measurements from Bluetooth LE 4.0 Beacons.

The Thrilateration algorithm with all required functions and data structures is available as a class in the trilateration.h and beacon.h files.

Find examples of using the algorithm and filling in data structures in the unit test inside the test_trilateration.cpp file.

##Pedometer

Pedometer algorithm implements a method of manual radiomap measurement. In case of using this algorithm, you need to walk inside the target location and add marks to the map via an Android device with the Navigine's demo app installed.

The Pedometer algorithm with all required functions and data structures is available as a class in the pedometer.h file.

Find examples of using the algorithm and filling in data structures in the unit test inside the test_pedometer.cpp file.

##Demo Applications

Navigine's demo applications demonstrate capabilities of the Navigine's navigation algorithms, as well as enable you to work with the locations, sub-locations, and maps, residing in your project's cloud folder.

All source files of the Navigine's demo applications are available in ./Indoor-navigation-algorithms/demo apps/

Make sure the target device supports Bluetooth LE 4.0.

##Documentation

Navigine's public documentation is available in the RST and HTML formats under the ./docs folder.
You can also find the documenation at the Readthedocs.org service via the following direct link: http://navigine-docs.readthedocs.org/en/latest/