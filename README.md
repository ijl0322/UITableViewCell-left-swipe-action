
---

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

`player.delegate = self`

#### isPlaying: Bool
Get only property. A boolean that denotes whether the video is currently playing.  

`player.isPlaying`

#### currentTime: Double
Get only property. Returns the current time of the video.

`player.currentTime`

#### duration: Double
Get only property. Returns the duration of the video.

`player.duration`

#### isMuted: Bool
Get/Set property. A boolean that denotes whether the video is muted. 

`if false == player.isMuted {
     player.isMuted = true
}`

#### initMedia(_ file: URL)

Loads 360 video or photo from the URL provided.

`player.initMedia("https://my360video.m3u8")`


#### play()

Plays video from current timecode.
Sets `player.isPlaying` to `true`.
If showing a new video, starts playing.
If the video is already playing, has no effect.
If the media is an image, has no effect.

`player.play()`

#### setVRMode(in viewController: UIViewController?, enable: Bool)
Enter or exit VR mode.


viewController - the viewController the LSPlayer is in. 
enable - when set to true, enters VR mode. Exits VR mode when set to false. 

#### pause()

Pauses video at current timecode.
Sets `player.isPlaying` to `false`.
If the video is already paused, has no effect.
If the media is an image, has no effect.

`player.pause()`


#### seek(to second: Int)

Programatically seek to specific timecode.

Set timecode to 30 seconds:
`player.seek(to: 30)`

Player behavior will follow what `player.isPlaying` is set to. ie if video is pause, video will stay paused at new timecode.
Automatically called when user interacts with timeline


#### invalidate()
`player.invalidate()`
destroy, clean up, and remove player.


#### lsMediaReadyWithImage()

???? hey Huy, I don't think this ever gets called right now.


>**Note:** The following functions do not apply to images, only videos

#### lsMediaReadyWithVideo(duration: Double)

Fires with video is ready to play... ??? is this really public?


#### lsMediaHasEnded()

Fires when video has ended.


#### lsMediaHasUpdated(currentTime: Double, bufferedTime: Double)

Fires when player has received updated frames to play.


#### lsMedia(isBuffering: Bool)

???????




### Gestures

#### Single Tap
Toggles the menu UI.

#### 
single finger pan rotates 360 photo/video
