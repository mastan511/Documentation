# ViewModel and LiveData
## ViewModel
The ViewModel class is designed to store and manage UI-related data in a lifecycle conscious way. The ViewModel class allows data to survive configuration changes such as screen rotations.

The Android framework manages the lifecycles of UI controllers, such as activities and fragments. The framework may decide to destroy or re-create a UI controller in response to certain user actions or device events that are completely out of your control.

If the system destroys or re-creates a UI controller, any transient UI-related data you store in them is lost. For example, your app may include a list of users in one of its activities. When the activity is re-created for a configuration change, the new activity has to re-fetch the list of users. For simple data, the activity can use the onSaveInstanceState() method and restore its data from the bundle in onCreate(), but this approach is only suitable for small amounts of data that can be serialized then deserialized, not for potentially large amounts of data like a list of users or bitmaps.

Another problem is that UI controllers frequently need to make asynchronous calls that may take some time to return. The UI controller needs to manage these calls and ensure the system cleans them up after it's destroyed to avoid potential memory leaks. This management requires a lot of maintenance, and in the case where the object is re-created for a configuration change, it's a waste of resources since the object may have to reissue calls it has already made.

UI controllers such as activities and fragments are primarily intended to display UI data, react to user actions, or handle operating system communication, such as permission requests. Requiring UI controllers to also be responsible for loading data from a database or network adds bloat to the class. Assigning excessive responsibility to UI controllers can result in a single class that tries to handle all of an app's work by itself, instead of delegating work to other classes. Assigning excessive responsibility to the UI controllers in this way also makes testing a lot harder.

It's easier and more efficient to separate out view data ownership from UI controller logic.

## Implement a ViewModel

Architecture Components provides ViewModel helper class for the UI controller that is responsible for preparing data for the UI. ViewModel objects are automatically retained during configuration changes so that data they hold is immediately available to the next activity or fragment instance. For example, if you need to display a list of users in your app, make sure to assign responsibility to acquire and keep the list of users to a ViewModel, instead of an activity or fragment, as illustrated by the following sample code:

```java
public class MyViewModel extends ViewModel {
    private MutableLiveData<List<User>> users;
    public LiveData<List<User>> getUsers() {
        if (users == null) {
            users = new MutableLiveData<List<User>>();
            loadUsers();
        }
        return users;
    }

    private void loadUsers() {
        // Do an asynchronous operation to fetch users.
    }
}
```
You can then access the list from an activity as follows:
```java
public class MyActivity extends AppCompatActivity {
    public void onCreate(Bundle savedInstanceState) {
        // Create a ViewModel the first time the system calls an activity's onCreate() method.
        // Re-created activities receive the same MyViewModel instance created by the first activity.

        MyViewModel model = new ViewModelProvider(this).get(MyViewModel.class);
        model.getUsers().observe(this, users -> {
            // update UI
        });
    }
}
```
If the activity is re-created, it receives the same MyViewModel instance that was created by the first activity. When the owner activity is finished, the framework calls the ViewModel objects's onCleared() method so that it can clean up resources.

ViewModel objects are designed to outlive specific instantiations of views or LifecycleOwners. This design also means you can write tests to cover a ViewModel more easily as it doesn't know about view and Lifecycle objects. ViewModel objects can contain LifecycleObservers, such as LiveData objects. However ViewModel objects must never observe changes to lifecycle-aware observables, such as LiveData objects. If the ViewModel needs the Application context, for example to find a system service, it can extend the AndroidViewModel class and have a constructor that receives the Application in the constructor, since Application class extends Context.

## The lifecycle of a ViewModel

