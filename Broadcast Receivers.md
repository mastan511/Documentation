# Broadcast Receivers
In this chapter you learn about broadcasts and broadcast receivers. *Broadcasts* are messaging components used for communicating across different apps, and also with the Android system, when an event of interest occurs. *Broadcast receivers* are the components in your Android app that listen for these events and respond accordingly.


### Broadcasts
Broadcasts are messages that the Android system and Android apps send when events occur that might affect the functionality of other apps. For example, the Android system sends an event when the system boots up, when power is connected or disconnected, and when headphones are connected or disconnected. Your Android app can also broadcast events, for example when new data is downloaded.

In general, broadcasts are messaging components used for communicating across apps when events of interest occur. There are two types of broadcasts:

- **System broadcasts** are delivered by the system.
- **Custom broadcasts** are delivered by your app.


### System broadcasts
A system broadcast is a message that the Android system sends when a system event occurs. System broadcasts are wrapped in Intent objects. The intent object's action field contains event details such as **android.intent.action.HEADSET_PLUG**, which is sent when a wired headset is connected or disconnected. The intent can also contain more data about the event in its extra field, for example a boolean extra indicating whether a headset is connected or disconnected.

Examples:

- When the device boots, the system broadcasts a system Intent with the action **ACTION_BOOT_COMPLETED**.
- When the device is disconnected from external power, the system sends a system Intent with the action field **ACTION_POWER_DISCONNECTED**.

System broadcasts aren't targeted at specific recipients. Interested apps must register a component to "listen" for these events. This listening component is called a broadcast receiver.

As the Android system evolves, significant changes are made to improve the system's performance. For example, starting from Android 7.0, the system broadcast actions ACTION_NEW_PICTURE and ACTION_NEW_VIDEO are not supported. Apps can no longer receive broadcasts about these actions, regardless of the targetSDK version on the device. This is because device cameras take pictures and record videos frequently, so sending a system broadcast every time one of these actions occurred would strain a device's memory and battery.

To get the complete list of broadcast actions that the system can send for a particular SDK version, check the **broadcast_actions.txt** file in your SDK folder, at the following path: **Android/sdk/platforms/android-xx/data**, where xx is the SDK version.

### Custom broadcasts
Custom broadcasts are broadcasts that your app sends out. Use a custom broadcast when you want your app to take an action without launching an activity. For example, use a custom broadcast when you want to let other apps know that data has been downloaded to the device and is available for them to use. More than one broadcast receiver can be registered to receive your broadcast.

To create a custom broadcast, define a custom Intent action.

**Note:** 
When you specify the action for the Intent, use your unique package name (for example com.example.myproject) to make sure that your intent doesn't conflict with an intent that is broadcast from a different app or from the Android system.

There are three ways to deliver a custom broadcast:

- For a normal broadcast, pass the intent to **sendBroadcast()**.
- For an ordered broadcast, pass the intent to **sendOrderedBroadcast()**.
- For a local broadcast, pass the intent to **LocalBroadcastManager.sendBroadcast()**.

Normal, ordered, and local broadcasts are described in more detail below.

### Normal broadcasts
The **sendBroadcast()** method sends broadcasts to all the registered receivers at the same time, in an undefined order. This is called a normal broadcast. A normal broadcast is the most efficient way to send a broadcast. With normal broadcasts, receivers can't propagate the results among themselves, and they can't cancel the broadcast.

The following method sends a normal broadcast to all interested broadcast receivers:

```java
public void sendBroadcast() {
   Intent intent = new Intent();
   intent.setAction("com.example.myproject.ACTION_SHOW_TOAST");
   // Set the optional additional information in extra field.
   intent.putExtra("data","This is a normal broadcast");
   sendBroadcast(intent);
}
```
### Ordered broadcasts
To send a broadcast to one receiver at a time, use the **sendOrderedBroadcast()** method:

- The android:priority attribute that's specified in the intent filter determines the order in which the broadcast is sent.
- If more than one receiver with same priority is present, the sending order is random.
- The Intent is propagated from one receiver to the next.
- During its turn, a receiver can update the Intent, or it can cancel the broadcast. (If the receiver cancels the broadcast, the Intent can't be propagated further.)


For example, the following method sends an ordered broadcast to all interested broadcast receivers:
```java
public void sendOrderedBroadcast() {
   Intent intent = new Intent();

   // Set a unique action string prefixed by your app package name.
   intent.setAction("com.example.myproject.ACTION_NOTIFY");
   // Deliver the Intent.
   sendOrderedBroadcast(intent);
}
```
### Local broadcasts
If you don't need to send broadcasts to a different app, use the **LocalBroadcastManager.sendBroadcast()** method, which sends broadcasts to receivers within your app. This method is efficient, because it doesn't involve interprocess communication. Also, using local broadcasts protects your app against some security issues.

To send a local broadcast:

1. To get an instance of LocalBroadcastManager, call getInstance() and pass in the application context.
2. Call sendBroadcast() on the instance. Pass in the intent that you want to broadcast.
```java
 LocalBroadcastManager.getInstance(this).sendBroadcast(customBroadcastIntent);
```

