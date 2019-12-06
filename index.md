---
layout: default
---

# Preface

This is a tutorial for the Google maps SDK to help developers get up and running with a simple hello world google maps android application 

## Setup

To begin developing for the app we must first get an IDE to house our code. For our instance we used Android Studio which is a free IDE that can be downloaded on google or at this link here: Download Here! Next, we are going to need a key for the Maps SDK provided by Google. This can also be found here, while on the page in the top right corner it will say get started. Click this button, Google will then ask you to sign in. After successfully signing in a pop up will appear titled “Enable Google Maps Platform”, you then pick the API’s we will need. In this case choose Maps, then choose a project name. Google will then route you to a page enabling a billing account, fill in this information. Next you should be faced with a home screen, making sure that your project is selected from the drop down menu in the toolbar. We then click on “APIs & Services” -> “Credentials”, then create credentials. Google will pop up another dialog box, click on the API key selection. This should give you your own personal API key. Then we make our way to “APIs & Services” -> “Library”, sometimes the Maps SDK API has issues so you should clarify that it is enabled and ready to use.

### Creating the application

Now we can load Android Studio and make a new project. You will be asked for which activity you would like to create, we’ll start with just a basic activity. Name your activity, package name, where you want it to be saved, and the language you are coding in. In our instance this will be Java, then click finish. Your project has now been started. In the top left corner of Android Studio there’s the option “File”. Follow these steps to create a new Google Maps Activity, File -> New -> Google -> Google Maps Activity. A new pop up will appear allowing you to choose the name and other aspects of your activity. Once finished, a new file titled google_maps_api.xml can be found by going to: app -> res -> values -> google_maps_api.xml. Once here you should see a string saying: 
```js
<string name="google_maps_key" templateMergeStrategy="preserve" translatable="false">YOUR KEY</string> </resources>
```

#### Syncing with google maps

The API key you received from Google will be copied and pasted into the section “YOUR KEY”. Next we will go to the AndroidManifest.xml file to copy and paste some code that will give permission for your device to access locations, internet, and write to external storage. This code is found here: 
```
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
```

##### Ready to start

We should now have our activities all set to start our coding. On our content_main.xml file we are able to see a textView that hold the value “Hello World!” we can now delete this. Instead we will add a button that will help us make an  intent to the maps activity. Make sure to constrain the button so it is formatted to how you would like. Next we will add the following code to the MainActivity.java file. 
```
public class MainActivity extends AppCompatActivity {

   public static final int selection = 1;

   @Override
   protected void onCreate(Bundle savedInstanceState) {
       super.onCreate(savedInstanceState);
       setContentView(R.layout.activity_main);
       Button toMap = findViewById(R.id.toMap);

       toMap.setOnClickListener(e-> {
                   System.out.println("Button was clicked for map");
                   Intent intent = new Intent(MainActivity.this, MapsActivity.class);
                   startActivityForResult(intent, selection);
       });
   }
}
```
This code is creating a new button that can be used within the class. The line of code that mentions ```findViewByID(R.id.toMap)``` the name toMap is the label of my button from the content_main.xml file. Next we make an intent that will start the maps activity when the button is clicked. Your content_main.xml file should now look like this:

(INSERT THE IMAGE HERE)

Within the MapsActivity.java file we can set the destination of a marker on our map, for this instance we chose the city of Grand Rapids for our marker point which can be seen here:

```
@Override
public void onMapReady(GoogleMap googleMap) {
   mMap = googleMap;

   // Add a marker in Grand Rapids and move the camera
   LatLng GrandRapids = new LatLng(42.9634, -85.6681);
   mMap.addMarker(new MarkerOptions().position(GrandRapids).title("Marker in Grand Rapids, MI"));
   mMap.moveCamera(CameraUpdateFactory.newLatLng(GrandRapids));
}
```

###### Give it a test

We can finally test the code to make sure the map is working correctly, we can do this by just running the emulator. 

(INSERT ANOTHER IMAGE HERE)



### Adding Polylines
Now to actually implement polylines we are going to have to change up the code within the MapsActivity.java file. We can start by implementing different listeners into the class by using this code:
```
public class MapsActivity extends FragmentActivity implements OnMapReadyCallback,GoogleMap.OnPolylineClickListener,GoogleMap.OnPolygonClickListener{
```

With this code you will be asked to implement methods otherwise you will receive errors. Next we will need to handle the onCreate method. For this we will need to set a content view which renders the map and add a notification for when the map is ready to be used. This can all be seen and copied from here:
```
@Override
protected void onCreate(Bundle savedInstanceState) {
   super.onCreate(savedInstanceState);
   setContentView(R.layout.activity_maps);
   SupportMapFragment mapFragment = (SupportMapFragment) getSupportFragmentManager()
           .findFragmentById(R.id.map);
   mapFragment.getMapAsync(this);
}
```
Finally, to add the actual polylines to the map we will need to get latitude and longitudinal coordinates for our points. For this example, the points chosen were randomly picked as to surround Grand Rapids with the polylines. All of the next code will be housed within the “onMapReady(GoogleMap googleMap)” method as shown here:
```
@Override
public void onMapReady(GoogleMap googleMap) {.
   Polyline polyline1 = googleMap.addPolyline(new PolylineOptions()
           .clickable(true)
           .add(
                   new LatLng(43.086806, -85.675232),
                   new LatLng(43.021329, -85.891182),
                   new LatLng(42.874761, -85.802613),
                   new LatLng(42.871623, -85.501837),
                   new LatLng(42.993224, -85.467274),
                   new LatLng(43.086806, -85.675232)));

   googleMap.moveCamera(CameraUpdateFactory.newLatLngZoom(new LatLng(43.086806, -85.675232), 10));

   googleMap.setOnPolylineClickListener(this);
   googleMap.setOnPolygonClickListener(this);
}
```
This code allows the user to create a polyline that house multiple waypoints. There can be as many waypoints as a user would like or as few as they would like. After choosing the waypoints we will need to update the camera as to find where our polyline actually resides. This is done by using the CameraUpdateFactory call. We can then pick a point on the map that is close to our polyline as well as our zoom factor. For this instance, a zoom factor of 10 gets close enough to see the roads of Grand Rapids but also far enough to see the entire polyline. Let’s run our new MapsActivity and see what it looks like. 

(INSERT IMAGE HERE)

If our steps were followed completely, your app should look like the image above. Polylines are very versatile in how they can be implemented, perhaps you want to make a flight path tracking app and want to see how flights are going to be routed, or you could use polylines for basic street directions as well. By further research a developer can find new ways to implement polylines for different interesting applications.
