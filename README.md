# What's New In SwiftUI for iOS Cheat Sheet - WWDC23
![what's new in SwiftUI for wwdc 2023](https://github.com/bigmountainstudio/What-is-new-in-SwiftUI-WWDC23/assets/24855856/5fad9a39-a33e-40a2-9e4f-3ed4867424d6)

By [Big Mountain Studio](https://www.bigmountainstudio.com/)

A list of everything new in SwiftUI after WWDC 2023.
# SwiftData (new book)
[SwiftData](https://developer.apple.com/documentation/SwiftData)

# Working with Data
* [Observation](https://developer.apple.com/documentation/Observation)
* [@Environment](https://developer.apple.com/documentation/swiftui/environment/init(_:)-7pint) - New init to replace @EnvironmentObject.
* [@Observable](https://developer.apple.com/documentation/observation/observable-swift.macro) - Replaces ObservableObject.
* [@Bindable](https://developer.apple.com/documentation/swiftui/bindable) - Replaces @ObservedObject.
* [onChange(of:)](https://developer.apple.com/documentation/SwiftUI/View/onChange(of:initial:_:)-4psgg) - Replaces previous onChange(of: perform:).
* [@StateObject](https://developer.apple.com/documentation/swiftui/stateobject) can now be replaced with the new @State for ObservableObject classes migrating to @Observable.

# SwiftUI Views
[SwiftUI Updates Reference](https://developer.apple.com/documentation/Updates/SwiftUI)
* [autocapitalization](https://developer.apple.com/documentation/swiftui/view/autocapitalization(_:)) - deprecated. Use [textInputAutocapitalization](https://developer.apple.com/documentation/swiftui/view/textinputautocapitalization(_:)).
* [ContentUnavailableView](https://developer.apple.com/documentation/SwiftUI/ContentUnavailableView) - A view to display when the content of your app is unavailable to users.
* [foregroundColor](https://developer.apple.com/documentation/swiftui/view/foregroundcolor(_:)) - deprecated. Use [foregroundStyle](https://developer.apple.com/documentation/swiftui/view/foregroundstyle(_:))
* [navigationDestination](https://developer.apple.com/documentation/SwiftUI/View/navigationDestination(item:destination:)) - Associates a destination view with a bound value.
* [toolbarTitleDisplayMode](https://developer.apple.com/documentation/SwiftUI/View/toolbarTitleDisplayMode(_:)) - Configures the toolbar title display mode for this view.
* [UnevenRoundedRectangle](https://developer.apple.com/documentation/swiftui/unevenroundedrectangle) - Create rectangles with individual rounded corners.

## Charts
* [SectorMark](https://developer.apple.com/documentation/charts/sectormark) - Used to make pie and donut charts.  

## List
* [alternatingRowBackgrounds](https://developer.apple.com/documentation/swiftui/view/alternatingrowbackgrounds(_:)) - Specifies alternating row backgrounds on list or table. Can be .enabled, .disabled, or .automatic.
* [backgroundProminence](https://developer.apple.com/documentation/swiftui/backgroundprominence) - Make rows stand out more. Can be .standard or .increased.
* [badgeProminence](https://developer.apple.com/documentation/swiftui/badgeprominence) - The badge is the value at the trailing side of the row. It can be .increased, .decreased, or .standard. 
* [listRowSpacing](https://developer.apple.com/documentation/SwiftUI/View/listRowSpacing(_:)) - Spacing between each row. (iOS 15)
* [selectionDisabled](https://developer.apple.com/documentation/SwiftUI/View/selectionDisabled(_:)) - Disable selection of individual rows.
### Sections
* [listSectionSpacing](https://developer.apple.com/documentation/swiftui/view/listsectionspacing(_:)-a2sn) - Spacing between sections (can be value or enum like .default or .compact).
* [Section(isExpanded:)](https://developer.apple.com/documentation/swiftui/section/init(isexpanded:content:header:)-561d7) - Control expandability of section with bound property.
### Search
* [searchable(text:editableTokens:placement:prompt:token:)](https://developer.apple.com/documentation/swiftui/view/searchable(text:editabletokens:placement:prompt:token:)-41gcr) - New search initializer.

## NavigationStack
* [navigationBarTitle](https://developer.apple.com/documentation/swiftui/view/navigationbartitle(_:)-6p1k7) - deprecated. Use [navigationTitle](https://developer.apple.com/documentation/swiftui/view/navigationtitle(_:)-5di1u).

## Toolbar
* [bottomBar](https://developer.apple.com/documentation/swiftui/toolbaritemplacement/bottombar?changes=latest_minor) - Puts view in the bottom bar.
* [topBarLeading](https://developer.apple.com/documentation/swiftui/toolbaritemplacement/topbarleading?changes=latest_minor) - Use this instead of navigationBarLeading (deprecated).
* [topBarTrailing](https://developer.apple.com/documentation/swiftui/toolbaritemplacement/topbartrailing?changes=latest_minor) - Use this instead of navigationBarTrailing (deprecated).
* [toolbarTitleDisplayMode](https://developer.apple.com/documentation/swiftui/view/toolbartitledisplaymode(_:)?changes=latest_minor) - Can be .automatic, .inline, .inlineLarge, .large. Note: This also works on NavigationStacks in place of navigationBarTitleDisplayMode (navigationBarTitleDisplayMode is not deprecated though so you can continue to use it).

# SwiftUI Animations
* [WithAnimation Completions](https://developer.apple.com/documentation/SwiftUI/withAnimation(_:completionCriteria:_:completion:)) - Perform action when animation completes.
* [PhaseAnimator](https://developer.apple.com/documentation/swiftui/view/phaseanimator(_:content:animation:)) - Animations per steps (phases) you provide.
* [SF Symbol Animations](https://developer.apple.com/documentation/symbols) - 4 sections for the different types of symbol animations.
* [CustomAnimation](https://developer.apple.com/documentation/SwiftUI/CustomAnimation) - How an animatable value should change over time.
* [UnitCurve](https://developer.apple.com/documentation/SwiftUI/UnitCurve) - Another way to define a custom animation curve.
* [spring](https://developer.apple.com/documentation/SwiftUI/Animation/spring(duration:bounce:blendDuration:)) - Another spring animation that works across all platforms.
* [Spring](https://developer.apple.com/documentation/SwiftUI/Spring) - Store your spring animation settings in an object so you can reuse the same spring animation.

![Gold logo](https://user-images.githubusercontent.com/24855856/173091471-81c1c475-fc64-4cb0-8149-343719a1cb12.png)
