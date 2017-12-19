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
  * [Hello Littlstar - Boilerplate Code Example](#hello-littlstar)
  * [API Usage](#lsplayerdelegate-protocol)
    * [LSPlayerDelegate Protocols](#lsplayerdelegate-protocol)
    * [LSPlayer Methods](#lsplayer)
    * [LSPlayer Gestures](#gestures)
<br><br>

Installation
-------
### Cocoapods
Get [Cocoapods](http://guides.cocoapods.org/using/getting-started.html#installation)

Add the pod to your podfile
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

After installing the cocoapod into your project import ls-ios-sdk with Swift
`import ls_ios_sdk`
<br><br>

### Carthage
….TODO
<br><br>

Hello Littlstar
-------

### Boilerplate code
```
import ls_ios_sdk
class ViewController: UIViewController {

  var player: LSPlayer!

  override func viewDidLoad() {
    super.viewDidLoad()
    
    player = LSPlayer(frame: self.view.frame)
    player.delegate = self
    self.view.addSubview(player)
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

#### lsPlayerReadyWithVideo(duration: Double) - ???????  

Called when LSPlayer is ready to play the video. duration - the duration of the video  
<br>

#### lsPlayerHasUpdated(currentTime: Double, bufferedTime: Double) - ??????  

Called when LSPlayer has updated its state.  
<br>


#### lsPlayerHasEnded() - required method  

Called when the current video playing in LSPlayer ended.  
<br>

#### lsPlayerDidTap() - optional method  

Called when LSPlayer receives a tap.  
<br><br>

## LSPlayer

#### init(frame: CGRect, withMenu: Bool = true)  

Initialize a LSPlayer with or without the control menu that contains UI elements for controlling the video. (Play/Pause/VR cardboard mode/Seek)

```swift
// init player with the control menu
var player = LSPlayer(frame: self.view.frame)
var player = LSPlayer(frame: self.view.frame, withMenu: true)

// init player without the control menu
var player = LSPlayer(frame: self.view.frame, withMenu: false)
```

#### initMedia(_ file: URL, withHeatmap: Bool = false)

Loads 360 video or photo from the URL provided. If withHeatmap is set to true, logs the heatmap.

```swift
// init media without logging the heatmap
player.initMedia("https://my360video.m3u8")
player.initMedia("https://my360video.m3u8", withHeatmap: false)

// init with heatmap logging
player.initMedia("https://my360video.m3u8", withHeatmap: true)
```

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

#### invalidate()
Destroy, clean up, and remove player.

#### seek(to second: Int)

Programatically seek to specific timecode.

Set timecode to 30 seconds:
```swift
player.seek(to: 30)
```
Player behavior will follow what `player.isPlaying` is set to. ie if video is pause, video will stay paused at new timecode.
Automatically called when user interacts with timeline

#### playLongerAnimation(completionCallback: @escaping () -> ())
Plays the Littlstar animation and execute the code in the completionCallback when the animation is done.

```swift
    player.playLongerAnimation {
      self.player.close()
    }
```

#### close()
Calls `self.invalidate()` to destroy the LSPlayer. 
If the parent view controller of the LSPlayer is on a navigation controller stack, this function pops the the view controller off the stack. 
To Implement a custom `close()` function, override this function and make sure to call `self.invalidate()` to properly destroy the LSPlayer and prevent memory leaks.

```swift
player.close()
```

#### play()

Plays video from current timecode.
Sets `player.isPlaying` to `true`.
If showing a new video, starts playing.
If the video is already playing, has no effect.
If the media is an image, has no effect.

```swift
player.play()
```

#### pause()

Pauses video at current timecode.
Sets `player.isPlaying` to `false`.
If the video is already paused, has no effect.
If the media is an image, has no effect.

```swift
player.pause()
```

#### setVRMode(enable: Bool)
Enter or exit VR mode.

viewController - the viewController the LSPlayer is in. 
enable - when set to true, enters VR mode. Exits VR mode when set to false. 

```swift
// Enter VR mode
player.setVRMode(enable: true)

// Exit VR mode
player.setVRMode(enable: false)
```

<br><br>

## Gestures

### Single Tap
Toggles the menu UI.

### Pan Gesture (single finger)
Rotates 360 video.
