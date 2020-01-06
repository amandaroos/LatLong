# LatLong
Uses Kotlin and google play services to show lat and long on Android
 
Add to gradle: implementation 'com.google.android.gms:play-services-location:17.0.0'

Add to manifest: < uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />

Use as main layout:
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
   xmlns:app="http://schemas.android.com/apk/res-auto"
   xmlns:tools="http://schemas.android.com/tools"
   android:layout_width="match_parent"
   android:layout_height="match_parent"
   tools:context=".MainActivity">

   <LinearLayout
       android:id="@+id/linearLayout"
       android:layout_width="wrap_content"
       android:layout_height="wrap_content"
       android:orientation="vertical"
       app:layout_constraintBottom_toBottomOf="parent"
       app:layout_constraintLeft_toLeftOf="parent"
       app:layout_constraintRight_toRightOf="parent"
       app:layout_constraintTop_toTopOf="parent">

       <LinearLayout
           android:layout_width="wrap_content"
           android:layout_height="wrap_content"
           android:orientation="horizontal">

           <TextView
               android:layout_width="wrap_content"
               android:layout_height="wrap_content"
               android:text="Latitude: "
               android:textSize="24dp" />

           <TextView
               android:id="@+id/latitude"
               android:layout_width="wrap_content"
               android:layout_height="wrap_content"
               android:textSize="24dp"
               tools:text="?" />
       </LinearLayout>

       <LinearLayout
           android:layout_width="wrap_content"
           android:layout_height="wrap_content"
           android:orientation="horizontal">

           <TextView
               android:layout_width="wrap_content"
               android:layout_height="wrap_content"
               android:text="Longitude: "
               android:textSize="24dp" />

           <TextView
               android:id="@+id/longitude"
               android:layout_width="wrap_content"
               android:layout_height="wrap_content"
               android:textSize="24dp"
               tools:text="?" />
       </LinearLayout>
   </LinearLayout>
</androidx.constraintlayout.widget.ConstraintLayout>



Main Activity Kotlin code:
package com.example.justcoordinates

import android.Manifest
import android.content.pm.PackageManager
import android.location.Location
import android.os.Bundle
import androidx.appcompat.app.AppCompatActivity
import androidx.core.app.ActivityCompat
import com.google.android.gms.location.FusedLocationProviderClient
import com.google.android.gms.location.LocationServices
import kotlinx.android.synthetic.main.activity_main.*

class MainActivity : AppCompatActivity() {

   val RequestPermissionCode = 1
   var mLocation: Location? = null
   private lateinit var fusedLocationClient: FusedLocationProviderClient

   override fun onCreate(savedInstanceState: Bundle?) {
       super.onCreate(savedInstanceState)
       setContentView(R.layout.activity_main)

       fusedLocationClient = LocationServices.getFusedLocationProviderClient(this)
       getLastLocation()
   }

   fun getLastLocation() {
       if (ActivityCompat.checkSelfPermission(
               this,
               Manifest.permission.ACCESS_FINE_LOCATION
           ) != PackageManager.PERMISSION_GRANTED
       ) {
           requestPermission()
       } else {
           fusedLocationClient.lastLocation
               .addOnSuccessListener { location: Location? ->
                   mLocation = location
                   if (location != null) {
                       latitude.text = location.latitude.toString()
                       longitude.text = location.longitude.toString()
                   }
               }
       }
   }

   private fun requestPermission() {
       ActivityCompat.requestPermissions(
           this,
           arrayOf(Manifest.permission.ACCESS_FINE_LOCATION),
           RequestPermissionCode
       )
       this.recreate()
   }
}


 


