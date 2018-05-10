# Advanced-Android-Concepts
This repo is a part of Udacity Scholarship Program



***Advanced Android Tips and Tricks***

## **Code Signing**

• It will help distinguish your production applications from debug versions of
the same applications
• Multiple applications signed with the same key can access each other’s
private files, if they are set up to use a shared user ID in their manifests
• You can only update an application if it has a signature from the same digital
certificate

*To manually create a production signing key, you will need to use `keytool`*

*Parameters used in a keystore -*

`Example Command` -
```keytool -genkey -v -keystore cw-release.keystore -alias cw-release -keyalg RSA -validity 10000 -keysize 2048```

1. *`-genkey`*, to indicate we want to create a new key
2. *`-v`*, to be verbose about the key creation process
3. *`-keystore`*, to indicate what keystore we are manipulating
(cw-release.keystore), which will be created if it does not already exist
4. *`-alias`*, to indicate what human-readable name we want to give the key
(cw-release)
5. *`-keyalg`*, to indicate what public-key encryption algorithm to be using for
this key (RSA)
6. *`-validity`*, to indicate how long this key should be valid, where 10,000 days
or more is recommended
7. *`-keysize`*, for indicating the length of the signing key (2,048 bits
recommended, or go higher if you prefer)


## **JARs and Artifacts**

The first implementation statement in app level gradle file is:


implementation fileTree(dir: 'libs', include: ['*.jar'])

This pulls in any JAR files that happen to be in the libs/ directory of this module.

*What are JARs*

JARs, are libraries containing Java code, as created by standard Java build tools (javac, jar, etc.). For the first decade-plus of Java’s existence, we distributed reusable bits of code in the form of JAR files. You would download a JAR from a Web site, drop it into your project, and through something like this implementation statement, say that your project should use the JAR. *In Android, the contents of these JARs are packaged into your APK. And, whatever public classes happen to be in those JARs are available to you at compile time.*

Nowadays using plain JARs are considered to be a bad idea. Instead, we should try to use artifacts, rather than bare JAR files.

*Artifact???*

An artifact is usually represented in the form of two files:

• The actual content, such as a JAR

• A metadata file, paired with the JAR, that has information about “transitive dependencies” (i.e., the other artifacts that this artifact depends upon)

`PS` - Artifacts are housed in artifact repositories. Those repositories not only contain the artifacts, but they organize the artifacts for easy access.
• JCenter, is a popular place for open source artifacts


## **EventBuses**

Event-driven programming has been around for nearly a quarter-century. Much of Android’s UI model is event-driven, where we find out about these events via callbacks and registered listeners. in 2012, event buses started to pop up, and these are very useful for organizing communication within your Android application and across threads.

An event bus can eliminate the need for AsyncTask and the other solutions for communicating back to the main application thread, while simultaneously helping you logically decouple independent pieces of your code.


`An event bus is designed to decouple the sources of events from the consumers of those events.`


Furthermore, an event bus provides a standard communications channel (or “bus”) that event producers and event consumers can hook into. Event producers merely need to hand the event to the bus; the bus will handle directing those events to relevant consumers. This reduces the coupling between the producers and consumers, sometimes even reducing the amount of code needed to source and sink these events.

*Greenrobots EventBus* (https://github.com/greenrobot/EventBus)
Event bus for Android and Java that simplifies communication between Activities, Fragments, Threads, Services, etc. Less code, better quality. With greenrobot’s EventBus, it is fairly easy to send a message from one part of your app to another disparate part of your app.
EventBus - Event bus for Android and Java that simplifies communication between Activities, Fragments, Threads, Services, etc. Less code, better quality.


## **Product Flavors**

A product flavor is an independent axis for varying your output.

*Product flavors are designed for scenarios where you want different release output for different cases.*

For example, you may want to have one version of your app built to use Google’s in-app purchasing APIs (for distribution through the Play Store) and another version of your app built to use Amazon’s in-app purchasing APIs (for distribution through the Amazon AppStore for Android).
In this case, both versions of the app will be available in release form, and you may wish to have *separate debug builds as well*. And most of the code for the two versions of the app will be the same.

*However, you will have different code for the different distribution channels* — not only does the right code have to run for the right channel, but there is no particular value in distributing the code for one channel through the other channel.


`Product flavors are optional. If you do not describe any product flavors in your build.gradle file, it is assumed that you have a single product flavor, referred to internally as default. Many apps will not need product flavors; this is a feature that you will opt into as needed.`


## **Types of Artifacts and Repositories**

There are two types of repositories, and associated artifact structures, supported by
*Gradle*: *Maven and Ivy*.
Each has their own format for the metadata and their own
structure for how the files are stored.

***Maven***
`Apache Maven` is a full-fledged build system. Part of that build system is a system of artifacts and repositories. While Gradle does not use Maven’s build system — rather, it largely replaces it — Gradle can consume artifacts published in a Maven structured repository. Maven Central, as one might expect, is one such repository, but it is eminently possible to set up your own, and some organizations have done that.

***Ivy***
`Apache Ivy` is an off-shoot of the Apache Ant project that gave us the Ant build system. Ivy is simply a way of declaring dependencies between components, including handling “transitive dependencies” (i.e., App A depends upon Library B, which in turn depends upon Libraries C and D).


## **Application ID**

Your Application ID should be unique. If
somebody tries downloading your application onto their device, and some other application is already installed with that same package name, your application will fail to install.

Your application ID defaults to be the value of your `package attribute` in your manifest element in the manifest.

*You can override the application ID using `applicationId` properties in `defaultConfig` or a product flavor in build.gradle.*

You can also append an applicationIdSuffix tied to a build type in Gradle as well.
Since the manifest’s package also provides the base Java package for your project, and since you hopefully named your Java packages with something based on a domain name you own or something else demonstrably unique, this should not cause a huge problem.

*PS - Also, bear in mind that your application ID must be unique across all applications on the Play Store.*


## **BuildConfig**

The Android development tools have been code-generating the `BuildConfig` class for some time now. Historically, the sole element of that class was the `DEBUG` flag, which is true for a debug build and false otherwise. This is useful for doing runtime changes based upon build type, such as only configuring StrictMode in debug builds.

Nowadays, the Android Plugin for Gradle also defines:yum::

*• BUILD_TYPE*, which is the build type used to build this APK.

*• FLAVOR*, which is the product flavor used to build this APK.

*• PACKAGE_NAME*, which is the name that serves as the application ID (i.e., it includes build type suffixes and product flavor overrides). This is useful for cases where you cannot just call `getPackageName()` on a Context because you do not have a handy Context.

*• VERSION_CODE*, which is the version code derived from your manifest in conjunction with any overrides coming from your `build.gradle` file.

*• VERSION_NAME*, which is the version name derived from your manifest in
conjunction with any overrides coming from your build.gradle file.
