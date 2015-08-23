Set Up the Infrastructure
=========================
In order to start implementing indoor navigation via the Navigine's products, you need to decide on the infrastructure components that you want to use.
Two basic approaches of deploying infrastructure exist:

- iBeacon Approach
- Wi-Fi Approach

Select the most suitable approach according to the descriptions given in the rest of the "Set Up the Infrastructure" section. You can also use a combination of the infrastructure components.
For complete guidelines on configuring the infrastructure, refer to `Setting Up the Infrastructure <1_infrastructure_setup.html>`__.

iBeacon Approach
----------------
Use the iBeacon approach for setting up the infrastructure if you aim to implement indoor navigation from scratch. In case if you don't have the Wi-Fi infrastructure inside your location, this is the best choice for you, as

- Beacons cost way less then Wi-Fi routers.
- Beacons are easier to install, as they are smaller and work on batteries. It takes a year or two for an iBeacon to run out of power. Then you can simply replace the battery.
- iOS devices do not support Wi-Fi network scanning, which means the beacons are the only choice for you if you want to create navigation app for public use.
- Configuring the infrastructure of multiple beacons is easier and takes less time, if compared with the Wi-Fi approach.

To set up the infrastructure with iBeacons, simply install about 8-15 beacons per 1000 square meters. For more information about configuring the beacon infrastructure, refer to `Configuring iBeacons <is_ibeacon_configuration.html>`__.

Wi-Fi Approach
--------------
Consider using the Wi-Fi approach only in case you

- Already have a network of interconnected Wi-Fi routers deployed. 
- Target using Android devices only, as iOS devices do not support Wi-Fi network scanning.

Even if the aforementioned parameters match your aims, we still recommend you to consider using the iBeacon approach as it has a wider future and is way easier to configure. Anyway, for more information on configuring an existing Wi-Fi network for indoor navigation, see `Configuring Wi-Fi Infrastructure <is_wifi_configuration.html>`__.
