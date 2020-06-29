# Notifications
### What is a notification?
A notification is a message your app displays to the user outside your app's normal UI. When your app tells the system to issue a notification, the notification appears to the user as an icon in the notification area, on the left side of the status bar.

<img class="center " style="max-width: 280.00px" src="https://github.com/mastan511/MastanImages/blob/master/2.png" alt=" The notification area is on the left side of the status bar, noborder" title=" The notification area is on the left side of the status bar, noborder">


If the device is unlocked, the user opens the notification drawer to see the details of the notification. If the device is locked, the user views the notification on the lock screen. The notification area, lock screen, and notification drawer are system-controlled areas that the user can view at any time.

<img class="center " style="max-width: 280.00px" src="https://github.com/mastan511/MastanImages/blob/master/3.png" alt=" The notification area is on the left side of the status bar, noborder" title=" The notification area is on the left side of the status bar, noborder">


1. An "open" notification drawer. The status bar isn't visible in this screenshot, because the notification drawer is open.

### App icon badge
In supported launchers on devices running Android 8.0 (API level 26) and higher, an app icon changes its appearance slightly when the app has a new notification to show to the user. The app icon shows a colored badge, also known as a notification dot, as shown on four of the five app icons in the screenshot below.

<img class="center" height=400 width=400 src="https://github.com/mastan511/MastanImages/blob/master/4.png" alt=" Notification badges on app icons and folders (only on API 26 or higher)" title=" Notification badges on app icons and folders (only on API 26 or higher)">

To see the notification for an app with a notification dot, the user long-presses the app icon. The notification menu appears, as shown below, and the user dismisses the notification or acts on it from the menu. This is similar to the way the user interacts with a notification in the notification drawer.

<img class="center" height=400 width=400 src="https://github.com/mastan511/MastanImages/blob/master/5.png" alt=" Notification badges on app icons and folders (only on API 26 or higher)" title=" Notification badges on app icons and folders (only on API 26 or higher)">


#### Notification channels
In the Settings app on an Android-powered device, users can adjust the notifications they receive. Starting with Android 8.0 (API level 26), you can assign each of your app's notifications to a notification channel. Each notification channel represents a type of notification, and you can group several notifications in each channel.

If you target only lower-end devices, you don't need to implement notification channels to display notifications, but it's good practice to target the latest available SDK, then check the device's SDK version before building the notification channel.

Notification channels are called Categories in the user-visible Settings app. For example, the screenshot on the left shows the notification settings for the Clock app, which has five notification channels. The screenshot on the right shows the settings for the "Firing alarms & timers" notification channel.


<img class="center " height=400 width=400 src="https://github.com/mastan511/MastanImages/blob/master/6.png" alt=" Notification settings for the Clock app and one of its notification channels (only on API 26 or higher)" title=" Notification settings for the Clock app and one of its notification channels (only on API 26 or higher)">




When you create a notification channel in your code, you set behavior for that channel, and the behavior is applied to all of the notifications in the channel. For example, your app might set the notifications in a channel to play a sound, blink a light, or vibrate. Whatever behavior you set for a notification channel, the user can change it, and they can turn off notifications from your app altogether.

         Note: If your app targets Android 8.0 (API level 26) or higher, you must implement one or more notification channels. 
         If your targetSdkVersion is set to 25 or lower,
         when your app runs on Android 8.0 (API level 26) or higher, 
         it behaves the same as it would on devices running Android 7.1 (API level 25) or lower.

### Creating a notification channel
To create a notification channel instance, use the NotificationChannel constructor. Specify an ID that's unique within your package, a user-visible channel name, and an importance for the channel:
```java
if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.O) {
    NotificationChannel notificationChannel = 
         new NotificationChannel(CHANNEL_ID, "Mascot Notification",
         NotificationManager.IMPORTANCE_DEFAULT);
}
```

          Important: Before you use the notification-channel APIs, check the SDK version. 
          Notification channels are only available in Android 8.0 (API level 26) and higher. 
          Notification channels are not available in the Android Support Library.
          
### Set the importance level
The NotificationChannel constructor, which is available in Android 8.0 (API level 26) and higher, requires an importance level. The channel's importance determines the instrusiveness of the notifications posted in that channel. For example, notifications with a higher importance might make sound and show up in more places than notifications with a lower importance. There are five importance levels, ranging from IMPORTANCE_NONE(0) to IMPORTANCE_HIGH(4).

To support Android 7.1 (API level 25) or lower, you must also set a priority for each notification. To set a priority, use the setPriority() method with a priority constant from the NotificationCompat class.
```java
mBuilder.setPriority(NotificationCompat.PRIORITY_HIGH);
```
On devices running Android 8.0 and higher, all notifications, regardless of priority and importance level, appear in the notification drawer and as app icon badges. After a notification is created and delivered, the user can change the notification channel's importance level in the Android Settings app. The following table shows how the user-visible importance level maps to the notification-channel importance level and the priority constants.


User-visible importance level | Importance (Android 8.0 and higher) | Priority (Android 7.1 and lower)
----------------------------- | ----------------------------------- | ---------------------------------     
Urgent Notifications make a sound and appear as heads-up notifications. | IMPORTANCE_HIGH | PRIORITY_HIGH or PRIORITY_MAX
High Notifications make a sound. | IMPORTANCE_DEFAULT | PRIORITY_DEFAULT
Medium Notifications make no sound. | IMPORTANCE_LOW | PRIORITY_LOW
Low Notifications make no sound and do not appear in the status bar. | IMPORTANCE_MIN | PRIORITY_M

### Configure the initial settings
Configure the notification channel object with initial settings such as an alert sound, a notification light color, and an optional user-visible description.
```java
notificationChannel.enableLights(true);
notificationChannel.setLightColor(Color.RED);
notificationChannel.enableVibration(true);
notificationChannel.setDescription("Notification from Mascot");
```
Starting from Android 8.1 (API Level 27), apps can only make a notification alert sound once per second. Alert sounds that exceed this rate aren't queued and are lost. This change doesn't affect other aspects of notification behavior, and notification messages still post as expected.

### Create the notification channel
To create the notification channel, pass the instance of NotificationChannel to the createNotificationChannel() method from the NotificationManager class.
```java
if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.O) {
NotificationManager mNotificationManager =
        (NotificationManager) getSystemService(Context.NOTIFICATION_SERVICE);
mNotifyManager.createNotificationChannel(notificationChannel);
}
```
Check the SDK version before using createNotificationChannel(), because this API is only available on Android 8.0 and higher and in support packages.

Once you create a notification and notification channel and submit it to the NotificationManager, you cannot change the importance level using code. However, the user can change their preferences for your app's channels using the Android Settings app on their device.

### Creating notifications
You create a notification using the NotificationCompat.Builder class. (NotificationCompat from the Android Support Library provides compatibility back to Android 4.0, API level 14. For more information, see Notification compatibility, below.) The builder classes simplify the creation of complex objects.

To create a NotificationCompat.Builder, pass the application context and notification channel ID to the constructor:
```java
NotificationCompat.Builder mBuilder = new NotificationCompat.Builder(this, CHANNEL_ID);
```
The NotificationCompat.Builder constructor takes the notification channel ID as one of its parameters. This parameter is only used by Android 8.0 (API level 26) and higher. Lower versions of Android ignore it.


### Set notification contents
You can assign components to the notification like a small icon, a title, and the notification message.
<img class="center " style="max-width: 435.00px" src="https://github.com/mastan511/MastanImages/blob/master/7.png" alt=" Notification contents" title=" Notification contents">
