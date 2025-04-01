# QR Code Scanner (Updated)

This package is a fork of the original [qr_code_scanner](https://github.com/juliuscanute/qr_code_scanner) Flutter plugin, which was built using the now-unmaintained [ZXing](https://github.com/zxing/zxing) for Android and [MTBBarcodeScanner](https://github.com/mikebuss/MTBBarcodeScanner) for iOS. Since the original package has not been updated in a long time, I have taken the initiative to update and enhance it to work with the latest dependencies and Flutter versions.

## Updates & Fixes

- **Updated Android Dependencies**: Migrated to the latest Gradle, Kotlin, and CameraX libraries.
- **Updated iOS Dependencies**: Refactored to work seamlessly with the latest AVFoundation updates.
- **Fixed Namespace Configuration Issue**: Resolved the error "Could not create an instance of type com.android.build.api.variant.impl.LibraryVariantBuilderImpl. Namespace not specified."
- **Enhanced Performance**: Optimized the camera and barcode scanning performance for smoother scanning.
- **Better Web Compatibility**: Improved QR code detection and scanning on web platforms.
- **Bug Fixes**: Addressed various issues from the original repository.

## Installation

Add the package to your `pubspec.yaml`:

```yaml
dependencies:
  qr_code_scanner:
    git:
      url: https://github.com/yourusername/updated_qr_code_scanner.git
```

## Usage

```dart
class _QRViewExampleState extends State<QRViewExample> {
  final GlobalKey qrKey = GlobalKey(debugLabel: 'QR');
  Barcode? result;
  QRViewController? controller;

  @override
  void reassemble() {
    super.reassemble();
    if (Platform.isAndroid) {
      controller!.pauseCamera();
    } else if (Platform.isIOS) {
      controller!.resumeCamera();
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Column(
        children: <Widget>[
          Expanded(
            flex: 5,
            child: QRView(
              key: qrKey,
              onQRViewCreated: _onQRViewCreated,
            ),
          ),
          Expanded(
            flex: 1,
            child: Center(
              child: (result != null)
                  ? Text(
                      'Barcode Type: \${describeEnum(result!.format)}   Data: \${result!.code}')
                  : Text('Scan a code'),
            ),
          )
        ],
      ),
    );
  }

  void _onQRViewCreated(QRViewController controller) {
    this.controller = controller;
    controller.scannedDataStream.listen((scanData) {
      setState(() {
        result = scanData;
      });
    });
  }

  @override
  void dispose() {
    controller?.dispose();
    super.dispose();
  }
}
```

## Android Setup

Update the following files:

- **`android/build.gradle`**:
  ```gradle
  ext.kotlin_version = '1.5.10'
  classpath 'com.android.tools.build:gradle:4.2.0'
  ```
- **`android/gradle/wrapper/gradle-wrapper.properties`**:
  ```properties
  distributionUrl=https\://services.gradle.org/distributions/gradle-6.9-all.zip
  ```
- **`android/app/build.gradle`**:
  ```gradle
  defaultConfig {
    minSdkVersion 20
  }
  ```

## iOS Setup

Add the following permissions in `Info.plist`:

```xml
<key>io.flutter.embedded_views_preview</key>
<true/>
<key>NSCameraUsageDescription</key>
<string>This app needs camera access to scan QR codes</string>
```

## Web Setup

Add the following script to `web/index.html`:

```html
<script src="https://cdn.jsdelivr.net/npm/jsqr@1.3.1/dist/jsQR.min.js"></script>
```

## Additional Features

- **Flip Camera**:
  ```dart
  await controller.flipCamera();
  ```
- **Flash Toggle**:
  ```dart
  await controller.toggleFlash();
  ```
- **Pause/Resume Camera**:
  ```dart
  await controller.pauseCamera();
  await controller.resumeCamera();
  ```

## Credits

This project is originally based on:

- **Android Scanner**: [ZXing](https://github.com/zxing/zxing)
- **iOS Scanner**: [MTBBarcodeScanner](https://github.com/mikebuss/MTBBarcodeScanner)
- **Original Flutter Plugin**: [qr_code_scanner](https://github.com/juliuscanute/qr_code_scanner)

Thanks to the open-source community for making barcode scanning more accessible!
