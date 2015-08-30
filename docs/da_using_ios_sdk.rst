Using Navigine SDK for iOS
==========================

The following is the Navigine's SDK for iOS API Reference.

Structure Initialization
------------------------

Before You Begin
^^^^^^^^^^^^^^^^

#. Add to your Info.plist file empty string “NSLocationAlwaysUsageDescription”.
#. Add to your Build Phases:
	* libz.dylib
	* libc++.dylib
	* Uikit.framework
	* CoreBluetooth.framework
	* CoreLocation.framework
    * CoreMotion.framework
	* Foundation.framework 
	* MobileCoreServices.framework

Default Settings
^^^^^^^^^^^^^^^^

.. code-block:: Java
   
   [[NavigineCore alloc] init];

Custom Settings
^^^^^^^^^^^^^^^

.. code-block:: Java
   
   [[NavigineCore alloc] initWithUUIDString: @"AAAAAAAA-BBBB-CCCC-1111-222222222222"];

Navigation
----------

Structure NavigationResults
^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: Java
   
   typedef struct \_NavigationResults
   {
		int outLocation;
		int outSubLocation;
		double X;
		double Y;
		double Z;
		double Yaw;
		*int ErrorCode;
   }NavigationResults;

**Fields:**

* *outLocation* – location id of your position.
* *outSubLocation* – sublocation id of your position. 
* *X* – X coordinate of your position(m).
* *Y* – Y coordinate of your position (m).
* *Yaw* – yaw angle(radians).
* *ErrorCode* – error code. If 0 – all is good.

Function downloadContent: location: forceReload:successBlock:failBlock:
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Function is used for downloading content from server and starting navigation. Attention: downloading process in background!

**Prototype:**

.. code-block:: Java
   
   - (void) downloadContent :(NSString \*)userHash
                   location :(NSString \*)location
                forceReload :(BOOL) forced
               processBlock :(void(^)(NSInteger loadProcess))processBlock
               successBlock :(void(^)(NSString \*success))successBlock
                  failBlock :(void(^)(NSString \*error))failBlock;

**Params:**

* *userID* - userID ID from web site.
* *location* - location location name from web site.
* *forced* — the boolean flag. If set, the content data would be loaded even if the same version has been downloaded already earlier. If flag is not set, the download process compares the current downloaded version with the last version on the server. If server version equals to the current downloaded version, the re-downloading is not done.
* *loadProcess* – values in interval [0, 99] mean that download is in progress. Value 100 means that download has been successfully finished.
* *success* – string, that speaks of a successful upload and start navigation.
* *error* — description if download process is impossible.

Function startNavigine
^^^^^^^^^^^^^^^^^^^^^^

Function is used for starting Navigine service.

**Prototype:**

.. code-block:: Java
   
   - (void) startNavigine;*

Function stopNavigine
^^^^^^^^^^^^^^^^^^^^^

Function is used for forced termination of Navigine service. (Throws exception if the navigation is not possible on device).

**Prototype:**

.. code-block:: Java
   
   - (void) stopNavigine;

Function startLocationLoader:::
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Function is used for creating a content download process from the server. Download is done in a separate thread in the non-blocking mode. Function startLocationLoader doesn't wait until download is finished and returns immediately.

**Prototype:**

.. code-block:: Java
   
   - - (int)startLocationLoader :(NSString \*)userID:(NSString\*)location:(BOOL)forced

* *userID* - userID ID from web site.
* *location* - location location name from web site.
* *forced* — the boolean flag. If set, the content data would be loaded even if the same version has been downloaded already earlier. If flag is not set, the download process compares the current downloaded version with the last version on the server. If server version equals to the current downloaded version, the re-downloading is not done.

**Return value:**

Integer number - download process identifier. This number is used further for checking the download process state and for download process terminating.

Function сheckLocationLoader:
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Function is used for checking the download process state and progress.

**Prototype:**

.. code-block:: Java
   
   - (int) сheckLocationLoader: (int)loaderId;

**Params:**

* *loaderId* - download process identifier.

**Return value:**

Integer number — the download process state:

* values in interval [0, 99] mean that download is in progress. In that case the value shows the download progress percentage.
* value 100 means that download has been successfully finished.
* other values mean that download process is impossible for some reason.

Function stopLocationLoader:
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Function is used for forced termination of download process, which has been started earlier. Function should be called when download process is finished (successfully or not) or by a timeout. 

**Prototype:**

.. code-block:: Java
   
   - (void) stopLocationLoader :(int)loaderId;

**Params**

* *loaderId* - download process identifier.

Function loadArchive:
^^^^^^^^^^^^^^^^^^^^^

Function is used for loading a location archive to the Navigation library.

**Prototype:**

.. code-block:: Java
   
   - (bool) loadArchive :(NSString \*)location;

**Params:**

* *location* - location location name from web site.

**Return value:**

* *true*, if archive was loaded successfully.
* *false*, if archive can't be loaded (e.g. invalid archive file).

Function getNavigationResults
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Function is used for getting result of navigation 

**Prototype:**

