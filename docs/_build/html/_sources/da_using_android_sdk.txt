Using Navigine SDK for Android
==============================

The following is the Navigine's SDK for Android systmes API Reference.

Class NavigationThread
------------------------

NavigationThread class is used for device navigation using WIFI, Bluetooth and sensors measurements. NavigationThread class works in a separate thread. NavigationThread have two different states: the IDLE state and the WORKING state. Unlike the WORKING state, in the IDLE state NavigationThread doesn't perform navigation of the device(s) and scanning of WIFI/Bluetooth/Sensors. IDLE state can be used for initializing navigation parameters (i.e. loading a location map, setting up navigation frequency, navigation logs, etc.). Initially NavigationThread is in the IDLE state.

**Prototype:**

.. code-block:: Java
   
   public class NavigationThread extends Thread

Constructor
^^^^^^^^^^^

Creates a NavigationThread. Initially NavigationThread is in the IDLE state.

**Prototype:**

.. code-block:: Java
   
   NavigationThread(Context context) throws AndroidException

**Parameters:**

* *context* — application context.

AndroidException is thrown if Android SDK API level is too low (should be >= 18) or if navigation is not possible on the device.

Function Terminate
^^^^^^^^^^^^^^^^^^

Function tells NavigationThread to stop. After calling the terminate function the caller must join with NavigationThread (wait until NavigationThread stops).

**Prototype:**

.. code-block:: Java
   
   public void terminate()

Function Idle
^^^^^^^^^^^^^

Function tells NavigationThread to go to the IDLE state.

**Prototype:**

.. code-block:: Java
   
   public void idle()

Function Work
^^^^^^^^^^^^^

Function tells NavigationThread to go to the WORKING state.

**Prototype:**

.. code-block:: Java
   
   public void work()

Function getDeviceList
^^^^^^^^^^^^^^^^^^^^^^

Function is used to determine device information, including the following:

* device identifier and type
* device location and sub-location
* device coordinates within the sub-location
* device orientation

**Prototype:**

.. code-block:: Java
   
   public List<DeviceInfo> getDeviceList()

**Return value:**

List, containing device info objects (see DeviceInfo class).

Function getErrorCode
^^^^^^^^^^^^^^^^^^^^^

Function is used to get navigation errors. 

**Prototype:**

.. code-block:: Java
   
   public int getErrorCode()

**Return value:**

Integer value – navigation error code. Zero value means no navigation error.

Functions getFrequency/setFrequency
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Functions are used for determine and specify the current navigation frequency. The greater frequency leads to the smoother device movement. The default navigation frequency is 10. 

**Prototype:**

.. code-block:: Java
   
   public int getFrequency()
   public void setFrequency(int freq)

**Parameters:**

* *freq* — the desired navigation frequency (must be in interval [1, 100]).

**Return value:**

The current navigation update frequency rate (in Hz).

Function loadArchive
^^^^^^^^^^^^^^^^^^^^

Function is used for loading a location archive to the Navigation library.

**Prototype:**

.. code-block:: Java
   
   public boolean loadArchive(String filename)

**Parameters:**

* *filename* — absolute path to the location archive file.

**Return value:**

* *true*, if archive was loaded successfully;
* *false*, if archive can't be loaded (e.g. invalid archive file).

Function getArchivePath
^^^^^^^^^^^^^^^^^^^^^^^

Function is used to determine the absolute path to the current location archive loaded.

**Prototype:**

.. code-block:: Java
   
   public String getArchivePath()

**Return value:**

The absolute path of the current location archive file loaded or null if no location archive was loaded.

Function getLocation
^^^^^^^^^^^^^^^^^^^^

Function is used for obtaining the current location archive loaded.

**Prototype:**

.. code-block:: Java
   
   public Location getLocation()

**Return value:**

The location object (see class Location) of the current location archive loaded or null if no location archive was loaded.

Function makeRoute
^^^^^^^^^^^^^^^^^^

Function is used for building routes between the two location points, specified in the parameters. Route is a polyline represented as an array of location points. Location points can belong to different sublocations. For example, this function can be used for building the optimal route from the current device position to the specified position in the location.

