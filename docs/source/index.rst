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
      ``implementation 'com.google.code.gson:gson:2.6.2' }``
	  
.. toctree::
   :maxdepth: 2
   :caption: Contents:



Indices and tables
==================

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`
