Littlstar iOS SDK

## Introduction

Littlstar iOS 360 photo/video player SDK
The Littlstar SDK is a developer library to easily build and implement mobile applications that utilize immersive **360° video & images** content. 360° video is a special type of video that covers the complete surroundings of the camera. Content can be hosted on the [*Littlstar*](http://littlstar.com) back-end service (recommended) or another back-end service. 360° videos and images can also be loaded and played from a users phone (sideloading).

Table of Contents
=================
  * [Introduction](#introduction)
  * [Installation](#installation)
  	* [Cocoapods](#cocoapods)
  	* [Carthage](#carthage)
  * [Hello Littlstar - Boilerplate Code Example](#hello-littlstar)
  * API Usage
    * [LSPlayerDelegate Protocols](#lsplayerdelegate-protocol)
    * [LSPlayer Methods](#lsplayer)


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

### Carthage
….TODO

## Hello Littlstar


### Boilerplate code
```
import ls_ios_sdk
class ViewController: UIViewController {

  var player: LSPlayer!

  override var prefersStatusBarHidden: Bool {
    return true
  }

  override func viewDidLoad() {
    super.viewDidLoad()

    player = LSPlayer(frame: self.view.frame, viewController: self)
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
```
## LSPlayerDelegate Protocol

Conform to this protocol to get notified of different events and state of the LSPlayer  


### lsPlayer(isBuffering: Bool) - required method  

Called when LSPlayer changes its buffering state. isBuffering - true when the video is buffering, false  otherwise.  



### lsPlayerReadyWithImage() - required method  

Called when LSPlayer is ready to display the image.  



### lsPlayerReadyWithVideo(duration: Double) - ???????  

Called when LSPlayer is ready to play the video. duration - the duration of the video  



### lsPlayerHasUpdated(currentTime: Double, bufferedTime: Double) - ??????  

Called when LSPlayer has updated its state.  



### lsPlayerHasEnded() - required method  

Called when the current video playing in LSPlayer ended.  



### lsPlayerDidTap() - optional method  

Called when LSPlayer receives a tap.  



---

## LSPlayer

### init(frame: CGRect)  

```swift
var player = LSPlayer(frame: self.view.frame)
```

### delegate: LSPlayerDelegate?
The delegate object that conforms to the LSPlayerDelegate protocol.    

```swift
player.delegate = self
```

#### isPlaying: Bool
Get only property. A boolean that denotes whether the video is currently playing.  

```swift
player.isPlaying
```

#### currentTime: Double
Get only property. Returns the current time of the video.

```swift
player.currentTime
```

#### duration: Double
Get only property. Returns the duration of the video.

```swif
player.duration
```

#### isMuted: Bool
Get/Set property. A boolean that denotes whether the video is muted. 

```swift
if false == player.isMuted {
     player.isMuted = true
}
```

#### initMedia(_ file: URL)

Loads 360 video or photo from the URL provided.

```swift
player.initMedia("https://my360video.m3u8")
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

#### setVRMode(in viewController: UIViewController?, enable: Bool)
Enter or exit VR mode.


viewController - the viewController the LSPlayer is in. 
enable - when set to true, enters VR mode. Exits VR mode when set to false. 

#### pause()

Pauses video at current timecode.
Sets `player.isPlaying` to `false`.
If the video is already paused, has no effect.
If the media is an image, has no effect.

```swift
player.pause()
```


#### seek(to second: Int)

Programatically seek to specific timecode.

Set timecode to 30 seconds:
```swift
player.seek(to: 30)
```
Player behavior will follow what `player.isPlaying` is set to. ie if video is pause, video will stay paused at new timecode.
Automatically called when user interacts with timeline


#### invalidate()
```swift
player.invalidate()
```
destroy, clean up, and remove player.


## Gestures

### Single Tap
Toggles the menu UI.

### Pan Gesture (single finger)
Rotates 360 video.
