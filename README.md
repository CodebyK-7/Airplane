**AndroidManifest.xml**
<manifest xmlns:android="http://schemas.android.com/apk/res/android"  
    package="com.example.airplanemodeapp">  

    <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED"/>  

    <application  
        android:allowBackup="true"  
        android:theme="@style/Theme.AirplaneModeApp">  

        <!-- Declare Broadcast Receiver for Airplane Mode -->  
        <receiver android:name=".AirplaneModeReceiver">  
            <intent-filter>  
                <action android:name="android.intent.action.AIRPLANE_MODE"/>  
            </intent-filter>  
        </receiver>  

        <activity android:name=".MainActivity">  
            <intent-filter>  
                <action android:name="android.intent.action.MAIN"/>  
                <category android:name="android.intent.category.LAUNCHER"/>  
            </intent-filter>  
        </activity>  

    </application>  
  
</manifest>  


**Main activity**
 
  package com.example.airplanemodeapp;  

import androidx.appcompat.app.AppCompatActivity;  
import android.content.IntentFilter;  
import android.os.Bundle;  
import android.widget.ImageView;  
import android.widget.LinearLayout;  
import android.widget.TextView;  

public class MainActivity extends AppCompatActivity {  

    private AirplaneModeReceiver receiver;  
    private ImageView airplaneImage;  
    private TextView statusText;  

    @Override  
    protected void onCreate(Bundle savedInstanceState) {  
        super.onCreate(savedInstanceState);  

        // Create Layout (Vertical)  
        LinearLayout layout = new LinearLayout(this);  
        layout.setOrientation(LinearLayout.VERTICAL);  
        layout.setPadding(50, 100, 50, 100);  

        // Create ImageView (Dynamic Image for Airplane Mode)  
        airplaneImage = new ImageView(this);  
        airplaneImage.setImageResource(R.drawable.ic_airplane_off);  
        layout.addView(airplaneImage);  

        // Create TextView (Shows Airplane Mode Status)  
        statusText = new TextView(this);  
        statusText.setText("Airplane Mode is OFF");  
        statusText.setTextSize(22);  
        layout.addView(statusText);  

        // Set Layout as Content View (No XML Needed)  
        setContentView(layout);  

        // Register Broadcast Receiver Dynamically (For Newer Android Versions)  
        receiver = new AirplaneModeReceiver(airplaneImage, statusText);  
        IntentFilter filter = new IntentFilter("android.intent.action.AIRPLANE_MODE");  
        registerReceiver(receiver, filter);  
    }

    @Override  
    protected void onDestroy() {  
        super.onDestroy();  
        unregisterReceiver(receiver);  
    }    
}  


**in  java folder create new class**

package com.example.airplanemodeapp;  

import android.content.BroadcastReceiver;  
import android.content.Context;  
import android.content.Intent;  
import android.widget.ImageView;    
import android.widget.TextView;  

public class AirplaneModeReceiver extends BroadcastReceiver {  

    private final ImageView airplaneImage;  
    private final TextView statusText;  
  
    public AirplaneModeReceiver(ImageView imageView, TextView textView) {  
        this.airplaneImage = imageView;  
        this.statusText = textView;  
    }  

    @Override  
    public void onReceive(Context context, Intent intent) {  
        if (Intent.ACTION_AIRPLANE_MODE_CHANGED.equals(intent.getAction())) {  
            boolean isAirplaneModeOn = intent.getBooleanExtra("state", false);  

            if (isAirplaneModeOn) {  
                airplaneImage.setImageResource(R.drawable.ic_airplane_on);  
                statusText.setText("Airplane Mode is ON");  
            } else {  
                airplaneImage.setImageResource(R.drawable.ic_airplane_off);  
                statusText.setText("Airplane Mode is OFF");  
            }    
        }  
    }  
}  




summary  
app  
│── manifests  
│   └── AndroidManifest.xml  
│── java  
│   └── com.example.airplanemodeapp  
│       ├── MainActivity.java   <-- (Your Main Java File)  
│       ├── AirplaneModeReceiver.java  <-- (Newly Created Receiver File)  
│── res  
│   ├── drawable  
│   │   ├── ic_airplane_on.png  <-- (Image when Airplane Mode is ON)  
│   │   ├── ic_airplane_off.png <-- (Image when Airplane Mode is OFF)  
