# Android : Most Important Topics

## Core Building Blocks or Fundamental or Android Components:

- 🧩**Activity** - An Activity is a single screen with a user interface (UI).
   - Lifecycle: onCreate(), onStart(), onResume(), onPause(), onStop(), onDestroy()
     
- 🧩**Services** - Long running background process. or Execute background tasks without a UI

      Lifecycle of Android Services
      Types of services
        1. Foreground Services Example: music player, file download
        2. Background Services: Android 8.0 (Oreo) restrictions: Background services have limitations;
          use JobScheduler or WorkManager instead
        3. Bound Services: ongoing interaction between your UI and background operations,
       especially when multiple components need to share the same service functionality.
        
       Started Service Lifecycle 
      
               onCreate() - Called when service is first created
               onStartCommand() - Called every time startService() is invoked
               Service runs until it stops itself or is stopped by the system
               onDestroy() - Called when service is terminating
      
        Bound Service Lifecycle
         
             onCreate() - Called when service is first created
             onBind() - Called when a component wants to bind to the service
             Service runs as long as at least one component is bound to it
             onUnbind() - Called when all components have unbound
             onDestroy() - Called when service is terminating
      
- [image](https://github.com/user-attachments/assets/758abccb-a8bd-4b64-a327-3a7e5d7c156f)
      
- 🧩**Content Providers** - Used to share data between the apps
  
      - ✅ Use Content Provider when you need to share data between apps securely.
      - ✅ Use it for accessing system-wide data like Contacts, Media, or Calendar.
      - 🧩 How it Works – URI Based Access
      - Use FileProvider to share files via URIs, especially for sending images via Intent or opening PDFs.
  
- 🧩**Broadcast Receiver** -  A BroadcastReceiver is a component in Android that listens for system-events or app-specific broadcast messages (intents).
- Example: Battery low, network change, SMS received.
- App-Local Communication (Custom Broadcasts)   
- Alternatives to BroadcastReceiver
  
      WorkManager → For background tasks that must complete (e.g., syncing, file uploads).
      LiveData / Flow → For real-time data updates within the app.
      EventBus / RxJava → For app-wide event-based communication.
      JobScheduler / AlarmManager -> (For Exact Timing) , Time-sensitive tasks (e.g., alarms, exact scheduling).

## 🧩**Activity Lifecycle:**
    - **onCreate()**: Called when the activity is first created. This is where initialization occurs.
    - **onStart()**: Called when the activity becomes visible to the user.
    - **onResume()**: Called when the activity starts interacting with the user.
      This is typically where animations and other things that require CPU resources should be started.
    - **onPause()**: Called when the activity is partially obscured by another activity. 
      This is a good place to commit unsaved changes or pause ongoing processes.
    - **onStop()**: Called when the activity is no longer visible to the user.
    - **onDestroy()**: Called before the activity is destroyed. This is where you should release any resources that aren't needed anymore.
    - **OnRestart()**: Called when the activity has been stopped and is restarting again.

     Activity (A - B)
  
     - (A)- oncreate()-> onstart() -> onResume() -> OnPause()
     - (B)- oncreate()-> onstart() -> onResume() - onstop() (A)
     - (BackToA) (B) - onRestart() -> onStart() -> onResume()
      
      **Activity Launch Modes**
      Define how a new Activity instance is created:
          standard (default): Creates a new instance every time.
          singleTop: Reuses the existing instance if it’s at the top of the stack.
          singleTask: Reuses the existing instance in the task (clears activities above it).
          singleInstance: Creates a new task with only this Activity.

## 🧩**Fragment Lifecycle:**
    * Fragments are reusable UI components within activities. They have their own lifecycle methods similar to activities
    - **onAttach()**: Attaches the fragment to its hosting activity.
    - **onCreate()**: Initializes the fragment.
    - **onCreateView()**: Creates the fragment's UI.
    - **onViewCreated()**: Called after onCreateView() to initialize UI components.
    - **onActivityCreated()**: Activity's onCreate() has completed; fragment's activity is fully initialized.
    - **onStart()**: Fragment becomes visible.
    - **onResume()**: Fragment starts interacting with the user.
    - **onPause()**: Fragment no longer interacts with the user.
    - **onStop()**: Fragment is no longer visible.
    - **onDestroyView()**: Removes the fragment's UI.
    - **onDestroy()**: Releases resources before the fragment is destroyed.
    - **onDetach()**: Detaches the fragment from its hosting activity.
    
    Activity → Fragment (Send Data) - Pass data via Bundle (during fragment creation)
    Both Ways passing data - Shared ViewModel (Recommended in MVVM)

    FragmentManager Use Cases
    Adding, replacing, or removing fragments.
    Managing the back stack for fragment navigation.

    Get FragmentManager:
    From an Activity: Use supportFragmentManager.
    From a Fragment: Use parentFragmentManager or childFragmentManager (for nested fragments).


- 🧩**Intents**
  
      - It is used to communication between different components (activities, services, broadcasts, etc.).
      - It helps in launching activities, starting services, sending data,and broadcasting system events.  

  **Types of Intents in Android**
    
        Explicit Intent → Used to start a specific component (Activity, Service).
        Implicit Intent → Used to request an action from another app/component (e.g., opening a web page, sending an email).
        Broadcast Intent → Used to send system-wide messages (e.g., battery low, network change).
        Pending Intent → Used for deferred execution, like notifications and alarms.

## 🧩**Android Architecture Components or Jetpack Components:**
   - **ViewModel**: Part of the Android Architecture Components.
        It is used to store and manage UI-related data in a lifecycle-aware manner, surviving configuration changes such as screen rotations.Prevents data loss on rotation
      - **LiveData**: Observes and reacts to UI data changes. It’s lifecycle-aware, meaning it only updates active observers.
      - **Navigation Component**: Simplifies the implementation of navigation between screens and supports features like deep linking and safe arguments.
      - **Paging**: A component that allows users to load and display large data sets with infinite scrolling
      - **Jetpack Compose**: A Kotlin-based toolkit for building native UI
      - **Android KTX**: A library that increases support for the Kotlin language for app development 
      - **WorkManager**: Handles reliable background tasks (e.g., syncing, file uploads).
      - **Room**: An abstraction layer over SQLite that handles database creation and management with type-safe access to database queries.
      - **DataBinding**: Reduces findViewById() calls & connects UI elements with data
      - **ViewBinding and DataBinding:** ViewBinding is a simpler approach for binding views, while DataBinding allows more complex UI bindings, supporting data binding expressions in XML layouts.

# 🧩ViewModel Use and Features
- ViewModel class is designed to store and manage UI-related data in a lifecycle conscious way.It is the main component in the MVVM architecture.
- To create viewmodel use viewModelProvider and  if ViewModel exits in ViewModelStore is linked to ViewModelStoreOwner
- Activity/fragment provides its owner to viewmodel provider, Each viewModelStoreOwner holds its own viewModelStore  

      - Activity/Fragment
        ⬇
      [ViewModelStoreOwner]  
        ⬇
      [ViewModelStore]  ← stores all ViewModels
        ⬇
      ViewModelProvider  ← creates/fetches ViewModels

- Use ViewModel, onSaveInstanceState(), or android:configChanges in the manifest to handle configuration changes.
![image](https://github.com/user-attachments/assets/8bdee800-74b0-451c-9b0c-ea5cba670d9f)

- Without ViewModel: When the screen rotates, onCreate() is called again, and the counter resets to 0.
- With ViewModel:The same ViewModel instance is used even after rotation.The counter value is retained.

## 🧩**RecyclerView**
      - A more advanced and flexible version of ListView, It’s used to display scrollable lists (like contacts, messages, 
      or social media feeds) but with better performance and flexibility.
      - Adapter and ViewHolder patterns are key concepts for recycling views and improving performance.
      - ViewHolder improves performance by caching views  
      - DiffUtil: Used for calculating the difference between two lists and updating only the items that have changed.
      - Async Image Loading:Use image-loading libraries like Glide or Picasso to load images asynchronously, preventing UI freezes.
      - RecyclerView LinearLayoutManager, GridLayoutManager
      
      1. Key Features of RecyclerView
      ✅ View Recycling – Reuses off-screen views to save memory
      ✅ Layout Managers – Supports linear, grid, and staggered layouts
      ✅ Animations – Built-in item animations (add/remove/change)
      ✅ Flexible Adapters – Customizable data binding with RecyclerView.Adapter
      ✅ Item Decorations – Add dividers, spacing, or custom decorations
      ✅ Efficient Updates – DiffUtil for smart data changes
      
## 🧩**Networking**
     
    - Retrofit: A type-safe HTTP client for Android used for making network requests and parsing responses using converters like Gson or Moshi.
    - OkHttp Interceptors in Android are powerful tools that allow you to intercept and modify HTTP requests and responses
    - They provide a flexible way to customize network behavior, add custom headers, log requests and responses, 
       implement authentication mechanisms, and perform other network-related tasks
    - Handling errors (e.g., network failures, HTTP status codes) and using try-catch or custom error handling mechanisms.
    
    Types of Interceptors
    Application Interceptor
        Runs once per request (even if retried).
        Good for logging, adding auth headers.

    Network Interceptor
        Runs after redirections (closer to the actual network call).
        Good for monitoring raw traffic.

 Why Do We Need Interceptors?
✅ Add Headers (e.g., Auth Token)
✅ Log Requests/Responses (Debugging)
✅ Modify Responses (e.g., Caching, Error Handling)
✅ Retry Failed Requests (Auto-refresh token)   
    
    # HTTP Libraries
    Old: HttpURLConnection (Java) - Low-level, manual handling
    Modern:
      - Retrofit (Recommended) - Type-safe REST client
      - Volley - Google's HTTP library (simpler than Retrofit)
      - Ktor - Kotlin-native alternative, Supports multiplatform (Android, iOS, backend)
    JSON Parsing
      - Gson (Google) - Simple, reflection-based
      - Moshi (Square) - More efficient, Kotlin-friendly
      - kotlinx.serialization (Official Kotlin)
    

## 🧩**Data Storage:**
- SharedPreferences: Simple key-value storage for saving small amounts of primitive data.
-  ✅ two methods apply() (asynchronous) and commit() (synchronous)
- DataStore (Jetpack): Modern alternative to SharedPreferences	
- Room Database: Structured storage using SQL with support for complex queries.
- SQLite: A database option for more manual management of structured data.
- Modern Alternatives

    For SharedPreferences:
  
        DataStore (Jetpack) – Improved version with Kotlin coroutines support.
        EncryptedSharedPreferences (for secure storage).
    For ContentResolver:

        Room Database (for local structured data).
        WorkManager + REST APIs (for cloud data).

## 🧩**Dependency Injection:**

- Dagger 2: A compile-time dependency injection framework providing dependency management and injection with minimal runtime overhead.
- It's based on annotations and code generation.
- Hilt: A simpler way to integrate Dagger 2, making it easier to set up and use in Android projects.
- Koin: A lightweight dependency injection library for Kotlin that’s easy to learn and integrate.

- @Inject	Marks a dependency to be injected
- @Provides	Defines how to create a dependency
- @Module	Groups dependency providers
- @InstallIn	Specifies component lifetime
- @AndroidEntryPoint	Enables injection in Android classes

## 🧩**Multithreading and Background Work:**
-
- Handler and Looper: Used for communicating between background threads and the main thread.
- WorkManager: Manages background tasks that should run even if the app is closed or the device is rebooted.
- ![image](https://github.com/user-attachments/assets/c5b8abf1-5ab2-4c0c-b25a-99b4901d8888)

- Coroutines provide a modern approach to handle background work more efficiently compared to older methods like AsyncTask.
    
# 🧩Serializable:
  Both Serializable and Parcelable are used for object serialization in Android.
 
    Java-native interface.
    Slower due to reflection.
    Consumes more memory.

# 🧩Parcelable:

    Android-specific interface (android.os.Parcelable).
    Use Parcelable for Android inter-component communication (Activities, Fragments)
    Used manual implementation for reading/writing data, making it much faster than Serializable.
    More optimized for Android IPC (Inter-Process Communication).
    Requires manual implementation.
    Kotlin Parcelize (Simpler Parcelable) @Parcelize  // Auto-generates Parcelable code

# 🧩MVVM, MVC, MVP

    MVC: Separates UI (View), Business Logic (Controller), and Data (Model).
    MVP: Similar to MVC, but Presenter handles business logic and updates the View.
    MVVM: ViewModel holds UI-related data and logic, and updates the View through LiveData/Flow.
    
# 🧩Live Data and Flow
    What? A simple "data holder" that updates the UI only when the screen is active (lifecycle-aware).
         - For communication between components within the same app
    Why? Prevents crashes & memory leaks by auto-unsubscribing when the app closes.
    Best for: Basic UI updates (e.g., showing text, lists).

  DisAdv: LiveData does not have built-in support for error handling
  Can’t handle complex async tasks (like combining multiple data sources).
 
     // MutableLiveData is a LiveData whose value can be changed
    private val _textLiveData = MutableLiveData<String>()
    // Expose LiveData to the UI (immutable)
    val textLiveData: LiveData<String> get() = _textLiveData
    // Observe the LiveData
        viewModel.textLiveData.observe(this, Observer { newText ->
            // Update the UI with the new data
            binding.textView.text = newText
        })

# 📌 Flow, SharedFlow, and StateFlow in Android (Kotlin Coroutines)    
- Flow is a Kotlin Coroutines feature for asynchronous cold streams (emits values sequentially).
-     What - A Kotlin "data stream" for advanced async operations (like RxJava but simpler).
      Why -  Handles backpressure, supports threading, and works outside Android.
      Best for: Database/network operations, complex data transformations.

- Provides built-in support for handling errors through operators.
- Powerful (supports map, filter, merge, etc.).
- Kotlin’s Flow, SharedFlow, and StateFlow are used for handling asynchronous data streams in Android.

1️⃣ Flow – Cold Stream (On-Demand) - Starts fresh for each collector (like a YouTube video replaying from start). 
    - Used for one-time operations (e.g., network call, DB query).
2️⃣ SharedFlow – Hot Stream (Always Holds Latest Value, Events (e.g., button clicks)) 
    - Emits data even without collectors (like a live TV broadcast).
3️⃣ SteteFlow – Hot Stream (For One-Time Events Holds a single "state" value (like LiveData but for coroutines))
    - UI state (e.g., loading, success, error).

# 🧩Room 
Room is a SQLite-based persistence library in Android that provides
an abstraction layer to make database operations easier, safer, and more efficient.

Why Use Room?
✅ Simplifies SQLite usage with clean APIs.
✅ Provides compile-time checks for SQL queries.
✅ Supports LiveData, Flow, and Coroutines for seamless UI updates.
✅ Manages database versioning with migrations

    - Entity (Defines table structure)
    - DAO (Data Access Object - queries)
    - Database (Main access point)
    
```
@Entity
data class User(
    @PrimaryKey val id: Int,
    val name: String,
    val email: String
)

@Dao
interface UserDao {
    @Query("SELECT * FROM User")
    fun getAllUsers(): List<User>

    @Insert
    fun insertUser(user: User)
}

@Database(entities = [User::class], version = 1)
abstract class AppDatabase : RoomDatabase() {
    abstract fun userDao(): UserDao
}

```
## 🧩Android App Security Best Practices
 1.Encrypt Sensitive Data
 
    Use EncryptedSharedPreferences instead of regular SharedPreferences.
    For local databases, use SQLCipher or Room with encryption.
    
 2.Secure Network Communication Security:
   
    Always use HTTPS (TLS 1.2+).
    Implement Certificate Pinning to prevent MITM attacks.
    Use ProGuard/R8 to strip logs in release builds.
    ProGuard/R8 (Basic Obfuscation)
    - Renames classes/methods to make decompiled code unreadable.
    - Removes unused code (shrinking).

 # 🧩Memory Management in Android   
 - user LeakCanary for detecting leaks.
 - Unregister Listeners in onDestroy()
 - Use Glide or Coil for image loading (automatic memory optimization).
 - Check with Android Profiler (in Android Studio).
 - Use ActivityManager to get memory info:
 - LeakCanary (for memory leak detection).
 - Android profiler - for monitoring your app’s CPU, Memory, Network, and Energy usage in real time

## 🧩**Android Context Two types**
- 1.Application Context - need to access resources that are not tied to any specific activity, use the Application context.
- 2.Activity Context - need to access resources that are tied to a specific activity, use the Activity context
- It allows us to access resources.
- It allows us to interact with other Android components by sending messages.
- It gives you information about your app environment.

## 🧩Image Loading libraries
- Picasso
- Glide
- Fresco
- COIL (Coroutine Image Loader)
- UIL (Universal Image Loader)

# 🧩Java Garbage Collection

Java garbage collection is an automatic memory management process where unused objects are identified and
removed to free memory, using techniques like mark-and-sweep.

# 🧩Pending intend

A PendingIntent is a tokenized intent that allows another application (e.g., a system service) to perform an action on behalf of your application.
It's commonly used in notifications and alarms.

# 🧩Doze and App Standby

    Doze: Optimizes battery by deferring background tasks when the device is idle.
    App Standby: Limits background activity for unused apps.

# 🧩Git Merge and Git Rebase

    Merge: Combines branches, preserving commit history.
    Rebase: Reapplies commits from one branch onto another, creating a linear history.

# 🧩Runnable vs Callable

    Runnable: Represents a task with no return value.
    Callable: Represents a task with a return value and can throw exceptions.

# Clean Architecture
- Clean Architecture is a software design pattern that organizes code into distinct layers, separating concerns and dependencies. The layers are typically:
- 1.Presentation Layer: Handles the UI and user interactions (e.g., Activities, Fragments, ViewModel).
- 2.Domain Layer: Contains business logic and use cases. It’s independent of any frameworks.
- 3.Data Layer: Manages data sources like APIs, databases, and repositories.
- ✅ MVVM is good for separating UI logic, but it does not handle business logic efficiently.
- ✅ Clean Architecture ensures better separation of concerns, testability, scalability, and maintainability.
- ✅ For larger projects, Clean Architecture is essential to avoid tight coupling and messy code.
- MVVM Issue: ViewModel often contains business logic, making it tightly coupled with data sources.
- Clean Architecture Fix: Introduces Use Cases (Interactors), ensuring that business logic is independent of UI logic.

- Why do we need Clean Architecture in Android?
    - Testability: Layers are independent, making unit testing easier.
    - Scalability: The app becomes modular and easier to maintain as it grows.
   -  Flexibility: Allows swapping data sources (e.g., changing from SQLite to Room) without affecting other layers.
    - Separation of Concerns: Each layer has a specific role, improving code readability and maintainability.

# SOLID Principles
S: Single Responsibility Principle (SRP)

    Definition: A class should have only one reason to change. It should have one job or responsibility.
    Why we need it: Reduces the risk of introducing bugs when making changes to the class.
    Example: A User class should only handle user details, not database operations or authentication.

O: Open/Closed Principle (OCP)

    Definition: A class should be open for extension but closed for modification.
    Why we need it: Allows adding new features without altering existing code, reducing the risk of introducing bugs.
    Example: Use interfaces or inheritance to extend functionality instead of modifying existing classes.

L: Liskov Substitution Principle (LSP)

    Definition: Objects of a superclass should be replaceable with objects of a subclass without affecting the program's behavior.
    Why we need it: Prevents unexpected behavior when using polymorphism.
    Example: A Square class should not break the behavior of a Rectangle class if it inherits from it.

I: Interface Segregation Principle (ISP)

    Definition: A class should not be forced to implement interfaces it does not use.
    Why we need it: Keeps interfaces specific to the needs of the client, reducing unnecessary dependencies.
    Example: Instead of a single Animal interface with unrelated methods like fly() and swim(), split it into smaller interfaces like Flyable and Swimmable.

D: Dependency Inversion Principle (DIP)

    Definition: High-level modules should not depend on low-level modules. Both should depend on abstractions.
    Why we need it: Promotes loose coupling between components, making the system more flexible.
    Example: A Notification class should depend on an abstraction (Notifier) rather than concrete classes like EmailNotifier or SMSNotifier.


