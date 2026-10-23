# Crypto Price Alarm (Android MVP)

A small Android app that monitors a Binance Spot trading pair using Binance's public WebSocket trade stream and rings the device's default alarm ringtone when the price crosses a target.

## Features
- Enter a Binance symbol such as `BTCUSDT`, `ETHUSDT`, or `WALUSDT` (use the exact Spot symbol available on Binance).
- Set a target price.
- Trigger on crossing above, below, or either direction.
- Foreground service with an ongoing notification while monitoring.
- Stop the alarm separately, or stop all monitoring.
- No Binance account/API key is needed; it uses public market data and does not place trades.

## Build an APK
1. Install the latest stable Android Studio.
2. Open the `CryptoPriceAlarm` folder in Android Studio.
3. Let Gradle sync. Internet is needed to download the Android Gradle Plugin and OkHttp dependency.
4. Connect an Android phone with USB debugging enabled, or use an emulator.
5. Click **Run** to install, or use **Build > Build Bundle(s) / APK(s) > Build APK(s)**.
6. Android Studio will show the APK location (normally `app/build/outputs/apk/debug/app-debug.apk`).
7. On the phone, open the app, allow notifications, enter a pair and target, and tap **START MONITORING**. Keep the persistent notification visible.

## Important behaviour and limitations
- The app checks actual incoming trade prices. It triggers on a crossing between consecutive messages, not merely because the first observed price is already above/below the target.
- This MVP monitors one pair at a time and is intended for personal sideloading/testing.
- Android battery management, loss of internet, Binance stream interruptions, force-stop, reboot, and OS restrictions can interrupt tracking. Exempt the app from battery optimization if your device offers that setting, but no phone-only app can guarantee 24/7 delivery.
- Android's `dataSync` foreground service type can be subject to OS limits (including time limits on newer Android versions). For robust long-duration alerts, a production version should use a backend service plus push notifications, or another Android-compliant architecture.
- The app does not request full-screen intent access; the alarm plays through the default alarm ringtone while the service is alive. Silent mode, Do Not Disturb, volume settings, or OEM restrictions may affect audibility.
- Target price is in the quote currency of the chosen pair. For `BTCUSDT`, target is in USDT.

## Tech
- Kotlin, Android SDK, OkHttp WebSocket
- Binance public trade stream: `wss://stream.binance.com:9443/ws/<symbol>@trade`

## Troubleshooting
- If it does not connect, check internet access and verify the symbol exists on Binance Spot.
- If notifications are missing, allow notifications for the app in Android Settings.
- If the sound is too quiet, raise the phone's Alarm volume and check Do Not Disturb settings.
