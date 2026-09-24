# GB-Link Android Bridge (prototype)

This is a native Android wrapper intended to solve the Android CH340/WebSerial path for `switch.gblink.io` while leaving the GB-Link web client and ESP32 firmware unchanged.

## Target hardware
- ESP32-WROOM-32 / CH340: VID `0x1A86`, PID `0x7523`
- Android phone/tablet with USB Host / OTG
- Switch running FireRed or LeafGreen

## What it does
- Opens the CH340 directly through Android USB Host instead of relying on WebSerial.
- Configures the UART at 921600 8N1, matching the GB-Link host protocol.
- Injects a small `navigator.serial` compatibility layer into `switch.gblink.io` so the existing web client can use the native USB connection.
- Keeps `.pk3` and `prod.keys` handling in the existing GB-Link web client.

## Important
This is a **prototype**. It has not been built or hardware-tested in this environment. In particular, the JavaScript Web Serial compatibility layer may need small adjustments if the current GB-Link web client relies on additional SerialPort/ReadableStream methods.

The upstream GB-Link repository documents that the original ESP32 uses a USB-to-UART chip at 921600 baud, and that the web client carries the PC-to-Switch protocol over USB. The repository also publishes the v1 serial protocol and the v2 bridge framing. The current project is AGPL-3.0; this wrapper is intended as a separate client and does not include GB-Link source code.

## Build
Open this directory in Android Studio with an Android SDK installed. Use a current Android Gradle Plugin compatible with your Android Studio. Build `app` and install the debug APK.

Then:
1. Connect the ESP32 to the phone with a USB-OTG cable.
2. Grant USB permission to the app.
3. The app opens `switch.gblink.io/#switch`.
4. Use **PC to Switch** exactly as on desktop.

## If the web client rejects the polyfill
The next step is to replace the compatibility layer with a native port of the upstream C# host. The upstream host source is explicitly designed around the same serial protocol and runs the LDN/Pia/RFU/trade state machine on the PC.
