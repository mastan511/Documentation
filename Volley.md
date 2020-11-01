# Volley Library in Android
Volley is an HTTP library that makes networking very easy and fast, for Android apps. It was developed by Google and introduced during Google I/O 2013. It was developed because there is an absence in Android SDK, of a networking class capable of working without interfering with the user experience. Although Volley is a part of the Android Open Source Project(AOSP), Google announced in January 2017 that Volley will move to a standalone library. It manages the processing and caching of network requests and it saves developers valuable time from writing the same network call/cache code again and again.

Volley is not suitable for large download or streaming operations since Volley holds all responses in memory during parsing.

## Features of Volley:

- Request queuing and prioritization
- Effective request cache and memory management
- Extensibility and customization of the library to our needs
- Cancelling the requests

## Advantages of using Volley:
- All the task that need to be done with Networking in Android, can be done with the help of Volley.
- Automatic scheduling of network requests.
- Catching
- Multiple concurrent network connections.
- Cancelling request API.
- Request prioritization.
- Volley provides debugging and tracing tools.

## How to Import Volley and add Permissions:

Before getting started with Volley, one needs to import Volley and add permissions in the Android Project. The steps to do so are as follows:
Open build.gradle(Module: app) and add the following dependency:
```gradle
dependencies{ 
    //...
   implementation 'com.android.volley:volley:1.1.0'
}
```
In AndroidManifest.xml add the internet permission:

```xml 
<uses-permission
    android:name="android.permission.INTERNET />"
```
Classes in Volley Library:

### Volley has two main classes:

**Request Queue:** It is the interest one uses for dispatching requests to the network. One can make a request queue on demand if required, but typically it is created early on, at startup time, and keep it around and use it as a Singleton.


**Request:** All the necessary information for making web API call is stored in it. It is the base for creating network requests(GET, POST).

#### Codelab For Volley Library:

In this code lab we are going to create Covid19Tracker application By using the below api
[https://api.covid19api.com/](https://api.covid19api.com/)

- Lets create one new AndroidStudio project with the name of Covid19Tracker
- First we will display the countries list by using the below link
    [https://api.covid19api.com/countries](https://api.covid19api.com/countries)
    
**activity_main.xml:**
```xml

<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <androidx.recyclerview.widget.RecyclerView
        android:id="@+id/recyclerview"
        android:layout_width="409dp"
        android:layout_height="729dp"
        android:layout_marginStart="8dp"
        android:layout_marginLeft="8dp"
        android:layout_marginTop="50dp"
        android:layout_marginEnd="8dp"
        android:layout_marginRight="8dp"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />
</androidx.constraintlayout.widget.ConstraintLayout>
```

**MainActivity.Java**
```java
package com.example.covid19tracker;

import androidx.appcompat.app.AppCompatActivity;
import androidx.recyclerview.widget.LinearLayoutManager;
import androidx.recyclerview.widget.RecyclerView;

import android.os.Bundle;
import android.util.Log;
import android.widget.Toast;

import com.android.volley.Request;
import com.android.volley.RequestQueue;
import com.android.volley.Response;
import com.android.volley.VolleyError;
import com.android.volley.toolbox.JsonArrayRequest;
import com.android.volley.toolbox.StringRequest;
import com.android.volley.toolbox.Volley;
import com.google.gson.Gson;
import com.google.gson.GsonBuilder;

import java.util.Arrays;
import java.util.List;

public class MainActivity extends AppCompatActivity {

    private RequestQueue mRequestQueue;
    String countriesURl = "https://api.covid19api.com/countries";
    private Gson gson;
    RecyclerView rv;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        rv = findViewById(R.id.recyclerview);
        mRequestQueue = Volley.newRequestQueue(this);
        gson = new GsonBuilder().create();
        StringRequest request = new StringRequest(Request.Method.GET, countriesURl, new Response.Listener<String>() {
            @Override
            public void onResponse(String response) {
                Toast.makeText(MainActivity.this,
                        "" + response, Toast.LENGTH_SHORT).show();
                List<Country> countryList = Arrays.asList(gson.fromJson(response,Country[].class));
                Log.i("SIZE",""+countryList.size());
                rv.setLayoutManager(new LinearLayoutManager(MainActivity.this));
                rv.setAdapter(new CountryAdapter(countryList,MainActivity.this));


            }
        }, new Response.ErrorListener() {
            @Override
            public void onErrorResponse(VolleyError error) {

            }
        });

        mRequestQueue.add(request);
    }


}

```
**Country.Java**
```java
package com.example.covid19tracker;

import com.google.gson.annotations.SerializedName;

public class Country {

    @SerializedName("Country")
    private String countryname;

    @SerializedName("ISO2")
    private String countrycode;

    public Country(String countryname, String countrycode) {
        this.countryname = countryname;
        this.countrycode = countrycode;
    }

    public String getCountryname() {
        return countryname;
    }

    public void setCountryname(String countryname) {
        this.countryname = countryname;
    }

    public String getCountrycode() {
        return countrycode;
    }

    public void setCountrycode(String countrycode) {
        this.countrycode = countrycode;
    }
}

```
**country_item.xml**
```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="wrap_content">

    <TextView
        android:id="@+id/country_name"
        android:layout_width="0dp"
        android:layout_height="50dp"
        android:layout_marginStart="16dp"
        android:layout_marginLeft="16dp"
        android:layout_marginTop="5dp"
        android:layout_marginEnd="16dp"
        android:layout_marginRight="16dp"
        android:background="#4B0857"
        android:gravity="center"
        android:text="TextView"
        android:textColor="#FFF"
        android:textSize="20sp"
        android:textStyle="bold"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />
</androidx.constraintlayout.widget.ConstraintLayout>
```
**CountryAdapter.java**
```java
package com.example.covid19tracker;

import android.content.Context;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;
import android.widget.TextView;

import androidx.annotation.NonNull;
import androidx.recyclerview.widget.RecyclerView;

import java.util.List;

public class CountryAdapter extends RecyclerView.Adapter<CountryAdapter.ViewInfo> {
    List<Country> myList;
    Context ct;
    public CountryAdapter(List<Country> countryList, MainActivity mainActivity) {
        ct = mainActivity;
        myList = countryList;
    }

    @NonNull
    @Override
    public CountryAdapter.ViewInfo onCreateViewHolder(@NonNull ViewGroup parent, int viewType) {
        View v = LayoutInflater.from(ct).inflate(R.layout.country_item,parent,false);
        return new ViewInfo(v);
    }

    @Override
    public void onBindViewHolder(@NonNull CountryAdapter.ViewInfo holder, int position) {
            holder.c_name.setText(myList.get(position).getCountryname());
    }

    @Override
    public int getItemCount() {
        return myList.size();
    }

    public class ViewInfo extends RecyclerView.ViewHolder {
        TextView c_name;
        public ViewInfo(@NonNull View itemView) {
            super(itemView);
            c_name = itemView.findViewById(R.id.country_name);
        }
    }
}

```
**OutPut**
![](https://raw.githubusercontent.com/mastan511/MastanImages/master/CountriesList.png)



















