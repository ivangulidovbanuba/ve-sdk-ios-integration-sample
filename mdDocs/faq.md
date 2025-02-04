# FAQ  

These are the answers to the most common questions asked about our SDK.

### 1. How do I start/stop recording with a tap?
  
By default, the user must hold the “record” button to film and release it to stop filming. 

To change that, set the **captureButtonMode** property of the RecorderConfiguration entity to .video.

``` json
 var config = VideoEditorConfig()
 config.recorderConfiguration.captureButtonMode = .video
```

### 2.How do I add an AR mask to the app (without AR cloud)

If you don’t want to pull the masks from the backend, you can include them in the app itself. 

A mask is a bundle of files within a specific folder in the YourProject/bundleEffects/ directory. The preview.png file in the filter folder is used as an icon within the app, and the name of the directory is also the name of the mask.  

 **Note** Please, don’t change the name of the bundleEffects folder, otherwise, the app will not work. If it doesn’t exist already, create it manually.

### 3. How do I start the Video Editor with a preselected audio track?

To do so, add the MediaTrack instance as a parameter to the presentVideoEditor method.

```
videoEditorSDK?.presentVideoEditor(
      from: YourViewController,
      animated: true,
      musicTrack: MediaTrack(...),
      completion: {...}
    )
```

### 4. How do I use the Video Editor several times from different entry points?

