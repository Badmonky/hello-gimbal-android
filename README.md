# Gimbal Android Basic Sample
Simplest Gimbal Integration Example on Android. After setting up your application, place(s) and communication(s) on Gimbal Manager the code below will yield **Place Events** and **Local Notifications**.

Before you create your Android application

- create your Gimbal account 
- create an **Application** (generates you API KEY)
- create at least one **Place** (using a Beacon or Geofence)
- create at least one **Communicate** (used for the local notification)

here:
[https://manager.gimbal.com/](https://manager.gimbal.com/)

Then fill in your API KEY and run the application.

**Note:** This sample uses Gimbal SDK version 2.9.1

Full Gimbal Docs [https://gimbal.com/docs](https://gimbal.com/docs)

```java
package com.gimbal.hellogimbalandroid;

import android.support.v7.app.ActionBarActivity;
import android.os.Bundle;
import android.widget.ArrayAdapter;
import android.widget.ListView;

import com.gimbal.android.CommunicationManager;
import com.gimbal.android.Gimbal;
import com.gimbal.android.PlaceEventListener;
import com.gimbal.android.PlaceManager;
import com.gimbal.android.Visit;


public class MainActivity extends ActionBarActivity {

    private PlaceManager placeManager;
    private PlaceEventListener placeEventListener;
    private ArrayAdapter<String> listAdapter;
    private ListView listView;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        listAdapter = new ArrayAdapter<String>(this, android.R.layout.simple_expandable_list_item_1);
        listView = (ListView) findViewById(R.id.list);
        listView.setAdapter(listAdapter);

        listAdapter.add("Setting Gimbal API Key");
        listAdapter.notifyDataSetChanged();
        Gimbal.setApiKey(this.getApplication(), "YOUR_API_KEY_HERE");

        placeEventListener = new PlaceEventListener() {
            @Override
            public void onVisitStart(Visit visit) {
                listAdapter.add(String.format("Start Visit for %s", visit.getPlace().getName()));
                listAdapter.notifyDataSetChanged();
            }

            @Override
            public void onVisitEnd(Visit visit) {
                listAdapter.add(String.format("End Visit for %s", visit.getPlace().getName()));
                listAdapter.notifyDataSetChanged();
            }
        };

        placeManager = PlaceManager.getInstance();
        placeManager.addListener(placeEventListener);
        placeManager.startMonitoring();

        CommunicationManager.getInstance().startReceivingCommunications();
    }

}
```
