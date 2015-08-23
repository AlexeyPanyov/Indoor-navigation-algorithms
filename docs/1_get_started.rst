Getting Started
===============

The following sections overview the main workflow of implementing indoor navigation from scratch using the Navigine's Indoor Locations Services.

Contents:

.. toctree::

	gs_setup_infrastructure
	gs_create_map
	gs_link_infrastructure
	gs_develop_app
	
To get started with Navigine Indoor Location Services, you need to acquire infrastructure components first. The following table provides brief information about the components that you need to set up your location's infrastructure.

+----------------------------+-------------------------------------------------------------------------------------------+
| .. rubric:: Component      | .. rubric:: Description                                                                   |
+----------------------------+-------------------------------------------------------------------------------------------+
| Navigine Account           | | `Register <http://client.navigine.com/register>`__ a Navigine Account to get access to  |
|                            | | Navigine Indoor Location Services.                                                      |
+----------------------------+-------------------------------------------------------------------------------------------+
| Access to the Location     | | You will need physical access to the location once you are ready to                     | 
|                            | | deploy the infrastructure components to their places.                                   |
+----------------------------+-------------------------------------------------------------------------------------------+
| Location's Dimensions      | | The building parameters are required at the stage of measuring                          |
|                            | | sub-locations. The more precise this data is, the better is                             |
|                            | | the navigation accuracy you get.                                                        |
+----------------------------+-------------------------------------------------------------------------------------------+
| BLE Beacons                | | You need iBeacon-compatible BLE beacons to set up your target location's                |
|                            | | infrastructure. 8-15 beacons should be enough for a location of                         |
|                            | | 1000 square meters.                                                                     |
+----------------------------+-------------------------------------------------------------------------------------------+
| Wi-Fi Routers (optional)   | | As an alternative to using BLE beacons for indoor navigation, you can                   |
|                            | | use a set of Wi-Fi routers, which rather brings less accuracy and                       |
|                            | | supports Android devices only.                                                          |
+----------------------------+-------------------------------------------------------------------------------------------+
| Android Device             | | Which might be an Android smartphone or tablet with Bluetooth 4.0. Make sure            |
|                            | | that the device supports Bluetooth LE 4.0 and iBeacon protocol.                         |
+----------------------------+-------------------------------------------------------------------------------------------+
| Linux/Windows Machine      | | You also need a Linux or Windows OS machine for integrating the                         |
|                            | | Navigine SDK into your navigations app for Android devices.                             |
+----------------------------+-------------------------------------------------------------------------------------------+
| Mac                        | | To integrate the Navigine SDK into iOS apps, you definitely need                        |
|                            | | a Mac OS machine and the corresponding developer's account.                             |
+----------------------------+-------------------------------------------------------------------------------------------+