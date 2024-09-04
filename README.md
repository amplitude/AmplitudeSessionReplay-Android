<p align="center">
  <a href="https://amplitude.com" target="_blank" align="center">
    <img src="https://static.amplitude.com/lightning/46c85bfd91905de8047f1ee65c7c93d6fa9ee6ea/static/media/amplitude-logo-with-text.4fb9e463.svg" width="280">
  </a>
  <br />
</p>

# AmplitudeSessionReplay-Android

## Integrations

AmplitudeSessionReplay integrates via plugin to your core analytics library. 

## Instructions
Depending on the analytics library you use, you can use one of the following integrations to capture session replays.

### Amplidue-Kotlin SDK

1. Install the dependencies
```gradle
implementation("com.amplitude:plugin-session-replay-android:[0.10.0, 1.0.0]")
implementation("com.amplitude:analytics-android:[1.16.7, 2.0.0]")
```
2. Initialize the SDK
```kotlin
// Initialize Amplitude Analytics SDK instance
val amplitude = Amplitude(Configuration(
    apiKey = API_KEY,
    context = applicationContext,
    defaultTracking = DefaultTrackingOptions(sessions = true),
))
 
// Create and Install Session Replay Plugin
// Recording will be handled automatically
val sessionReplayPlugin = SessionReplayPlugin(sampleRate = 1.0)
amplitude.add(sessionReplayPlugin)
```

### Ampitude-Android SDK (Legacy SDK)
1. Install the dependencies
```gradle
implementation("com.amplitude:middleware-session-replay-android:[0.10.0, 1.0.0]")
implementation("com.amplitude:android-sdk:[2.40.1,3.0.0]")
```
2. Initialize the SDK
```Kotlin
val amplitude = Amplitude.getInstance()
    .initialize(this, AMPLITUDE_API_KEY)
    // Replay events will be flushed on close as well
    // If setFlushEventsOnClose(false) you must call flush() manually
    .setFlushEventsOnClose(true)
 
// Create Session Replay Middleware
val sessionReplayMiddleware = SessionReplayMiddleware(amplitude, sampleRate = 1.0)
 
// Add session replay middleware
// Recording will be handled automatically
amplitude.addEventMiddleware(sessionReplayMiddleware)
```
