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
        saveButton.setOnClickListener(this);
        retriveButton.setOnClickListener(this);
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
- Let's create some static constant String varibles and int varibles in the DbHelper class.

```java
    public static final String DB_NAME="SqlDb";
    public static final String TABLE_NAME="users";
    public static final String COL_0="id";
    public static final String COL_1="name";
    public static final String COL_2="mobileno";
    public static final int VERSION=1;
    public static final String TABLE_QUERY = "create table if not exists "+TABLE_NAME+"("+COL_0+" integer primary key autoincrement,"+COL_1+" varchar(200),"+COL_2+" varchar(10));";
```

**ContentValues Class**
ContentValue class lets you put information inside an object in the form of Key-Value pairs for columns and their value. The object can then be passed to the insert() method of an instance of the SQLiteDatabase class to insert or update your WritableDatabase.

**Cursor Class**
Cursor on the other hand, is a kind of a pointer(not the C/C++ one) which you get after calling the query() method on an instance of a ReadableDatabase of the SQLiteDatabase class. The cursor points to individual rows retuned after querying the database and you can use several methods like getInt(), getString() etc to extract information from specific columns of the current row. You can also traverse rows using methods like moveToNext(), moveToFirst() etc on the cursor object

- let's add two methods in the DBHelper class for,to insert the data into the table and retrive the data from the table. i.e **insertMyData()** and **retriveMydata()**

```java
public long insertMyData(ContentValues contentValues){
        SQLiteDatabase db = getWritableDatabase();
        long i = db.insert(TABLE_NAME,null,contentValues);
        return i;
    }

    public Cursor retriveMydata(){
        SQLiteDatabase db = getReadableDatabase();
        Cursor c = db.rawQuery("SELECT *FROM "+TABLE_NAME,null);
        return c;
    }

```
- Now its time to call that DbHelper class into the MainActivity.Java 
```java

DbHelper helper = new DbHelper(this);

```
- when we click the saveButton it will invokes **saveData()** method, So now we should have to assign users data to the ContentValues class object. After that we can send send our ContentValues calss object to the **insertMyData()** which is available in the DbHelper class.
```java

  private void saveData() {
        String name = et_name.getText().toString();
        String mobileno = et_mobileno.getText().toString();
        ContentValues values=new ContentValues();
        values.put(DbHelper.COL_1,name);
        values.put(DbHelper.COL_2,mobileno);
        long i = helper.insertMyData(values);
        if(i>0){
            Toast.makeText(this, 
                    "Data inserted Successfully", Toast.LENGTH_SHORT).show();
        }else{
            Toast.makeText(this, 
                    "Insertion Failed", Toast.LENGTH_SHORT).show();
        }

    }

```
- Now we should have to retrive the data from the table

```java

private void retriveData() {
        Cursor c = helper.retriveMydata();
        StringBuilder builder = new StringBuilder();
        while (c.moveToNext()){
            builder.append(c.getInt(0)+" "+c.getString(1)+" "+c.getString(2)+"\n");
        }
        tv_result.setText(builder.toString());
    }


```

# ContentProvider

