![](https://developer.android.com/images/topic/libraries/architecture/viewmodel-lifecycle.png)




## Starter App for ViewModel Example

[Please download and import this starter App code Into your Android Studio](https://github.com/mastan511/ViewModelandLiveData-startercode/tree/master/ViewModelWithLiveData_starter)






**OutPut of the above starter code**


**In Potraite Mode**


<img src="https://raw.githubusercontent.com/mastan511/MastanImages/master/v1.png"  height=300 > 



**LandScape Mode**



<img src="https://raw.githubusercontent.com/mastan511/MastanImages/master/v2.png" width=400 >

- Now we can see by using ViewModel how to save the data which is available on the screen in potraite mode in landscape mode also.

## ViewModel Implementation.
- First we should have add the dependency
```gradle
def lifecycle_version = "2.2.0"

    // ViewModel
    implementation "androidx.lifecycle:lifecycle-viewmodel:$lifecycle_version"
  
```
- Lets create a one java class with the name of MainViewModel.java and this class must be extended with ViewModel class.

```java
import androidx.lifecycle.ViewModel;

public class MainViewModel extends ViewModel {
    
    public MainViewModel() {
        Log.i("MainViewModel","ViewModel is created");
    }

    @Override
    protected void onCleared() {
        super.onCleared();
        Log.i("MainViewModel","ViewModel is destroyed");
    }
    
}

```
- Lets call that ViewModel class into the MainActivity.java
```java

public class MainActivity extends AppCompatActivity {

    private int count = 0;
    private TextView count_tv;
    private MainViewModel mvm;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        //Initialize the viewmodel
        mvm = new ViewModelProvider(this).get(MainViewModel.class);
        Log.i("MainActivity","MainViewModel is initialized");
        count_tv = findViewById(R.id.count_textView); // Connecting the count_textView with count_tv TextView Instance
    }

```
- now you can run the app check the logcat messages in both(Potraite and Landscape) Modes. When you rotate ypur device from potraite to landscape the ViewModel class id reinitialized that means it will not create the ViewModel again.

![](https://raw.githubusercontent.com/mastan511/MastanImages/master/viewModellogcat.PNG)


**MainViewModel.Java**

```java
package com.example.viewmodelandlivedata;

import android.util.Log;

import androidx.lifecycle.MutableLiveData;
import androidx.lifecycle.ViewModel;

public class MainViewModel extends ViewModel {
    public int count = 0;
    public MainViewModel() {
        Log.i("MainViewModel","ViewModel is created");
    }

    @Override
    protected void onCleared() {
        super.onCleared();
        Log.i("MainViewModel","ViewModel is destroyed");
    }

   public void increment(){
        count++;
    }
    public void decreament(){
        count--;
    }

}

```
**MainActivity.Java
```java
package com.example.viewmodelandlivedata;

import androidx.appcompat.app.AppCompatActivity;
import androidx.lifecycle.ViewModelProvider;

import android.os.Bundle;
import android.util.Log;
import android.view.View;
import android.widget.TextView;

public class MainActivity extends AppCompatActivity {


    private TextView count_tv;
    private MainViewModel mvm;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        //Initialize the viewmodel
        mvm = new ViewModelProvider(this).get(MainViewModel.class);
        Log.i("MainActivity","MainViewModel is initialized");
        count_tv = findViewById(R.id.count_textView); // Connecting the count_textView with count_tv TextView Instance
        count_tv.setText(String.valueOf(mvm.count));
    }

    /*The Following method, when invoked, increases the value on the
     * textview by 1.*/
    public void increment(View view)
    {
        //count++;
        mvm.increment();
        count_tv.setText(String.valueOf(mvm.count));
        //reducing the current value by 1
        //count_tv.setText(String.valueOf(count));// Setting the value of Count on the count textview on UI.
        // String.value() - converts the type of count value from Integer to String
    }

    /*The Following method, when invoked, decreases the value on the
     * textview by 1.*/
    public void decrement(View view)
    {
        mvm.decreament();
        count_tv.setText(String.valueOf(mvm.count));
        //count--;                                 // reducing the current value by 1
        //count_tv.setText(String.valueOf(count)); // Setting the value of Count on the count textview on UI.
        // String.value() - converts the type of count value from Integer to String
    }
}

```
**Potraite Mode Output:**

<img src='https://raw.githubusercontent.com/mastan511/MastanImages/master/v1.png' height=300>

**LandScape Mode Ouput:**

<img src='https://raw.githubusercontent.com/mastan511/MastanImages/master/vl.png' width=400>

## LiveData

LiveData is an observable data holder class. Unlike a regular observable, LiveData is lifecycle-aware, meaning it respects the lifecycle of other app components, such as activities, fragments, or services. This awareness ensures LiveData only updates app component observers that are in an active lifecycle state.

LiveData considers an observer, which is represented by the Observer class, to be in an active state if its lifecycle is in the STARTED or RESUMED state. LiveData only notifies active observers about updates. Inactive observers registered to watch LiveData objects aren't notified about changes.

You can register an observer paired with an object that implements the LifecycleOwner interface. This relationship allows the observer to be removed when the state of the corresponding Lifecycle object changes to DESTROYED. This is especially useful for activities and fragments because they can safely observe LiveData objects and not worry about leaks—activities and fragments are instantly unsubscribed when their lifecycles are destroyed.

### The advantages of using LiveData

**Ensures your UI matches your data state**
LiveData follows the observer pattern. LiveData notifies Observer objects when the lifecycle state changes. You can consolidate your code to update the UI in these Observer objects. Instead of updating the UI every time the app data changes, your observer can update the UI every time there's a change.
**No memory leaks**
Observers are bound to Lifecycle objects and clean up after themselves when their associated lifecycle is destroyed.
**No crashes due to stopped activities**
If the observer's lifecycle is inactive, such as in the case of an activity in the back stack, then it doesn’t receive any LiveData events.
**No more manual lifecycle handling**
UI components just observe relevant data and don’t stop or resume observation. LiveData automatically manages all of this since it’s aware of the relevant lifecycle status changes while observing.
**Always up to date data**
If a lifecycle becomes inactive, it receives the latest data upon becoming active again. For example, an activity that was in the background receives the latest data right after it returns to the foreground.
**Proper configuration changes**
If an activity or fragment is recreated due to a configuration change, like device rotation, it immediately receives the latest available data.
**Sharing resources**
You can extend a LiveData object using the singleton pattern to wrap system services so that they can be shared in your app. The LiveData object connects to the system service once, and then any observer that needs the resource can just watch the LiveData object. For more information, see Extend LiveData.

### Implementation of LiveData.
- First we should have to add the dependency.
```gradle
 def lifecycle_version = "2.2.0"
    // LiveData
    implementation "androidx.lifecycle:lifecycle-livedata:$lifecycle_version"

```
**MainViewModel.Java**
```java
package com.example.viewmodelandlivedata;

import android.util.Log;

import androidx.lifecycle.MutableLiveData;
import androidx.lifecycle.ViewModel;

public class MainViewModel extends ViewModel {
    //public int count = 0;
    MutableLiveData<Integer> count = new MutableLiveData<>();
    public MainViewModel() {
        Log.i("MainViewModel","ViewModel is created");
        count.setValue(0);
    }

    @Override
    protected void onCleared() {
        super.onCleared();
        Log.i("MainViewModel","ViewModel is destroyed");
    }

   public void increment(){
        count.setValue(count.getValue()+1);
    }
    public void decreament(){
        count.setValue(count.getValue()-1);
    }

}


```
**MainActivity.Java**
```java
package com.example.viewmodelandlivedata;

import androidx.appcompat.app.AppCompatActivity;
import androidx.lifecycle.Observer;
import androidx.lifecycle.ViewModelProvider;

import android.os.Bundle;
import android.util.Log;
import android.view.View;
import android.widget.TextView;

public class MainActivity extends AppCompatActivity {


    private TextView count_tv;
    private MainViewModel mvm;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        //Initialize the viewmodel
        mvm = new ViewModelProvider(this).get(MainViewModel.class);
        Log.i("MainActivity","MainViewModel is initialized");
        count_tv = findViewById(R.id.count_textView); // Connecting the count_textView with count_tv TextView Instance
        //count_tv.setText(String.valueOf(mvm.count));
        mvm.count.observe(this, new Observer<Integer>() {
            @Override
            public void onChanged(Integer integer) {
                count_tv.setText(String.valueOf(integer));

            }
        });
    }

    /*The Following method, when invoked, increases the value on the
     * textview by 1.*/
    public void increment(View view)
    {
        //count++;
        mvm.increment();
        //count_tv.setText(String.valueOf(mvm.count));
        //reducing the current value by 1
        //count_tv.setText(String.valueOf(count));// Setting the value of Count on the count textview on UI.
        // String.value() - converts the type of count value from Integer to String
    }

    /*The Following method, when invoked, decreases the value on the
     * textview by 1.*/
    public void decrement(View view)
    {
        mvm.decreament();
        //count_tv.setText(String.valueOf(mvm.count));
        //count--;                                 // reducing the current value by 1
        //count_tv.setText(String.valueOf(count)); // Setting the value of Count on the count textview on UI.
        // String.value() - converts the type of count value from Integer to String
    }
}

```




