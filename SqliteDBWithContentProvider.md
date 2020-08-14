# Sqlite Database

SQLite is an open-source relational database i.e. used to perform database operations on android devices such as storing, manipulating or retrieving persistent data from the database.

It is embedded in android bydefault. So, there is no need to perform any database setup or administration task.

Here, we are going to see the example of sqlite to store and fetch the data. Data is displayed in the TextView. 


**SQLiteOpenHelper** class provides the functionality to use the SQLite database.

## SQLiteOpenHelper class
The android.database.sqlite.SQLiteOpenHelper class is used for database creation and version management. For performing any database operation, you have to provide the implementation of onCreate() and onUpgrade() methods of SQLiteOpenHelper class.

### Constructors of SQLiteOpenHelper class
There are two constructors of SQLiteOpenHelper class.

Constructor|Description
-----------|------------
SQLiteOpenHelper(Context context, String name, SQLiteDatabase.CursorFactory factory, int version)|creates an object for creating, opening and managing the database.
SQLiteOpenHelper(Context context, String name, SQLiteDatabase.CursorFactory factory, int version, DatabaseErrorHandler errorHandler)|creates an object for creating, opening and managing the database. It specifies the error handler.

### Methods of SQLiteOpenHelper class
There are many methods in SQLiteOpenHelper class. Some of them are as follows:
Method|Description
------|-----------
public abstract void onCreate(SQLiteDatabase db)|called only once when database is created for the first time.
public abstract void onUpgrade(SQLiteDatabase db, int oldVersion, int newVersion)|called when database needs to be upgraded.
public synchronized void close ()|closes the database object.
public void onDowngrade(SQLiteDatabase db, int oldVersion, int newVersion)|called when database needs to be downgraded.

## SQLiteDatabase class

It contains methods to be performed on sqlite database such as create, update, delete, select etc.

### Methods of SQLiteDatabase class
There are many methods in SQLiteDatabase class. Some of them are as follows:
Method|Description
------|------------
void execSQL(String sql)|executes the sql query not select query.
long insert(String table, String nullColumnHack, ContentValues values)|inserts a record on the database. The table specifies the table name, nullColumnHack doesn't allow completely null values. If second argument is null, android will store null values if values are empty. The third argument specifies the values to be stored.
int update(String table, ContentValues values, String whereClause, String[] whereArgs)|updates a row.
Cursor query(String table, String[] columns, String selection, String[] selectionArgs, String groupBy, String having, String orderBy)|returns a cursor over the resultset.

## Example of android SQLite database
Let's see the simple example of android sqlite database.

<img src='https://raw.githubusercontent.com/mastan511/MastanImages/master/Sqlite.PNG'>

- In the above diagram we have taken two values i.e Name and Mobileno, Our requirement is we should have to store the Name and mobileno in the sqlite database.
- let's create a new Android Studio project with the name of **SqliteDbExample**
- Take a **Empty Activity**
- Design the screen as per the above diagram 

**activity_main.xml**

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    tools:context=".MainActivity">

    <EditText
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="Enter your name"
        android:id="@+id/nameEditText"
        />
    <EditText
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="Enter your Mobileno"
        android:id="@+id/mobileEditText"
        />
    
    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="horizontal"
        android:gravity="center"
        android:layout_marginTop="10dp" 
        >
        <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="save"
            android:id="@+id/saveButton"
            />

        <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Retrive"
            android:id="@+id/retriveButton"
            />
    </LinearLayout>
    <ScrollView
        android:layout_width="match_parent"
        android:layout_height="match_parent">
    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Result"
        android:textSize="25sp"
        android:textStyle="bold"
        android:textColor="#000000"
        android:id="@+id/resultTextview"
        />
    </ScrollView>
</LinearLayout>

```
**MainActivity.Java**

```java
import androidx.appcompat.app.AppCompatActivity;

import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;
import android.widget.TextView;

public class MainActivity extends AppCompatActivity implements View.OnClickListener {

    EditText et_name,et_mobileno;
    TextView tv_result;
    Button saveButton,retriveButton;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        et_name = findViewById(R.id.nameEditText);
        et_mobileno = findViewById(R.id.mobileEditText);
        tv_result = findViewById(R.id.resultTextview);
        saveButton = findViewById(R.id.saveButton);
        retriveButton = findViewById(R.id.retriveButton);
    }

    @Override
    public void onClick(View view) {
        switch (view.getId()){
            case R.id.saveButton:
                saveData();
                break;
            case R.id.retriveButton:
                retriveData();
                break;
        }
        
    }
    
    private void saveData() {
        String name = et_name.getText().toString();
        String mobileno = et_mobileno.getText().toString();
    }
    
    private void retriveData() {
    }
    
}

```
- It's time to create a **DbHelper** class extends with **SqliteOpenHelper** class
- After creating the class implement the required methods i.e **onCrete()** and **onUpgrade()**
- And also implement the Constructor mathing with super class,

**DbHelper.Java**
```java
import android.content.Context;
import android.database.sqlite.SQLiteDatabase;
import android.database.sqlite.SQLiteOpenHelper;

import androidx.annotation.Nullable;

public class DbHelper extends SQLiteOpenHelper {
    
    public DbHelper(@Nullable Context context, @Nullable String name, @Nullable SQLiteDatabase.CursorFactory factory, int version) {
        super(context, name, factory, version);
    }

    @Override
    public void onCreate(SQLiteDatabase sqLiteDatabase) {

    }

    @Override
    public void onUpgrade(SQLiteDatabase sqLiteDatabase, int i, int i1) {

    }
}

```










