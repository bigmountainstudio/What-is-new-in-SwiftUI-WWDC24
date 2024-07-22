# What's New In SwiftUI for iOS Cheat Sheet - WWDC24
![what's new in SwiftUI for WWDC 2024](https://github.com/bigmountainstudio/What-is-new-in-SwiftUI-WWDC23/assets/24855856/5fad9a39-a33e-40a2-9e4f-3ed4867424d6)

By [Big Mountain Studio](https://www.bigmountainstudio.com/)

A list of everything new in SwiftUI after WWDC 2024.

# SwiftUI Essentials - Architecting and Scalable & Maintainable Apps
## @State, @StateObject
* [@Previewable](https://developer.apple.com/documentation/swiftui/previewable()) Use in front of `@State` and `@StateObject` in previews.

# SwiftUI Views
## CONTROL VIEWS
* [Form - ForEach(subviewOf:)](https://developer.apple.com/documentation/swiftui/foreach/init(subviews:content:))
* [ScrollView(limitBehavior: .alwaysByFew)](https://developer.apple.com/documentation/swiftui/viewalignedscrolltargetbehavior) Limits number of views scrolled to (one or few). Note: It's the parameter that is new.
* [ScrollView.onScrollGeometryChange(for:)](https://developer.apple.com/documentation/swiftui/view/onscrollgeometrychange(for:of:action:)/) React to scrolling changes.
* [ScrollView.onScrollPhaseChange](https://developer.apple.com/documentation/SwiftUI/View/onScrollPhaseChange(_:)-7mica) React to changes in the scroll phase (animating, decelerating, idle, interacting, tracking)
* [ScrollView.onScrollVisibilityChange(threshold:)](https://developer.apple.com/documentation/SwiftUI/View/onScrollVisibilityChange(threshold:_:)) Trigger action when scrolled past a point.
* [ScrollView.scrollPosition(:anchor:](https://developer.apple.com/documentation/SwiftUI/View/scrollPosition(_:anchor:)) Scroll to a view, offset, or edge.
* [Tab](https://developer.apple.com/documentation/swiftui/tab) The `tabItem` modifier is deprecated. Use this new `Tab` view to construct tabs.
* [Text(format:](https://developer.apple.com/documentation/foundation/formatstyle) New format styles functions (offset, reference, stopwatch, timer counting down & up)
## IMAGE (Might consider extracting Symbols into its own section)
* [ReplaceSymbolEffect](https://developer.apple.com/documentation/symbols/replacesymboleffect) New magic replace function
* [SymbolEffect](https://developer.apple.com/documentation/symbols/symboleffect/) (New effects: .breath, .rotate, .wiggle)
## OTHER VIEWS
* [Color .mix(with:)](https://developer.apple.com/documentation/swiftui/color/mix(with:by:in:)/) Create a new color by mixing with another.
## PAINTS
* [MeshGradient](https://developer.apple.com/documentation/swiftui/meshgradient/) A two-dimensional gradient defined by a 2D grid of positioned colors.

# SwiftUI Animations
## Navigation Animations (New chapter)
* [navigationTransition](https://developer.apple.com/documentation/swiftui/view/navigationtransition(_:)) - Like adding matchedGeometryEffect to views.

# SwiftData
## Mock Data
* [PreviewModifier + PreviewTrait](https://developer.apple.com/documentation/swiftui/previewmodifier) Allows set up of mock data that is accessible through the `#Preview(trait:)` parameter.
* [@Previewable](https://developer.apple.com/documentation/swiftui/previewable()) Use in front of `@Query` in previews.
## @Query
* [`#Expression`](https://developer.apple.com/documentation/foundation/predicate/4162327-expression) Create code that can be evaluated within the `#Predicate` macro.
## Model Options
* [@Attribute(.preserveValueOnDeletion](https://developer.apple.com/documentation/swiftdata/schema/attribute/option/preservevalueondeletion) Persists values in the persistent history when the model is deleted.
* [`#Index`](https://developer.apple.com/documentation/swiftdata/index(_:)-74ia2/) Specify properties in a model that are filtered on a lot to improve performance.
* [`#Unique`](https://developer.apple.com/documentation/SwiftData/Unique(_:)) Defines a model property as a unique constraint.

# Swift Testing
[New framework released](https://developer.apple.com/xcode/swift-testing/)

# Advanced SwiftUI
## Controls
* [ControlWidgetButton](https://developer.apple.com/documentation/widgetkit/controlwidgetbutton/) A control template representing a button.

  
(📕 = Added to books)

# Helpful References
* [SwiftUI Updates](https://developer.apple.com/documentation/updates/swiftui)
* [SwiftUI Deprecations](https://developer.apple.com/documentation/swiftui/view/autocapitalization(_:))

![color logo book](https://github.com/bigmountainstudio/What-is-new-in-SwiftUI-WWDC23/assets/24855856/4509ce75-14ee-43e7-a62d-c46d7200ddda)
