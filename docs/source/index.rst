Documentation for Insense MCMS Android Lib
============================================

Introduction
============
Import this android library module to fetch exclusive insense offers details for near by location of users .
This library suports minimium of SDK version 15 i.e Ice Cream Sandwich android devices .


Installation Guide
==================

Add Insense AAR File 
--------------------
Add insense AAR file as a module dependency to your project, proceed as follows:
1. Add the compiled AAR (or JAR) file

  * Click **File > New > New Module**.;
  * Click **Import .JAR/.AAR Package** then click **Next**.;
  * Enter the location of the compiled AAR or JAR file then click **Finish**.;
  
2. Make sure the library is listed at the top of your **settings.gradle** file, as shown here for a library named **"my-library-module"**:
 
 ``include ':app', ':my-library-module'``
 
3. Open the app module's **build.gradle** file and add a new line to the **dependencies** block as shown in the following snippet:
  
  ``dependencies {``
    	``implementation project(":my-library-module")``
	``}``
	
4. Click **Sync Project with Gradle Files**.

Adding Dependency
-----------------
- Add the following code in your **build.gradle** file under app module, version of different packages may change, if already had same package then no need to add again

  ``dependencies {``
      ``implementation 'com.android.volley:volley:1.1.0'``
      ``implementation 'com.google.android.gms:play-services-location:11.8.0'``
      ``implementation 'com.google.android.gms:play-services-maps:11.8.0'``
      ``implementation 'com.google.code.gson:gson:2.6.2'``
   ``}``

- Add following application permissions to AndroidManifest.xml .

    ``<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>``
	
Code Integration
-----------------
Adding code snipet to fetch the user location detail :
	* Create instance of LocationProvider to access location detail of user

	``@Override``
	``protected void onCreate(Bundle savedInstanceState) {``
		``super.onCreate(savedInstanceState);``
		``setContentView(R.layout.activity_main);``
		``this.mContext = this;``
		``LocationProvider.Builder builder =  new LocationProvider.Builder();``
		``LocationProvider mLocationProvider = builder.withContext(mContext)``
				``.withListener(this)          // activity must implemet ILocationReceivedCallback``
				``.build();``
	``}``
	
	* This instantiate a LocationProvider with default config of **30 meter** displacement and **15 minutes** waiting duration for next location response .
	Other custom configurations can be done
	
	``LocationProvider mLocationProvider = builder.withContext(mContext)``
	             ``.withDistance(100)  // 100 meter displacement``
				 ``.withDuration(10)  // 10 minutes wait duration``
				 ``.withListener(this)`` 
				 ``.build();``
				 
   * Activity/Fragment class must implement **ILocationReceivedCallback interface**, to receive locationUpdates , this interface instance is assigned to LocationProvider **.withListener(this)**
   
   ``public class MainActivity extends AppCompatActivity implements ILocationReceivedCallback {``
   
   ``@Override``
	``public void onLocationReceived(Location location) {``
		``Log.d(TAG, "Location received : lat => "+ location.getLongitude() + " long=> " +location.getLongitude());``
	``}``
	
	* After initilization, register/remove the location request listener in **onStart()** / **onDestroy()** of your activity, you can add same in **onResume()** / **onPause()** of your activity
		
	``@Override``
	``protected void onStart() {``
		``super.onStart();
		``if(mLocationProvider!=null) {``
			``mLocationProvider.setupLocationRequest();``
		``}``
	``}``

	``@Override``
	``protected void onDestroy() {``
		``super.onDestroy();``
		``if(mLocationProvider!=null) {``
			``mLocationProvider.stopLocationUpdates();``
		``}``
	``}``
	
	* Now once you receive current location of user inside **onLocationReceived()** , then fetch excllusive offers near by user location .
	Activity must implement IResponseListner interface to receive success / failure callbacks of API call.
	
	``public class MainActivity extends AppCompatActivity implements ILocationReceivedCallback, IResponseListner {``
	
	``@Override``
	``public void onLocationReceived(Location location) {``
		``Request mRequest = new Request(mContext);``
		``String custId = "USER_CUST_ID"   // Current User CustomerId``;
		``mRequest.fetchOffers(custId, location.getLatitude(), location.getLongitude(), this);``
	``}``
	
	``@Override``
	``public void onResponse(JSONArray jsonArray) {``
		``Log.d(TAG, "Response received !" + jsonArray.toString());``
		``Toast.makeText(mContext,"Response received !" + jsonArray.toString() , Toast.LENGTH_SHORT).show();``
	``}``

	``@Override``
	``public void onFailure(Throwable throwable) {``
		``Log.d(TAG, "Failed : " + throwable.getMessage());``
		``Toast.makeText(mContext,"Failed : " + throwable.getMessage() , Toast.LENGTH_SHORT).show();``
	``}``
	
	* parse the JSONArray response to get offer detail 
	
	
	
	
	

 

.. toctree::
   :maxdepth: 2
   :caption: Contents:



Indices and tables
==================

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`
