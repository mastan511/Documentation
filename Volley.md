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
    implementation 'com.android.volley:volley:1.0.0'
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