**Prototype:**

.. code-block:: Java
   
   public LocationPoint[] makeRoute(LocationPoint P, LocationPoint Q)

**Parameters:**

* P – a source location point (see LocationPoint class);
* Q – a destination location point (see LocationPoint class).

**Return value:**

If route is successfully build, function returns the array of location points, starting from the source point P, ending in the destination point Q. If route can't be build function returns null.

Usage Example
^^^^^^^^^^^^^

.. code-block:: Java

   public class MainActivity extends Activity { NavigationThread
   mNavigation = null;
   
   @Override public void onCreate(Bundle savedInstanceState) {
   mNavigation = new NavigationThread(getApplicationContext());
   mNavigation.loadArchive(“map.zip”);
   
   mNavigation.work();
   
   ...
   
   }
   
   @Override public void onDestroy() {
   
   mNavigation.terminate();
   
   try {
   
   mNavigation.join(1000);
   
   } catch (Throwable e) {
   
   // Error: unable to join NavigationThread
   
   }
   
   ...
   
   }
   
   }
   
   super.onDestroy();
   
   
Class LocationLoader
--------------------

Class LocationLoader contains a list of static functions for downloading and uploading location archives in a non-blocking mode.

Function getLocationDir
^^^^^^^^^^^^^^^^^^^^^^^

When location data is downloaded from server, it is stored in some certain directory in the Android filesystem. Function getLocationDir is used to determine the absolute path of the specified location directory in the Android filesystem.

**Prototype:**

.. code-block:: Java
   
   public static String getLocationDir(Context context, String location)

**Parameters:**

* *context* — application/activity/service context;
* *location* — location name, empty string or null value.

**Return value:**

The absolute path (in the Android filesystem) of the specified location directory. In case of location parameter is an empty string or a null value, function returns the absolute path of the parent directory, in which all location directories are stored.

Function getLocationFile
^^^^^^^^^^^^^^^^^^^^^^^^

When location data is downloaded from server, it is stored in some certain ZIP-file in the Android filesystem. Function getLocationFile is used to determine the absolute path of the specified location ZIP-file in the Android filesystem.

**Prototype:**

.. code-block:: Java
   
   public static String getLocationFile(Context context, String location)

**Parameters:**

* *context* — application/activity/service context;
* *location* — location name.

**Return value:**

The absolute path (in the Android filesystem) of the specified location ZIP-file.

Function getLocalVersion
^^^^^^^^^^^^^^^^^^^^^^^^

When location ZIP-archive is downloaded from server, its version is stored in the archive. Function getLocalVersion can be used to obtain the version of the downloaded location archive.

**Prototype:**

.. code-block:: Java
   
   public static String getLocalVersion(Context context, String location)

**Parameters:**

* *context* — application/activity/service context;
* *location* — location name.

**Return value:**

If location archive with the specified name is found (in the corresponding path in the Android filesystem), function returns the string – the current location version. If location archive is not found or if the version can't be obtained, function returns null.

Function startLocationLoader
----------------------------

Function is used for creating a download process. Download is done in a separate thread in the non- blocking mode. Function startLocationLoader doesn't wait until download is finished and returns immediately.

**Prototype:**

.. code-block:: Java
   
   public static int startLocationLoader(String userId, String location, String filename, boolean forced)

**Parameters:**

* *userId* — user account identifier;
* *location* — location name;
* *filename* — path to file, where the location archive should be stored;
* *forced* — the boolean flag. If set, the location data would be loaded even if the same version has been downloaded already earlier. If flag is not set, the download process compares the current downloaded version with the last version on the server. If server version equals to  the current downloaded version, the re-downloading is not done.

**Return value:**

Integer number — the download process identifier. This number is used further for checking the download process state and for download process terminating.

Function checkLocationLoader
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Function is used for checking the download process state and progress.

**Prototype:**

.. code-block:: Java
   
   public static int checkLocationLoader(int id)

**Parameters:**

* *id* — download process identifier.

**Return value:**

