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



