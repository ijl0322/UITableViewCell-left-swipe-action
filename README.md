Littlstar iOS SDK
=================
<br>

Introduction
-------
Littlstar iOS 360 photo/video player SDK
The Littlstar SDK is a developer library to easily build and implement mobile applications that utilize immersive **360° video & images** content. 360° video is a special type of video that covers the complete surroundings of the camera. Content can be hosted on the [*Littlstar*](http://littlstar.com) back-end service (recommended) or another back-end service. 360° videos and images can also be loaded and played from a users phone (sideloading).
<br><br>

Table of Contents
-------
  * [Introduction](#introduction)
  * [Installation](#installation)
  	* [Cocoapods](#cocoapods)
  	* [Carthage](#carthage)
  * [Configuration](#configuration)
  * [Hello Littlstar](#hello-littlstar)
    * [Boilerplate Code - Swift](#boilerplate-code---swift)
    * [Boilerplate Code - Objective-C](#boilerplate-code---objective-c)
  * [API Usage](#lsplayerdelegate-protocol)
    * [LSPlayerDelegate Protocols](#lsplayerdelegate-protocol)
    * [LSPlayer](#lsplayer)
      * [Initialization](#initialization)
      * [Properties](#properties)
      * [Functions](#functions)
    * [LSPlayer Gestures](#gestures)
<br><br>

Installation
-------
### Cocoapods
Get [Cocoapods](http://guides.cocoapods.org/using/getting-started.html#installation)<br>
Navigate to the directory of the Xcode project.<br>
run<br>
```
$ pod init
```
A podfile should be created. Add `pod 'ls-ios-sdk'` to your podfile
```
platform :ios, "9.0"
workspace 'Hello Littlstar 360 Player'

target "Hello Littlstar 360 Player" do
    pod 'ls-ios-sdk'
end
```
run
```
$ pod install
```
Make sure to open the xcworkspace generated rather than the xcodeproj file.
After installing the cocoapod into your project import ls-ios-sdk.

```swift
// Swift
import ls_ios_sdk
```

```Objective-C
// Objective-C
@import ls_ios_sdk;
```
<br><br>

### Carthage
….TODO
<br><br>

Configuration
-------
...TODO
<br><br>

Hello Littlstar
-------

### Boilerplate code - Swift
```swift
import ls_ios_sdk
class ViewController: UIViewController {

  var player: LSPlayer!

  override func viewDidLoad() {
    super.viewDidLoad()
    
    // Initialize LSPlayer
    player = LSPlayer(frame: self.view.frame)
    player.delegate = self
    self.view.addSubview(player)

    // Initialize Media
    let hlsExample = URL(string:"https://360.littlstar.com/production/76b490a5-2125-4281-b52d-8198ab0e817d/mobile_hls.m3u8")!
    player.initMedia(hlsExample)
  }
  
  @objc func play() {
    if player.isPlaying {
      player.pause()
    } else {
      player.play()
    }
  }
  
  @objc func mute() {
    if player.isMuted {
      player.isMuted = false
    } else {
      player.isMuted = true
    }
  }
}

// Conform to the LSPlayerDelegate protocol and implement all required and/or optional delegate methods
extension ViewController: LSPlayerDelegate {
  func lsPlayer(isBuffering: Bool) {
    if isBuffering {
      print("Video is buffering")
    } else {
      print("Video is not buffering")
    }
  }

  func lsPlayerReadyWithImage() {
    print("Player is ready to display the image")
  }

  func lsPlayerReadyWithVideo(duration: Double) {
    player.play()
    print("Player is ready to display the 360 video")
  }

  func lsPlayerHasUpdated(currentTime: Double, bufferedTime: Double) {
    print("Player has updated its state")
  }

  func lsPlayerHasEnded() {
    player.close()
    print("Video has ended")
  }

  func lsPlayerDidTap() {
    print("The user tapped the player")
  }
}

```

### Boilerplate code - Objective-C

```Objective-C
@import ls_ios_sdk;

@interface ObjectiveCViewController() <LSPlayerDelegate>

@end

@implementation ObjectiveCViewController

LSPlayer *player;

- (void)viewDidLoad {
    [super viewDidLoad];
    
    // Initialize LSPlayer
    player = [[LSPlayer alloc] initWithFrame:self.view.frame withMenu:true];
    player.delegate = self;
    [self.view addSubview:player];
    
    // Initialize Media
    [player initMedia:[NSURL URLWithString:@"https://360.littlstar.com/production/76b490a5-2125-4281-b52d-8198ab0e817d/mobile_hls.m3u8"] withHeatmap:false];
}

// Conform to the LSPlayerDelegate protocol and implement all required and/or optional delegate methods
- (void)lsPlayerReadyWithVideoWithDuration:(double)duration {
    NSLog(@"Player is ready to display the 360 video");
    [player play];
}

- (void)lsPlayerHasEnded {
    NSLog(@"Video has ended");
    [player close];
}

- (void)lsPlayerReadyWithImage {
    NSLog(@"Player is ready to display the image");
}

- (void)lsPlayerWithIsBuffering:(BOOL)isBuffering {
    if (isBuffering) {
        NSLog(@"Player is buffering");
    } else {
        NSLog(@"Player is not buffering");
    }
}

- (void)lsPlayerHasUpdatedWithCurrentTime:(double)currentTime bufferedTime:(double)bufferedTime {
    NSLog(@"Player has updated its state");
}

@end
```
<br><br>
## LSPlayerDelegate Protocol

Conform to this protocol to get notified of different events and state of the LSPlayer  
<br>
#### lsPlayer(isBuffering: Bool) - required method  

Called when LSPlayer changes its buffering state. isBuffering - true when the video is buffering, false  otherwise.  
<br>

#### lsPlayerReadyWithImage() - required method  

Called when LSPlayer is ready to display the image.  
<br>

#### lsPlayerReadyWithVideo(duration: Double) - required method

Called when LSPlayer is ready to play the video. duration - the duration of the video  
<br>

#### lsPlayerHasUpdated(currentTime: Double, bufferedTime: Double) - required method

Called when LSPlayer has updated its state.<br>  
currentTime - the current timecode of the video.<br>
bufferedTime - the number of seconds buffered.
<br>

#### lsPlayerHasEnded() - required method  

Called when the current video playing in LSPlayer ended.  
<br>

#### lsPlayerDidTap() - optional method  

Called when LSPlayer receives a tap.  
<br><br>

## LSPlayer

### Initialization

#### init(frame: CGRect, withMenu: Bool = true)  

Initialize a LSPlayer with or without the control menu that contains UI elements for controlling the video. (Play/Pause/VR cardboard mode/Seek)

```swift
// Swift

// init player with the control menu
var player = LSPlayer(frame: self.view.frame)
var player = LSPlayer(frame: self.view.frame, withMenu: true)

// init player without the control menu
var player = LSPlayer(frame: self.view.frame, withMenu: false)
```

```objective-c
// Objective-C

// init player with the control menu
LSPlayer *player = [[LSPlayer alloc] initWithFrame:self.view.frame withMenu:true];

// init player without the control menu
LSPlayer *player = [[LSPlayer alloc] initWithFrame:self.view.frame withMenu:false];
```

#### initMedia(_ file: URL, withHeatmap: Bool = false)

Loads 360 video or photo from the URL provided. If withHeatmap is set to true, logs the heatmap.

```swift
// Swift

// init media without logging the heatmap
player.initMedia("https://my360video.m3u8")
player.initMedia("https://my360video.m3u8", withHeatmap: false)

// init with heatmap logging
player.initMedia("https://my360video.m3u8", withHeatmap: true)
```

```objective-c
// Objective-C

// init media without logging the heatmap
[player initMedia:[NSURL URLWithString:@"https://my360video.m3u8"] withHeatmap:false];

// init with heatmap logging
[player initMedia:[NSURL URLWithString:@"https://my360video.m3u8"] withHeatmap:true];
```
---

### Properties

#### delegate: LSPlayerDelegate?
The delegate object that conforms to the LSPlayerDelegate protocol.

```swift
player.delegate = self
```

#### isPlaying: Bool
Get only property. A boolean that denotes whether the video is currently playing.  

```swift
player.isPlaying
```

#### currentTime: Double?
Get only property. Returns the current time of the video.

```swift
player.currentTime
```

#### duration: Double?
Get only property. Returns the duration of the video.

```swif
player.duration
```

#### isMuted: Bool
Get/Set property. A boolean that denotes whether the video is muted. 

```swift
if player.isMuted == false {
     player.isMuted = true
}
```
---
### Functions

#### invalidate()
Destroy, clean up, and remove player. <br>
```swift
// Swift
player.invalidate()
```
```objective-c
// Objective-C
[player invalidate];
```

#### seek(to second: Int)

Programatically seek to specific timecode.

Set timecode to 30 seconds: <br>

```swift
// Swift
player.seek(to: 30)
```

```objective-c
// Objective-C
[player seekTo:30];
```
Player behavior will follow what `player.isPlaying` is set to. ie if video is pause, video will stay paused at new timecode.
Automatically called when user interacts with timeline

#### playLongerAnimation(completionCallback: @escaping () -> ())
Plays the Littlstar animation and execute the code in the completionCallback when the animation is done.

```swift
// Swift
player.playLongerAnimation {
  self.player.close()
}
```

```objective-c
// Objective-C
[player playLongerAnimationWithCompletionCallback:^{
    [player close];
}];
```

#### close()
Calls `self.invalidate()` to destroy the LSPlayer. 
If the parent view controller of the LSPlayer is on a navigation controller stack, this function pops the the view controller off the stack. 
To Implement a custom `close()` function, override this function and make sure to call `self.invalidate()` to properly destroy the LSPlayer and prevent memory leaks.

```swift
// Swift
player.close()
```

```objective-c
// Objective-C
[player close];
```

#### play()

Plays video from current timecode.
Sets `player.isPlaying` to `true`.
If showing a new video, starts playing.
If the video is already playing, has no effect.
If the media is an image, has no effect.

```swift
// Swift
player.play()
```

```objective-c
// Objective-C
[player play];
```
#### pause()

Pauses video at current timecode.
Sets `player.isPlaying` to `false`.
If the video is already paused, has no effect.
If the media is an image, has no effect.

```swift
// Swift
player.pause()
```

```objective-c
// Objective-C
[player pause];
```

#### setVRMode(enable: Bool)
Enter or exit VR cardboard mode.
enable - when set to true, enters VR cardboard mode. Exits VR cardboard mode when set to false. 

```swift
// Swift

// Enter VR cardboard mode
player.setVRMode(enable: true)

// Exit VR cardboard mode
player.setVRMode(enable: false)
```

```objective-c
// Objective-C

// Enter VR cardboard mode
[player setVRModeWithEnable:true]; 

// Exit VR cardboard mode
[player setVRModeWithEnable:false]; 
```

<br><br>

## Gestures

#### Single Tap
Toggles the menu UI.
Calls the LSPlayerDelegate method lsPlayerDidTap() if it is implemented. 

#### Pan Gesture (single finger)
Rotates 360 video.