Integer number — the download process state:

* values in interval [0, 99] mean that download is in progress. In that case the value shows the download progress percentage;
* value 100 means that download has been successfully finished;
* other values mean that download process is impossible for some reason.

Function stopLocationLoader
---------------------------

Function is used for forced termination of download process which has been started earlier. Function should be called when download process is finished (successfully or not) or by a timeout.

**Prototype:**

.. code-block:: Java
   
   public static void stopLocationLoader(int id)

**Parameters:**

* *id* — download process identifier.

Function startLocationUploader
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Function is used for creating an upload process. Upload is done in a separate thread in the non- blocking mode. Function startLocationUploader doesn't wait until upload is finished and returns immediately.

**Prototype:**

.. code-block:: Java
   
   public static int startLocationUploader(String userId, String location, String filename)

**Parameters:**

* *userId* — user account identifier; *location* — location name
* *filename* — file name to upload

**Return value:**

Integer number — the upload process identifier. This number is used further for checking the upload process state and for upload process terminating.

Function checkLocationUploader
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Function is used for checking the upload process state and progress.

**Prototype:**

.. code-block:: Java
   
   public static int checkLocationUploader(int id)

**Parameters:**

* *id* — upload process identifier.

**Return value:**

Integer number — the upload process state:

* Values in interval [0, 99] mean that download is in progress. In that case the value shows the download progress percentage.
* Value 100 means that upload has been successfully finished.
* Other values mean that upload process is impossible for some reason.

Function stopLocationUploader
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Function is used for forced termination of upload process which has been started earlier. Function should be called when upload process is finished (successfully or not) or by a timeout.

**Prototype:**

.. code-block:: Java
   
   public static void stopLocationUploader(int id)

**Parameters:**

* *id* — upload process identifier.

Usage Example
^^^^^^^^^^^^^

.. code-block:: Java
   
   public class MainActivity extends Activity { ProgressBar mProgressBar = null;
   
   Button mStatusButton = null;
   
   int mLoaderUid = ­1;
   
   ...
   
   public void startLocationLoader(String location) {
   
   String filename = LocationLoader.getLocationFile(this, location);
   
   mLoaderUid = LocationLoader.startLocationLoader(
   “xxxx­xxxx­xxxx­xxxx”, location, filename, false);
   
   }
   
   public void stopLocationLoader() {
   LocationLoader.stopLocationLoader(mLoaderUid);
   
   }

   public void updateLocationLoader() {
   
   if (mLoaderUid < 0)
   
   return;
   
   int progress = LocationLoader.checkLocationLoader(mLoaderUid);
   
   if (progress == 0) {
   
   mProgressBar.setIndeterminate(true);
   
   }
   
   else if (1 <= progress && progress <= 99) {
   mProgressBar.setIndeterminate(false);
   mProgressBar.setProgress(progress);
   
   }
   
   else if (progress == 100) { mProgressBar.setIndeterminate(false);
   mProgressBar.setProgress(progress);
   mStatusButton.setText(“Finished”);
   
   }
   
   else {
   
   mProgressBar.setIndeterminate(false);
   mProgressBar.setProgress(progress); mStatusButton.setText(“Failed”);
   LocationLoader.stopLocationLoader(mLoaderUid);
   
   }
   
   }
   
   }

Class Location
--------------

Public fields
^^^^^^^^^^^^^

* *public int id* — location identifier
* *public String name* – location name
* *public String archiveFile* — absolute path to the location archive file
* *public List<SubLocation> subLocations* — a list of sub-locations

Constructors
^^^^^^^^^^^^

* *Location()* — default constructor
* *Location(Location loc)* — copy constructor

Function getSubLocation
^^^^^^^^^^^^^^^^^^^^^^^

Function is used for obtaining the sub-location with the specified identifier from the current location.

**Prototype:**

.. code-block:: Java
   
   public SubLocation getSubLocation(int id)

**Parameters:**

* *id* — the sub-location identifier

**Return value:**

The sub-location of the current location with the specified identifier, if it exists. If sub-location with the specified identifier doesn't exist, function returns null.

