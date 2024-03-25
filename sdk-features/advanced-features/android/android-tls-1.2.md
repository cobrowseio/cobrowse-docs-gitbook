---
description: >-
  Ensuring compatibility with Android 4.4 and some 5.0 devices on Cobrowse by
  backporting TLS 1.2.
---

# Backporting TLS 1.2 to Android 4.4 (API 19)

Android 4.4  and some Android 5.0 devices do not support TLS 1.2 by default. Our servers now require clients to use TLS 1.2 with an up to date set of ciphers. You may need to add the following dependency to use some legacy Android versions.

1. In `build.gradle`:

```
dependencies {
    ...
    implementation 'org.conscrypt:conscrypt-android:2.2.1'
}
```

1. In `MainApplication#oncreate()`:

```
    @Override
    public void onCreate() {
        super.onCreate();

        Security.insertProviderAt(Conscrypt.newProvider(), 1);

        ...
    }
```
