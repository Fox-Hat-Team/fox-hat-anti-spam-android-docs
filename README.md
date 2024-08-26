# Integrating FoxHat AntiSpam

This guide will explain how to integrate FoxHat AntiSpam into your Android application .

## Features

- Emulator Detection
- Root Detection
- Secure Api

## Adding FoxHat AntiSpam to Your Project

### Step 1: Add the .aar File to Your Project

1. Copy the downloaded `.aar` file into the `libs` directory of your Android project.
2. Open your `build.gradle` file (usually located in the `app` module) and add the following dependency:

```gradle
implementation files('libs/FoxHat.aar')
```

# Initializing FoxHat AntiSpam

Now that you have integrated the FoxHat AntiSpam SDK into your project.
Emulator and Root Detection enabled by default

```java
class BaseApplication : Application() {
    override fun onCreate() {
        super.onCreate()
         /**
         * FoxHat.initialize:
         *
         * @param apiKey 32-bit API key required for authentication. The API key is a unique identifier for your app.
         * @param ivKey 16-bit initialization vector (IV) key used for encryption processes.
         * @param context The application context to be used for the initialization process.
         * @param debugMode A Boolean flag for enabling or disabling debug logs. 
         *                  Set to `false` in production for better performance and cleaner logs.
         * @param testMode A Boolean flag for enabling test mode. When set to `true`, the SDK will operate in a test environment.
         * @param forceMode A Boolean flag for forcing certain behaviors. 
         *                  Set to `true` to enable force mode, which overrides specific configurations.
         */
        FoxHat.initialize(
            "89012335678901234567890123456712",  // 32-bit API key
            "7890123456123456",                 // 16-bit IV key
            this,                               // Application context
            false,                              // Disable debug mode
            false,                              // Disable test mode
            true                                // Enable force mode
        )
 

    }
}
```

# Secure API Usage with FoxHat AntiSpam

To add the _foxhat header to your API requests, you need to use the challenge provided by FoxHat AntiSpam. Here's how to do it:

```java
import okhttp3.Interceptor
import okhttp3.Request
val headerInterceptor = Interceptor { chain ->
    val original: Request = chain.request()
    
    val token = FoxHat.getFoxHat().token()?.let { token -> 
        token
    } ?: ""

    val request: Request = original.newBuilder()
        .header("X-FoxHat", token)
        .method(original.method(), original.body())
        .build()

    chain.proceed(request)
}
```
