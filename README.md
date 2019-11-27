**This is an sample application that shows setup and configure the Firebase Crashlytics in the Flutter (IOS and Android)**

**Introduction**

Following notes describes the way that firebase official plugin can be added to flutter in the way that it can send all of the reports in android and IOS platform to the console.



**Steps**

In order to setup the firebase for flutter app in Android and IOS platforms, these steps should be taken:

 - Create firebase account
 - Create firebase project
 - Create and configure android application in firebase console
 - Create and configure IOS application in firebase console



**Firebase account**

In order to start the project  surf to this address: https://firebase.google.com. Then if you have signed with your gmail address by clicking on “Go to console” button on the top right hand side of the page you will be redirected to the web page in which you can add any project.



**Firebase project**

The webpage (address: https://console.firebase.google.com/) contains all of the defined projects under your account. In order to define a project, it is enough to click on “add project” button. In this step the project name is defined. User can define whatever name that he wants, but  It should be noted that if in the name “_” is being used, it can not be installed on IOS. At the end of this step, a project is defined which can include some applications.



**Create and configure flutter application on Android platform**

After initializing the project, in the webpage it is possible to add applications:

Click “add App” button, then select the android platform.
Then, register the app. In the Android package name field, you should write your Android package name, which you’ll obtain in the following directory of your Flutter app directory:

>-root-of-project-/android/app/src/main/AndroidManifest.xml


```
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
   package="your package name">
```


then click on “Register app” button.

Then a config file is generated (“google-services.json”) which should be downloaded and  located at this address:   

>-root-of-project-/android/app


Then update this file:   

>-root-of-project-/android/build.gradle

```
buildscript {
  repositories {
    // Check that you have the following line (if not, add it):
    google()  // Google's Maven repository
    maven {
        url   'https://maven.fabric.io/public'
    }
 }

  dependencies {
    ...
    // Add this line
    classpath 'com.google.gms:google-services:4.3.3'
    classpath 'io.fabric.tools:gradle:1.31.2' 
  }
}

allprojects {
  ...
  repositories {
    // Check that you have the following line (if not, add it):
    google()  // Google's Maven repository
    ...
  }
  
```


Update this file:   

>-root-of-project-/android/app/build.gradle

```
apply plugin: 'com.android.application'

dependencies {
  // add the Firebase SDK for Google Analytics
  implementation 'com.google.firebase:firebase-analytics:17.2.1'

  // add SDKs for any other desired Firebase products
  // https://firebase.google.com/docs/android/setup#available-libraries
}
...
// Add to the bottom of the file
apply plugin: 'io.fabric'
apply plugin: 'com.google.gms.google-services'
```


**Create and configure flutter application on IOS platform**

Similar to the Android platform, an IOS application should be added:

Click “add App” button, then select the IOS platform
Then register the app. In order to do so, you have to enter your project’s bundle identifier.  Open command line and proceed to this address : 

>-root-of-project-/ios


Then run this command to open xcode: 

```
open Runner.xcworkspace 
```


(The last line open the project in Xcode, and you can do it manually)
After opening Xcode, choose Runner folder from left hand side and select the general tab, copy and paste project’s Bundle identifier field.
Then click on “Register App” button.
In the next step a config file is generated (“googleService-info.plist”) which should be downloaded and located inside the project. Copying the file is a little tricky, since some kind of process should be done in the background. In order to do that, it is advisable to select the file in finder app, drag it to the opened project inside the xcode app and release it in this address: 

>-root-of-project-/ios/Runner/Runner


Then a new window is opened inside the xcode, just click the “finish” button.



**General notes**

 - If you open crashlytics console for your application, automatically a filter is defined(Event type = “Crashes”).In the flutter plugin all caught errors are defined as Non-fatal, so in the Firebase console (Crashlytics section), if you want to see the errors you must remove the default filter. 
There is an issue regarding using firebase_crashlytics (issue1149).That means if you want to see all of the logs in the firebase console you have to run the project in terminal:

```
cd <root-of-project->
Flutter clean
Flutter run
```


 - In case of running the project inside IDEs you will receive only the exceptions that are thrown asynchronously. One possible solution for this case is to throw all of them in this pattern (not advisable):

```
Future<void>.delayed(const Duration(seconds: 0), () {
                     throw StateError(“......’’);
                     // or  Crashlytics.instance.crash(); 
                   });
```


 - In case of testing the flutter application on Android emulator, just make sure that emulator has Google Play installed.
