Insense Mobile Campaign Management System (mCMS) Android Library
=========================================================

Introduction
============
Import this Android Library module to send the user's location to Insense's API at a regular interval.
This library suports a minimium SDK version of 15, i.e, Ice Cream Sandwich and above Android devices.


Installation Guide
==================

Import AAR File 
--------------------
Add Insense's AAR file as a module dependency to your project as follows:
1. Add the compiled AAR (or JAR) file

  * Click **File > New > New Module**
  * Click **Import .JAR/.AAR Package** then click **Next**
  * Enter the location of the compiled AAR or JAR file then click **Finish**
  
2. Make sure the library is listed at the top of your **settings.gradle** file, as shown here for a library named **"my-library-module"**:

.. code:: java

       include ':app', ':my-library-module'
 
3. Open the app module's **build.gradle** file and add a new line to the **dependencies** block as shown in the following snippet:

.. code:: java

  	   dependencies {
       		implementation project(":my-library-module")
	   }
	
4. Click **Sync Project with Gradle Files**

Adding Third Party Dependencies
-------------------------------
This library uses the following third party dependencies:

- **Android Volley**: For API request calls
- **Google Play Services Location**: For listening to the user's location
- **Google GSON Library**: For JSON manipulation

If you are already using these dependencies, you do not need to add them again. However, if some the above dependencies are not currently used, you may add the same as follows:

**Update your build.gradle** file under app module like so:

.. code:: java

       dependencies {
           implementation 'com.android.volley:volley:1.1.0'
           implementation 'com.google.android.gms:play-services-location:11.8.0'
           implementation 'com.google.android.gms:play-services-maps:11.8.0'
           implementation 'com.google.code.gson:gson:2.6.2'
       }


Permissions
-----------
Add the following application permissions to AndroidManifest.xml to access the user's location in case you already do not have these in the app:

.. code:: java

       <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>
	
Code Integration for Listening to Location Changes
---------------------------------------------------

To fetch the user's location, note the steps below:

1. Create an instance of LocationProvider to access the user's location:

.. code:: java

		LocationProvider.Builder builder =  new LocationProvider.Builder();
		LocationProvider mLocationProvider = builder.withContext(mContext)
			.withListener(this)          // the container class must implement ILocationReceivedCallback
			.build();
	
2. The default configuration for listening to the user's location is a min. **30 meter** displacement and **5 minutes** update frequency for the next location callback trigger. You may customize this configuration as follows:
	
.. code:: java	
	
	   LocationProvider mLocationProvider = builder.withContext(mContext)
		.withDistance(100)  // 100 meter displacement
		.withDuration(10)  // for location updates at a 10 min. frequency
		.withListener(this)
		.build();
				 
3. Your listener class must implement the **ILocationReceivedCallback interface**, to receive locationUpdates. In the examples above, we have assumed that the class that creates the instance is also the listener class using **withListener(this)**. This interface has a callback method **onLocationReceived** which will contain the location of the user. An example is as follows:

.. code:: java
       
	@Override
	public void onLocationReceived(Location location) {
		Log.d(TAG, "Location received : lat => "+ location.getLongitude() + " long=> " +location.getLongitude());
	}
	
4. After initilization, register/remove the location request listener in using the methods **setupLocationRequest** and **stopLocationUpdates**. An example is as follows:

.. code:: java

	   @Override
	   protected void onStart() {
		  super.onStart();
		  if(mLocationProvider!=null) {
			 mLocationProvider.setupLocationRequest();
		  }
	  }

	  @Override
	  protected void onDestroy() {
		 super.onDestroy();
		 if(mLocationProvider!=null) {
			mLocationProvider.stopLocationUpdates();
		 }
	  }
	
Code Integration for Sending Location Updates to Insense's API
--------------------------------------------------------------

Code Integration for Fetching Offers through Insense's API
----------------------------------------------------------

1. For listening to any Insense API success or failure responses, your class must implement the **IResponseListener interface**. An example is as follows:
	  
.. code:: java	

	   public class MainActivity extends AppCompatActivity implements ILocationReceivedCallback, IResponseListener {``
	
2. Next, create a **Request** object and call the **fetchOffers** method while passing the customer ID, latitude, longitude and response listener instance parameters as in the example below. This will fetch the offers for the customers and the response will be received in  **onResponse** (if success) or **OnFailure** (if failure)

.. code:: java	

	@Override
	public void onLocationReceived(Location location) {
		Request mRequest = new Request(mContext);
		String custId = "USER_CUST_ID"   // Current User CustomerId;
		mRequest.fetchOffers(custId, location.getLatitude(), location.getLongitude(), this);
	}

	@Override
	public void onResponse(JSONArray jsonArray) {
		Log.d(TAG, "Response received !" + jsonArray.toString());
		Toast.makeText(mContext,"Response received !" + jsonArray.toString() , Toast.LENGTH_SHORT).show();
	}

	@Override
	public void onFailure(Throwable throwable) {
		Log.d(TAG, "Failed : " + throwable.getMessage());
		Toast.makeText(mContext,"Failed : " + throwable.getMessage() , Toast.LENGTH_SHORT).show();
	}
	
3. Finally, parse the JSONArray response to get the offer details
	
	
.. toctree::
   :maxdepth: 2
   :caption: Contents:

