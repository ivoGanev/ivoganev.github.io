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

#### Example uses of a Service?

Playing music, handle network transactions, interacting with content providers.

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

{% highlight kotlin %}
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

Suggested Reading: https://guides.codepath.com/android/Storing-Secret-Keys-in-Android.

https://en.wikipedia.org/wiki/Android_(operating_system)
