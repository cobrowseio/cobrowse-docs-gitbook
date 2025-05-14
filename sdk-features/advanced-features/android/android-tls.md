---
description: >-
  Ensuring compatibility with legacy Android OS devices on Cobrowse by
  backporting TLS 1.2 and TLS 1.3.
---

# Backporting TLS to older Android versions

Cobrowse SDK uses [OkHttp](https://github.com/square/okhttp) for HTTPS communication and relies on default SSL implementation provided by Android OS. Android 4.4 and some Android 5.0 devices do not support TLS 1.2 by default, and TLS 1.3 support is only enabled starting Android 10. Our servers now require clients to use TLS 1.2 with an up to date set of ciphers. To use some legacy Android versions with modern TLS you might want to consider using one of alternative [security providers](https://square.github.io/okhttp/security/security_providers/) with OkHttp, e.g. Conscrypt.

### Example of using a Conscrypt-based `OkHttpClient` with Cobrowse SDK

1. In `build.gradle`:

```groovy
dependencies {
    ...
    implementation 'org.conscrypt:conscrypt-android:2.5.3'
}
```

2. Create your own `OkHttpClient` with a custom `SSLSocketFactory`:

```java
/**
 * Builder of custom {@link OkHttpClient} which supports TLS v1.2 and TLS v1.3 on older platforms.
 */
public class ModernTlsOkHttpClient {

   private static final String TAG = "ModernTlsOkHttpClient";

   private static class ReusableSingleton {
      private static final OkHttpClient INSTANCE = create();
      private static final Provider CONSCRYPT = Conscrypt.newProvider();
   }

   private ModernTlsOkHttpClient() {
   }

   /**
    * Returns an app-wide reusable {@link OkHttpClient} instance.
    */
   public static OkHttpClient reuse() {
      return ReusableSingleton.INSTANCE;
   }

   /**
    * Returns an app-wide reusable {@link Provider} instance from Conscrypt.
    */
   public static Provider conscrypt() {
      return ReusableSingleton.CONSCRYPT;
   }

   /**
    * Creates a new {@link OkHttpClient} instance.
    */
   public static OkHttpClient create() {
      OkHttpClient.Builder builder = new OkHttpClient.Builder()
              .pingInterval(60, TimeUnit.SECONDS)
              .connectTimeout(2000, TimeUnit.MILLISECONDS);
      return enableModernTls(builder).build();
   }

   @NonNull
   private static OkHttpClient.Builder enableModernTls(@NonNull OkHttpClient.Builder client) {
      if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.Q) {
         // No modifications on Android 10+
         return client;
      }
      try {
         X509TrustManager tm = Conscrypt.getDefaultX509TrustManager();
         SSLContext sslContext = SSLContext.getInstance("TLS", conscrypt());
         sslContext.init(null, new TrustManager[] { tm }, null);
         client.sslSocketFactory(new ModernTlsSocketFactory(sslContext.getSocketFactory()), tm);
      } catch (Exception e) {
         Log.e(TAG, "Error while setting TLS", e);
      }
      return client;
   }
}
```

```java
/**
 * Enables TLS v1.2 and TLS v1.3 when creating SSLSockets.
 * <p/>
 * Android supports TLS v1.2 from API {@link Build.VERSION_CODES#JELLY_BEAN}, but enables it
 * by default only from API {@link Build.VERSION_CODES#LOLLIPOP}.TLS v1.3 is enabled only
 * from API {@link Build.VERSION_CODES#Q}.
 *
 * @link https://developer.android.com/reference/javax/net/ssl/SSLSocket.html
 * @see SSLSocketFactory
 */
public final class ModernTlsSocketFactory extends SSLSocketFactory {
   private static final String[] TLS_12_AND_13_ONLY = new String[] {"TLSv1.2", "TLSv1.3"};
   private final SSLSocketFactory delegate;

   public ModernTlsSocketFactory(SSLSocketFactory sslSocketFactory)  {
      delegate = sslSocketFactory;
   }

   @Override
   public String[] getDefaultCipherSuites() {
      return delegate.getDefaultCipherSuites();
   }

   @Override
   public String[] getSupportedCipherSuites() {
      return delegate.getSupportedCipherSuites();
   }

   @Override
   public Socket createSocket() throws IOException {
      return enableTLSOnSocket(delegate.createSocket());
   }

   @Override
   public Socket createSocket(Socket s, String host, int port, boolean autoClose)
           throws IOException {
      return enableTLSOnSocket(delegate.createSocket(s, host, port, autoClose));
   }

   @Override
   public Socket createSocket(String host, int port) throws IOException {
      return enableTLSOnSocket(delegate.createSocket(host, port));
   }

   @Override
   public Socket createSocket(String host, int port, InetAddress localHost, int localPort)
           throws IOException {
      return enableTLSOnSocket(delegate.createSocket(host, port, localHost, localPort));
   }

   @Override
   public Socket createSocket(InetAddress host, int port) throws IOException {
      return enableTLSOnSocket(delegate.createSocket(host, port));
   }

   @Override
   public Socket createSocket(InetAddress address, int port, InetAddress localAddress, int localPort)
           throws IOException {
      return enableTLSOnSocket(delegate.createSocket(address, port, localAddress, localPort));
   }

   private Socket enableTLSOnSocket(Socket socket) {
      if (socket instanceof SSLSocket) {
         ((SSLSocket) socket).setEnabledProtocols(TLS_12_AND_13_ONLY);
      }
      return socket;
   }
}
```

3. Set up Conscrypt provider in your `MainApplication#oncreate()`:

```java
@Override
public void onCreate() {
    super.onCreate();

    if (Build.VERSION.SDK_INT < Build.VERSION_CODES.Q) {
        Security.insertProviderAt(ModernTlsOkHttpClient.conscrypt(), 1);
    }
    ...
}
```

4. Pass your `OkHttpClient` to the Cobrowse SDK before calling `start()`:

```java
CobrowseIO.instance().okHttpClient(ModernTlsOkHttpClient.reuse());
CobrowseIO.instance().start();
```