Public Fields
^^^^^^^^^^^^^

* *public int id* — sub-location identifier
* *public String name* – sub-location name
* *public String svgFile* — file name in archive, which contains SVG-image of sub-location or empty string if SVG-image is not specified
* *public String pngFile* — file name in archive, which contains PNG-image of sub-location or empty string if PNG-image is not specified
* *public String jpgFile* — file name in archive, which contains JPEG-image of sub-location or empty string if JPEG-image is not specified
* *public float width* — sub-location width in meters; *public float height* — sub-location height in meters; *public float azimuth* — sub-location azimuth in radians
* *public double gpsLatitude –* GPS latitude of the point (0, 0, 0) within the sub-location
* *public double gpsLongitude –* GPS longitude of the point (0, 0, 0) within the sub-location
* *public Picture picture* — sub-location picture or null (if picture was not loaded)
* *public Bitmap bitmap* – sub-location bitmap or null (if bitmap was not loaded)
* *public String archiveFile* — absolute path to the location archive file

Constructors
^^^^^^^^^^^^

* *SubLocation()* — default constructor
* *SubLocation(SubLocation subLoc)* — copy constructor

Function getSvgImage()
^^^^^^^^^^^^^^^^^^^^^^

Function is used for obtaining raw SVG data for the current sub-location.

**Prototype:**

.. code-block:: Java
   
   public byte[] getSvgImage()

**Return value:**

Array of bytes corresponding to the SVG file specified for the current sub-location. If SVG file is not specified, function returns null.

Function getPngImage()
^^^^^^^^^^^^^^^^^^^^^^

Function is used for obtaining raw PNG data for the current sub-location.

**Prototype:**

.. code-block:: Java
   
   public byte[] getPngImage()

Array of bytes corresponding to the PNG file specified for the current sub-location. If PNG file is not specified, function returns null.

Function getJpgImage()
^^^^^^^^^^^^^^^^^^^^^^

Function is used for obtaining raw JPG data for the current sub-location.

**Prototype:**

.. code-block:: Java
   
   public byte[] getJpgImage()

**Return value:**

Array of bytes corresponding to the JPG file specified for the current sub-location. If JPG file is not specified, function returns null.

Function getPicture
^^^^^^^^^^^^^^^^^^^

Function is used for obtaining the sub-location picture. Picture can be directly used for displaying a sub-location map in the user interface (e.g. via PictureDrawable class in Android).

**Prototype:**

.. code-block:: Java
   
   public Picture getPicture()

**Return value:**

If SVG-image is specified for the sub-location, function parses SVG-image from location archive and returns the Picture object, corresponding to that image. If an SVG-image is not specified for the sub-location or if an error occurs during parsing SVG-image, function returns null.

Function getBitmap
^^^^^^^^^^^^^^^^^^

Function is used for obtaining the sub-location bitmap. Bitmap can be directly used for displaying a sub-location map on the user interface (e.g. via BitmapDrawable class in Android).

**Prototype:**

.. code-block:: Java
   
   public Bitmap getBitmap()

**Return value:**

If PNG- or JPG-image file is specified for the sub-location, function parses the corresponding image file from location archive and returns the Bitmap object, corresponding to that image. If both PNG- and JPG- image files are not specified for the sub-location or if parse error occurs during parsing image data, function returns null.

Function getGpsCoordinates
^^^^^^^^^^^^^^^^^^^^^^^^^^

Function is used for converting “local” sub-location coordinates to the global geographic coordinates (latitude and longitude) using the geographic binding of the sub-location.

**Prototype:**

.. code-block:: Java
   
   public double[] getGpsCoordinates(float x, float y)

**Parameters:**

* *x, y* — sub-location “local” coordinates.

The array of two double elements – the geographical latitude and longitude, corresponding to the position within the sub-location with the specified coordinates (x, y).


Class DeviceInfo
----------------

DeviceInfo class is used for storing device position, orientation and other meta-data.

**Prototype:**

.. code-block:: Java
   
   public class DeviceInfo