.. code-block:: Java
   
   - (NavigationResults) getNavigationResults;

**Return value:** 

* structure NavigationResults.

Function localToGps::::::
^^^^^^^^^^^^^^^^^^^^^^^^^

Function is used for converting local coordinates to GPS coordinates

**Prototype:**

.. code-block:: Java
   
   - (void) localToGps: (float) x :(float) y :(float) azimuth:(double) latitude :(double)longitude :(double\*) data;

**Params:**

* *x* – x-coordinate.
* *y* – y-coordinate.
* azimuth – sublocation azimuth
* latitude – sublocation latitude
* longitude – sublocation longitude
* data – GPS coordinates

Example of Usage:
^^^^^^^^^^^^^^^^^

**1. Simple usage**

.. code-block:: Java
   
   \_navigineCore = [[NavigineCore alloc] init];
   [\_ navigineCore downloadContent:@"1234-1234-1234-1234"
                           location:@"navigine"
                        forceReload:NO
                       processBlock:^(NSInteger loadProcess) {
               NSLog(@"Load process: %zd",loadProcess);
                     } successBlock:^(NSString \*success) {
                           NSlog(@”SUCCESS!”)
                        } failBlock:^(NSString \*error) {
                          if(error) NSLog(@"ERROR %@",error);
                        }];*

**2. Expert usage**

.. code-block:: Java
   
   \_navigineCore = [[NavigineCore alloc] init];
   
   int \_loaderID = [\_navigineCore startLocationLoader :@"1234-1234-1234-1234" :@ "navigine" :NO];
   int loadProcess = 0;
   loadProcess = [\_navigineCore checkLocationLoader :\_loaderID];
   while (loadProcess < 100){
     loadProcess = [\_navigineCore checkLocationLoader :\_loaderID];
     if(loadProcess == 255) {
         [\_navigineCore stopLocationLoader];
         return;
     }
   }
   [\_navigineCore stopLocationLoader];
   [\_navigineCore loadArchive:@”navigine”];
   @try {
    [\_navigineCore startNavigine];
    }
   @catch (NSException \*exception) {
    NSLog(@"Exception caught: reason: %@",exception.description);
   }
   @finally {
   NSLog(@"Finally");
   }
   NavigationResults res=[\_navigineCore getNavigationResults];

Making Route
------------

Class Vertex
^^^^^^^^^^^^

.. code-block:: Java
   
   @interface Vertex : NSObject{}
   @property (assign,nonatomic) int subLocation;
   @property (assign,nonatomic) double x;
   @property (assign,nonatomic) double y;
   @end

**Fields:**

* *subLocation* – sublocation id of vertex\ *. x* – X coordinate of vertex (m).
* *y* – Y coordinate of vertex (m).

Function makeRoute::::::
^^^^^^^^^^^^^^^^^^^^^^^^

Function is used for making route from one position to other.

**Prototype:**

.. code-block:: Java
   
   - (NSArray\*) makeRoute :(int)id1 :(double)x1 :(double)y1 :(int)id2:(double)x2*:(double)y2;

**Params:**

* *id{1,2}* – sublocation id of start or finish point.
* *x{1,2}* – X coordinate of start or finish point.
* *y{1,2}* – Y coordinate of start or finish point.

**Return value:**

NSArray object – array with Vertex structures.

Example of Usage:

.. code-block:: Java
   
   NSString \*path = [\_navigineCore makeRoute :1234 :0.0 :0.0 :1234:100.0 :100.0];

Location
--------

Function getLocationId:
^^^^^^^^^^^^^^^^^^^^^^^

Function is used for getting current location id

**Prototype:**

.. code-block:: Java
   
   -(int) getLocationId:(NSInteger \*)id;

**Params:**

* *id* – current sublocation id.

**Return value:** 

error (0 if ok).

Function getSublocDictionary:
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Function is used for getting "index"->"id" sublocation dictionary

**Prototype:**

.. code-block:: Java
   
   - (int) getSublocDictionary:(NSDictionary \*\*)sublocDictionary;

**Params:**

* *sublocDictionary* - sublocation dictionary.

**Return value:** 

error (0 if ok).

Function getCurrentVersion:
^^^^^^^^^^^^^^^^^^^^^^^^^^^

Function is used for getting current location version

**Prototype:**

.. code-block:: Java
   
   - (int) getCurrentVersion:(NSInteger \*)currentVersion;

**Params:**

* *currentVersion* - current location version.

**Return value:** 

error (0 if ok).

Images
------

Function getSVGImageByIndex:: / getPNGImageByIndex::
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Function is used for getting image from zip (SVG, PNG)

**Prototype:**

.. code-block:: Java
   
   - (int) getSVGImageByIndex :(NSInteger)index :(NSData \*\*)imData;
   - (int) getPNGImageByIndex :(NSInteger)index :(NSData \*\*)imData;

**Params:**

* *index* - the ordinal sublocation in admin panel.
* *imData* - image data.

**Return value:**

error (0 if ok).

Function’s getSVGImageById:: / getPNGImageById::
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Function is used for getting image from zip (SVG, PNG)

