## Room, LiveData, and ViewModel

### What are Android Architecture Components?

<br>
<p align="center">
<img  src="https://github.com/mastan511/MastanImages/blob/master/architecture_components_logo.png" width = "250dp">
</p>
<br>

The Android OS manages resources aggressively to perform well on a huge range of devices, and sometimes that makes it challenging to build robust apps. [Android Architecture Components](https://developer.android.com/topic/libraries/architecture/index.html) provide guidance on app architecture, with libraries for common tasks like lifecycle management and data persistence.

Architecture Components help you structure your app in a way that is robust, testable, and maintainable with less boilerplate code. Architecture Components provide a simple, flexible, and practical approach that frees you from some common problems so you can focus on building great experiences.

### Recommended Architecture Components

To introduce the terminology, here is a short introduction to the Architecture Components and how they work together. Each component is explained more in the following sections. This diagram shows a basic form of this architecture.

<br>
<p align="center">
<img  src="https://github.com/mastan511/MastanImages/blob/master/dg_architecture_comonents.png">
</p>
<br>

* **_Entity_**: When working with Architecture Components, the entity is an annotated class that describes a database table.

* **_SQLite database_**: On the device, data is stored in an SQLite database. For simplicity, additional storage options, such as a web server, are omitted from this chapter. The [Room persistence library](https://developer.android.com/training/data-storage/room/index.html) creates and maintains this database for you.

* **_DAO_**: Data access object. A mapping of SQL queries to functions. You used to have to define these queries in a helper class, such as [SQLiteOpenHelper](https://developer.android.com/reference/android/database/sqlite/SQLiteOpenHelper.html). When you use a DAO, you call the methods, and the components take care of the rest.

* **_Room database_**: Database layer on top of SQLite database that takes care of mundane tasks that you used to handle with a helper class, such as SQLiteOpenHelper. Provides easier local data storage. The Room database uses the DAO to issue queries to the SQLite database.

* **_Repository_**: A class that you create for managing multiple data sources, for example using the WordRepository class.

* **_ViewModel_**: Provides data to the UI and acts as a communication center between the Repository and the UI. Hides the backend from the UI. ViewModel instances survive device configuration changes.

* **_LiveData_**: A data holder class that follows the [observer pattern](https://en.wikipedia.org/wiki/Observer_pattern), which means that it can be observed. Always holds/caches latest version of data. Notifies its observers when the data has changed. LiveData is lifecycle aware. UI components observe relevant data. LiveData automatically manages stopping and resuming observation, because it's aware of the relevant lifecycle status changes.

### RoomWordSample architecture overview

The following diagram shows the same basic architecture form as the diagram above, but in the context of an app. The following sections delve deeper into each component.

<br>
<p align="center">
<img  src="https://github.com/mastan511/MastanImages/blob/master/dg_app_architecture.png">
</p>
<br>

### Implementation Practical:

### Gradle files

To use the Architecture Components libraries, you must manually add the latest version of the libraries to your Gradle files.

Add the following code to your build.gradle (Module: app) file, at the end of the dependencies block and Sync Now:

```gradle
    // Room components
    implementation "androidx.room:room-runtime:2.2.1"
    annotationProcessor "androidx.room:room-compiler:2.2.1"
    androidTestImplementation "androidx.room:room-testing:2.2.1"

    // Lifecycle components
    implementation "androidx.lifecycle:lifecycle-extensions:2.2.0"
    annotationProcessor "androidx.lifecycle:lifecycle-compiler:2.2.0"
```

### Entity

The data for an app with a database centers around the data stored in the database. Of course, there can be other data from different sources, but for simplicity, this discussion ignores other data.

<br>
<p align="center">
<img  src="https://github.com/mastan511/MastanImages/blob/master/dg_app_architecture_entity.png">
</p>
<br>

Room models an SQLite database, and is implemented with an SQLite database as its backend. SQLite databases store their data in tables of rows and columns. Each row represents one entity (or record), and each column represents a value in that entity. For example, an entity for a dictionary entry might include a unique ID, the word, a definition, grammatical information, and links to more info.

Another common entity is for a person, with a unique ID, first name, last name, email address, and demographic information. The entities in such a database might look like this:


| ID  | First name| Last name |
| ------------- | ------------- | ------- |
| 12345  | Sai  | Sankar|
| 12346 | Masthan  | Vali |

When you write an app using Architecture Components (or an SQLite database), you define your entity or entities in classes. Each instance of the class represents a row, and each column is represented by a member variable.

Here is code representing a Person entity:

```java
public class Student {
    String rollNumber;
    String Mailid;
    String phoneNumber;
    String name;
}
```

### Entity annotations

For Room to work with an entity, you need to give Room information that relates the contents of the entity's Java class (for example Person) to what you want to represent in the database table. You do this using annotations.

Here are some commonly used annotations:

* _@Entity(tableName ="word_table")_ Each @Entity instance represents an entity in a table. Specify the name of the table if you want it to be different from the name of the class.
* _@PrimaryKey_ Every entity needs a primary key. To [_auto-generate_](https://developer.android.com/reference/android/arch/persistence/room/PrimaryKey.html) a unique key for each entity, add and annotate a primary integer key with autoGenerate=true, as shown in the code below. See [Defining data using Room entities].
* @NonNull Denotes that a parameter, field, or method return value can never be null. The primary key should always use the @NonNull annotation. Use this annotation for mandatory fields in your rows.
* @ColumnInfo(name ="word") Specify the name of the column in the table, if you want the column name to be different from the name of the member variable.

Every field that's stored in the database must either be public or have a "getter" method so that Room can access it. The code below provides a getWord() "getter" method rather than exposing member variables directly.

Here is annotated code representing a _Student_ entity:

```java
@Entity(tableName = "StudentInfo")
public class Student {

    @PrimaryKey @NonNull
    String rollNumber;

    @ColumnInfo(name = "StudentMailId")
    String Mailid;
    String phoneNumber;

    @ColumnInfo(name = "StudentName")
    String name;
}
```
You can also use annotations to define relationships between entities. For details and a complete list of annotations, see the Room package summary reference.






















