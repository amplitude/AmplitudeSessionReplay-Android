<p align="center">
  <a href="https://amplitude.com" target="_blank" align="center">
    <img src="https://static.amplitude.com/brand/amplitude-logo-with-text.svg" width="280">
  </a>
  <br />
</p>

# AmplitudeSessionReplay-Android

## Integrations

AmplitudeSessionReplay integrates via plugin to your core analytics library. 

## Instructions
Depending on the analytics library you use, you can use one of the following integrations to capture session replays.

### Amplitude-Kotlin SDK

1. Install the dependencies
```gradle
implementation("com.amplitude:plugin-session-replay-android:0.30.0")
implementation("com.amplitude:analytics-android:1.30.0")
```
2. Initialize the SDK
```kotlin
// Initialize Amplitude Analytics SDK instance
val amplitude = Amplitude(Configuration(
    apiKey = API_KEY,
    context = applicationContext,
    defaultTracking = DefaultTrackingOptions(sessions = true),
))
 
// Create and install Session Replay Plugin
// Recording is handled automatically
val sessionReplayPlugin = SessionReplayPlugin(
    context = applicationContext,
    sampleRate = 1.0,
)
amplitude.add(sessionReplayPlugin)
```
[Developer docs](https://amplitude.com/docs/sdks/session-replay/session-replay-android-plugin)

### Amplitude-Android SDK (Legacy SDK)
1. Install the dependencies
```gradle
implementation("com.amplitude:middleware-session-replay-android:0.30.0")
implementation("com.amplitude:android-sdk:2.40.1")
```
2. Initialize the SDK
```kotlin
val amplitude = Amplitude.getInstance()
    .initialize(this, AMPLITUDE_API_KEY)
    // Replay events are flushed on close as well
    // If setFlushEventsOnClose(false) you must call flush() manually
    .setFlushEventsOnClose(true)
 
// Create Session Replay Middleware
val sessionReplayMiddleware = SessionReplayMiddleware(amplitude, sampleRate = 1.0)
 
// Add session replay middleware
// Recording is handled automatically
amplitude.addEventMiddleware(sessionReplayMiddleware)
```
[Developer docs](https://amplitude.com/docs/sdks/session-replay/session-replay-android-middleware)

### Standalone SDK
Use the standalone SDK when capturing replays alongside another analytics provider or custom event pipeline.

1. Install the dependency
```gradle
implementation("com.amplitude:session-replay-android:0.30.0")
```
2. Initialize the SDK
```kotlin
val sessionReplay = SessionReplay(
    apiKey = API_KEY,
    context = applicationContext,
    deviceId = "DEVICE_ID",
    sessionId = System.currentTimeMillis(),
    sampleRate = 1.0,
)

// Add session replay properties to your analytics events
val sessionProperties = sessionReplay.getSessionReplayProperties()
```
[Developer docs](https://amplitude.com/docs/sdks/session-replay/session-replay-android-standalone)

## Masking

Session Replay masks text input fields by default. The `maskLevel` in your `PrivacyConfig` controls how much beyond that is masked, including `light`, `medium` (the default), or `conservative`. See [Mask on-screen data](https://amplitude.com/docs/sdks/session-replay/session-replay-android-plugin#mask-on-screen-data) for the full reference.

Use the options below to override masking for individual elements.

### Layout XML

Set one of the Amplitude privacy tags as a view's `android:tag`:

| Tag | Effect |
|-----|--------|
| `amp-mask` | Captures the element's text as a series of asterisks. |
| `amp-unmask` | Captures the element unmasked, opting it out of the current mask level. |
| `amp-block` | Replaces the element with a placeholder of the same dimensions. |

```xml
<TextView android:tag="amp-mask" android:text="Mask this" />
<EditText android:tag="amp-unmask" android:text="Unmask this" />
<ImageView android:tag="amp-block" />
```

### Views in code

Call the equivalent method on `SessionReplay` with any `View` reference:

```kotlin
import com.amplitude.android.sessionreplay.SessionReplay

SessionReplay.mask(view)
SessionReplay.unmask(view)
SessionReplay.block(view)
```

`SessionReplayPlugin` and `SessionReplayMiddleware` expose the same three methods, so you can call `SessionReplayPlugin.mask(view)` instead if you prefer not to import `SessionReplay` directly.

These methods work by setting the view's tag, so they replace any tag already set on that view.

### Jetpack Compose

Compose elements have no `android:tag`, so use the modifiers in `com.amplitude.android.sessionreplay.compose`:

| Modifier | Effect |
|----------|--------|
| `Modifier.ampMask()` | Captures the composable's text as a series of asterisks. |
| `Modifier.ampUnmask()` | Captures the composable unmasked, opting it out of the current mask level. |
| `Modifier.ampBlock()` | Replaces the composable with a placeholder of the same dimensions. |

```kotlin
import com.amplitude.android.sessionreplay.compose.ampBlock
import com.amplitude.android.sessionreplay.compose.ampMask
import com.amplitude.android.sessionreplay.compose.ampUnmask

Text("Mask this", modifier = Modifier.ampMask())

TextField(value = value, onValueChange = onValueChange, modifier = Modifier.ampUnmask())

Image(painter = painter, contentDescription = null, modifier = Modifier.ampBlock())
```

A modifier applies to the composable it's attached to and to everything below it in the composition, so you can mask an entire subtree by placing `ampMask()` on its container.

## Need Help?
If you have any issues using our SDK, feel free to [create a GitHub issue](https://github.com/amplitude/AmplitudeSessionReplay-Android/issues/new/choose) or submit a request on [Amplitude Help](https://help.amplitude.com/hc/en-us/requests/new).
