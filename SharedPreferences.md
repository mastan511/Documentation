# Shared preferences

Shared preferences allow you to store small amounts of primitive data as key/value pairs in a file on the device. To get a handle to a preference file, and to read, write, and manage preference data, use the SharedPreferences class. The Android framework manages the shared preferences file itself. The file is accessible to all the components of your app, but it is not accessible to other apps.

For managing large amounts of data, use an SQLite database or other suitable storage option

### Shared preferences vs. saved instance state

Shared preferences | Saved instance state
------------------ | --------------------
Persists across user sessions, even if your app is stopped and restarted, or if the device is rebooted.| Preserves state data across activity instances in the same user session.
Used for data that should be remembered across user sessions, such as a user's preferred settings or their game score.| Used for data that should not be remembered across sessions, such as the currently selected tab, or any current state of an activity.
Represented by a small number of key/value pairs. | Represented by a small number of key/value pairs.
Data is private to the app. | Data is private to the app.
Common use is to store user preferences. | Common use is to recreate state after the device has been rotated.

### Writing the data into Shared Preference

If you want to work with shared prefrences first we should have to create the object for **SharedPrefrence** class.

There are Two ways to get the **SharedPrefrence** class Object:
```java
//It is used to store the multiple Activitys data into a Shared Prefrence
SharedPrefrence shpf = getSharedPreferences(sharedPrefFile, MODE_PRIVATE);
(OR)
//It is used to store single Activity Data into a Shared Prefrence
SharedPrefrence shpf = getPreference(MODE_PRIVATE);
```
### Code lab for SharedPrefrence
1. First create a one new AndroidStudio
