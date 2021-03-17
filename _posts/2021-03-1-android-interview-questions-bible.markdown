---
layout: post
title:  "Android Interview Questions Bible"
date:   2021-03-1 16:25:08 +0000
categories: OOP
---

##### What is it?
---
This project is created to ask the most fundamental questions that you should, in my opinion, know for Android and could be a good source to brush up your skills before an interview as opposed to browsing through the massive Android developer guide. 

However, the Android developer guide remains the main origin of the information provided here and if something doesn't add up you should go and read the full documentation.

##### Future plans
---

I am planning to create another html version where all the answers are hidden so you can practice without viewing the material.

## Android Platform

#### What is Android? 

Android is a mobile operating system based on a modified version of the Linux kernel and other open source software, designed primarily for touchscreen mobile devices such as smartphones and tablets.


[Wikipedia](https://en.wikipedia.org/wiki/Android_(operating_system)){: .post.a }{:target="_blank"}


#### How Does Android's Software Architecture look like?

![Software Stack](/assets/android-interview/a-stack.png)

#### What Are Some of The Android SDK Tools?


##### Android NDK

The Native Development Kit (NDK) is a set of tools that allows you to use C and C++ code with Android, and provides platform libraries you can use to manage native activities and access physical device components, such as sensors and touch input.

- Simple Perf - CPU profiling including -- which function/libraries takes the longest to execute and finding percentage of time spent in threads/object modules.


##### Emulator

The emulator simulates Android devices on your computer so that you can test your application on a variety of devices and Android API levels without needing to have each physical device. Some pros are:

- Faster data transfer
- Can test multiple API Levels

##### ADB (Android Debug Bridge) 

A CLI Client-Server tool which can run commands such as: copy files back and forth, install and uninstall apps, run shell commands, and more.  on a connected over USB Android device 

##### AAPT2 (Android Asset Packaging Tool 2)

AAPT2 parses, indexes, and compiles the resources into a binary format that is optimized for the Android platform.

AAPT2 supports faster compilation of resources by enabling incremental compilation:

- Compile: compiles resource files into binary formats.
- Link: merges all compiled files and packages them to a single package.

This division helps improve performance for incremental builds. For example, if there are changes in a single file, you need to recompile only that file.

#### What is the Java API framework in Android?
Its an API written in Java which consists of:

- View System to build an app’s UI, including lists, grids, text boxes, buttons, and even an embeddable web browser
- A Resource Manager, providing access to non-code resources such as localized strings, graphics, and layout files
- A Notification Manager that enables all apps to display custom alerts in the status bar
- An Activity Manager that manages the lifecycle of apps and provides a common navigation back stack
- Content Providers that enable apps to access data from other apps, such as the Contacts app, or to share their own data


#### What is the Application class?

Base class for maintaining global application state. It is first to be created when the application starts and the last to get destroyed.

The application object is not guaranteed to stay in memory forever, it will get killed so don’t use mutable state inside it because it will get wiped.

#### What is a Context?

It allows access to application-specific resources and classes, and application operations, such as launching activities, broadcasting and receiving intents, etc. 

#### What is an Android component?

An android component is simply a piece of code that has a well defined lifecycle.

#### What are the four Android components?

1. Activities
2. Services
3. Broadcast Receiver
4. Content Provider

#### How can you prevent other apps from using any components registered in Manifest.xml?

Set the component attribute to `exported=false`

#### What is an Activity?
An activity serves as an entry point for an app's interaction with the user.
- It is a single focused thing that the user can do.
- It provides the window which the app draws its UI.

#### Explain the lifecycle callbacks?
As a user navigates through, out of, and back to your app, the Activity instances in your app transition through different states in their lifecycle. 

#### Why do you need to implement lifecycle callbacks?
To avoid:

- Crashing if the user receives a phone call or switches to another app while using your app.
- Consuming valuable system resources when the user is not actively using it.
- Losing the user's progress if they leave your app and return to it at a later time.

#### What are the core six lifecycle callbacks?

You can remember them as CSR(Caesar), PSD(The Photoshop file format)

- `onCreate()` - Called when the activity is created. Used for static set up: create views, bind data to lists, etc. Provides wth Bundle containing the activity's previously frozen state, if there was one.

- `onStart()` - Called after your activity has been stopped, prior to it being started again.

- `onResume()` - When activity becomes visible to the user.

- `onPause()` - When the activity starts interacting with the user. 

- `onStop()` - Activity is no longer visible to the user. This may occur, for example, when a newly launched activity covers the entire screen. 

- `onDestroy()` - Called when the user exits the app, `finish()` is invoked, or configuration change (such as device rotation)

#### How does the Android OS decides when to kill an activity?

It depends on the process state of the activity.

| Likelihood of being killed | Process state | Activity state |
|----------------------------|---------------|----------------|
| Least | Foreground (having or about to get focus) | Created,Started,Resumed |
|More	|Background (lost focus)|	Paused |
|Most	|Background (not visible)|	Stopped |
|Most   |Empty	| Destroyed |

#### Is the user input data restored when an activity is recreated?

The system destroys the activity including all the user input data and creates a new one when, for example, the screen is rotated or user switches to multi-windowed mode. The old input data is not automatically transferred to the new instance of the activity so you need to restore it.

#### How do you store and restore the data the user had input?
Use `onSaveInstanceState(Bundle)` to store the user data as a key-value pair.

Use `onCreateView(Bundle)` or alternatively `onRestoreInstanceStateBundle(Bundle)`

#### What is an Android task?
- A task stores activity instances
- Can go in the background
- Can return to the foreground


#### What is Task affinity?

- It’s the task’s name
- Used to determine the task that
should hold the activity
- By default: all activities share the same
affinity(package name)
- It's defined in the Manifest.XML inside the \<activity> tag.

#### What does launchMode do for an activity in Manifest.XML?

It specifies how a new instance of an activity should be added to a task each time it is launched.

#### What is a "standard" launchMode?

- Creates new instance in the calling activity’s task
- Activity can have multiple instances
- Instances can reside in different tasks
- One task can have multiple instances

NOTE: In order to create a new task you need
to add a flag `FLAG_ACTIVITY_NEW_TASK` when starting the activity.

Example:

Suppose activity stack: A⏴B⏴C. Then launching activity B would produce: A⏴B⏴C⏴B .

#### What is a "singleTop" launchMode?

- If an instance of an activity is on top of the stack an
activity won’t be created, instead `onNewIntent()` will be called.
- Activity can have multiple instances
- Instances can reside in different tasks
- On task can have multiple instances

Example:

Suppose activity stack: A ⏴B ⏴C.
Then launching C again would produce: A ⏴B ⏴C onNewIntent().  

#### What is a "singleInstance" launchMode?

- Activity will be the only instance in the task
- Activity is always the root of the task
- Activity is the only member of the task
- New activities will launch in a different task

#### What is a "singleTask" launchMode?
- Activity can only have one instance
- Activity is always the root of the task
- If an instance of the activity already exists the system will call `onNewIntent()` callback and won't create a new instance.

1. We launch 'C' with `FLAG_ACTIVITY_NEW_TASK`
Example:

- Task 1: (A, singleTask)⏴B 
- Task 2: C

2. Now if we launch A from C:

- Task 1:A ⏴[B gets destroyed]
- Task 2:C

#### What is a Service?
A Service is a component that can perform long-running operations in the background of Android OS. It does not provide a user interface. 

Note: A service runs on the main thread of its hosting process. You should run all blocking operation in a separate thread.

#### What is a foreground service?
A foreground service performs some operation that is noticeable to the user. 
Example: an audio app would use a foreground service to play an audio track. 
Foreground services must display a Notification. 
Foreground services continue running even when the user isn't interacting with the app.

#### What is a background Service
A background service performs an operation that isn't directly noticed by the user. For example, if an app used a service to compact its storage, that would usually be a background service.

#### What is a bound Service
A bound service is the server in a client-server interface. It allows components (such as activities) to bind to the service, send requests, receive responses, and perform interprocess communication (IPC). Multiple components can bind to the service at once, but when all of them unbind, the service is destroyed.

#### What is a BroadcastReceiver?

It’s a component which listens for messages from the system(system events) or other apps. 

Here are a few system events that you can listen for:
```
Intent.ACTION_BOOT_COMPLETED, 
Intent.ACTION_POWER_CONNECTED
Intent.ACTION_POWER_DISCONNECTED
Intent.ACTION_BATTERY_LOW
Intent.ACTION_BATTERY_OKAY
```

#### How do you register to listen for system events?

1. Create a class, e.g. `MyReceiver`, to extend `BroadcastReceiver`, and override `onReceive(context: Context, intent: Intent)` method.

2. Register your receiver in the Manifest.xml file for example to listen for a low battery event:

{% highlight xml %}
<receiver android:name=".MyReceiver" android:exported="true">
    <intent-filter>
        <action android:name="android.intent.action.BATTERY_LOW"/>
    </intent-filter>
</receiver>
{% endhighlight %}

The broadcast will be in packed in the `Intent` argument in `onReceive` in `MyReceiver` as an action.


#### What is a ContentProvider?

A content provider is used to supply data from one application to another. 

You can look at it as an standard interface with methods like `insert(..)`, `delete(..)`, `update(..)`, etc. that need you to override them and provide an implementation of how they would access your app's data. 

In order to access the content provider and the methods you override mentioned above you need to use the `ContentResolver` which is available in any Context inheriting object(e.g. Activity) by using `getContentResolver()`.

A URI is used to identify the data in the provider. 

- The URI's authority is used to name the provider in order to prevent name clashes with other system providers. You would use the app package name in reverse like `com.myCompany.myApp.provider.MyContentProvider`
- The URI path is used to point the table

#### What does resource externalization bring?

By externalizing resources you make them easier to maintain, update, and manage. This also lets you easily define alternative resource values to support different hardware and internationalization.

## Optimizations

#### What image format can you use to reduce apk size?

WebP is the preferred image format for Android apps and is lossless and lossy which means it supports both types of compression, unlike PNG's and JPG's.

Lossy compression permanently removes data.

Lossless compression means that as the file size is compressed, the picture quality remains the same - it does not get worse.

## Build Configuration

#### What does minSdkVersion do? 

It is the earliest release of the Android SDK that your application can run on. The Android system will prevent the user from installing the application if the system's API Level is lower than the value specified in this attribute.

#### What does targetSdkVersion do? 

The targetSdkVersion tells the OS that you have tested your app up until the version you have specified and make it forward compatible.

Example:

Let's say your targetSDKVersion is 22 and you have set a permission in AndroidManifest.xml to allow phone calls. In API 23, runtime permissions were introduced which requires you to request for dangerous permission,such as calling a phone, at runtime. If a user with a phone using API 23 runs your app and tries to call someone there will be no problem; but if you change later your targetSDKVersion to 23, the user tries to call someone again and you haven't checked for a runtime permission your app will crash.

#### What does compileSdkVersion do? 

When you write code for Android in your IDE you have access to any SDK by just downloading it. To make sure you are not using any API from an SDK that is not available for your phone targets, warnings and errors are issued so that you are able to fix those problems.

#### What is the recommended recipe for defining sdk versions? 

minSdkVersion <= targetSdkVersion <= compileSdkVersion

#### How do you configure project wide properties?

In your build top-level `build.gradle` you can define properties inside an `ext` block:

```
buildscript {...}
allprojects {...}

ext {
    compileSdkVersion = 28
    supportLibVersion = "28.0.0"
    ...
}
```

#### Tell one way to store API keys?

First, create a file apikey.properties in your root directory with the values for different secret keys:

```
CONSUMER_KEY="XXXXXXXXXXX"
CONSUMER_SECRET="XXXXXXX"
```

Double quotes are required.

To avoid these keys showing up in your repository, make sure to exclude the file from being checked in by adding to your <i>.gitignore</i> file:

`apikey.properties`

Next, add this section to read from this file in your app/build.gradle file. You'll also create compile-time options that will be generated from this file by using the buildConfigField definition:

```
def apikeyPropertiesFile = rootProject.file("apikey.properties")
def apikeyProperties = new Properties()
apikeyProperties.load(new FileInputStream(apikeyPropertiesFile))

android {

  defaultConfig {
     
    // should correspond to key/value pairs inside the file   
    buildConfigField("String", "CONSUMER_KEY", apikeyProperties['CONSUMER_KEY'])
    buildConfigField("String", "CONSUMER_SECRET", apikeyProperties['CONSUMER_SECRET'])
  }
}

```

You can now access these two fields anywhere within your source code with the BuildConfig object provided by Gradle:
```
// inside of any of your application's code
String consumerKey = BuildConfig.CONSUMER_KEY;
String consumerSecret = BuildConfig.CONSUMER_SECRET;
Now you have access to as many secret values as you need within your app, but will avoid checking in the actual values into your git repository. 
```
[Suggested Reading](https://guides.codepath.com/android/Storing-Secret-Keys-in-Android){: .post.a }{:target="_blank"}

## OOP Principles and Clean Code

This section uses the following sources of information:

- Clean Code - the book,
- [SOLID principles, David Halewood's blog](https://davidhalewood.com/)

#### What is abstraction?

is a model of a real-world object or phenomenon, limited to a specific context, which represents all details relevant to this context with high accuracy and omits all the rest. Examples:

- A video game controller has only a few buttons, but underneath the controller has a complex control mechanism.
- A car is a very complex machine but the interface is simple( a steering whell, a gas pedal and a gear shift).

Anytime you see a simple interface covering a more complex system you should think "abstraction".

#### What is polymorphism?

Polymorphism is the ability of different objects to respond in a unique way to the same message.

##### Subtyping

Subtyping allows a function to be written to take an object of a certain type T, but also work correctly, if passed an object that belongs to a type S that is a subtype of T.

In the following example we make cats and dogs subtypes of animals. The procedure letsHear() accepts an animal, but will also work correctly if a subtype is passed to it:

{% highlight java %}
abstract class Animal {
    abstract String talk();
}

class Cat extends Animal {
    String talk() {
        return "Meow!";
    }
}

class Dog extends Animal {
    String talk() {
        return "Woof!";
    }
}

static void letsHear(final Animal a) {
    println(a.talk());
}

static void main(String[] args) {
    letsHear(new Cat());
    letsHear(new Dog());
}
{% endhighlight %}

##### Static Binding
In Java it is achieved by method overloading. The compiler looks at the method signature and decides which method to invoke for a particular method call at compile time.

{% highlight java %}
public int add(int x, int y){  //method 1
return x+y;
}

public int add(int x, int y, int z){ //method 2
return x+y+z;
}
{% endhighlight %}

##### Dynamic Binding

In Java it is achieved by method overriding. Java waits until runtime to determine which object is actually being pointed to by the reference.

## What is inheritance?

It is the ability to build new classes on top of existing ones.

Pros:

- No need for code duplication if a new class with a slightly different behavior is needed.
- Reliability. The base class has already been tested.

Cons:

- Subclasses have the same interface as their parent class.
- You can’t hide a method in a subclass if it was declared in the superclass.
- You must also implement all abstract methods, even if they don’t make sense for your subclass.

#### What is encapsulation?

is the ability of an object to hide its part of its state and behaviours from other objects exposing only a limited interface. To *encapsulate* something means to make it private. Encapsulation is the realization of abstraction.

#### What is the difference between objects and data structures?

Objects hide their data behind abstractions and expose functions that operate on that data. Data structures expose their data and have no meaningful functions.

{% highlight java %}
// Object
public interface Vehicle {
  double getFuelTankCapacityInGallons();
  double getGallonsOfGasoline();
}
{% endhighlight %}

{% highlight java %}
// Data structure
public interface Vehicle {
  double getPercentFuelRemaining();
}
{% endhighlight %}

#### What is important to remember when talking about procedural code vs OO code?

Procedural code (code using data structures) makes it easy to add new functions without changing the existing data structures. OO code, on the other hand, makes it easy to add new classes without changing existing functions.

The complement is also true:

Procedural code makes it hard to add new data structures because all the functions must
change. OO code makes it hard to add new functions because all the classes must change.

#### What is a DTO?
A data structure is a class with public variables and no functions. This is sometimes called a data transfer object, or DTO. 

DTOs are very useful structures, especially when communicating with databases or parsing messages from sockets and so on. They often become the first in a series of translation stages that convert raw data in a database into objects in the application code.

#### What is the single responsibility principle?

This principle states that each class should have one, and only one reason to change. The responsibility of the class should be scoped to a specific functionality and encapsulated within a single class or module. Primarily it’s concerned with limiting the impact of future change.

It’s fairly easy to spot when a class is violating single responsibility. Let’s look at a simple example:

{% highlight java %}
public class ReportPrinter {

    public void print(String content) {
        String formattedContent = format(content);

        printheader();
        printContent(formattedContent);
        printFooter();
    }

    private String format(String content) {
        //format the string
    }

    //code
}
{% endhighlight %}

What’s clear here is that this class can be changed for two reasons. First, the content of the report could change. Second, the format of the report could change. These two things change for very different reasons. These two aspects of the problem (printing the content) are really two separate responsibilities. They should, therefore, be in separate classes. This would resolve the coupling of these two separate concerns.

So let’s do that:

{% highlight java %}
public class ReportPrinter {
    private ReportFormatter formatter;

    public ReportPrinter(ReportFormatter formatter) {
        this.formatter = formatter;
    }

    public void print(String content) {
        String formattedContent = formatter.format(content)

        printHeader();
        printContent(formattedContent);
        printFooter();
    }

    //code
}
{% endhighlight %}

## What is the open/closed principle?

The main idea of this principle is to keep existing code from
breaking when you implement new features.

A class is open if you can extend it, produce a subclass and do whatever you want with it—add new methods or fields, override base behavior, etc.

Some programming languages let you restrict further extension of a class with special keywords, such as final. After this, the class is no longer open. At the same time, the class is closed (you can also say complete) if it’s 100% ready to be used by other classes—its interface is clearly defined and won’t be changed in the future.

{% highlight java %}
public class AreaCalculator
{
    public double area(Object[] shapes)
    {
        double area = 0;
        for (Object shape : shapes) {
            if (shape instanceOf Rectangle) {
                Rectangle rectangle = (Rectangle) shape;
                area += rectangle.getWidth() * rectangle.getHeight();
            }

            if (shape instanceOf Circle) {
                Circle circle = (Circle) shape;
                int radius = circle.getRadius()
                area += radius * radius * Math.PI;
            }
        }

        return area;
    }
}
{% endhighlight %}

A better way:

{% highlight java %}
public class AreaCalculator
{
    public double area(Shape[] shapes)
    {
        double area = 0;
        for (Shape shape : shapes) {
            area += shape.area();
        }

        return area;
    }
}
{% endhighlight %}

It is **open to extension** in that it can accept collections of an ever increasing array of Shape’s but it is **closed for modification** as it does what we need it to do now and in the future.

#### What is Liskov's substitution principle?

This dictates that, for your code to adhere to LSP, all subclasses must be completely interchangeable with their parent class. Let's look at a violation example:

{% highlight java %}
public abstract class Car {
    //other methods

    public abstract void fillUpWithFuel();
    public abstract void chargeBattery();
}

public class PetrolCar extends Car {
    //code
    @Override
    public void fillUpWithFuel() { 
        this.fuelTankLevel = FUEL_TANK_FULL; 
    }

    @Override
    public void chargeBattery() {
        throw new UnsupportedOperationException("A petrol car cannot be recharged");
    }
    //code
}

public class ElectricCar extends Car {
    //code
    @Override
    public void fillUpWithFuel() {
        throw new UnsupportedOperationException("It's an electric car");
    }

    @Override
    public void chargeBattery() {
        this.batteryLevel = BATTERY_FULL;
    }
    //code
}
{% endhighlight %}

By not properly supporting all of the parent class methods and instead throwing exceptions both the PetrolCar and ElectricCar fail to meet the requirements set out by Liskov.

#### What is interface segregation?

A client should never be forced to implement an interface that it doesn’t use.

This is in a similar vain to the *Single Responsibility Principle*. Your abstractions should be small, individual units of business logic so that your clients implement the slice of the pie they need not the whole pie (nor the ice cream, cherry or sprinkles).

{% highlight java %}
public interface Shape {
    public int area();
    public int volume();
}
{% endhighlight %}

Does this behavior belongs in the Shape interface? Hmm, not quite. A `Square` doesn't have a volume. A better way to do this:

{% highlight java %}
public interface Solid {
    public int volume();
}

public interface Shape {
    public int area();
}

class Cube implements Shape, Solid {
    // code
    public int area() {
        // calculate surface area
    }

    public int volume() {
        // calculate volume
    }
}
{% endhighlight %}

#### What is dependency inversion?

It states that the high-level module must not depend on the low-level module, but they should depend on abstractions. Let's look at the example where the `AccountProvider` is our high level module and it currently relies on the low level module `SQLConnection`.:

{% highlight java %}
public class AccountProvider {
    SQLConnection dbConnection;

    public AccountProvider(SQLConnection sqlConnection) {
        dbConnection = sqlConnection;
    }
}
{% endhighlight %}

**A better way**
The AccountProvider should not care about what database your application uses. To fix this we can create a layer of abstraction, in this case an interface:

{% highlight java %}
public interface DbConnection {
    public DbConnection getConnection();
}
public class SQLConnection implements DbConnection {
    public DbConnection getConnection() {
        //return connection 
    }
}
public class AccountProvider {
    DbConnection dbConnection;

    public AccountProvider(DbConnection dbConnection) {
        dbConnection = dbConnection;
    }
}
{% endhighlight %}

#### What is inversion of control

Inversion of Control is what you get when your program callbacks, e.g. like a gui program or a framework. Inversion of control is what separates library from a framework.

#### What is a callback?

Think of a callback as an object that it's only purpose is to be called from another object.

{% highlight java %}
interface CallBack {
    void methodToCallBack();
}

class CallBackImpl implements CallBack {
    public void methodToCallBack() {
        System.out.println("I've been called back");
    }
}

class Caller {

    public void register(CallBack callback) {
        callback.methodToCallBack();
    }

    public static void main(String[] args) {
        Caller caller = new Caller();
        CallBack callBack = new CallBackImpl();
        caller.register(callBack);
    }
}
{% endhighlight %}

