# ScratchOff



**ScratchOff** is an iOS framework that allows developers to create a scratching effect over content. It enables users to "scratch" off a top layer to reveal hidden images underneath. ScratchOff offers two approaches for the scratching effect: one with two images (a cover image and a hidden image) and another with a single image and an overlay color. This gives developers flexibility to adapt the effect to different use cases, such as promotional content or interactive games.

The framework includes callback handlers like `percentageScratched`, `didStartScratching`, and `didStopScratching`, which give developers control over the interaction. The `percentageScratched` callback tracks the progress of the scratch, allowing developers to update UI elements or trigger actions based on how much has been revealed. The `didStartScratching` and `didStopScratching` callbacks notify when the user starts and finishes scratching, enabling the integration of animations or state changes.

`ScratchOff` can be used for scratch card features, promotional campaigns, gamified interactions, or any feature that involves revealing content by scratching. It offers flexibility and ease of use for creating interactive experiences in iOS apps.

## Features

- **Two Scratching Approaches:**
    - Use two images: a cover image and a hidden image.
    - Use a single image with an overlay color.
- **Callback Handlers:**
    - `percentageScratched`: Tracks the progress of the scratch.
    - `didStartScratching`: Notifies when the user starts scratching.
    - `didStopScratching`: Notifies when the user finishes scratching.
- **Customizable Trail Effect:**
    - Enable or disable the trail effect.
    - Customize the trail color
- **Additional Scratching Options:**
    - Enable or disable live coverage percentages.
    - Prioritize scratching gestures
- **Flexible Use Cases:**
    - Scratch card features.
    - Promotional campaigns.
    - Gamified interactions.

## Installation

**TBA**

## Usage

To use the scratchable view, first import the `ScratchOff` package into your project.

```swift
import ScratchOff
```

Create a scratchable view using the `ScratchableView` class from the package.

```swift
private(set) lazy var scratchableView: ScratchableView = {
    let scratchableView = ScratchableView(overlayImage: coverImage, hiddenImage: hiddenImage)
    return scratchableView
}()
```

Scratching Effect Implementation Approaches
The `ScratchableView` class provides two initialization approaches for creating a scratching effect, each with a different way of overlaying the visible image on top of the hidden image.

### 1. Using Two Images
In this approach, the `overlayImage` acts as the top layer, covering the `hiddenImage`. When the user interacts with the view, the top image is scratched away, revealing the hidden image beneath.

```swift
let scratchableView = ScratchableView(overlayImage: coverImage, hiddenImage: hiddenImage)
```

* `hiddenImage`: The image underneath.
* `overlayImage`: The image that serves as the top layer for scratching.
* `showTrail`: (Optional) Configures the trail effect.
* `trailColor`: (Optional) Customizes the trail color.
* `updatePercentagesDuringScratching`: A boolean flag that determines whether the scratch completion percentage should be updated in real-time while scratching (default is false).
* `prioritizeScratchingGestures`: A boolean flag that, when enabled, gives priority to scratching gestures. This is particularly useful when ScratchableView is embedded within UIScrollView instances.
> [!CAUTION]
> For better performance, it is recommended to keep `updatePercentagesDuringScratching` set to `false`, as enabling it may impact rendering speed

### 2. Using a Single Image with an Overlay Color
Instead of using a separate image as the overlay, this approach allows you to create the overlay using a solid color. The `overlayColor` parameter defines the color of the overlay, and the `hiddenImage` remains the same.

```swift
let scratchableView = ScratchableView(hiddenImage: hiddenImage, overlayColor: .blue)
```
* `hiddenImage`: The image underneath.
* `overlayColor`: The color of the overlay.
* `showTrail`: (Optional) Configures the trail effect.
* `trailColor`: (Optional) Customizes the trail color.
* `updatePercentagesDuringScratching`: A boolean flag that determines whether the scratch completion percentage should be updated in real-time while scratching (default is false).
* `prioritizeScratchingGestures`: A boolean flag that, when enabled, gives priority to scratching gestures. This is particularly useful when ScratchableView is embedded within UIScrollView instances.
> [!CAUTION]
> For better performance, it is recommended to keep `updatePercentagesDuringScratching` set to `false`, as enabling it may impact rendering speed
## User Interaction Callbacks for Scratching Effect

### var percentageScratched: ((CGFloat) -> ())?

Description: A callback that **provides the percentage of the image that has been scratched away**. The callback is called whenever the scratching action progresses, passing a `CGFloat` value between 0 and 100, representing the percentage of the overlay image that has been scratched. This can be used to update UI elements like progress bars or to trigger other actions based on the scratching progress.

> [!WARNING]
> Note that this percentage has a ±0.5 error margin.

Example Usage
```swift
scratchingView.percentageScratched = { [weak self] percentage in
    self?.progressBar.setProgress(Float(percentage / 100), animated: true)
}
```

### var didStartScratching: (() -> ())?

Description: A callback that is triggered when the user starts scratching the overlay image. This can be used to perform any actions that should occur when the scratching interaction begins, such as initiating animations, changing states, or tracking the start of the user interaction.

Example Usage
```swift
scratchingView.didStartScratching = {
    print("Scratching started!")
}
```

### var didStopScratching: (() -> ())?

Description: A callback that is called when the user stops scratching the image. This is useful for handling events that should occur when the user finishes the scratching interaction, such as finalizing the progress, displaying a message, or stopping any related animations.

Example Usage
```swift
scratchingView.didStopScratching = {
    print("Scratching stopped!")
}
```


## Features  

### Changing the Scratch Point Size  

To adjust the size of the points used during the scratching interaction, use the `changePointSize(to size: Int)` method:  

```swift
changePointSize(to: 20)  // Sets the scratch point size to 20
```

### Updating the Trail Configuration  

The `updateTrailConfig(_ showTrail: Bool, _ selectedColor: UIColor)` method allows enabling or disabling the trail effect and setting its color:

```swift
updateTrailConfig(true, .red)  // Enables the trail effect with a red color
```

### Resetting the Scratching Progress

To restore the scratchable area to its initial, unscratched state, call `resetScratchingProgress()`:

```swift
resetScratchingProgress()
```

### Revealing the Hidden Content

If you want to instantly reveal the entire hidden image, use the `showScratched()` method:
```swift
showScratched()
```