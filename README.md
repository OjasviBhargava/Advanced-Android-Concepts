# Advanced-Android-Concepts
This repo is a part of Udacity Scholarship Program



***Advanced Android Tips and Tricks***

**Code Signing**

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


**JARs and Artifacts**

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