Before you want to use VideoEditor again, you need to deinitialize your current editor instance in your [entry point class scope](https://github.com/Banuba/ve-sdk-ios-integration-sample/blob/d9733e78a6a752dd8fad849f6aa6d5553eb07f56/Example/Example/ViewController.swift#L675). You need to set 'yourVideoEditorSdkInstance' = nil after following funcs called([done](https://github.com/Banuba/ve-sdk-ios-integration-sample/blob/d9733e78a6a752dd8fad849f6aa6d5553eb07f56/Example/Example/ViewController.swift#L660) and [cancel](https://github.com/Banuba/ve-sdk-ios-integration-sample/blob/d9733e78a6a752dd8fad849f6aa6d5553eb07f56/Example/Example/ViewController.swift#L678)).

```

// Video Editor Delegate implementation example
extension ViewController: BanubaVideoEditorDelegate {
  func videoEditorDone(_ videoEditor: BanubaVideoEditor) {
    // User finished editing sessoin, need to dismiss video editor and export video
    videoEditorSDK?.dismissVideoEditor(
      animated: true
    ) { [weak self] in
      self?.exportVideo(...) { ... in
         self?.'yourVideoEditorSdkInstance' = nil
      }
    }
  }
  
  func videoEditorDidCancel(
    _ videoEditor: BanubaVideoEditor
  ) {
    // User canceled editing sessoin, need to dismiss video editor
    videoEditorSDK?.dismissVideoEditor(
      animated: true,
      completion: {
        self?.'yourVideoEditorSdkInstance' = nil
      }
    )
  }
}

```

Use the following approach if you want to [create BanubaVideoEditor instance](https://github.com/Banuba/ve-sdk-ios-integration-sample/blob/d9733e78a6a752dd8fad849f6aa6d5553eb07f56/Example/Example/ViewController.swift#L42) again. 

For example on your tap button action:

```

@IBAction func videoEditorButtonTapped(...) {
   if 'yourVideoEditorSdkInstance' = nil {
      'yourVideoEditorSdkInstance' = BanubaVideoEditor(...)
   }
}

```

### 5. How do I add a color filter (LUT)?

Color filters (LUTs) are special graphic files placed into the /luts directory inside the host project folder.

To add your own icon that will be used to represent this particular effect in the list, you should place it into the /assets folder.

The name of the icon resource must be the same as the graphic file `ID` the / luts directory.

| Default image color filter | Name      | ID      |
| ---------- | ---------  | ----------- |
|<img src="https://github.com/Banuba/ve-sdk-ios-integration-sample/blob/main/Example/Example/Assets.xcassets/Filters%20Preview/101000_preview.imageset/glitch-1.png" width="50"> | glitch | 101000
|  <img src="https://github.com/Banuba/ve-sdk-ios-integration-sample/blob/main/Example/Example/Assets.xcassets/Filters%20Preview/101001_preview.imageset/instant-1.png" width="50"> | instant  | 101001
| <img src="https://github.com/Banuba/ve-sdk-ios-integration-sample/blob/main/Example/Example/Assets.xcassets/Filters%20Preview/101002_preview.imageset/grunge-1.png" width="50">  | grunge | 101002
| <img src="https://github.com/Banuba/ve-sdk-ios-integration-sample/blob/main/Example/Example/Assets.xcassets/Filters%20Preview/101003_preview.imageset/retro-1.png" width="50">  |  retro | 101003
| <img src="https://github.com/Banuba/ve-sdk-ios-integration-sample/blob/main/Example/Example/Assets.xcassets/Filters%20Preview/101004_preview.imageset/pinkvine-1.png" width="50">  |  pinkvine | 101004
| <img src="https://github.com/Banuba/ve-sdk-ios-integration-sample/blob/main/Example/Example/Assets.xcassets/Filters%20Preview/101005_preview.imageset/england-1.png" width="50">  |  england | 101005
| <img src="https://github.com/Banuba/ve-sdk-ios-integration-sample/blob/main/Example/Example/Assets.xcassets/Filters%20Preview/101006_preview.imageset/spark-1.png" width="50">  |  spark | 101006
|  <img src="https://github.com/Banuba/ve-sdk-ios-integration-sample/blob/main/Example/Example/Assets.xcassets/Filters%20Preview/101007_preview.imageset/korben-1.png" width="50"> |  korben | 101007
|  <img src="https://github.com/Banuba/ve-sdk-ios-integration-sample/blob/main/Example/Example/Assets.xcassets/Filters%20Preview/101008_preview.imageset/remy-1.png" width="50"> |  remy | 101008
|  <img src="https://github.com/Banuba/ve-sdk-ios-integration-sample/blob/main/Example/Example/Assets.xcassets/Filters%20Preview/101009_preview.imageset/canada-1.png" width="50"> |  canada | 101009
| <img src="https://github.com/Banuba/ve-sdk-ios-integration-sample/blob/main/Example/Example/Assets.xcassets/Filters%20Preview/101010_preview.imageset/sunny-1.png" width="50">  |  sunny | 101010
|  <img src="https://github.com/Banuba/ve-sdk-ios-integration-sample/blob/main/Example/Example/Assets.xcassets/Filters%20Preview/101011_preview.imageset/egypt-1.png" width="50"> |  egypt | 101011
| <img src="https://github.com/Banuba/ve-sdk-ios-integration-sample/blob/main/Example/Example/Assets.xcassets/Filters%20Preview/101012_preview.imageset/new_zeland-1.png" width="50">  |  new_zeland | 101012
|  <img src="https://github.com/Banuba/ve-sdk-ios-integration-sample/blob/main/Example/Example/Assets.xcassets/Filters%20Preview/101013_preview.imageset/chroma-1.png" width="50"> |  chroma | 101013
|  <img src="https://github.com/Banuba/ve-sdk-ios-integration-sample/blob/main/Example/Example/Assets.xcassets/Filters%20Preview/101014_preview.imageset/byers-1.png" width="50"> |  byers | 101014
| <img src="https://github.com/Banuba/ve-sdk-ios-integration-sample/blob/main/Example/Example/Assets.xcassets/Filters%20Preview/101015_preview.imageset/lilac-1.png" width="50">  |  lilac | 101015
|  <img src="https://github.com/Banuba/ve-sdk-ios-integration-sample/blob/main/Example/Example/Assets.xcassets/Filters%20Preview/101016_preview.imageset/hyla-1.png" width="50"> |  hyla | 101016
|  <img src="https://github.com/Banuba/ve-sdk-ios-integration-sample/blob/main/Example/Example/Assets.xcassets/Filters%20Preview/101017_preview.imageset/bubblegum-1.png" width="50"> |  bubblegum | 101017
| <img src="https://github.com/Banuba/ve-sdk-ios-integration-sample/blob/main/Example/Example/Assets.xcassets/Filters%20Preview/101018_preview.imageset/vinyl-1.png" width="50">  | vinyl | 101018
| <img src="https://github.com/Banuba/ve-sdk-ios-integration-sample/blob/main/Example/Example/Assets.xcassets/Filters%20Preview/101019_preview.imageset/lux-1.png" width="50">  |  lux | 101019
| <img src="https://github.com/Banuba/ve-sdk-ios-integration-sample/blob/main/Example/Example/Assets.xcassets/Filters%20Preview/101020_preview.imageset/japan-1.png" width="50">  |  japan | 101020
|  <img src="https://github.com/Banuba/ve-sdk-ios-integration-sample/blob/main/Example/Example/Assets.xcassets/Filters%20Preview/101021_preview.imageset/sunset-1.png" width="50"> |  sunset | 101021
|  <img src="https://github.com/Banuba/ve-sdk-ios-integration-sample/blob/main/Example/Example/Assets.xcassets/Filters%20Preview/101022_preview.imageset/neon-1.png" width="50"> |  neon | 101022
|  <img src="https://github.com/Banuba/ve-sdk-ios-integration-sample/blob/main/Example/Example/Assets.xcassets/Filters%20Preview/101023_preview.imageset/chile-1.png" width="50"> |  chile | 101023
|  <img src="https://github.com/Banuba/ve-sdk-ios-integration-sample/blob/main/Example/Example/Assets.xcassets/Filters%20Preview/101024_preview.imageset/norway-1.png" width="50"> |  norway | 101024

If you want to change the name, you need to specify the new name in the string resources:
```swift
/* glitch filter name */
"com.banuba.filter.name.lux" = "lux";
/* remy filter name */
"com.banuba.filter.name.remy" = "remy";
/* hyla filter name */
"com.banuba.filter.name.hyla" = "hyla";
/* neon filter name */
"com.banuba.filter.name.neon" = "neon";
/* retro filter name */
"com.banuba.filter.name.retro" = "retro";
/* sunny filter name */
"com.banuba.filter.name.sunny" = "sunny";
/* egypt filter name */
"com.banuba.filter.name.egypt" = "egypt";
/* spark filter name */
"com.banuba.filter.name.spark" = "spark";
/* byers filter name */
"com.banuba.filter.name.byers" = "byers";
/* lilac filter name */
"com.banuba.filter.name.lilac" = "lilac";
/* vinyl filter name */
"com.banuba.filter.name.vinyl" = "vinyl";
/* japan filter name */
"com.banuba.filter.name.japan" = "japan";
/* chile filter name */
"com.banuba.filter.name.chile" = "chile";
/* glitch filter name */
"com.banuba.filter.name.glitch" = "glitch";
/* grunge filter name */
"com.banuba.filter.name.grunge" = "grunge";
/* canada filter name */
"com.banuba.filter.name.canada" = "canada";
/* chroma filter name */
"com.banuba.filter.name.chroma" = "chroma";
/* norway filter name */
"com.banuba.filter.name.norway" = "norway";
/* korben filter name */
"com.banuba.filter.name.korben" = "korben";
/* sunset filter name */
"com.banuba.filter.name.sunset" = "sunset";
/* instant filter name */
"com.banuba.filter.name.instant" = "instant";
/* england filter name */
"com.banuba.filter.name.england" = "england";
/* pinkvine filter name */
"com.banuba.filter.name.pinkvine" = "pinkvine";
/* bubblegum filter name */
"com.banuba.filter.name.bubblegum" = "bubblegum";
/* new zeland filter name */
"com.banuba.filter.name.new_zeland" = "new_zeland";
```

### 6. I want to enabled slideshow animation 

To be able to turn off slideshow animation use following property of RecorderConfiguration and CombinedGalleryConfiguration entities.

```
let videoEditorConfig = VideoEditorConfig()

videoEditorConfig.recorderConfiguration.shouldUseImageEffect = true
videoEditorConfig.combinedGalleryConfiguration.shouldUseImageEffect = true

```
### 7. I want to change cursor color

All you need is just to set your color into **cursorColor: UIColor** parameter in MainOverlayViewControllerConfig entity.

Default flag is false.

### 8. I want to change progress bar position

Progress bar position contains two types of layout:
- top
- bottom (**by default**)

To change progress bar position you need to modify **progressBarPosition** property of **RecorderConfiguration** entity.

```

let videoEditorConfig = VideoEditorConfig()
videoEditorConfig.recorderConfiguration.progressBarPosition = .top

```

### 9. How does video editor work when token expires?

[Token](https://github.com/Banuba/ve-sdk-android-integration-sample#token) provided by sales managers has an expiration term to protect Video Editor SDK from malicious access. When the token expires the following happens:
 - video resolution will be lowered to 360p on camera, after trimmer and after export
 - Banuba watermark is applied to every exported video

 Also [FaceAR SDK](https://docs.banuba.com/face-ar-sdk/overview/token_management) you may expect the following actions if the token expires:
 - on the first expired month a watermark with "Powered by Banuba" label will be added on the top of both recorded and exported videos
 - after the first month the camera screen will be blurred and a full-screen watermark will be displayed

 Please keep your licence up to date to avoid unwanted behavior.
 
 ### 10. Which buttons available if Face AR disabled?
 
 AdditionalEffectsButtons contains options set which describes buttons' identifiers. 
 
 Without Face AR you could use buttons with following identifiers. 
 
 **Camera Screen**:
 - sound
 - toggle
 - flashlight
 - timer
 - speed
 - muteSound
 
 **Postprocessing Screen**:
 - sticker
 - sound
 - text
 - effects
 - time
 - color

 ### 11. I want to change screens' layout.
 
 There are two screens which could be modified with additional layout:
 
 - Camera
 - Postprocessing

To be able to change layout you need to set **useHorizontalVersion** equals true. This properties are parts of RecorderConfiguration and EditorConfiguration entities.

### 12. I want to change music button position.

The music button consists of three positions:

  - bottom
  - center
  - top

<img src="screenshots/bottom.PNG" width="150" /> <img src="screenshots/center.PNG" width="150" /> <img src="screenshots/top.PNG" width="150" />


To be able to change the location of the button, you need to set the desired value in the array with additionalEffectsButtons, for the button with the identifier **.sound**, set up the [position](/Example/Example/Extension/RecorderConfiguration.swift#L72) property. 

### 13. How can I get a track name of the audio used in my video after export?
```swift
/// Video Editor main entity and entry point.
/// Can present and hide root view controller.
/// Has default export method.
public class BanubaVideoEditor {
/// Simple metadata of music composition settings
public var musicMetadata: MusicEditorMetadata? { get }
...
}
```
`MusicEditorMetadata` contains the array of `MusicEditorTrack` which contains the following fields: 

```swift
// MARK: - MusicEditorTrack
public struct MusicEditorTrack: Codable {
///Track URL
public var url: URL
///Track original URL
public var originalURL: URL
///Track title
public var title: String
///Track id
public var id: Int32
...
}
```
or if you want to know what track was played on the camera screen you can use:

```swift
/// Video Editor main entity and entry point.
/// Can present and hide root view controller.
/// Has default export method.
public class BanubaVideoEditor {
/// Music track which will be played on camera recording
public var musicTrack: MediaTrack? { get }
...
}
```
### 14 I want to change the font

You can change the font for the whole video editor by calling in `VideoEditorConfig` this method:
 
 ```swift
  func applyFont(_ font: UIFont)
```
or change for each screen separately by calling the appropriate methods:
```swift
  func updateAlertFonts(_ font: UIFont)
  
  func updateRecorderFonts(_ font: UIFont)
  
  func updateMultiTrimFonts(_ font: UIFont)
  
  func updateEditorFonts(_ font: UIFont)
  
  func updateToastFonts(_ font: UIFont)
  
  func updateFullScreenActivityFonts(_ font: UIFont)
  
  func updateAlbumsFonts(_ font: UIFont)
  
  func updateTextEditorFonts(_ font: UIFont)
  
  func updateSlideShowFonts(_ font: UIFont)
  
  func updateTrimGalleryVideoFonts(_ font: UIFont)
  
  func updateFilterFonts(_ font: UIFont)
  
  func updateVideoCoverSelectionFonts(_ font: UIFont)
  
  func updateExtendedVideoCoverSelectionFonts(_ font: UIFont)
```

Changing the font does not affect its size. The font size will be taken by default or specified by you in the entity configuration.

### 15. I want to check whether my token is expired.

Starting from '1.0.18' version it is available to check if token is expired.

```swift
  /// Check whether token is expired
  /// - Parameters:
  ///   - token: your token that you want to verify.
  public static func isTokenExpired(
    token: String
  ) -> Bool 
```
You need to import `BanubaLicenseServicingSDK`.

Then call the static method `isTokenExpired(token: String)` on the `License` entity. 

For example:
```swift
    let token: String = "Put token"
    let result: Bool = License.isTokenExpired(token: token)
```

if you are using `BanubaTokenStorageSDK` here is a usage example:

```swift
self.loadToken { token in
     let result = License.isTokenExpired(token: token)
}
```

### 16. The file “luts” couldn’t be opened because there is no such file.

This error occurs because your application bundle doesn't contains required luts folder.

You need to copy [luts](https://github.com/Banuba/ve-sdk-ios-integration-sample/tree/main/Example/Example/luts) folder to your project.

### 17. I want to add audio filters 

Filters availability depends on the token. However, in order for them to be available, you need to add an implementation of the VoiceFilterProvider entity.

Example how to inherit VoiceFilterProvider to your own entity:

```swift
import BanubaMusicEditorSDK
import UIKit

/// Example voice filter provider
struct ExampleVoiceFilterProvider: VoiceFilterProvider {
  private let filters: [VoiceFilter]
  
  // MARK: - VoiceFilterProvider
  
  func provideFilters() -> [VoiceFilter] {
    return filters
  }
  
  init() {
    filters = [
      VoiceFilter(
        type: .elf,
        title: NSLocalizedString("com.banuba.musicEditor.elf", comment: "Elf filter title"),
        image: UIImage(named:"elf")
      ),
      VoiceFilter(
        type: .baritone,
        title: NSLocalizedString("com.banuba.musicEditor.baritone", comment: "Baritone filter title"),
        image: UIImage(named:"baritone")
      ),
      VoiceFilter(
        type: .echo,
        title: NSLocalizedString("com.banuba.musicEditor.echo", comment: "Echo filter title"),
        image: UIImage(named:"echo")
      ),
      VoiceFilter(
        type: .giant,
        title: NSLocalizedString("com.banuba.musicEditor.giant", comment: "Giant filter title"),
        image: UIImage(named:"giant")
      ),
      VoiceFilter(
        type: .robot,
        title: NSLocalizedString("com.banuba.musicEditor.robot", comment: "Robot filter title"),
        image: UIImage(named:"robot")
      ),
      VoiceFilter(
        type: .squirrel,
        title: NSLocalizedString("com.banuba.musicEditor.squirrel", comment: "Squirrel filter title"),
        image: UIImage(named:"squirrel")
      )
    ]
  }
}
```

Then the instance of the ExampleVoiceFilterProvider needs to be passed to the configuration.

```swift
  var config = VideoEditorConfig()
  config.musicEditorConfiguration.audioTrackLineEditControllerConfig.voiceFilterProvider = ExampleVoiceFilterProvider()
```

### 18. I want to change icons and name for effects.

The name of the icon for the effect must match the identifier of the effect.
Below is a table with the name, ID and icon of the default effect.

| Default image effect | Name      | ID      |
| ---------- | ---------  | ----------- |
| <img src="https://github.com/Banuba/ve-sdk-ios-integration-sample/blob/main/Example/Example/Assets.xcassets/Effects%20Preview/104000_preview.imageset/btn_slowmotion2.png" width="50"> | 0.5x | 104000
| <img src="https://github.com/Banuba/ve-sdk-ios-integration-sample/blob/main/Example/Example/Assets.xcassets/Effects%20Preview/104001_preview.imageset/btn_rapid2.png" width="50"> | 2x | 104001
| <img src="https://github.com/Banuba/ve-sdk-ios-integration-sample/blob/main/Example/Example/Assets.xcassets/Effects%20Preview/102004_preview.imageset/Acid-whip.png" width="50"> | Acid whip | 102004
| <img src="https://github.com/Banuba/ve-sdk-ios-integration-sample/blob/main/Example/Example/Assets.xcassets/Effects%20Preview/102007_preview.imageset/cathode.png" width="50"> | Cathode | 102007
| <img src="https://github.com/Banuba/ve-sdk-ios-integration-sample/blob/main/Example/Example/Assets.xcassets/Effects%20Preview/102016_preview.imageset/ic_dslrkaleidoscope.png" width="50"> | DSLR Kaleidoscope | 102016
| <img src="https://github.com/Banuba/ve-sdk-ios-integration-sample/blob/main/Example/Example/Assets.xcassets/Effects%20Preview/102018_preview.imageset/ic_dv_cam.png" width="50"> | DV Cam | 102018
| <img src="https://github.com/Banuba/ve-sdk-ios-integration-sample/blob/main/Example/Example/Assets.xcassets/Effects%20Preview/102000_preview.imageset/btn_flash2.png" width="50"> | Flash | 102000
| <img src="https://github.com/Banuba/ve-sdk-ios-integration-sample/blob/main/Example/Example/Assets.xcassets/Effects%20Preview/102003_preview.imageset/glitch.png" width="50"> | Glitch | 102003
| <img src="https://github.com/Banuba/ve-sdk-ios-integration-sample/blob/main/Example/Example/Assets.xcassets/Effects%20Preview/102010_preview.imageset/ic_glitch2.png" width="50"> | Glitch 2 | 102010
| <img src="https://github.com/Banuba/ve-sdk-ios-integration-sample/blob/main/Example/Example/Assets.xcassets/Effects%20Preview/102011_preview.imageset/ic_glitch3.png" width="50"> | Glitch 3 | 102011
| <img src="https://github.com/Banuba/ve-sdk-ios-integration-sample/blob/main/Example/Example/Assets.xcassets/Effects%20Preview/102021_preview.imageset/ic_heatmap.png" width="50"> | Heat Map | 102021
| <img src="https://github.com/Banuba/ve-sdk-ios-integration-sample/blob/main/Example/Example/Assets.xcassets/Effects%20Preview/102015_preview.imageset/ic_Kaleidoscope.png" width="50"> | Kaleidoscope | 102015
| <img src="https://github.com/Banuba/ve-sdk-ios-integration-sample/blob/main/Example/Example/Assets.xcassets/Effects%20Preview/102017_preview.imageset/ic_lumiere.png" width="50"> | Lumiere | 102017
| <img src="https://github.com/Banuba/ve-sdk-ios-integration-sample/blob/main/Example/Example/Assets.xcassets/Effects%20Preview/102023_preview.imageset/ic_pixeldynamics.png" width="50"> | Pixel Dynamic | 102023 
| <img src="https://github.com/Banuba/ve-sdk-ios-integration-sample/blob/main/Example/Example/Assets.xcassets/Effects%20Preview/102022_preview.imageset/ic_pixelstatics.png" width="50"> | Pixel Static | 102022
| <img src="https://github.com/Banuba/ve-sdk-ios-integration-sample/blob/main/Example/Example/Assets.xcassets/Effects%20Preview/102009_preview.imageset/polaroid.png" width="50"> | Polaroid | 102009
| <img src="https://github.com/Banuba/ve-sdk-ios-integration-sample/blob/main/Example/Example/Assets.xcassets/Effects%20Preview/102008_preview.imageset/btn_rave2.png" width="50"> | Rave | 102008
| <img src="https://github.com/Banuba/ve-sdk-ios-integration-sample/blob/main/Example/Example/Assets.xcassets/Effects%20Preview/102006_preview.imageset/btn_soul2.png" width="50"> | Soul | 102006
| <img src="https://github.com/Banuba/ve-sdk-ios-integration-sample/blob/main/Example/Example/Assets.xcassets/Effects%20Preview/102020_preview.imageset/ic_stars.png" width="50"> | Stars | 102020
| <img src="https://github.com/Banuba/ve-sdk-ios-integration-sample/blob/main/Example/Example/Assets.xcassets/Effects%20Preview/102002_preview.imageset/btn_foamtv2.png" width="50"> | TV-Foam | 102002
| <img src="https://github.com/Banuba/ve-sdk-ios-integration-sample/blob/main/Example/Example/Assets.xcassets/Effects%20Preview/102012_preview.imageset/ic_transition1.png" width="50"> | Transition | 102012
| <img src="https://github.com/Banuba/ve-sdk-ios-integration-sample/blob/main/Example/Example/Assets.xcassets/Effects%20Preview/102013_preview.imageset/ic_transition4.png" width="50"> | Transition 4 | 102013
| <img src="https://github.com/Banuba/ve-sdk-ios-integration-sample/blob/main/Example/Example/Assets.xcassets/Effects%20Preview/102001_preview.imageset/btn_vhs2.png" width="50"> | VHS | 102001
| <img src="https://github.com/Banuba/ve-sdk-ios-integration-sample/blob/main/Example/Example/Assets.xcassets/Effects%20Preview/102019_preview.imageset/ic_vhs2.png" width="50"> | VHS 2 | 102019
| <img src="https://github.com/Banuba/ve-sdk-ios-integration-sample/blob/main/Example/Example/Assets.xcassets/Effects%20Preview/102005_preview.imageset/zoom.png" width="50"> | Zoom | 102005
| <img src="https://github.com/Banuba/ve-sdk-ios-integration-sample/blob/main/Example/Example/Assets.xcassets/Effects%20Preview/102014_preview.imageset/ic_zoom2.png" width="50"> | Zoom 2 | 102014
| <img src="https://github.com/Banuba/ve-sdk-ios-integration-sample/blob/main/Example/Example/Assets.xcassets/Effects%20Preview/102013_preview.imageset/ic_transition4.png" width="50"> | Transition 4 | 102013

In order to change the name of the effect, you need to do it in the [localization file](https://github.com/Banuba/ve-sdk-ios-integration-sample/blob/main/Example/Example/en.lproj/Localizable.strings#L254).
