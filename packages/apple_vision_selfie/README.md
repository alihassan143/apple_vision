# apple\_vision\_selfie

[![Pub Version](https://img.shields.io/pub/v/apple_vision_selfie)](https://pub.dev/packages/apple_vision_selfie)
[![analysis](https://github.com/Knightro63/apple_vision/actions/workflows/flutter.yml/badge.svg)](https://github.com/Knightro63/apple_vision/actions/)
[![Star on Github](https://img.shields.io/github/stars/Knightro63/apple_vision.svg?style=flat&logo=github&colorB=deeppink&label=stars)](https://github.com/Knightro63/apple_vision)
[![License: MIT](https://img.shields.io/badge/license-MIT-purple.svg)](https://opensource.org/licenses/MIT)

Apple Vision Selfie Detection is a Flutter plugin that enables Flutter apps to use [Apple Vision Selfie Segmentation](https://developer.apple.com/documentation/vision/applying_matte_effects_to_people_in_images_and_video).

- This plugin is not sponsor or maintained by Apple. The [authors](https://github.com/Knightro63/apple_vision/blob/main/AUTHORS) are developers who wanted to make a similar plugin to Google's ml kit for macos.

## Requirements

**MacOS**
 - Minimum osx Deployment Target: 12.0
 - Xcode 13 or newer
 - Swift 5
 - ML Kit only supports 64-bit architectures (x86_64 and arm64).

**iOS**
 - In develpoment not yet supported
 - Minimum ios Deployment Target: 13.0
 - Xcode 13 or newer
 - Swift 5
 - ML Kit only supports 64-bit architectures (x86_64 and arm64).

## Getting Started

You need to first import 'package:apple_vision/apple_vision.dart';

```dart
  final GlobalKey cameraKey = GlobalKey(debugLabel: "cameraKey");
  late AppleVisionSelfieController visionController = AppleVisionSelfieController();
  InsertCamera camera = InsertCamera();
  Size imageSize = const Size(640,640*9/16);
  String? deviceId;
  bool loading = true;

  List<Uint8List?>? selfieImage;
  late double deviceWidth;
  late double deviceHeight;

  @override
  void initState() {
    camera.setupCameras().then((value){
      setState(() {
        loading = false;
      });
      camera.startLiveFeed((InputImage i){
        if(i.metadata?.size != null){
          imageSize = i.metadata!.size;
        }
        if(mounted) {
          Uint8List? image = i.bytes;
          visionController.processImage(image!, i.metadata!.size, PictureFormat.png).then((data){
            selfieImage = data;
            setState(() {
              
            });
          });
        }
      });
    });
    super.initState();
  }
  @override
  void dispose() {
    camera.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    deviceWidth = MediaQuery.of(context).size.width;
    deviceHeight = MediaQuery.of(context).size.height;
    return Stack(
      children:<Widget>[
        SizedBox(
          width: imageSize.width, 
          height: imageSize.height, 
          child: loading?Container():CameraSetup(camera: camera, size: imageSize)
        ),
        if(selfieImage?[0] != null) SizedBox(
          width: imageSize.width, 
          height: imageSize.height, 
          child: Image.memory(selfieImage![0]!, fit: BoxFit.fill,)
        )
      ]
    );
  }

  Widget loadingWidget(){
    return Container(
      width: deviceWidth,
      height: deviceHeight,
      color: Theme.of(context).canvasColor,
      alignment: Alignment.center,
      child: const CircularProgressIndicator(color: Colors.blue)
    );
  }
```

## Example

Find the example for this API [here](https://github.com/Knightro63/apple_vision/tree/main/packages/apple_vision_selfie/example/lib/main.dart).

## Contributing

Contributions are welcome.
In case of any problems look at [existing issues](https://github.com/Knightro63/apple_vision/issues), if you cannot find anything related to your problem then open an issue.
Create an issue before opening a [pull request](https://github.com/Knightro63/apple_vision/pulls) for non trivial fixes.
In case of trivial fixes open a [pull request](https://github.com/Knightro63/apple_vision/pulls) directly.
