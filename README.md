# Advanced-Android-Concepts
This repo is a part of Udacity Scholarship Program



*Advanced Android Tips and Tricks*

*Code Signing*

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
