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

### Example app architecture

The following diagram shows the same basic architecture form as the diagram above, but in the context of an app. The following sections delve deeper into each component.

<br>
<p align="center">
<img  src="https://github.com/mastan511/MastanImages/blob/master/dg_app_architecture.png">
</p>
<br>