Public Fields
^^^^^^^^^^^^^

* *public String id* — device identifier (IMEI, MAC-address or similar)
* *public String type* — device type (android, windows, iOS)
* *public String time* — device time label
* *public int index* — device index
* *public int location –* current device location identifier
* *public int subLocation –* current device sub-location identifier
* *public float x* — x-coordinate of the current device position within the sub-location
* *public float y* — y-coordinate of the current device position within the sub-location
* *public float z* — z-coordinate of the current device position within the sub-location
* *public float r* — trusting radius of the current device position within the sub-location
* *public float azimuth* — device azimuth angle (in radians)
* *public float pitch* — device pitch angle (in radians)
* *public float roll* — device roll angle (in radians)

Constructors
^^^^^^^^^^^^

* *DeviceInfo()* — default constructor
* *DeviceInfo(DeviceInfo info)* — copy constructor

Class LocationPoint
-------------------

Class LocationPoint is used for representing certain positions (points) in the location.

Public Fields
^^^^^^^^^^^^^

* *public int subLocation* — sub-location identifier
* *public float x* – x-coordinate of the location point within the sub-location
* *public float y* – y-coordinate of the location point within the sub-location

Constructors
^^^^^^^^^^^^

* *LocationPoint()* — default constructor
* *LocationPoint(LocationPoint subLoc)* — copy constructor
* *LocationPoint(int \_subLocation, float \_x, float \_y)* — initializing constructor

Push Notifications
------------------

A push notification is an advertising message that is displayed in the notification drawer of your Android device. NavigineSDK provides an easy API for push notifications to appear in your application when the device enters the specified area and beacon corresponding to the advertising push notification is be detected. Push notification functionality is provided be means of BeaconService class.

To use this functionality you must undertake the following steps:

1. Declare a service and a push activity in your *AndroidManifest.xml*:

.. code-block:: Java
   
   <application>
   
   ...
   
   <service android:name="com.navigine.naviginesdk.BeaconService" android:exported="true"/>
   
   <activity android:name="com.navigine.naviginesdk.BeaconActivity"/>
   
   </application>

2. Declare a broadcast receiver for BOOT\_COMPLETED event (to add BeaconService in the autoload) in your *AndroidManifest.xml*:

.. code-block:: Java
   
   <application>
   
   ...
   
   <receiver android:name="com.navigine.naviginesdk.BeaconReceiver"
   android:enabled="true" android:exported="true">
   
   <intent­filter>
   
   <action android:name="android.intent.action.BOOT\_COMPLETED"/>
   
   </intent­filter>
   
   </receiver>
   
   </application>

3. For BeaconService to start when your application starts add the following lines to your main activity onCreate() method:

.. code-block:: Java
   
   if (!BeaconService.isStarted())
   
   startService(new Intent(this, BeaconService.class));
   
4. For notifications to show in your application you must add the *notification.png* icon to you res/drawable directory:

|image0|

AndroidManifest
---------------

For NavigineSDK to work your Android SDK API level should be greater or equal to 18. Besides, you should add the following permissions to your *AndroidManifest.xml*:

.. code-block:: Java
   
   <uses­permission android:name="android.permission.ACCESS\_FINE\_LOCATION"/>
   
   <uses­permission android:name="android.permission.ACCESS\_WIFI\_STATE"/>
   
   <uses­permission android:name="android.permission.BLUETOOTH"/>
   
   <uses­permission android:name="android.permission.BLUETOOTH\_ADMIN"/>
   
   <uses­permission android:name="android.permission.CHANGE\_WIFI\_STATE"/>
   
   <uses­permission android:name="android.permission.INTERNET"/>
   
   <uses­permission android:name="android.permission.READ\_PHONE\_STATE"/>
   
   <uses­permission android:name="android.permission.RECEIVE\_BOOT\_COMPLETED"/>
   
   <uses­permission android:name="android.permission.WRITE\_EXTERNAL\_STORAGE"/>
   
   <uses­feature android:name="android.hardware.bluetooth\_le" android:required="true"/>
   
.. |image0| image:: _static/notification.png