**Prototype:**

.. code-block:: Java
   
   - (int) getSVGImageById :(NSInteger)id :(NSData \*\*)imData;
   - (int) getPNGImageById :(NSInteger)id :(NSData \*\*)imData;

**Params:**

* *id* - sublocation id.
* *imData* image data

**Return value:**

error (0 if ok).

Function’s getWidthAndHeight:ByIndex:
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Function is used for getting width and height of image 

**Prototype:**

.. code-block:: Java
   
   - (int)getWidthAndHeight: (double\*)width :(double\*)height byIndex:(NSInteger)index;

**Params:**

* *width* – image width
* *height* – image height
* *index* - the ordinal sublocation in admin panel.

**Return value:**

error (0 if ok).

Function’s getWidthAndHeight:ById:
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Function is used for getting width and height of image 

**Prototype:**

.. code-block:: Java
   
   - (int)getWidthAndHeight: (double\*)width :(double\*)heightbyIndex:(NSInteger)id;

**Params:**

* *width* – image width
* *height* – image height
* *id* –image sublocation id

**Return value:**

error (0 if ok).

Pushes & Venues
---------------

Class Push
^^^^^^^^^^

.. code-block:: Java
   
   @interface Push : NSObject{}
   @property(nonatomic, strong) NSString \*pushTitle;
   @property(nonatomic, strong) NSString \*pushContent;
   @property(nonatomic, strong) NSString \*pushImage;
   @end

**Fields:**

* *pushTitle* - title of push. *pushContent* - content of push.
* *pushImage* - url path to image of push.

Class Venues
^^^^^^^^^^^^

.. code-block:: Java
   
   @interface Venue : NSObject{}
   @property(nonatomic, strong) NSString \*name;
   @property(nonatomic, strong) NSString \*realX;
   @property(nonatomic, strong) NSString \*realY;
   @property(nonatomic, strong) NSString \*image;
   @property(nonatomic, strong) NSString \*phone;
   @property(nonatomic, strong) NSString \*descript;
   @end

**Fields:**

* *name* - name of venue.
* *realX* - X coordinate of venue (m).
* *realY* - Y coordinate of venue (m).
* *image* - url path to image of venue content.
* *phone* - phone number of venue.
* *descript* - other info about venue.
 
Protocol NavigineCoreDelegate
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Delegate for NavigineCore.

.. code-block:: Java
   
   @protocol NavigineCoreDelegate;

Function startRangePushes
^^^^^^^^^^^^^^^^^^^^^^^^^

Function is used for checking pushes from web site.

**Prototype:**

.. code-block:: Java
   
   - (void) startRangePushes;

Function didRangePushWithTitle: Content: Image
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Tells the delegate that push in range. Function is called by the timeout of the web site.

**Prototype:**

.. code-block:: Java
   
   - (void) didRangePushWithTitle :(NSString \*)title Content :(NSString \*)content Image :(NSString \*)image;

**Params:**

* *pushTitle* - title of push.
* *pushContent* - content of push.
* *pushImage* - url path to image of push.

Function didRangePushWithTitle: Content: Image: Id:
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Tells the delegate that push in range. Function is called by the timeout of the web site.

**Prototype:**

.. code-block:: Java
   
   - (void) didRangePushWithTitle :(NSString \*)title Content :(NSString \*)content Image :(NSString \*)image Id:(NSInteger) id;

**Params:**

* *pushTitle* - title of push.
* *pushContent* - content of push.
* *pushImage* - url path to image of push.
* *id – push id*

Function startRangeVenues:
^^^^^^^^^^^^^^^^^^^^^^^^^^

Function is used for checking venues from web site.

**Prototype:**

.. code-block:: Java
   
   - (void) startRangeVenues;

Function didRangeVenues:
^^^^^^^^^^^^^^^^^^^^^^^^

Tells the delegate that venue in range. 

**Prototype:**

.. code-block:: Java
   
   - (void) didRangeVenues :(NSArray \*)venues;

**Params:**

* NSArray object – array with Venue structures.

Background Navigation
---------------------

Before Starting
^^^^^^^^^^^^^^^

Select “Your target” -> “Capabilities” -> enable “Background mode” -> “Location updates” and “Uses Bluetooth LE accessories”

Function navigationResultsInBackground
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Function is used for getting result of navigation when app in background or app not running. If application is in the background function is called once per second.

If application not run function is called by OS signal.

But there are a couple of caveats:

* Your app launches into the background only for a few seconds.
* iOS can take its own sweet time to decide you entered a CLBeaconRegion. I have seen it take up to four minutes.

More info:
`http://developer.radiusnetworks.com/2013/11/13/ibeacon-monitoring-in-the-background-and-foreground.html <http://developer.radiusnetworks.com/2013/11/13/ibeacon-monitoring-in-the-background-and-foreground.html>`__\

**Prototype:**

.. code-block:: Java
   
   - (void) navigationResultsInBackground :(NavigationResults)navigationResults;

**Params:**

* *navigationResults* - structure NavigationResults.