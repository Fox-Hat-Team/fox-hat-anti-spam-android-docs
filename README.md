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
          FoxHat.initialize(
                        this,
                        false, //debugMode
                        false,//testMode
                        true  //forceMode
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
        .header("_foxhat", challenge) 
        .header("X-FoxHat", token)
        .method(original.method(), original.body())
        .build()

    chain.proceed(request)
}
```
