# Slack swiftui channel

@Cristian-Mihai
 asked:
With the new SwiftUI @Observable macro, are there any cases where ObservableObject would still be a better alternative?
9 replies

WWDC Bot
APP  
@Curt C (Apple) answered:
Use ObservableObject when you need to back deploy to before iOS 17.
:raised_hands:
:+1:

Cristian-Mihai
Thanks!


Evan
So we should just focus on @State and @Environment going forward, correct? (edited) 


Cristian-Mihai
  
What happens if an attribute is modified, but indirectly? For example, in the case of using get & set. Would the view still update automatically? (edited) 


Matthew
  
...and @Environment, to handle what iOS 16 and prior uses both @Environment and @EnvironmentObject for?


francisco
  
i love the new @Observable macro
:raised_hands:
1
:heavy_plus_sign:
2



Curt C (Apple)
  
SwiftUI registers dependencies is a value is read during the evaluation of body. Indirect modifications should invalidate the dependencies.

----

@Marin
 asked:
Where can I get that document editor the video kept showing? The one where he was editing the dog tags. Is that built in, or is that custom?
2 replies

WWDC Bot
APP  
@Curt C (Apple)
 answered:
You can get sample code using the Code Copy feature in the developer app. The DocumentGroup scene type is the thing to use to create your own document-based app.

----

@Jakub
 asked:
I love the static lookup for colors, but does it also work for custom fonts?

5 replies

WWDC Bot
APP  
@Jeff R (Apple)
 answered:
Hi - thanks for the question! The new static member syntax should work for both colors and images that are defined in your asset catalog.

----

@Matthew
 asked:
For the containerRelativeFrame, does that work cleanly in a List as well as a ScrollView?
2 replies

WWDC Bot
APP  
@Harry L (Apple)
 answered:
Yes, a List establishes a container relative frame like ScrollView. You can see the docs for this modifier for more examples of what other views establish a container relative frame:
https://developer.apple.com/documentation/swiftui/view/containerrelativeframe(_:alignment:)

----

@Marius
 asked:
If I created an Entity in CoreData can I use it as a class with SwiftData? Thank you!
3 replies

WWDC Bot
APP  
@Daniel D (Apple)
 answered:
Yes! There will be a session on June 8th "Migrate to SwiftData" that covers this topic.

----

@Mateus
 asked:
The documentation says that Bindable is “a property wrapper type that supports creating bindings to the mutable properties of observable objects” but I tried and this can also be achieved using the good old Binding. Is there any advantage in using Bindable except for not requiring to reference a State using $? Can I use Binding if I prefer it? For me, it gives more clarity at the point of use that the view will binding to a property of that object
See less
:thinking_face:
1
:eyes:
2

1 reply

WWDC Bot
APP  
@Matt R (Apple)
 answered:
Hi Mateus, can you clarify how you're using Binding in this case — are you referring to creating a custom Binding using get/set closures?
Creating Bindings using @Bindable will provide a more succinct and convenient syntax in many cases, but will also be better for performance: Bindings created via get/set closures are more expensive, and we optimize this internally with @Bindable.

----

@Jonathan
 asked:
The ability to get the id of the view at the top of the ScrollView is fantastic :heart:. Is there a way to do the same for the bottom?
15 replies

WWDC Bot
APP  
@Harry L (Apple)
 answered:
There is not though I'd love a feedback with more details for your use case like whether you want to know both the top and bottom view or you want to know the bottom instead of the top.


Jonathan
  
The specific use case I'm thinking of in the app I work on, is when a button to continue to the next screen is only enabled once the user has scroll to the bottom, for example accepting terms & conditions.
:eyes:
1

Jonathan
  
(I know this isn't good UX, but the legal team insist on it)


Klayton
  
You should be able to add a Spacer to the bottom of the ScrollView and use onAppear on that.


Jonathan
  
Wouldn't onAppear trigger as soon as the view is added to the hierarchy rather than when it becomes visible?


Kevin
  
@Jonathan
 same boat, same reason. We put an invisible view at the bottom and check it's minY in the scroll view's coordinatespace is less than the height of the scroll view (which is set with a preferencekey). (edited) 


Jonathan
  
I'm doing that now, but it's not nearly as nice as the new scrollPosition(id:) API, It just needs an extra parameter to switch to the bottom :pray:

Another use case  that might be more common is loading another page of items when the user reaches the end
:+1:
4

Kevin
  
Agreed.


Harry L (Apple)
  
These seem like valid use cases. I’d love a feedback. In the meantime, I’d recommend using onAppear with your view inside of a LazyVStack.
For loading more content, checking if the view is within the last few views would suffice and probably provide a better experience to load the content before you reach the bottom.

----

@Tudor-Mihai
 asked:
The most performant way I found to do an injection of viewModels while keeping the @StateObject performance is to use this specific syntax. Is there a better way of achieving property injection into state objects?

```
class Bar {}

class FooVM: ObservableObject {
    let externalDependency: Bar

    init(externalDependency: Bar) {
        self.externalDependency = externalDependency
    }
}

struct FooView: View {
    @StateObject private var viewModel: FooVM

    init(viewModel: @autoclosure @escaping () -> FooVM) {
        self._viewModel = StateObject(wrappedValue: viewModel())
    }

    var body: some View {
        Text("Hello World")
    }

}
```

:raised_hands:
1

11 replies

WWDC Bot
APP  
@Luca B (Apple)
 answered:
The is the correct (and only) syntax available to initializer a property wrapper in the init of a type.

----

@Cristina
 asked:
Hi! Great talk and great news! When targeting iOS 17+ on Xcode 15+, will the compiler help in some way with the migration of the code? E.g. warnings telling to remove the old annotations and quick fixes
:wwdc23_apple:
1

2 replies

WWDC Bot
APP  
@Curt C (Apple)
 answered:
We try to keep existing code working, so you shouldn't need to migrate in general. Use the new API when you need the new features or when writing new code.

----

@Nrick
 asked:
Is it better to use containerBackground() for a TabView rather than background() for a iOS/iPadOS app ?
1 reply

WWDC Bot
APP  
@Harry L (Apple)
 answered:
No, the tabView placement is only available on watchOS.
https://developer.apple.com/documentation/swiftui/containerbackgroundplacement/tabview

----

@Adel
 asked:
Can I use SwiftData to cache the APIs response and make the app work offline in case the internet disconnect?
3 replies

WWDC Bot
APP  
@Luca B (Apple)
 answered:
Yes, that is perfectly reasonable usage for storing data on disk through SwiftData

----

@Kevin
 asked:
Does the new @Observable wrapper automatically apply @MainActor - we've been using MainActor on our ViewModel classes to ensure @Published vars are updated on main.
2 replies

WWDC Bot
APP  
@Matt R (Apple)
 answered:
@Observable does not require @MainActor, though it's still best practice to always update UI-related models on the main actor.
Updating @Observable model properties on non-main actors that are used by SwiftUI views will trigger SwiftUI view updates as expected, however those updates may not work correctly with animations and/or may not guarantee atomicity for a set of model updates relative to a single view update. So there are cases where background updates to @Observable models will work just fine, but for "view models" or models heavily used directly by SwiftUI views, having those aligned to the main actor is a best practice.

----

@Kiril
 asked:
In iOS app, I have a view with ScrollView and refreshable modifier. Most of the time it works fine, but sometimes, when a user performs pull to refresh, the content moves down, and doesn't come back. This can be reproduced even with the simple playground code (below): run that code in playground, pull to refresh a few times and you will observe this behavior.
Is this a bug / limitation / or incorrect usage of refreshable? How would you recommend to change the implementation to avoid this issue?
Code:
```
struct MyView: View {

  var body: some View {
      VStack {
          ScrollView {
              LazyVStack {
                  Text("Hello")
              }
              .background(Color.yellow)
              .frame(maxWidth: .infinity)
          }
          .background(Color.red)
      }
      .refreshable(action: {
          print("refreshed")
      })
  }
}

struct DisplayedView: View {
    
    var body: some View {
        MyView()
            .frame(
                width: 300,
                height: 700
            )
    }
    
}

PlaygroundPage.current.setLiveView(DisplayedView())
```
See less
:eyes:

3 replies

WWDC Bot
APP  
@Harry L (Apple)
 answered:
This seems like a bug with the refreshable modifier. Do you have a feedback?

----

@Joseph
 asked:
I was setting up a quick Document-based app, but all of the ReferenceFileDocument APIs appear to be synchronous only. Is there a path to using that with async API? In particular, I'd hoped to call "await" in the closure from a Document snapshot/save to push the data elsewhere. Am I missing something obvious there?
See less
7 replies

WWDC Bot
APP  
@Julia V (Apple)
 answered:
Serializing/deserializing APIs are called on background threads, and they do not block UI if that's what you are worried about.
Some file operations block the contents of the Document from being edited, and at that point document's `isEditable` property is equal to `false`. This is required to prevent the user from editing the document when it is being saved, for example.
:+1:
1

Joseph
  
I was more trying to use Actor and swift concurrency to protect a back-end data store that I was reading and writing from, which requires the await  to invoke methods to load/store data. That's what didn't seem to be working with the Document-based APIs, but I wasn't sure if I wasn't just missing some more obvious pattern of doing that kind of thing.


Julia V (Apple)
  
That's what didn't seem to be working
Can you describe what was not working?


Joseph
  
When I wrap the model that I'm loading from disk in Actor, and I want to initialize a new Document, or save out the document in snapshot(), those closures are all synchronous - so I can't call await getMyData() from the actor-protected model - the compiler won't allow it, since they're synchronous contexts, and establishing a free-floating Task() to get into an asynchronous context doesn't seem like a good idea in that space.


Joseph
  
In my case, my "model" is actually a static library imported that needs thread protection for general access, hence trying out the Actor setup to protect it vs. a serialized DispatchQueue on all the accessors


Julia V (Apple)
  
I see. Can you please file feedback for this?

----

@Thomas
 asked:
Is it possible to position the content of a ScrollView by screen coordinates. I know it is possible to use scrollTo with the id of a view
:lock:
1
:+1:
1

3 replies

WWDC Bot
APP  
@Harry L (Apple)
 answered:
No but I would love a feedback providing some more details on your use case.


Vincent
  
+1 would prefer if this is possible.


Doug L (Apple)
  3 days ago
@Thomas thanks for creating the feedback. I’ve directed it to the team (cc @Vincent)

----

@Fredric Michael
 asked:
Hi,
Is there any documentation that states which default keyboard shortcuts are utilized by SwiftUI components? For example when using a list sidebar on macOS, typing a letter will by default select the list item that starts with the same letter. Sometimes it's desired to deactivate this behavior.
Thanks!
Michael
See less

3 replies

WWDC Bot
APP  
@Cody B (Apple)
 answered:
Nothing specific to SwiftUI. SwiftUI tries to follow long-standing platform conventions, so for example the documentation for macOS keyboard shortcuts should be relevant: https://support.apple.com/en-us/HT201236
Beyond that, any suggestions you have would make for good feedback!
Apple SupportApple Support
Mac keyboard shortcuts
By pressing certain key combinations, you can do things that normally need a mouse, trackpad, or other input device. (221 kB)
https://support.apple.com/en-us/HT201236



Taylor K (Apple)
  
There is the new `typeSelectEquivalent(_:)` API that can be used to either customize or disable (with an empty string) the type select behavior for Lists and Tables: https://developer.apple.com/documentation/swiftui/view/typeselectequivalent(_:)-6lmlh?changes=latest_minor

----

@Ratnesh
 asked:
Let say we have 3 sections, so first section uses a single property from @Observable class, so whole body will be recomputed or that perticular section will be?
2 replies

WWDC Bot
APP  
@Luca B (Apple)
 answered:
The whole body is recomputed in this case.

----

@Cristian-Mihai
 asked:
Regarding an Observable class, what happens if an attribute is modified, but indirectly? For example, in the case of having an attribute that uses getters & setters. Would the view still update automatically?
Example:
```
class Doggo🐶 {
    var isWoofing: Bool = false
    var doggoState: Bool {
        get {
            return isWoofing
        }
        set {
            isWoofing = newValue
        }
    }
}
```
2 replies

WWDC Bot
APP  
@Daniel D (Apple)
 answered:
This is possible! Watch the session "Discover Observation in SwiftUI" to learn more :)

----

@Jason
 asked:
With the old property wrappers, @ObservedObject was used when a view did not own its state (and the ObservedObject was often injected into the view via constructor), while @StateObject was used when a view did own its state (and the StateObject could just be constructed there). How do these ownership models translate to the new @Observable macro? Is it the case that @State is used if the view owns its state, and a plain old property is used if the view doesn't?

4 replies

WWDC Bot
APP  
@Luca B (Apple)
 answered:
That's exactly right.

---

@Tomas
 asked:
Are the SwiftUI components for making watchOS apps with Dial/Infographic/List layouts and the vertically paged layout with scrollview (like watchOS 10 messages) already available in beta 1 sdk?
1 reply

WWDC Bot
APP  
@Anne H (Apple)
 answered:
There are several new features in SwiftUI to help you make these layouts in your app for watchOS 10. Check out the "Design and build apps for watchOS 10" session for more information.

----

@Alexandros
 asked:
Could you give any examples of Macros that are backwards compatible? For example, is `#Preview`?
1 reply

WWDC Bot
APP  
@Curt C (Apple)
 answered:
In general if the code generated by a macro is backwards compatible, then the macro itself is. `#Preview` is not backward compatible in seed 1.

----

@Greg
 asked:
There was a slide in the Platforms State of the Union, at timestamp 11:02, that showed a whole bunch of new SwiftUI features, and included a reference to “Custom Gestures.” I haven’t seen any mention of this in documentation, nor is it clear if any of the sessions this week will cover it. Where can I find more info? Thanks!
See less
2 replies

WWDC Bot
APP  
@Kyle M (Apple)
 answered:
I believe this was referring visionOS and the ability to compose modifiers to customize gestures, similar to SwiftUI on other platforms.

----

@Fredric Michael
 asked:
Hi,
When using a list with selection as a sidebar on macOS the highlight color seems to be the current accent color. Is there any way to override this highlight color or change the accent color locally for the list? I've noticed that many other apps use a color different to the accent color.
Thanks!
Michael
:+1:
2

29 replies

WWDC Bot
APP  
@Haotian Z (Apple)
 answered:
Hi, currently the sidebar will follow the app-wide accent color - by 'change accent color locally for the list' do you mean you want the List to have its own accent color that is different from the one in app?


Fredric Michael
  
Yes that was my idea since I haven't been able to override the color any other way. The solution I've come up with right now is to change the app wide accent color but this is not very desirable, the other solution is to not use List selection at all and instead set listRowBackground and use tap gesture. However, the tap gesture content area is smaller than the built in tappable selection area.
:+1:
1


Taylor K (Apple)
  
Do you have an example of what you want to change the color to? Or what you are seeing other apps do that you are trying to be similar to?


Fredric Michael
  
@Taylor K (Apple)
 I am basically trying to get the same behavior that you see in the Photos or Mail app. There you can see that the highlight color isn't the accent color.


Taylor K (Apple)
  
If I follow correctly, those are not changes to the tint of the sidebar color, but a change to their focus behavior — specifically that they don't become focused on click and so normally appear with their unfocused appearance (the vibrant gray).


Taylor K (Apple)
  
Notably they do show the normal accent color-based appearance if you Tab or Shift-Tab back to the sidebar from whereever focus is at.
photos-focus.gif
 
photos-focus.gif


Fredric Michael
  
@Taylor K (Apple)
 Ah yes I can see that now. In the mail app it becomes accent color when I hit tab but I have never seen that in the mail app before. So I assume it must discard the focus change when I tap on mailbox? How could I achieve this? I would prefer the sidebar row to never be focused.


Taylor K (Apple)
  
There might not currently be a way, 
@Cody B (Apple)
 could help say for sure. Seems like a good Feedback to have if not!


Fredric Michael
  
Good to know what the other apps are doing now :slightly_smiling_face: So if there is now way to do this in SwiftUI I will have to stick with my earlier hack?


Cody B (Apple)
  
Yeah, there isn't any way to configure the kind of focus behavior you see with the Mail.app sidebar. Feedback describing your use case would be great!


Fredric Michael
  
@Cody B (Apple)
 Would there be any way to do this via Introspect or other? Or is my solution using listRowBackground the best way?


Fredric Michael
  
Here is a screenshot of what my current solution looks like and what I am trying to accomplish.
Screenshot 2023-06-07 at 23.16.22.png
 
Screenshot 2023-06-07 at 23.16.22.png


Cody B (Apple)
  
I'm not sure there's a "best" way in this case—selection color is supposed to be a function of the list's focus state, so any change you make to the color without also changing focus behavior is liable to make the overall experience feel inconsistent to folks who rely on focus.


Fredric Michael
  
Understood. In my use case the focus should almost always be on the content view which showcases photos. Having a bright blue color on the left side is distracting for my use case which is why probably the Photos app also avoids focusing the folders if possible.


Fredric Michael
  
@Cody B (Apple)
 Is it possible to achieve the focus state of thumbnail like Photos app does it?


Cody B (Apple)
  
Makes sense! I wish I had a better answer for you, but I can't think of a good way to override the list's focus behavior like that. If you want to file a quick "please support customizable list focus interactions" feedback, you can put that # here and I'll see it.


Fredric Michael
  
Understood. I will file a feedback ASAP and report back. And regarding the thumbnail use case there is no way to set the focus to a specific view and then tab between these focus points?


Cody B (Apple)
  
For that the content view itself needs to be focusable also. Pressing Tab should then move focus between the list and the content view. This is how collection views tend to work on macOS: the overall container has focus, and it manages its selection state internally in response to key presses and the like.


Fredric Michael
  
Understood. Is LazyVGrid focusable?


Cody B (Apple)
  
Not automatically, but it can be made so, e.g.
```
LazyVGrid(columns: ...) {
    ...
}
.focusable()
```


Cody B (Apple)
  
If you want to suppress the focus ring that appears around the grid, you can add .focusEffectDisabled().


Fredric Michael
  
Cool. And possible to focus on grid cell view?


Fredric Michael
  
So wouldn't it be possible to always set focus on the grid when the folder state changes and thus defocusing the folder list row?


Cody B (Apple)
  
Something like that should work, it just seems like it'd be a weird experience for people who are trying to use the keyboard to navigate. E.g. with Photos, you can press the Tab key to get focus into the sidebar and then arrow around to the different items.


Fredric Michael
  
I understand what you mean. I guess I will have to wait until this is fully supported by SwiftUI. For now I will use my listRowBackground solution.


Fredric Michael
  4 days ago
@Cody B (Apple)
 Here is the feedback # FB12275125
:pray:
1

Fredric Michael
  4 days ago
I've also been playing with .focusable today. Unfortunately something seems broken on Ventura as I can't get the focus ring to appear when I apply .focusable on any view. Am I missing something?


Cody B (Apple)
  4 days ago
There are some focus-related problems when mixing SwiftUI and AppKit/UIKit views, but in general I'm not seeing the kind of problem you're describing. For instance, this results in circles with circular focus rings, whether or not keyboard navigation is enabled system wide (on older versions of macOS, focusable only applies when keyboard navigation is enabled):
```
struct ContentView: View {
    var body: some View {
        HStack {
            ForEach(0..<5) { _ in
                Circle()
                    .focusable()
            }
        }
    }
}
```
What might be different in the cases you're trying?


Cody B (Apple)
  4 days ago
Oh, just re-read—you're trying on Ventura and not seeing it have an effect. In that case you need to enable keyboard navigation in System Settings.

----

@Jason
 asked:
Follow-up question about data ownership and `@Observable`! StateObject has an init(wrappedValue:) initializer which can be carefully used for data dependency injection, with all the usual caveats about how it will not be updated if that data changes because it's an autoclosure, etc. 
Is there an analogous way to inject data for `@State` data owned by the view? I know there's an init(initialValue:) but I think we've been more strongly warned off from that in the past.
2 replies

WWDC Bot
APP  
@Daniel D (Apple)
 answered:
State gained an autoclosure version of the initializer for `@Observable` objects.

----

@Chris
 asked:
Is there a pattern for making struct members `@Observable` if the struct is provided outside your app? Eg, if I'm importing the struct from a third party library but I still want SwiftUI to reflect changes to its members?
1 reply

WWDC Bot
APP  
@Matt R (Apple)
 answered:
Hi Chris!
First, `@Observable` is primarily designed for observing properties to classes. Using a struct within your SwiftUI views should work correctly automatically. Can you clarify the kind of use case you have in mind?
For extending other types in general, there are several approaches you could take:
- You can wrap other types in your own custom `@Observable` types, which is the easiest approach.
- It's also possible to add observation support manually, by conforming to the Observable protocol directly and writing the same code that the `@Observable` macro itself emits (which you can see yourself by creating your own @Observable types and expanding them in Xcode). So you could extend existing types to conform to Observable and add new computed properties that support observation.
However, since observation is based on changes to a type's storage, it's not possible to simply make an existing type `@Observable` — some amount of wrapping functionality using the approaches above is required.

----

@Mateus
 asked:
what I mean is that the following code works just fine
```
@Observable
class Model {
    var count = 0
}

struct ContentView: View {
    
    @State var model = Model()
    
    var body: some View {
        VStack {
            Text("count = \(model.count)")
            ModelView2(model: $model) // Binding
        }
    }
    
}

struct ModelView1: View {
    
    @Bindable var model: Model
    
    var body: some View {
        Stepper(value: $model.count) {
            EmptyView()
        }
    }
    
}
```
See less
1 reply

WWDC Bot
APP  
@Matt R (Apple)
 answered:
Ah I see — yes, it's fully supported to use `@Observable` models with `@State` and create bindings from that! 
No `@Bindable` necessary in that case.
`@Bindable` is to help provide the same kinds of conveniences as @State in cases where `@State` isn't appropriate, such as referencing an object in a view that's not created/owned by that view.
There is a subtle distinction between a Binding<T> and a Bindable<T> — A Binding<T> means we have a writable reference to the object, i.e. we could swap out that object with an entirely new object. A Bindable<T> allows creating bindings to properties of an object, but you can't get a read-write binding to the reference of the object itself. For example, ModelView1 in your example can't do something like `self.model = newModel` or get a Binding to `$model` itself.

----

@Yuhao
 asked:
Hi! How does `NavigationSplitView` decides when to reload the detailView? I want to be able to persist multiple navigation path like what Apple News does. Is it possible with `NavigationSplitView`?

7 replies

WWDC Bot
APP  
@Curt C (Apple)
 answered:
A `NavigationSplitView` with a `NavigationStack` in the detail updates the detail when the selection changes in the sidebar or the path changes in the detail.
What you want is a bit tricky, because changing the selection pops the stack.


Curt C (Apple)
  
There are a couple of approaches you could try.


Curt C (Apple)
  
One is to use an onChange modifier to detect the selection change, then update the path of the detail using a DispatchQueue.main.async to delay the path change.


Curt C (Apple)
  
If you can require iOS 17, take a look at the new preferredTopColumn argument to NavigationSplitView, which let’s you control what happens when a split view is shown on iPhone.


Yuhao
  
Thanks! That’s really useful!


Yuhao
  
I understand that TabView is able to have multiple NavigationStack which works great on iPhone and can save scrolling progress, that said it doesn’t look very good especially on large screens, is it possible to achieve the similar things with a ‘sidebar’ look? (edited) 
:+1:
1

Curt C (Apple)
  
Not today. I’d love a feedback describing your use case, especially if you can share sketches of how you want the narrow and regular versions to look!

----

In response to the WWDC23 talk: "Wind your way through advanced animations in SwiftUI"

@Bardia
 asked:
Can these improvements work in combination with matchedGeometryEffect?
:eyes:
1

1 reply

WWDC Bot
APP  
@Tim D (Apple)
 answered:
Great question! The APIs in this talk are building blocks that can be combined with other features, including `matchedGeometryEffect` – and I'm sure there are creative combinations that I haven't even thought of yet. Do you have something in mind that you are planning to build?

----

@Michael
 asked:
How can I get the new appearance/materials in the Xcode 15 welcome window?
1 reply

WWDC Bot
APP  
@Taylor K (Apple)
 answered:
The welcome window uses standard material backgrounds, e.g. `.background(.thinMaterial)`

----

@Marvin
 asked:
Will ’matchedGeometryEffect’ work between sheets and navigation stacks in this release?
:eyes:
1

3 replies

WWDC Bot
APP  
@Curt C (Apple)
 answered:
matchedGeometryEffect does not work across those containers.

Marvin
  
Is this a bug or just as designed?


Curt C (Apple)
  
I might say an “unimplemented feature”. :slightly_smiling_face: That’s effectively a bug though, so sorry about that.

----

@Viennarz
 asked:
Is there a hard limit on how much we can add KeyframeTrack?
2 replies

WWDC Bot
APP  
@Tim D (Apple)
 answered:
Thanks for asking – there is not a hard limit when using KeyframeTrack (though if you include nine billion keyframes you may... not have a great time)

----

@Marvin
 asked:
Is there a way to make the hit area of a textfield bigger?
:eyes:
1

5 replies

WWDC Bot
APP  
@Harry L (Apple)
 answered:
Not explicitly, but you can programmatically focus a text field with FocusState and the focused modifier. Using that, you could create a tappable area outside of the TextFields normal tappable area.

Marvin
  
Unfortunately that workaround doesn’t work while holding and using the cursor


Harry L (Apple)
  
So in that case you want the tappable area of the cursor to also be affected?


Marvin
  
Yes, correct


Harry L (Apple)
  
I see. There’s not a way to do that but a feedback requesting this would be appreciated.

----

@David
 asked:
When using things like UIViewRepresentable, how should bindings be synced with the coordinator?
1 reply

WWDC Bot
APP  
@Curt C (Apple)
 answered:
A Binding is a reference to the underlying state, so you can just pass the reference to the coordinator once. For other sorts of state, use the update method in your representable to propagate new values.

----

@David
 asked:
I want to accept files using Transformable, but the list of acceptable file types is provided by the server. It seems like I have to provide each file type statically. Is there a way to specify a dynamic, runtime list of file types I can accept? Or is there a way to accept all file types but get the…
See more
8 replies

WWDC Bot
APP  
@Julia V (Apple)
 answered:
Hey David! Can you describe the use case a bit more? Is my understanding correct, that you want to use Transferable with drag and drop or some other API?


David
  
Eventually with drag and drop, but currently I’m only using it with the photo picker. The API I work with (https://docs.joinmastodon.org/methods/instance/#v2) gives me a list of supported mime types.
docs.joinmastodon.orgdocs.joinmastodon.org
instance API methods
Discover information about a Mastodon website.


David
  
But my understanding of Transformable requires you to provide supported mime types in code at build time.


Julia V (Apple)
  
I am curious how you want to process the received files. Will there be a difference when handling an image vs video?


Julia V (Apple)
  
And to address your question: yes, Transferable expects the client code to know what they can handle.


David
  
I just upload the raw file to the server. It then processes it and determines if it’s a video/photo/audio file.


Julia V (Apple)
  
is using public.data content type an option for this case? All the data blobs, including audio and video, conform to this type identifier


David
  4 days ago
The 2 issues with that is 1) it would accept all types but 2) I don’t think there’s a way to get the file type when you do that?

----

@Ryan
 asked:
In animation packages a cubic bezier keyframe occurs at a point in time with handles on the left and right. With KeyframeTrack it seems like keyframes have a duration and instead handle the tail of the previous point and the start of the next. I haven't tested yet but I'm wondering if this would make it more difficult to create smooth transitions from one keyframe to the next. Am I thinking about this right? Is there some kind of smoothing going on at the transition between keyframes?
See less
5 replies

WWDC Bot
APP  
@Tim D (Apple)
 answered:
Great question. Similar to the earlier point about the use of duration instead of absolute timestamps, there are some differences between this API and keyframe editors in visual animation editors. In SwiftUI, each keyframe defines the curve to the keyframe's value, which works well when mixing different kinds of keyframes.
Each keyframe preserves velocity when possible. If you use a sequence of CubicKeyframes, the automatic velocity across each keyframe will give you a Catmull-Rom spline. If you specify a custom end velocity on a one keyframe, then follow it with another cubic keyframe that does not specify a start velocity, the interpolation curve to the second keyframe will automatically use the custom velocity that you specified at the beginning. The framework will always do the 'best thing' by automatically choosing a continuous velocity across keyframe boundaries when possible.
See less


Betsy L (Apple)
  
I refuse to believe that “spline” is a real word
:slightly_smiling_face:
1

Ryan
  
wait until you hear about “nurbs”
:scream:
2

Ryan
  
The velocity preservation stuff sounds really interesting. I look forward to testing it out

Tim D (Apple)
  
:bulb: Anything can be a word if you say if often enough (and convince your friends)! 
@Betsy L (Apple)
 I'll ask you the next time I need to name an API.

----

@Yuhao
 asked:
Is Tree manipulation supported in the new version of SwiftUI? For example, is it possible to edit nested data structures like folders in the same view with standard delete and move operations in edit mode?
5 replies

WWDC Bot
APP  
@Curt C (Apple)
 answered:
There isn't automatic support for editing tree-based data.

Curt C (Apple)
  
I typically use a view model for something like this where each element knows its place in the tree and its index in the list representation (a pre-order traversal if you like the computer science term).

Curt C (Apple)
  
Then the onMove and onDelete operations can use the view model to translate from indexes back to the tree data structure. (edited) 

Curt C (Apple)
  
I’d love a Feedback with your particular use case — like what operations do you need to support and what does your data model look like? (edited) 

Yuhao
  
Just simple parent-child relationship, there is a root object that contains ordered folders, then each folder have ordered items that can either be a file or another folder. I hope to be able to drag and drop into different levels (just like what apple notes does). I am aware that this might be tricky in SwiftUI. Previously I saw UICollectionView demo that can achieve just that, so I was wondering if there are any updates on this.

----

@David
 asked:
I’ve noticed that SwiftUI.ScrollView maintains @State as views are scrolled offscreen even when using Lazy* views. That’s probably the correct behavior in most cases. But for very long lists, things like image assets and just general state can build up over time leading to performance and memory issues. What’s the best way to evict memory in this case?
See less
2 replies

WWDC Bot
APP  
@Kyle M (Apple)
 answered:
Views in and of themselves generally do not have large memory footprint. If a view is holding onto something large, like an image, you could try nil'ing out the state when the view scrolls offscreen using onDisappear or keeping the image in an LRU cache that automatically evicts things stale items (then the view would only have to hold onto the key)

----

@Adedeji
 asked:
Just curios Is there any chance the 10 view limit in a stack gets reviewed in future versions?
2 replies

WWDC Bot
APP  
@Matt R (Apple)
 answered:
@ViewBuilder currently still has that limit.
However, note that there are a few different ways to work around that syntactic limit in your own code. For example, you can include a Group of views, which only takes up one "slot" in a builder:
```
VStack {
    ContentView1()
    ContentView2()
    ...
    ContentView9()
    Group {
        ContentView1()
        ContentView2()
        ...
        ContentView9()
        Group {
            // etc, no limit on nesting!
        }
    }
}
```
Or, if it's more convenient, you can use a @ViewBuilder-annotated helper variable, property, or function in place of Group for the same trick:
```
VStack {
    ContentView1()
    ContentView2()
    ...
    ContentView9()
    additionalContent
}
...
@ViewBuilder var additionalContent: some View {
    ContentView1()
    ContentView2()
    ...
    ContentView9()
    // etc
}
```
See less

----

@Marvin
 asked:
Is there a way to have a safeAreaInsets view that works similarly to toolbar with Material that only shows up when the content below is being shown in a scroll view?
1 reply

WWDC Bot
APP  
@Harry L (Apple)
 answered:
There's not a specific API for this.
You could use a GeometryReader to extract an anchor preference out of the scroll view to drive the opacity value of a material background in a safeAreaInset()
You could also use the visualEffect() API for a view inside of the scroll view to adjust its opacity and keep it pinned to the top which would achieve a similar effect to a safeAreaInset():
https://developer.apple.com/documentation/swiftui/view/visualeffect(_:)

----

@Marco
 asked:
Is TipKit available in Beta1? I am unable to import that framework nor find its documentation and the respective sample code does not work in an app that just imports SwiftUI
1 reply

WWDC Bot
APP  
@Curt C (Apple)
 answered:
You're correct that TipKit is not in beta 1. It's planned for later. We're not the experts on TipKit here. When the framework is available, I'd encourage you to take questions to the Developer Forums where there is a TipKit tag.

----

@Austin
 asked:
What are some good examples of new features from Apple this WWDC that were built with SwiftUI?
1 reply

WWDC Bot
APP  
@Kyle M (Apple)
 answered:
Vision Pro, watchOS 10, and the new widget features are some of my favorites

----

Do you have any general guidelines for improving build times when working with SwiftUI previews?
1 reply

WWDC Bot
APP  
@Tim D (Apple)
 answered:
Good question – previews can be a great way to iterate on animations using some of these new APIs, and keeping previews fast makes that easier.
I usually start by using the build log in Xcode to look at which operations take the most time during a build.
For smaller projects, sometimes that can be a file that uses complex generics, or some other language feature that takes more time during compilation.
As the project grows, I also like to split off UI components into their own module (sometimes using a Swift Package) to make sure that I'm only building the smallest possible portion of my project while using previews and building animations.

----

@Christopher
 asked:
Can Text render ruby attributed strings for things like Japanese?
1 reply

WWDC Bot
APP  
@Jacob R (Apple)
 answered:
No, Ruby annotations is not supported by SwiftUI or TextKit. To render Ruby annotations you'll have to drop down to CoreText.

----

@Ryan
 asked:
Is there a way using phase or keyframe animations to have an animation that starts off doing one thing then continues with a looping animation?
2 replies

WWDC Bot
APP  
@Tim D (Apple)
 answered:
The modifier loops over the phases that you specify in a collection – and you can choose which type of Collection to use. You could even build a custom Collection that dynamically returns phases in any order that you want, if you want full control

----

----
Kevin C (Apple) and the Previews team are answering questions.

----

@Paul
 asked:
Does Previews for UIKit support storyboards?
6 replies

WWDC Bot
APP  
@Charles A (Apple)
 answered:
Yes! You can instantiate a UIView or UIViewController in your preview from your storyboard.

Marcel
  
This is nice! Where can I find more info about this in documentation?

Charles A (Apple)
  
Take a look at the UIStoryboard documentation for more information. Specifically the instantiateViewControllerWithIdentifier: function. (edited) 
Apple Developer DocumentationApple Developer Documentation
UIStoryboard | Apple Developer Documentation
An encapsulation of the design-time view controller graph represented in an Interface Builder storyboard resource file. (22 kB)
https://developer.apple.com/documentation/uikit/uistoryboard?language=objc


Marcel
  
Oh, sorry. I meant a documentation about how to instantiate a VC in a preview....


Charles A (Apple)
  
Yes, if you want to load a view controller from your storyboard in a preview you'll need to use UIStoryboard. If, for example, you had a view controller with the identifier "ViewControllerIdentifier" defined in your storyboard, your preview would look something like this:

```#Preview {
    let storyboard = UIStoryboard(name: "Main", bundle: Bundle.main)
    let viewController = storyboard.instantiateViewController(withIdentifier: "ViewControllerIdentifier")
    return viewController
}
```

----

@Naftali
 asked:
Many times working with previews becomes difficult (almost like writing unit tests) is there a way to have Previews set default data, so for example lets say I'm passing a Bool into a view can previews by default give it a value so instead of SomeView(someBool: Bool)
it will be SomeView(someBool: .co…
See more
20 replies

WWDC Bot
APP  
@Eliza B (Apple)
 answered:
There isn't a previews-specific affordance for this, but you could set your view up to have an init with no arguments and have it provide reasonable defaults.


Naftali
  
can you elaborate?


Eliza B (Apple)
  
For example, if your view normally takes a bool, like this:
struct MyView: View {
   var toggle: Bool
   var body: some View { ... }
}
you could add an init with a default parameter:
init(toggle: Bool = false) { ... }


Eliza B (Apple)
  
however, I'm wondering if you're really asking about a case where the view takes a binding?


Naftali
  
That was one issue, another issue is, when adding in coredata, even with adding a preview container, it still is not the same since everytime it will be recreating the data, I would love for it to work the way app preview does


Eliza B (Apple)
  
like this:
struct MyToggleView: View {
   var isOn: Binding<Bool>
}

#Preview {
   MyToggleView( /* what do you do here */)
}
?


Naftali
  
Yes


Eliza B (Apple)
  
The answer is different for those two questions -- let me address the binding one here and you can post a different question about getting default data in swift data / core data?
:+1:
1



Eliza B (Apple)
  
There are a few patterns you can use if you're previewing a view that takes a binding.
```
#Preview {
   struct WrapperView: View {
      @State var isOn: Bool = false
      var body: some View {
          MyToggleView(isOn: $isOn)
      }
   }
   return WrapperView()
}
```

That's the general pattern you need but there are ways to write conveniences that make that easier.
:ok_hand:
1



Radu
  
I don't work for Apple, so not an official advice, but this is a problem I've faced since day one.
On top of what 
@Eliza B (Apple)
 already said related to the original question: architecture matters here, you'll want your views to ingest the simplest kind of data (e.g. String, Ints, not some complex objects that you'll have trouble constructing - I'm assuming that's why you'd like them auto-created). The way you achieve that is not always trivial of course.
Also, default arguments are not necessarily the best idea, you actually want to create some variation in state for your previews (when testing). So, a solution that allows you to easily variate state is more desirable to one that simply shows how a view looks with the default params.


Eliza B (Apple)
  
For example, outside of any particular preview you could define this:
```
struct WithPreviewState<S>: View {
   @State var state: S
   var makeBody: (Binding<State>) -> any View

   init<V: View>(
      _ state: S, 
      @ViewBuilder makeBody: @escaping (Binding<State>) -> V
   ) {
      self.state = state
      self.makeBody = makeBody
   }

   var body: some View {
      makeBody($state)
   }   
}
```

(I typed that into Slack so apologies if it doesn't quite build)

Then in your preview you could do this:
```
#Preview {
   WithPreviewState(false) { isOn in
      MyToggle(isOn: isOn)
   }
}
```

Naftali
  
Thanks!!


Eliza B (Apple)
  
It compiles in my head but should be adjustable to compile in Xcode. 

Okay this version actually works:
```
struct WithPreviewState<S>: View {
    @State var state: S
    var makeBody: (Binding<S>) -> any View
    
    init(
        _ state: S,
        @ViewBuilder makeBody: @escaping (Binding<S>) -> some View
    ) {
        self._state = State(initialValue: state)
        self.makeBody = makeBody
    }
    
    var body: some View {
        AnyView(makeBody($state))
    }
}

#Preview {
    WithPreviewState(false) {
        Toggle("toggle", isOn: $0)
    }
}
```

----

@Artyom
 asked:
`#Preview` macro is a Swift 5.9 feature and besides that, it generates Swift code that gets compiled. Also, new preview registries are marked with availability marks for iOS 17 and generated previews are iOS 17 as well.
Currently I receive build error when I’m trying to:
1. Run preview for iOS version older than iOS 17
2. Run preview for iOS 17 while minimum deployment target is iOS 15
The first makes sense to me, but the second seems like a bug since with old APIs everything was working is preview struct was marked available only on iOS 16 for example. What do you think?
See less
1 reply

WWDC Bot
APP  
@Kevin C (Apple)
 answered:
Yes, both of those cases with backwards deployment are known issues with the `#Preview` macro at this time.

----

@Nicholas
 asked:
Why does an Xcode Preview build ALL test targets that the current scheme depends on?
8 replies

WWDC Bot
APP  
@Jonathan P (Apple)
 answered:
Previews does a best-effort attempt to avoid targets that aren't necessary for the preview. Sounds like your situation is unique and will be best diagnosed by looking at the previews diagnostics. You submit them through the Feedback app, and if you post the Feedback ID here in this thread we can try to take a look during the session.

----

@Radu
 asked:
Is it currently possible to snapshot all previews in a workspace/project? Preferable from the CLI? It would be an amazing opportunity for snapshot testing. I feel there's a gigantic overlap between the views and states I manually test in previews and those that I'd like to snapshot test.
:heart:
1
:heavy_plus_sign:
1

1 reply

WWDC Bot
APP  
@Kristofer K (Apple)
 answered:
Unfortunately we don't have CLI for snapshotting all of your views in the workspace. Send us some Feedback!

----

@Alperen
 asked:
Will new 'Preview' macro work with apps that have a lower target like iOS 16?
1 reply

WWDC Bot
APP  
@Kevin C (Apple)
 answered:
It is a known issue that currently using the #Preview macro does not allow building in projects with a deployment target < 17.0 or running on a device running a version of iOS < 17.0.

----

@Dylan
 asked:
When working with the new Preview macros, what is the correct way to define properties for use within a preview?
Previously it would be something like this:
```
struct HistoryView_Previews: PreviewProvider {
    static var history: History {
        History(attendees: [
            DailyScrum.Attendee(name: "Jon"),
            DailyScrum.Attendee(name: "Darla"),
            DailyScrum.Attendee(name: "Luis")
        ],
                transcript: "Test Transcript")
    }
    
    static var previews: some View {
        HistoryView(history: history)
    }
}
```
4 replies

WWDC Bot
APP  
@Jonathan P (Apple)
 answered:
You can write local variables and constants inline in the macro and return the previewed view at the end!
```
#Preview {
  let name = "Hello!"
  return Text("\(name)")
}
```
:exploding_head:
4
:ok_hand:
4

Jan
  
@Jonathan P (Apple)
 and they no longer need to be static? If so, why?


Jonathan P (Apple)
  
Ah, good question. The PreviewProvider is used on a type with the static previews property. The only way to reference other properties on that type would also be to make them static.
The new #Previews macro simply takes a closure that returns the thing to be previewed. So you can declare local variables or do other setup necessary in there just like you would in any other closure. (edited) 

----

@Jan
 asked:
What is the best way to make SwiftData work in Previews? In Core Data I used a separate persistenceController.preview instance that was in memory using  "/dev/null"
5 replies

WWDC Bot
APP  
@Charles A (Apple)
 answered:
With SwiftData you should configure a model container in the environment of your view in the preview.

Charles A (Apple)
  
You would do this using the modelContainer modifier, specifying true for the inMemory parameter.
:raised_hands:
3

Jan
  
Thank you. It comes with this parameter predefined in one of the inits?

Charles A (Apple)
  
It's a parameter to the modelContainer View modifier. So more complete code might look like this:
```
#Preview {
    ContentView()
        .modelContainer(for: [MyModel.self], inMemory: true)
}
```

----

@Tien
 asked:
are previews conforming to PreviewProvider automatically stripped from enterprise builds?
1 reply

WWDC Bot
APP  
@Jonathan P (Apple)
 answered:
If you have all optimizations turned on for release builds and do not reference the conformers to PreviewProvider, then dead code stripping should help in this regard. If you want to make 100% sure to exclude code from a release build, then we recommend wrapping it in an "#if DEBUG" compile time conditional block.

----

@David
 asked:
Hi. I'm developing a SwiftUI app with Core Data backed by Cloudkit. There are c. 5 entities involved with relationships between them. I'm really struggling getting Previews to work, I've figured out how to generate sample data but the Previews keep crashing. Are there any good tutorials / code examples for this scenario?
See less
1 reply

WWDC Bot
APP  
@Charles A (Apple)
 answered:
There aren't any examples that are specifically for this scenario, but I would recommend setting up a separate in memory only persistent container that you use inside your previews. There is actually a simple example of this strategy in the new project template that you can see by creating a new project with the SwiftUI interface option and CoreData storage option selected.

----

@Pyry
 asked:
In a modularised project with Swift modules depending on each other in two or more layers, is there any way to make Xcode less picky on having the right build scheme selected?
(In one case, previews were constantly failing and ultimately the reason was having two editors with previews open side by side, but the files belonged in different modules, so no build scheme was right.)
See less
:100:
2
4 replies

WWDC Bot
APP  
@Kristofer K (Apple)
 answered:
Previews will try to pull in various streams of information like your open file(s) and active scheme, but it won't try to infer which scheme a file should belong to. You may be able to setup a new scheme that builds both targets those files would belong to.


Kristofer K (Apple)
  
There's a lot of different project structures, so I could also accidentally mistake your setup. Hearing feedback from you would be another way for us to dive into your workspace!

----

@Naftali
 asked:
When adding in coredata/ swiftdata to previes, even with adding a preview container, everytime it will be recreating the data, I would love for it to work the way app preview does, where the data stays the (somewhat) persists
4 replies

WWDC Bot
APP  
@Charles A (Apple)
 answered:
Filing feedback with the Feedback Assistant is a great way to let us know what you would find useful!
:+1:
1

Charles A (Apple)
  
Having said that, the data in a preview can be persisted.


Charles A (Apple)
  
If you setup an NSPersistentContainer that uses an on disk persistence strategy, it should be persisted. The challenge will be clearing that data store if you need to migrate the schema or reset the data. (edited) 


Charles A (Apple)
  
(the same is true for SwiftData, where you can use a modelContainer view modifier to create a container that will persist to disk)

----

@Alex
 asked:
For you, the Preview developers, is the new macro just a nicer experience for the users or has this new Swift capability allowed you to build something better that was not doable before?
:heart:
3
1 reply

WWDC Bot
APP  
@Kevin C (Apple)
 answered:
The new macro introduces underlying new API for defining previews. For SwiftUI this is mostly an ergonomic improvement, but it also fixes some longstanding bugs when using views that have ViewBuilder and you would expect the system to put it into an implicit VStack (like what happens at runtime), but instead generated multiple previews. Now it correctly creates a single preview with an implicit stack. It also adds support for UIKit & AppKit previews directly without needing to even importing SwiftUI.

----

@Valentin
 asked:
Should the PreviewProvider protocol be used in cases where we need to create @State or other dynamic properties or do the new macros support those cases as well?
8 replies

WWDC Bot
APP  
@Eliza B (Apple)
 answered:
Nope, there's a couple ways you can make this work with #Preview.

The pattern you want is something like this:
```
#Preview {
   struct Wrapper: View {
      @State var toggled: Bool = false
      Toggle("toggle", isOn: $toggled)
   }
   return Wrapper()
}
```

(Where I'm imagining that you're trying to preview a Toggle but obviously you could define whatever state you need to preview your actual view).


But there's a convenience you can define to make this doable with less boilerplate.

Define this outside of your preview:
```
struct WithPreviewState<S>: View {
    @State var state: S
    var makeBody: (Binding<S>) -> any View
    
    init(
        _ state: S,
        @ViewBuilder makeBody: @escaping (Binding<S>) -> some View
    ) {
        self._state = State(initialValue: state)
        self.makeBody = makeBody
    }
    
    var body: some View {
        AnyView(makeBody($state))
    }
}
```

Then you can write previews like this:
```
#Preview {
   WithPreviewState(true) { toggled in
      Toggle("toggle", isOn: toggled)
   }
}
```

----

@Deeje
 asked:
All of my views use CoreData, which makes previews challenging, thoughts?
:+1:
1
2 replies

WWDC Bot
APP  
@Charles A (Apple)
 answered:
The easiest thing to do is to setup an in memory only persistent container for use with previews. You can see a simple example of this approach in the new project template if you select the SwiftUI interface option and the CoreData storage option. Are there more specific challenges that you're wondering about?

----

@Bill
 asked:
I'm using `#Preview` to display a UIKit view (not SwiftUI), but no matter what frame I provide, the view always has the same (too small) size in the preview screen. Is there something else I need to do to adjust the size? I also tried with a simple UILabel and gave it a red background color - it seemed to be sized precisely to the text size, and my frame had no effect.
See less
18 replies

WWDC Bot
APP  
@Charles A (Apple)
 answered:
Previews use AutoLayout, so the frame you pass would be overridden by the UILabel content size. Have you tried constraining the view with AutoLayout?


Bill
  
Thanks for the reply - I was wondering if that might be the problem, but the view doesn’t have a superview at the time I create it


Charles A (Apple)
  
You could create a specific height and width constraint if you wanted to or create a wrapper view with different constraints in your preview.


Bill
  
Oh perfect! Setting just the widthAnchor and heightAnchor did the trick. Thank you!
:raised_hands:
1


Bill
  
One more question here: what does “Selectable” mode do when previewing UIKit?

For SwiftUI views, I see what it does, but it doesn’t seem to have any effect for UIKit previews


Charles A (Apple)
  
Selection of individual views is not supported in UIKit, but you can still use traits to adjust the canvas.
:+1:
1



Bill
  
One more thing I ran into: is it possible to constrain the view so it’s at the top of its parent? Since it hasn’t been added to a superview, I’m only able to set constant width/height constraints but I’d like to see the view at the top of the iPhone frame it renders in.


Charles A (Apple)
  
Please file a request in feedback assistant for that! I think right now you would have to hard code a container view that was the size of the UI.
:+1:
1

Bill
  
Okay cool, will do - thanks for answering all my questions!


Charles A (Apple)
  
It's maybe also possible you could create a view with translatesAutoresizingMaskIntoConstraints set to false and then put your view at the top of that with constraints.
:pray:
1

Bill
  
Hey that worked!! Great idea. I also had to set the subview’s translatesAutoresizingMaskIntoConstraints to false, but after doing that, it worked.
:raised_hands:
1

Charles A (Apple)
  
In general if you're using autolayout you should always turn translatesAutoresizingMaskIntoConstraints to false, or you'll almost certainly get errors at runtime because it will add constraints for the old springs and struts behavior.

Bill
  
That makes sense, and is what I usually do in real code. For some reason, the preview was working with Auto Layout constraints despite missing that setting.
:+1:
1

Charles A (Apple)
  
Sometimes you get lucky with which constraints the system breaks. :slightly_smiling_face:

Bill
  
Hehe :slightly_smiling_face:

Bill
  
Well thank you again for the help! I’ve already been improving some of my designs in just the few hours since I started playing with UIKit previews.

Charles A (Apple)
  
Glad you're enjoying it. If you run into anything else we'll have another slack activity tomorrow and will be keeping an eye on the forums.

----

@Bruno
 asked:
When using SwiftUI preview targeting Apple TV, is it possible to interact with the preview somehow?
:white_check_mark:
1

WWDC Bot
APP  
@Eliza B (Apple)
 answered:
Unfortunately not, events for tvOS previews are currently not working. This is a known issue but feel free to file a Feedback to indicate that you'd like this fixed!

----

@Brian
 asked:
If you're doing a live preview of either AppKit or SwiftUI macOS code in Xcode 15, what happens if your code brings up another window, like a sheet or a popover? Do those come up within the Xcode preview as well?
7 replies

WWDC Bot
APP  
@Jonathan P (Apple)
 answered:
It does not show up if you are previewing within the canvas in Xcode. Popovers and presentations like that need to be suppressed.
But if you toggle the preview to be "Preview as App" (control-click on the preview button in the bottom left of the canvas to see the popup menu) then an app launches with…
See more
:+1:
1



Daisy H (Apple)
  
This is how the option will look:
[Long-press the play button for a menu to show "Preview in Canvas" and "Preview as App"]
Screenshot 2023-06-07 at 12.38.44 PM.png

:raised_hands:
1

Bill
  
Does that only work for SwiftUI views?


Daisy H (Apple)
  
The above is also for AppKit previews. :slightly_smiling_face:


Bill
  
Hmm nothing happens for me when I control-click the play button


Daisy H (Apple)
  
Ah, yes. The two options are only available with Xcode 15 on macOS Sonoma.  Though if on pre-macOS Sonoma the preview can be live in a separate window by simply clicking the play/interactive button which will allow for sheets and other presentations to not be suppressed. (edited) 

----

WWDC Bot
APP  
@Dennis
 asked:
The new #Preview doesn't seem to work when you inject an @MainActor @Observable object into the environment of a View.
```
@Observable @MainActor final class ViewModel {}

#Preview {
    ContentView()
        .environment(ViewModel())
}
```
The preview crashes with the error: "Compiling failed: call to main actor-isolated initializer 'init()' in a synchronous nonisolated context".
Is that intentional or a bug?
Also I wanted to say: I love the new previews, it makes everything so much easier and more convenient :)
See less

7 replies

WWDC Bot
APP  
@Eliza B (Apple)
 answered:
Ah, this is because the preview closure is not @MainActor. Please file a feedback about this for us -- it probably should be!

Dennis
  
Alright, will do :slightly_smiling_face: Thanks!


Eliza B (Apple)
  
As a workaround for now, you can be confident that your closure is in fact invoked on the main actor, so you can safely use this new static method on MainActor:
```
static func assumeIsolated<T>(
    _ operation: @MainActor () throws -> T,
    file: StaticString = #fileID,
    line: UInt = #line
) rethrows -> T
```
so:
```
#Preview {
   MainActor.assumeIsolated {
      ContentView().environment(ViewModel())
   }
}
```

maybe that builds?

Dennis
  
Ah cool, thanks :slightly_smiling_face: Nice opportunity to use that new method :smile:
This builds and the preview works fine now :+1:

----
Switching to general SwiftUI Q&A session.
----

@Ricky
 asked:
Is it true that you are adding Paging to scroll views in iOS 17?
2 replies

WWDC Bot
APP  
@Curt C (Apple)
 answered:
Yes! Check out the scrollTargetLayout and scrollTargetBehavior modifiers. They support paging, viewAligned, and even custom alignment.
:raised_hands:
1

Curt C (Apple)
  
Watch “Beyond scroll views” to learn more.

----

@Jason
 asked:
In iOS 16, we could use @StateObject to declare data that a View owns, while @ObservableObject was for data injected into a View that the View didn't own, and it was pretty easy to create bindings to a property on either a @StateObject or an @ObservableObject.
Now, @State takes over the role of @StateObject, but @Bindable is what's used if you want to be able to bind to a property on an object. So if a View owns a reference-type source of data (like a view model), but you need to be able to create bindings to that view model's properties, should you use @State or @Bindable?
See less
2 replies

WWDC Bot
APP  
@Luca B (Apple)
 answered:
You can can get a Binding from @State. @Bindable is for all the cases where you need to get a binding, but don’t need to use @State.


Jason
  
Oh, of course, just like the way @State works currently :man-facepalming: Thank you!

----

@Aviel
 asked:
Unlike @Observable available starting from iOS17, @State attribute is not new, but it didn’t use to handle reference types before. If I target iOS 16 (or earlier), should I continue using @StateObject, or should I start using @State even while using the old ObservableObject protocol and @Published properties?
See less
3 replies

WWDC Bot
APP  
@Curt C (Apple)
 answered:
Yes, continue to use @StateObject for these use cases if you're deploying to earlier operating systems.
:raised_hands:
1
:white_check_mark:
1

Aviel
  
If I accidentally use @State on a class that does not have the Observable macro (ie before iOS 17) is there a warning at compile/runtime?


Curt C (Apple)
  
There is not.

----

@Andre
 asked:
Was the DatePicker only added to WatchOS or also to tvOS? I am really missing it on tvOS
3 replies

WWDC Bot
APP  
@Franck N (Apple)
 answered:
Hey Andre, unfortunately DatePicker was only added to watchOS this year.
Please feel free to file a feedback, describing your use-case if possible.

----

@Jérôme
 asked:
Is the new operator .scrollPosition(id:) intended to actually scroll to a new position? In other words, is ScrollViewReader deprecated?
3 replies

WWDC Bot
APP  
@Harry L (Apple)
 answered:
You can use the scrollPosition(id:) modifier to accomplish similar kinds of things you can do with the ScrollViewReader API and it does additional things that ScrollViewReader can't do. But you might still want to use a ScrollViewReader for scrolling to views in List and Table. Or if you want more fine grain control over scrolling to nested views within non-lazy stacks.
See more

Jérôme
  
oh ! scrollPosition(id:) doesn’t work at all on List and Table?

----

@Austin
 asked:
Is generally an anti-pattern to use GeometryReader in SwiftUI layouts?

5 replies

WWDC Bot
APP  
@Robert M (Apple)
 answered:
It's worth considering alternatives due to its costs and how it interacts with the view hierarchy, for example GeometryReader takes up all available space. Often there's a more idiomatic way to achieve the results that you want, and the Layout protocol has been added recently https://developer.apple.com/documentation/swiftui/layout
See less
Apple Developer DocumentationApple Developer Documentation
Layout | Apple Developer Documentation
A type that defines the geometry of a collection of views. (22 kB)
https://developer.apple.com/documentation/swiftui/layout



Magnus
  
Costs? Is it expensive to create?


Robert M (Apple)
  
It's always best to profile your application on all intended devices. Often, it alters the view it's contained in (as it stretches to the available space)


Magnus
  
Yea I understand the layout behavior of it - I just read the above as it itself was expensive to use


Robert M (Apple)
  
If a layout change happens, it involves a separate iteration. There's cost involved, and what is expensive ultimately depends on your circumstances. GeometryReader has its use, which is why it's provided, but it's best to consider the more directly applicable SwiftUI idioms first, and, profile!

----

@Aviel
 asked:
From looking at the expanded @Observable macro, it looks like Observation tracks “what is used” but not “who used” (I see nothing about the caller, I expected something like _enclosingInstance…).
Yet when I have 2 views, each using a different var from the same class, only the relevant view is updated (I’m using _printChanges to check).
Does the @Observable class know not to notify the other view of the change? Or does SwiftUI know not to diff the other view? Something else?
How does it work or what am I missing here?
See less

WWDC Bot
APP  
@Luca B (Apple)
 answered:
To give a concrete example: if you have a view A that reads the model.x stored property and view B that reads model.y from the same model instance. If you write only model.y only B would be run again its body.
Say that A is also reading model.z that is computed property from model.y. If you write .y both A and B will run their body.
Every individual view keeps track of the properties that is reading and invalidate only when those are changing.

----

@junyao
 asked:
There doesn't seem to be a good swiping gesture solution for List or ScrollView in swiftUI. Either swiping left and right in List doesn't customize the background of the button, etc., or swiping left and right in scrollview using DragGesture conflicts with scrollview gestures, such as stuck and delayed. How is it possible in the Diary app for iOS 17 to swipe to the right to show the Favorites button, and the swipe gesture does not conflict with the vertical gesture for List or scrollview? Or is it implemented in UIKit? Is it also necessary to face and solve these gesture problems in spatial-computing?

8 replies

WWDC Bot
APP  
@Haotian Z (Apple)
 answered:
Hi! You could try use the .swipeActions modifier on the row view to add buttons and customize the background appearance of the button using .tint. Is there a specific goal you want to achieve here?


junyao
  
Yes. I want to implement swipeAction like iOS 17 Journal app. How can I display icons without tint? Or is this just UIKit? (edited) 


junyao
  
I've seen a lot of apps implement swipeAction like the iOS 17 Journal app, showing icons without tint. But when I use SwiftUI, I find that difficult to implement.


Haotian Z (Apple)
  
Got it! Could you provide a screenshot of what you are referring to? (edited) 


junyao
  
Sure! As the red circle in the screenshot shows, there is a icon button without tint. (edited) 
截屏2023-06-07 23.21.47.png

Haotian Z (Apple)
  
Thanks for the image! That helps us to understand the issue. Right now swipe actions follows platform conventions so it might not be obvious to have a customize swipe button. I would suggest filing a feedback so we could keep track of this feature request!
:raised_hands:
1

Haotian Z (Apple)
  
Also cc 
@Raj R (Apple)

junyao
  
Ok, got it. Thank you very much for your explanation.

----

@Simon
 asked:
Is there anything built into SwiftUI + SwiftData that lets the user reorder items in LazyVGrid and persist that ordering?
1 reply

WWDC Bot
APP  
@Curt C (Apple)
 answered:
You could store information in your model about the ordering to persist it. LazyVGrid doesn't have built in support for reordering.

----

@Dennis
 asked:
When you access an @Observable object via the @Environment property wrapper, is it possible to create bindings to its properties?
Using @Bindable doesn't work here, right?
Example:
```
@Observable @MainActor final class MyObservableModel {
    var name: String = ""
}

struct ContentView: View {
    @Environment(MyObservableModel.self) var model
    
    var body: some View {
        TextField("Name", text: $model.name) // Does not compile
    }
}
```
See less
6 replies

WWDC Bot
APP  
@Daniel D (Apple)
 answered:
You can derived a @Bindable from your view body in this case
    var body: some View {
        @Bindable var model = model // add this line
        TextField("Name", text: $model.name) // works!
    }
:raised_hands:
5

Paul
  
why can’t this happen automatically?


Dennis
  
Ah, neat trick! Thanks


Aviel
  
my guess is because @Bindable has extra overhead cost that simple @Environment doesn’t maybe?


Paul
  
caused us some headaches when we were starting out


Daniel D (Apple)
  
why can’t this happen automatically?
For that to work, we'll need an API that adds a projectedValue to @Environment. If that's something you'd like to see, please file a feedback!

----

@Matthew
 asked:
Not sure if the SwiftUI team can address, but when I run an AppIntent from a SwiftUI button with the new support, the AppIntent loses the ability to talk to the HomeKit API if I background the app while it’s running — this same code works fine in the background for 10 seconds when run from the Shortcuts App or via “Hey Siri”.  This seems like an oversight - can we get the same support when an action is run from a Button?
See less
4 replies

WWDC Bot
APP  
@Luca B (Apple)
 answered:
It would be great if you could file a feedback with a repro sample of what you are trying to achieve here so that we can take a look at what is not working.


Matthew
  
I have filed a feedback:  FB12239247.  I will work on a reproduction scenario tonight


Luca B (Apple)
  
Thank you! Appreciate you taking the time to do that!


Matthew
  
Thanks Luca - this would address one of my customers biggest complaints!

----

@Jan
 asked:
Hi, I wanted to ask how to change the background colour of Button on macOS. The .tint modifier not work (FB12215240)
4 replies

WWDC Bot
APP  
@Jason S (Apple)
 answered:
Hi Jan! Thanks for your question. You can use tint modifier with `buttonStyle(.borderedProminent)` to customize the background color.

Jan
  
Hi Jason! I wish you were right but that's not the case. Even the Apple code in documentation for Button tint does not work. See attached screenshot and FB12215240
Screenshot 2023-06-07 at 22.17.33.png
 
Screenshot 2023-06-07 at 22.17.33.png

Jason S (Apple)
  
Have you had a chance to try the macOS Sonoma beta? It looks like a bug that's been fixed in that version. And I appreciate the feedback about the documentation!
Alternatively, you can tint the Button's label. That could look like one of these examples, depending on whether you want to inherit the tint color from ancestor views.
```
Button(action: myAction, label: {
    Text("Hello")
        .foregroundStyle(.green)
})

Button(action: myAction, label: {
    Text("Hello")
        .foregroundStyle(.tint)
})
.tint(.green)
```

Jan
  
I haven't tried Sonoma yet, thanks for the tip! Yeah I'm using a workaround like this at the moment, but then I lose the subtle decorations present in Button's default appearance.

----

@Sheng
 asked:
One question for SwiftUI ScrollView:
What is the best way to get ScrollView's contentOffset? Is below code right way to get the contentOffset?
Here is the sample code for these two questions.
```
struct ContentView: View {
   let columns = Array(repeating: GridItem(.fixed(100)), count: 10)
   var body: some View {
       ScrollView([.horizontal, .vertical], showsIndicators: true) {
           ZStack {
               GeometryReader { proxy in
                   Color.clear.preference(key: ScrollViewOffsetPreferenceKey.self,
                                          value: proxy.frame(in: .named("scroll")).origin)
               }
               LazyVGrid(columns: columns, spacing: 8) {
                   ForEach(0..<1000, id: \.self) { i in
                       Text("\(i/columns.count), \(i % columns.count)")
                   }
               }
           }
       }
       .coordinateSpace(name: "scroll")
       .onPreferenceChange(ScrollViewOffsetPreferenceKey.self) { offset in
           print("ScrollView's conentOffset is \(offset)")
       }
       .padding()
   }
}
struct ScrollViewOffsetPreferenceKey: PreferenceKey {
   typealias Value = CGPoint
   static var defaultValue = CGPoint.zero
   
   static func reduce(value: inout CGPoint, nextValue: () -> CGPoint) {}
}
```
See less
2 replies

WWDC Bot
APP  
@Harry L (Apple)
 answered:
There's not an API that provides you a binding to a CGPoint as a scroll view's content offset. However, there are several new APIs that allow you to transform views based on its relative position within a ScrollView.
The scrollTransition() API lets you apply customizations like scale, offset, and rot…
See more
Apple Developer DocumentationApple Developer Documentation
scrollTransition(_:axis:transition:) | Apple Developer Documentation
Applies the given transition, animating between the phases of the transition as this view appears and disappears within the visible region of the containing scroll view, or other container specified using the parameter. (22 kB)
https://developer.apple.com/documentation/swiftui/view/scrolltransition(_:axis:transition:)

Apple Developer DocumentationApple Developer Documentation
visualEffect(_:) | Apple Developer Documentation
Applies effects to this view, while providing access to layout information via . (22 kB)
https://developer.apple.com/documentation/swiftui/view/visualeffect(_:)

Sheng
  
I put a correct size Color to ScrollView for scrolling and read its contentOffset to draw a real content in another View.

----

@Jan
 asked:
In SwiftUI, Core Data objects conform to ObservableObject, so our views can update automatically when the object changes. For example: struct DetailView: View { @ObservedObject var item: Item ... } With Observable replacing ObservableObject/ObservedObject property wrappers, will CoreData objects automatically conform to Observable?
See less
5 replies

WWDC Bot
APP  
@Julia V (Apple)
 answered:
@Model macro used to mark SwiftData models automatically provides conformance of the model to Observable. This way, the models have the capabilities that you'd expect.
While SwiftData can coexist with CoreData and connect to the same database, these two worlds don't mix. NSManagedObject conforms to ObservableObject, and @Model-tagged types conform to Observable.
See less

Jan
  
Ok, right now I have to stick with Core Data because my use case (shared database Cloudkit sync) is not supported through SwiftData. Is there a risk ObservableObject / ObservedObject will be deprecated? Because this would break large parts of my app due to the conformance described above

Julia V (Apple)
  
I can suggest discussing this during the SwiftData Q&A session that will happen on Friday @ 2:00 - 3:00 p.m in 
data-frameworks
:+1:
1

Jan
  
ok, thanks!
:pray::skin-tone-2:
1

Julia V (Apple)
  
Oh, and you might be interested in the sample project that illustrates the migration: https://developer.apple.com/documentation/SwiftUI/Migrating-from-the-observable-object-protocol-to-the-observable-macro

----

@Artyom
 asked:
I'm curious about @backDeployed attribute from Swift 5.8
https://github.com/apple/swift/blame/main/CHANGELOG.md#2023-03-30-xcode-143
Is it something that cannot be used for new SwiftUI APIs or just was not adopted yet?
For example .scrollTargetBehavior modifier. It seems like it requires completely new type for parameter hence is not back deployed. Are there any @backDeployed APIs in SwiftUI this year?
See less
1 reply

WWDC Bot
APP  
@Jacob R (Apple)
 answered:
Only functions and computed properties can be back deployed; anything that relies on new types or system functionality can't be back deployed.

----

@Jérôme
 asked:
Is there a way to get a matchedGeometryEffect working across views from different "view controllers", like during a push/pop transition of NavigationStack or a present/dismiss of a sheet?
:eyes:
1
5 replies

WWDC Bot
APP  
@Curt C (Apple)
 answered:
A matchedGeometryEffect doesn't currently work across most complex container views, like List, NavigationStack, or NavigationSplitView.


Curt C (Apple)
  
I’d definitely appreciate Feedback with specific use cases. This is an area I’d like to improve.


Jérôme
  
@Curt C (Apple)
 use cases are all uses cases addressed by custom transition in UIKIt (aka UIViewControllerTransitioningDelegate)

----

@John
 asked:
(On watchOS,) is it possible to programmatically set the focus to a text field and begin text entry, with dictation as the text input method? The use case I have in mind is in the context of a note-taking app, where I want to reduce as much as possible the required physical interaction to start (and end) taking a note. Presently, that involves launching the app, tapping a text field, and then tapping again on the mic icon to start dictation (and then tapping the done button). Ideally, I'd like to get the entire interaction down to just one or two taps in total (e.g. open the app, then tap to start; or even use an app intent fired from the Apple Watch Ultra's action button to make it simply one physical button press to start; and then read the in-flight text entry to scan for a keyword to commit entry.)
See less
2 replies

WWDC Bot
APP  
@Matthew K (Apple)
 answered:
TextField always launches to the keyboard first on watchOS, and you can easily get to dictation with the mic button. Keyboard is a wonderful and quick way to enter text, and it's always the preferred input method on watchOS. Please file feedback, explaining your use case, for why you'd want to launch to dictation first.

----

@Alexander
 asked:
Hi!
I wonder if there is any performance difference between .opacity(0) and .hidden()? Also, is there any way to cause call of onDisappear on a subview, but preserve its state (for instance, if it's a ScrollView)? Simply removing view from hierarchy does the first, .opacity(0) does the second, but I'm struggling to find a way to do both.
Thanks,
Alexander
See less
:raising_hand:
1

7 replies

WWDC Bot
APP  
@Harry L (Apple)
 answered:
onDisappear is tied to the view's lifetime so whenever its called the state inside that view will be reset. So there is no way to achieve saving that state but also calling onDisappear. However, you can always lift the state higher than the view and keep the state around yourself.


Alexander
  
I could be mistaking, but NavigationStack does exactly what I’m asking: if you navigate off root screen to some subscreen, root screen receives onDisappear, but when you’re back all its ScrollViews have same state as before (and also onAppear is called)


Alexander
  
The problem with NavigationStack is that it’s available only on iOS 16+, and unfortunately it’s too buggy to use even on iOS 16 (the simplest example is that NavigationLink doesn’t reliably work in Menu which seems to be fixed in iOS 17, and there are plenty more issues that can’t be worked around)


Alexander
  
@Harry L (Apple)
 sorry for pinging, should I file separate question about that? For the NavigationStack behavior (regarding it saving ScrollView state but calling onAppear /onDisappear for root view when going to subscreen and back), I’ve just double checked this fact and I can provide a sample project and/or video recording that confirms it. (edited) 


Harry L (Apple)
  
I think a feedback detailing your request would be great!
You are correct that NavigationStack does have that behavior but there is no API that lets you achieve something similar.


Alexander
  
Thank you! You mean that I should file a feature request to feedback assistant? As you could guess, this behavior is desired to be able to write a view similar to NavigationStack to use on older iOS versions :slightly_smiling_face:


Harry L (Apple)
  
Right, a feedback requesting this feature.

----

@Matthew
 asked:
I have an app that I’m adding CloudKit Sharing support to, using the Core Data+CloudKit integration.  I have an NSManagementObject that I’ve conformed to Transferable using the `CKShareTransferRepresentation(exporter:)`, and that works great for creating new shares.  However, if I later open the ShareLink on an entity that is already shared, I get basically the same view, not the view that shows the existing participants.  Is this expected?  Is there a different control I should use here?
See less
3 replies

WWDC Bot
APP  
@Julia V (Apple)
 answered:
This does not look like expected behavior. Have you filed any feedback on this?


Matthew
  
I just created FB12262594 -- I had a CloudKit lab earlier, and had other non-SwiftUI related questions that are more urgent for me, but will try to create a reproduction scenario with this tonight or tomorrow


Julia V (Apple)
  
Thank you for filing it!

----

@Jamie
 asked:
Is there a way for a parent view to communicate with an off screen child inside a Lazy Stack or Grid? I want a child that has been scrolled off screen to know when the parent view has done something and adjust the behavior of its next onAppear, maybe with a boolean flag or something similar.

1 reply

WWDC Bot
APP  
@Harry L (Apple)
 answered:
When the view is scrolled offscreen in a lazy stack, it won't receive updates but you can communicate information by passing the info directly to the view.
For example, you could have a counter the parent keeps track of and pass the count to the child view. When the view next updates it will receive an up to date value of count.
Hope that helps!

----

@Luis
 asked:
Observation seems to bring good performance improvements, as far as I've heard that's due to Observable objects make SwiftUI only recalculate the body of views when observed properties change. But SwiftUI still diffs changes (right?), are there good tips to consider, like if it's better to use many Observable objects vs 1, or If it's ok to nest Observable objects inside Observable objects?
See less

2 replies

WWDC Bot
APP  
@Luca B (Apple)
 answered:
The diffing behavior remains unchanged in that once the view is invalidated we will still run body and avoid invalidating any downstream view that hasn't changed.
We do support Observable inside Observable and that is perfectly fine, and SwiftUI will track only the instance that you read in body.
I think it still important for you to consider how to better model your domain and what works best for you specific solution. With these new changes you should be able to have more expressive power in your design but still retain great performances.

----

@Brandon
 asked:
When transitioning from an ObservableObject to the new Observable macro, do we still need to use @State for our conforming view models?  Or is that only if we need to write to a property in a model?
2 replies

WWDC Bot
APP  
@Daniel D (Apple)
 answered:
You no longer need to use @State for displaying or writing properties of a Observable. I recommend watching the session "Discover Observation in SwiftUI" to learn more.

----

@Aviel
 asked:
In the SwiftUI animation talk, Kyle shows the new animation modifier which takes a content to avoid passing the animation down the attribute graph. Like so:
content
  .animation(.smooth) { $0.scaleEffect(...) }
Can the same be achieved with extra modifier to cancel the animation after the attribute I wish to animate? Like so:
```
content
  .transaction { $0.animation = nil }
  .scaleEffect(...)
  .animation(.smooth)
```
See less
2 replies

WWDC Bot
APP  
@Tim D (Apple)
 answered:
That's right – both approaches control the animation that is set on the transaction at the level of the modifier (.scaleEffect in this case).
The benefit of using the new .animation(...) { ... } modifier is that it only impacts the modifiers that you apply within the closure, and passes the original transaction+animation down to the rest of the hierarchy.
Using a separate .transaction modifier to 'reset' the animation would require you to know what the original animation was (unless you know that it should actually be set to nil for the rest of the hierarchy, as in your second code block above).
See less


Aviel
  
amazing thank you!

----

@Valentin
 asked:
How does SwiftUI keep track of what properties were used inside a body to invalidate views?
7 replies

WWDC Bot
APP  
@Matt R (Apple)
 answered:
Hi Valentin, great question! There's a lot we could say about that, but I'll try to provide a brief overview:
In general, SwiftUI will compare the values of the properties stored in your views to detect if those values have changed. If any property of a view changes, the view will update and body will get called. Part of how SwiftUI can perform these checks efficiently is due to SwiftUI views being lightweight structs.
However, views can also store references to reference-type model objects, such as ObservableObjects or classes using the new @Observable macro and protocol. For ObservableObjects, if any published property is set, SwiftUI will update all views depending on it. This is why it's often important for performance to avoid having many views in the same app all reference the same ObservableObject, since they will all get updated regardless of the properties they use.
With the new @Observable API, we're able to improve on that significantly. When a view calls its body property, SwiftUI will record any getter accesses on properties of @Observable instances, and start listening for changes to those specific properties. If any of them change, SwiftUI will reevaluate the dependent views, re-record any property getter accesses that occur during the execution of body, and repeat the process. This ensures that views will only update if their dependent data has changed. We think it will be a big win for performance in most apps compared to ObservableObject!
See less
:heart:
1
:pray:
1
:raised_hands:
1

Matt R (Apple)
  
@Observable is powered by the new open-source Swift Observation feature. You can read the open-source proposal to learn more:
https://github.com/apple/swift-evolution/blob/main/proposals/0395-observability.md
GitHubGitHub
swift-evolution/0395-observability.md at main · apple/swift-evolution
This maintains proposals for changes and user-visible enhancements to the Swift Programming Language. - swift-evolution/0395-observability.md at main · apple/swift-evolution (64 kB)
https://github.com/apple/swift-evolution/blob/main/proposals/0395-observability.md

:raised_hands:
2
:pray:
1
:heart:

Valentin
  
Thanks for the detailed answer!


Aviel
  
@Matt R (Apple)
 so when an @Observable object change one of its properties, does that mean views using other properties in that class are not even created, or they are created but body isn’t called?


Matt R (Apple)
  
Hi Aviel, I may not fully understand the question, but to try to answer: SwiftUI never itself instantiates your view structs, those only get instantiated within the execution of other body properties that are creating those structs.
So if a view depends on an @Observable object and no other properties, and the only piece of data that's changed is an observed property of that object, then the body of that view will be reevaluated without the view struct itself needing to be reinstantiated.


Aviel
  
ok that definitely helps! I mostly try to wrap me head around who’s the one keeping score - does the observable class track “view A cares about my foo property and view B cares about my bar”, or is it that view A knows “me & my body care about model.foo” and view B knows “me & my body care about model.bar”
Apologies if this is getting too much into the weeds! :smile:


Matt R (Apple)
  
It's the latter: SwiftUI is tracking the dependencies for the views — model objects do not have any internal knowledge of SwiftUI.

----

@Kiril
 asked:
There's a fairly well known "flow layout" which can be described like this: it puts components in a row, sized to their preferred size. If the remaining horizontal space on the same row is too small to put the next component, the component is then shifted to the next row.
What would be the best way to achieve something similar in SwiftUI? Thanks in advance
See less

2 replies

WWDC Bot
APP  
@Jacob R (Apple)
 answered:
The rough way for doing this would be to implement a custom layout conforming to Layout. Let's say you are implementing a horizontal flow you'd then use HStackLayout inside your implementation to measure and check each row.
* Propose a size to each child (nil width) to get back it's ideal width. Use this to determine how many of the children will be in the current row.
* Once we have a row we can use HStackLayout to stretch and place them.
* We now continue to the next row and we stop once all children has been consumed.

----

@Jan
 asked:
Hi, I wanted to ask how to remove row insets in List on macOS. I was not able to do this (FB12223245)

1 reply

WWDC Bot
APP  
@Haotian Z (Apple)
 answered:
Hi! Thanks for filing a feedback. The listRowInsets modifier is mostly to control layout within the List, so to use it to align with other elements in VStack might not give desired results. We will further look into the feedback and follow up with you through that channel, thank you!

----

@Naresh
 asked:
If we have a horizontal scroll view inside a list within a section, is there a way to make just that scroll view go end to end while the rest of the list get paddings?

1 reply

WWDC Bot
APP  
@Harry L (Apple)
 answered:
I imagine listRowInsets() would support this:
https://developer.apple.com/documentation/swiftui/view/listrowinsets(_:)
You can provide no insets by doing the following:
```
ScrollView(.horizontal) { ... }
  .listRowInsets(EdgeInsets())
```
Apple Developer DocumentationApple Developer Documentation
listRowInsets(_:) | Apple Developer Documentation
Applies an inset to the rows in a list. (22 kB)

----

@Alexander
 asked:
Is there an easy way to decide on a view contents based on space available? For instance, I have two buttons, first has icon + text in label, and second has just text. They are put in HStack and I want to remove text on a first button (leave just its icon) if one or both labels are going to be truncated.
For now to achieve this behavior I have to build monstrous constructions which involve hidden "probe" button instances and geometry readers to determine "preferred" text length by placing it in Group { label }.frame(width: 1000).frame(width: 0) where first .frame gives enough space for text to render and second .frame shrinks our "probe" Group to prevent it affecting overall layout.
3 replies

WWDC Bot
APP  
@Jake S (Apple)
 answered:
That's a great question! ViewThatFits might be a good solution to this. It allows SwiftUI to pick from a few possible Views according to which is appropriate for the available space.
You can see more about it here: https://developer.apple.com/documentation/swiftui/viewthatfits
Apple Developer DocumentationApple Developer Documentation
ViewThatFits | Apple Developer Documentation
A view that adapts to the available space by providing the first child view that fits. (22 kB)
https://developer.apple.com/documentation/swiftui/viewthatfits


Jake S (Apple)
  
One example of how to do something like you described could look like this:
```
struct ContentView: View {
    var body: some View {
        HStack {
            Button(action: {}, label: {
                ViewThatFits {
                    Label(title: {Text("Hello")}, icon:
                            {Image(systemName:"info")})
                    Text("Hello")
                }
            })
            Button(action: {}, label: {
                Text("World")
            })
        }
    }
}
```
Does this solve the issue that you're describing?

Alexander
  
Wow, what a cool new view I missed, thank you!

----

@Vanderlei
 asked:
Hello. from macOS 13.1 beta 4 to macOS 13.2, NavigationStack animations (when using animated binding) worked for both view insertion and view removal. The animation feature is documented in the macOS release notes. Since macOS 13.3 beta 1 animation only appears to work on view removal. It still does…
See more
8 replies

WWDC Bot
APP  
@Nick T (Apple)
 answered:
This is not by design. Can you drop your feedback number in here?


Vanderlei
  
For sure. The number is FB12061269. Thank you!


Nick T (Apple)
  
Hm, I've actually added some information to that particular feedback already. I really appreciated that bug! It boiled down to some unexpected ordering using an .onChange modifier. I expected that to have been fixed in latest 13 betas as well as 14. Let me check on that behavior and release alignment.


Vanderlei
  
That’s great. I’ll keep following. Thank you once again.


Nick T (Apple)
  
Hi Vanderlei, something is getting lost in the animation plumbing of your use case:
```
struct ContentView: View {
    @State private var path = [Path]()

    private var animatedPath: Binding<[Path]> {
        .init(
            get: {
                withAnimation(.easeInOut(duration: 5)) {
                    $path.wrappedValue
                }
            },
            set: { value in
                withAnimation(.easeInOut(duration: 5)) {
                    $path.wrappedValue = value
                }
            }
        )
    }

    var body: some View {
        NavigationStack(path: animatedPath) {
            MainView()
                .navigationDestination(for: Path.self) {
                    switch $0 {
                    case .detail:
                        DetailView(path: animatedPath)
                    }
                }
        }
    }
}
```
I've undup'ed your feedback and will need to hold onto it for a while longer. Does specifying your animation like this instead solve your use case

struct ContentView: View {
    @State private var path = [Path]()

    var body: some View {
        NavigationStack(path: $path.animation(.easeInOut(duration: 5))) {
            MainView()
                .navigationDestination(for: Path.self) {
                    switch $0 {
                    case .detail:
                        DetailView(path: animatedPath)
                    }
                }
        }
    }
}

Vanderlei
  
This worked! :slightly_smiling_face: (On macOS 14 beta 1, I will try on macOS 13 as well.)

Nick T (Apple)
  
:partying_face: For efficiency reasons, the latter is the better construction because get/set Bindings can't be compared for equality as efficiently as non-get/set Bindings. But if you do need the super fined grain control of a different animation per get/set that may also be changing, I could see how you have to use that construction.

----

@Anton
 asked:
 ```
@Observable @MainActor final class SomeModel {}
struct ContentView: View {
   let model = SomeModel() // Doesn't work
   var body: some View {...}
}
```
How to run an @Observable class on the MainActor?
See less


2 replies

WWDC Bot
APP  
@Daniel D (Apple)
 answered:
Since you annotated your model @MainActor, you'll need the same annotation on the parent scope per Swift concurrency rules.
```
@MainActor // <====
struct ContentView: View {
  var model = SomeModel() // should work now!
}
```

----

@Paul
 asked:
We arrived at a policy of never using `@State` because subviews of a ForEach would very often cause an edit to the model that supplied the array of that ForEach, among other cases that caused `@State` to get obliterated. Environment and ObservableObjects became our goto. Were we doing it wrong?
5 replies

WWDC Bot
APP  
@Anil K (Apple)
 answered:
It sounds like the identity of your view might not be stable across whatever edits you're making, or might depend on the properties that are being edited, which would cause the `@State` data to be cleared. If the IDs of the model type you're using in your ForEach change after an edit, that would change the view identities and clear the associated state. The Demystify SwiftUI session (https://developer.apple.com/wwdc21/10022) has more information about view identity.

Paul
  
Getting the IDs right helped, but it was the other properties. List items showed some state about the thing, and subviews edited that state.


Anil K (Apple)
  
Hmm, if the edited state somehow affects the identity of the parent view of the ForEach, that would also cause all state within the ForEach to be reset. If you can consistently reproduce the issue, definitely file a feedback—changes like you're describing seem like they shouldn't reset `@State` properties

----

@Sam
 asked:
Is there a way to wrap text around some other view? Something similar to a dropcap effect?
:heart:
1 reply

WWDC Bot
APP  
@Jacob R (Apple)
 answered:
This is not supported in SwiftUI. You'll have to use UIKit / AppKit to do something like this via exclusion paths.

----

@Andreas
 asked:
I have a bottom sheet with scrollable content in my app, similar like the one in Apple Maps. In some conditions, e. g. when the sheet is fully presented and the scroll offset is 0, I would like, that you can only scroll in one direction.
At the moment, I use `gestureRecognizerShouldBegin` of UIScrollView in an UIViewRepresentable for that.
But I would like, to accomplish that just with SwiftUI. Unfortunately, `ScrollTargetBehavior` is only called, when a scroll gesture ends. Is there something similar, for when a scroll gesture starts? Or do you have other ideas, to achieve that behavior?

2 replies

WWDC Bot
APP  
@Harry L (Apple)
 answered:
I dont believe this is possible at the moment but a feedback detailing your request would be appreciated!

----

@Jérôme
 asked:
Is there a way to programmaticaly control the progress of an animation in SwiftUI? A kind of UIViewPropertyAnimator but for SwiftUI?
7 replies

WWDC Bot
APP  
@Tim D (Apple)
 answered:
There is not a tool directly analogous to UIViewPropertyAnimator, but there are a few different approaches that you can use depending on the situation.
One new API this year is `KeyframeTimeline`, which lets you directly compute keyframe-driven interpolated values for a time that you specify, which you can then apply to the view using modifiers. You could drive this using a gesture, or any other source of 'animation progress.'

Jérôme
  
@Tim D (Apple)
 I don’t see how to update the progress of the animation based on a gesture. Is this covered in a session?

Tim D (Apple)
  
It's briefly covered in "Wind your way through advanced animations in SwiftUI."
If you create a `KeyframeTimeline`:
```
let myKeyframes = KeyframeTimeline(initialValue: 0.0) {
    CubicKeyframe(17.0, duration: 1.0)
    CubicKeyframe(-24.0, duration: 1.0)
    CubicKeyframe(0, duration: 1.0)
}
```
You can manually compute a value for a specific progress like this:
`let interpolatedValue = myKeyframes.value(progress: 0.3)`
...which you could then use to update the view.

Jérôme
  
In a TimelineView for example?

Tim D (Apple)
  
Yes, you could use a TimelineView if you want to update based on time. Or, if it's gesture-driven, you could use a gesture-controlled @State property in your view as a way to compute progress.

Jérôme
  
Ok I see, but this works for explicit animations only right? There’s nothing to control an implicit appear/disappear transition for example?

Tim D (Apple)
  
That's right. If you'd like to drive transition progress programmatically, I'd love to hear more about your specific use case. A feedback would be greatly appreciated! (and you can mention this thread)

----

@Anton
 asked:
In SwiftUI there is a PropertWrapper to write and read UserDefaults. Is there something similar for KeyChain?
2 replies

WWDC Bot
APP  
@Jeff R (Apple)
 answered:
Hi - thanks for your question! This isn't something that we have API for at the moment. A feedback for it, along with any information you can provide about your use case would definitely be appreciated.

Stan
  
I recently saw this, which may be helpful https://medium.com/@ihamadfuad/swiftui-keychain-as-property-wrapper-ca60812e0e5e

----

@Jamie
 asked:
What is the best way to know if a child view has been scrolled off screen as opposed to the parent view being taken off screen? (Basically different causes for `onDisappear`.) The best I've been able to find so far is using a `GeometryReader` and checking to see if the child view's geometry is within the parent's.
See less
1 reply

WWDC Bot
APP  
@Harry L (Apple)
 answered:
You could maybe use the new `scrollPosition(id:)` modifier though that won't give you the state of every currently visible view.
Using the `GeometryReader` is probably the best solution if you really want to care about only the visible region.

----

@Ricky
 asked:
Just out of pure curiosity why do we have a ForEach loop for views and for loop for everything else?
3 replies


Sam L (Apple)
  
Really interesting question! For loops are used in a few different ways throughout Swift and SwiftUI, so I think the best way to answer this is to take a quick look at each, and highlight why the solution we chose was the right one for SwiftUI.
The forms we most commonly see for loops in swift are the following:
1. For loops the control structure `for i in 0..<10 { print(i) }`
2. For loops in result builders
3. `ForEach` in SwiftUI
In SwiftUI for each use case, we’re not running imperative code (executing each case of the loop one after the other), so use case 1 isn’t super applicable. Instead, we’re building up a declarative representation of the multiple views specified by the for each. That leaves the remaining two approaches as possibilities.
At a quick glance, option two seems like it could work. Result builders don’t just execute each call in sequence, they transform things like `for item in items { … }` into values by combining the results produced in the for loops associated block. This could work for creating a primitive SwiftUI-like syntax. You could write something like:
```
for person in contacts {
   Text(person.name)
}
```
So why doesn’t SwiftUI do that? 
This works at the surface level, but imagine if the contents of the contacts array were to change. How would you know how to animate that change?
Say you went from a list containing “Amanda, Ben, Carly” to “Amanda, Carly, Ben”. Did the list just get reordered, or did Ben change their name to Carly, and Carly change their name to Ben? You can’t disambiguate between these two fundamentally different changes.
SwiftUI needs additional information for each item in the list, something to provide it a unique identifier. We use that unique identifier to keep track of which item is which even when they change locations in the underlying data structure.
That’s why we went with option three, a unique type, ForEach, which requires you to provide a way to determine the unique identifier for each element in the collection.
:pray:

Ricky
  
You say you can’t disambiguate between these two fundamentally different changes, Isn't this what HashCode calculations are for, or the memory address location that was assigned to the object once it was initialised. Only the thing that changed is the property / field, the base object albeit a view / object hasn't really changed in memory space. ?


Sam L (Apple)
  
There are cases where the data model you want to `ForEach` over isn’t necessarily `Hashable` in its entirety. Using an `Identifiable` conformance or id keypath allows you to instead just point to a single part of your data model type that _is_ `Hashable`, and which uniquely identifies data. Using the memory location isn’t really helpful here either as since the data we expect you to `ForEach` over are value types, whether they point at the same underlying pointer shouldn’t matter.

----

@Simon
 asked:
Is there any way to make a view refresh animated upon changes to a `@Query` parameter when deleting an object from a ModelContext while also avoiding the view animating upon the first load of the data from the ModelContext?
1 reply

@Curt C (Apple)
 answered:
Thanks for the question!
There's not a simple way to do that today. I'd love a Feedback with that use case!
If your model is equatable you could probably hack something together along these lines:
```
@Query... var models
@State private var hasLoaded = false

MyView()
    .animation(hasLoaded ? .spring : .none, value: models)
    .onChange(of: models) { 
         hasLoaded = true
    }
```
Or you might use task instead of onChange and enable animations after some short delay.

----

@Nayan
 asked:
I think I saw something about Text being embedded in another Text view. Is this correct? If so, what features does it support? Could it be an alternative to using AttributedString?
5 replies

WWDC Bot
APP  
@Luca B (Apple)
 answered:
Yes, `Text` does support doing string interpolation with other Text (and `Images` too!)
You can write something like
```
Text("Hello \( Text(person.name).foregroundStyle(Color.green) ) !")
```

Valentin
  
I’ve had a lot of fun with a custom resultBuilder that creates Text views, which can be used like this:


Valentin
  
  ```
        Text {
            for index in 0..<4 {
                Image(systemName: "\(index).circle")
                    .foregroundColor(.secondary)
            }
            
            Text(" ")
            
            if showLabel {
                Text("Label")
                    .font(.headline)
            }
        }
```
:exploding_head:

Nayan
  
@Valentin
 What is this result builder? It’s not built-in.

Valentin
  
@Nayan
 here’s a gist with the implementation https://gist.github.com/ValentinWalter/fe36ba6c2b865764b73a59a528eba39b

----

@Bardia
 asked:
Is it possible to now have a ScrollView that can move in either direction (up/down) with no limits (infinite scroll to any date in the past or future, think calendar) without any stutter/jump? The problem would occur when inserting items at the top of the list. FB10102752
9 replies

WWDC Bot
APP  
@Harry L (Apple)
 answered:
There have been improvements to scrolling backwards in iOS 17.0 in general and if you use a scrollPosition modifier, the ScrollView will try to keep the view currently visible at the same relative offset when your data changes.
However, there still isn't a concept of an "infinite" scroll view. It has a finite size for its content so you would be responsible for adding / removing views as a user gets close to / away from the end.
See less
:+1:


Shannon
  4 days ago
@Harry L (Apple)
 Is there a code example anywhere for this:
if you use a scrollPosition modifier, the ScrollView will try to keep the view currently visible at the same relative offset when your data changes.


Harry L (Apple)
  4 days ago
The docs for the scroll position modifier give an example usage and describe some of this in more detail:
https://developer.apple.com/documentation/swiftui/view/scrollposition(id:)
There’s not an example that shows the behavior you quote though adding some code to the example in the docs to insert an element at the beginning would show that.
Apple Developer DocumentationApple Developer Documentation
scrollPosition(id:) | Apple Developer Documentation
Associates a binding to be updated when a scroll view within this view scrolls. (22 kB)
https://developer.apple.com/documentation/swiftui/view/scrollposition(id:)


Shannon
  4 days ago
ah. Oh well, I tried to make it work based on those docs for a while yesterday and eventually gave up.


Harry L (Apple)
  4 days ago
What aspect wasn’t working? The modifier in general or the keeping a relative position behavior?


Shannon
  4 days ago
that's tough to answer. The code on that page isn't complete enough to wor as-is so I have to guess what's missing vs what's not working yet vs what I'm doing wrong vs what is incorrectly documented.


Harry L (Apple)
  4 days ago
Ah I do see the docs have some mistakes in them now with regards to the modifier’s spelling. You could try this self contained example which illustrates the insertion / removal behavior
```
struct ContentView: View {
    @State var data: [String] = (0 ..< 25).map { String($0) }
    @State var dataID: String?

    var body: some View {
        ScrollView {
            VStack {
                Text("Header")

                LazyVStack {
                    ForEach(data, id: \.self) { item in
                        Color.red
                            .frame(width: 100, height: 100)
                            .overlay {
                                Text("\(item)")
                                    .padding()
                                    .background()
                            }
                    }
                }
                .scrollTargetLayout()
            }
        }
        .scrollPosition(id: $dataID)
        .safeAreaInset(edge: .bottom) {
            Text("\(Text("Scrolled").bold()) \(dataIDText)")
            Spacer()
            Button {
                dataID = data.first
            } label: {
                Label("Top", systemImage: "arrow.up")
            }
            Button {
                dataID = data.last
            } label: {
                Label("Bottom", systemImage: "arrow.down")
            }
            Menu {
                Button("Prepend") {
                    let next = String(data.count)
                    data.insert(next, at: 0)
                }
                Button("Append") {
                    let next = String(data.count)
                    data.append(next)
                }
                Button("Remove First") {
                    data.removeFirst()
                }
                Button("Remove Last") {
                    data.removeLast()
                }
            } label: {
                Label("More", systemImage: "ellipsis.circle")
            }
        }
    }

    var dataIDText: String {
        dataID.map(String.init(describing:)) ?? "None"
    }
}
```
:raised_hands:

Shannon
  4 days ago
There are not enough emoji available to accurately convey my gratitude.

----

@Valentin
 asked:
Can Boolean operations be applied to shapes or views? For example, can a circle be subtracted from a shape to create a transparent cutout, like in vector editing software?
4 replies

WWDC Bot
APP  
@Robert M (Apple)
 answered:
There are operators like https://developer.apple.com/documentation/swiftui/shape/subtracting(_:eofill:) https://developer.apple.com/documentation/swiftui/shape/symmetricdifference(_:eofill:) https://developer.apple.com/documentation/swiftui/shape/union(_:eofill:)
Apple Developer DocumentationApple Developer Documentation
subtracting(_:eoFill:) | Apple Developer Documentation
Returns a new shape with filled regions from this shape that are not in the given shape. (22 kB)
https://developer.apple.com/documentation/swiftui/shape/subtracting(_:eofill:)

Apple Developer DocumentationApple Developer Documentation
symmetricDifference(_:eoFill:) | Apple Developer Documentation
Returns a new shape with filled regions either from this shape or the given shape, but not in both. (22 kB)
https://developer.apple.com/documentation/swiftui/shape/symmetricdifference(_:eofill:)

Apple Developer DocumentationApple Developer Documentation
union(_:eoFill:) | Apple Developer Documentation
Returns a new shape with filled regions in either this shape or the given shape. (22 kB)
https://developer.apple.com/documentation/swiftui/shape/union(_:eofill:)



Jan
  
I'm sure I've seen something like this in tutorials on the web


Magnus
  
You can do this with blendmode, like https://magnuskahr.dk/posts/2023/03/blendmode-trick-swiftui-reverse-mask
magnuskahrmagnuskahr
Blendmode trick: SwiftUI reverse mask | magnuskahr
Adding reverse masks to a view to cut out those pixels.


Robert M (Apple)
  
There are performance tradeoffs to watch out for, for example, `blendMode` is being used in compositing, resulting in pixel-based operations. As always, worth profiling on a diversity of the target devices, ensuring that your app has good performance for the broadest set of users

----

@Andrew
 asked:
Maybe I’m remembering it wrong, but I don’t recall seeing at `@Bindable` discussed in what’s new for SwiftUI session. Is this correct? However, "Discover Observation in SwiftUI" session did cover `@Bindable` along with `@Observable` and `@State`. Am I remembering this wrong or was there a reason that `@Bindable` was not covered in the first session?

2 replies

WWDC Bot
APP  
@Daniel D (Apple)
 answered:
We sure have a lot of SwiftUI changes this year :sweat_smile:. I think the  What's New talk simply wasn't long enough to cover every single changes such as `@Bindable`. No particular other reason. Great observation from you, though!

----

@Tyson
 asked:
How can we add jump bars like in contacts to our ListViews, ScrollViews, etc?
1 reply

WWDC Bot
APP  
@Curt C (Apple)
 answered:
SwiftUI doesn't currently support a ScrollView or List index.

----

@Anton
 asked:
 ```
@Observable @MainActor final class SomeModel {}

@MainActor
struct ContentView: View {
   var model = SomeModel()
   var body: some View {
       Text("")
   }
}
```
Unfortunately, this does not work.
How can I run my @Observable Model on the MainActor?
See less
5 replies

WWDC Bot
APP  
@Daniel D (Apple)
 answered:
What error are you seeing?

Anton
  
"Call to main actor-isolated initializer 'init()' in a synchronous nonisolated context."


Daniel D (Apple)
  
Hmm, strange. Is it a stale compiler error by any chance?

Anton
  
I had to add `@MainActor` to the preview. I am sorry...:sweat_smile:

Daniel D (Apple)
  
No worries, glad it worked out :partying_face:

----

@Robert
 asked:
With the new SwiftData, if I understand correctly we access the model using an @Environment variable on the view. Doesn't this contradict the idea of MVVM which the idea is to have your view be as simple and be driven by the View Model. What is Apples recommended architecture or method to handle this?
See less
:-1:

WWDC Bot
APP  
@Matt R (Apple)
 answered:
The environment is a data flow tool for helping to conveniently pass data through large portions of your view hierarchy, as well as scope data dependencies to only the views that need them.
It's not required for models to be passed through the environment. You could pass models directly between views as normal properties, to give a simple example. The right way to pass data through your app often depends on the specific data your working with and how it's used.
So the environment is just a tool, and should be used for the cases where it works best. For example, many apps leverage the environment to make it easy to pass larger models between views that represent the main screens of the app, helping avoid boilerplate. But then within those screens, that break apart those models into smaller pieces of data that are passed as normal properties to smaller component views, like a specific custom control, so that each view's data is scoped to just what that particular view needs, i.e. allowing those views to be as simple as possible.
When architecting your views, I'd focus on that general guidance: for this particular view, what data does it need? If it's a smaller view that's driven by a few specific properties, then it might not be appropriate to pass in a whole model via the environment. If it's a larger view that's constantly being iterated on, like an entire screen of the app, then  accessing the full model conveniently via the environment might be the best and most convenient option.

----

@Luis
 asked:
not a question, I read on the docs we will be able to use the Observable macro alongside ObservedObject on the same type and it will just work, SwiftUI will know what to do on new and old OS versions, so just wanted to say THANK YOU!!! Finally something we can use without having to wait to drop support for previous versions :heart:
See less
:heart:
4
:swiftui:
3
:raised_hands:
2

WWDC Bot
APP  
@Robert M (Apple)
 answered:
Thanks for the feedback!

----

@Rafal
 asked:
Hi! What would be the best way to have global default values for @AppStorage properties? Right now, I seem to have to replicate the defaults in every view I want to access them...

WWDC Bot
APP  
@Jeff R (Apple)
 answered:
Hi - thanks for your question! You have a couple of options for this, I think.
- Global constants, optionally wrapping them into a struct if you want to namespace them.
- Creating a custom `DynamicProperty` that encapsulates `AppStorage` with your constant value.
:pray:

Jérôme
  
`AppStorage` uses `UserDefaults` under the hood, so registering defaults `register(defaults registrationDictionary: [String : Any])` should work right?
:pray:

Rafal
  
Thanks! A global constant seems straightforward. Do you have any examples how to use DynamicProperty for this purpose?

Jérôme
  
or maybe `AppStorage` modifies the keys so we can’t exactly replicate them? Never tried
:pray:

Jeff R (Apple)
  
I don't have an example handy, but the general idea is to make a new type that conforms to DynamicProperty and internally uses `@AppStorage` with your key and default value specified. Then, you can use this new dynamic property in the same places you were previously using `@AppStorage`.

----

@Fredric Michael
 asked:
Hi,
Is there any new API in Sonoma where I can set the sidebar toolbar color? The current API in Ventura will set the entire toolbar color for both sidebar and content.
Thanks
3 replies

WWDC Bot
APP  
@Jeff R (Apple)
 answered:
Hi - we don't have anything new for this in macOS Sonoma. If you could file a feedback for this, and include any info about your use case, that would be helpful. Thanks!

----

@David
 asked:
I have a SwiftUI `List` that can receive dropped images via `.onInsert(of:)`, which passes `NSItemProviders`. How can I get a `NSFilePromiseReceiver` from that provider? I want it to support file promises to get full-resolution images from apps that provide them, e.g. the Photos app (on macOS, though presumably similar on iOS).
See less
16 replies

WWDC Bot
APP  
@Julia V (Apple)
 answered:
Can you clarify how you want to use the promise receivers as opposed to `tiff` or `jpeg` data?


David
  
When dragging a photo from Photos on Mac to my list, it only provides a low-resolution as a JPEG. The full resolution is included as a file promise. I want the full resolution, so need to resolve that.


David
  
I can detect a promise via this code:
```
func itemPromiseType(for provider: NSItemProvider) -> NSPasteboard.PasteboardType? {
    let types = NSFilePromiseReceiver.readableDraggedTypes.map { NSPasteboard.PasteboardType($0) }
    
    for type in types {
        if provider.hasItemConformingToTypeIdentifier(type.rawValue) {
            return type
        }
    }
    
    return nil
}
```
But my attempt to handle it doesn’t work; the switch falls through to the default:
```
func createBlockWithPromise(from provider: NSItemProvider, type: NSPasteboard.PasteboardType, at dropIndex: Int) {
    provider.loadItem(forTypeIdentifier: type.rawValue) { [self] item, error in
        switch item {
            case let filePromiseReceiver as NSFilePromiseReceiver:
                filePromiseReceiver.receivePromisedFiles(atDestination: destinationURL, options: [:],
                                                         operationQueue: workQueue) { (fileURL, error) in
                    if let error = error {
                        print(error)
                    } else {
                        print(fileURL)
//                            self.handleFile(at: fileURL)
                    }
                }
            case let fileURL as URL:
                print(fileURL)
            default: break
        }
    }
}
```


Julia V (Apple)
  
Have you tried using `dropDestination` API? If there are any promises provided, we attempt to load them for you
Apple Developer DocumentationApple Developer Documentation
dropDestination(for:action:isTargeted:) | Apple Developer Documentation
Defines the destination of a drag and drop operation that handles the dropped content with a closure that you specify. (22 kB)
https://developer.apple.com/documentation/swiftui/view/dropdestination(for:action:istargeted:)



David
  
Interesting; I haven’t tried that. I’d have to have a separate drop view for that, though. I’d prefer to be able to drop multiple photos in a list, to insert them in arbitrary locations.


Julia V (Apple)
  
If you have a `ForEach` that builds up the `List`, `dropDestination` behaves similarly to `onInsert` (it is available for `DynamicViewContent`). Can this work for your use case?


David
  
Oh excellent. I just tried adding `.dropDestination(for: NSImage.self)`, and got an error Instance method 'dropDestination(for:action:)' requires that 'NSImage' conform to 'Transferable'. What should I pass instead of NSImage.self to accept photos from promises?


Julia V (Apple)
  
try `Image.self`

And that's interesting that you got this error, because NSImage conforms to Transferable https://developer.apple.com/documentation/appkit/nsimage
Apple Developer DocumentationApple Developer Documentation
NSImage | Apple Developer Documentation
A high-level interface for manipulating image data. (22 kB)
https://developer.apple.com/documentation/appkit/nsimage

This issue might be caused by using older SDK


David
  
`Image.self` worked.  But can I get a NSImage from the Image so I can save it?


Julia V (Apple)
  
Is there an option to use newer SDK and leverage the NSImage: Transferable conformance? I don't think that there are any ways to perform conversion in this direction.


David
  
Yes, I won’t be releasing this app until Sonoma is out, so should be able to accept NSImage if that works there. I think that’ll solve my issue. Thank you very much for your help working through this!
:+1::skin-tone-2:
1



Julia V (Apple)
  
Thank you for a great question!
And also, if you got a minute, I'd appreciate a feedback about the problem you described at the beginning (failing to load the promise from the `NSItemProvider`).


David
  
Done: FB12263488.


Julia V (Apple)
  
Thank you!

----

@Jordan
 asked:
When I put a Text in the second column of a columns style Form, the SwiftUI view gets extended beyond the available horizontal space (a fixed width constraint on the UIHostingController's view) seemingly due to not line wrapping quite right. Is there a way to ensure it won’t grow larger than the containing view or could this be a bug? I have a sample project in feedback FB11809780. :)
See more
1 reply

WWDC Bot
APP  
@Jacob R (Apple)
 answered:
This looks like a bug; thanks for filing it!

----

@Matthew
 asked:
Is there a way to combine some kind of an overlay button in a SwiftUI TextField, like the ‘clear button’ that can be used in UIKit’s UITextField?  I’ve tried experimenting with this in the past, but have never found the right combination (seems more likely that I’m doing something wrong than something that’s not possible)
See less
4 replies

WWDC Bot
APP  
@Curt C (Apple)
 answered:
There isn't a really clean way to do this today. We'd love a Feedback to help us prioritize making this easier.
:eyes:
1



Curt C (Apple)
  
In the meantime, you might try adding an `.overlay(alignment: .trailing) { … }` to the `TextField`. A button in the overlay could clear the text binding that’s passed to the field.
:eyes:


Matthew
  
Thanks Curt!  I tried that recently, but to be honest, I don't remember what problem I ran into -- I'll give that another shot in the next few days, and file a feedback if it needs it!
:eyes:


Curt C (Apple)
  
A Feedback would be great. Thanks!!

----

@Jonathan
 asked:
I see that iOS 17 brings TextEditorStyle which we can make ourselves as it has a makeBody method https://developer.apple.com/documentation/swiftui/texteditorstyle/makebody(configuration:) Is there anyway to do this for TextField as it's TextFieldStyle is not public in the same way.
I'm trying to make a TextField's that have a border that become green when focused, but I can't seem to find a way to get the FocusState in a view that wraps TextField. As in
`MyTextField(...)
.focused($focusState, equals: .email)`
in MyTextField:     @Environment(\.isFocused) will not be set to true when the focusState above it is set to .email.
Is it possible to make a design that reacts to the focus state?
See less
Apple Developer DocumentationApple Developer Documentation
makeBody(configuration:) | Apple Developer Documentation
Creates a view that represents the body of a text editor. (22 kB)
https://developer.apple.com/documentation/swiftui/texteditorstyle/makebody(configuration:)

:eyes:
1

5 replies

WWDC Bot
APP  
@Cody B (Apple)
 answered:
Hi Jonathan,
The `isFocused` environment value tells you if you have an ancestor view that has focus. That won't be the case for MyTextField, since the text field that it wraps is what actually has focus.
Instead, I'd expect some combination of `@FocusState/focused(_:equals:)` to work for you. For instance, with MyTextField, you could use a local private focus state binding to see when focus is in the wrapped text field, and draw a border accordingly. Something like this:
```
struct MyTextField: View {
    @FocusState private var isFocused: Bool
    ...

    var body: some View {
        TextField(...)
            .focused($isFocused)
            // stuff for drawing border if isFocused == true
    }
}
```

Jonathan
  
Would I then be able to set the focus of MyTextField programatically still, eg:
```
MyTextField(...)
.focused($formFocus, equals: .password)
```

Cody B (Apple)
  
Yes, I'd expect that to work. When you put .focused() on a view that isn't directly focusable, we look for a focus target among that view's descendants.

----

@Jan
 asked:
Can we use `@Query` outside of views? For example, in a DataFetcherService class? If not, how to fetch SwiftData outside of views like this, in a way that will make views update themselves when the underlying data changes?
2 replies

WWDC Bot
APP  
@Daniel D (Apple)
 answered:
`Query` is designed to work dircetly with SwiftUI views. SwiftData provides imperative APIs for accessing data. In a "service", for example you could construct a `ModelContainer`, and fetch or modify models via its `mainContext`. Your "view model" can be an `Observable` object that stores result of those fetches, and it'll work great with SwiftUI. The docs for working with `ModelContainer` can be found [here](https://developer.apple.com/documentation/SwiftData).

----

@Jamie
 asked:
I have a child view nested within a parent (parent) view, nested within its own Lazy Stack parent (grandParent). If I scroll on grandParent such that parent goes out of view, an then some more, when I scroll back to parent the child view is now gone. I tracked the memory allocations via Instruments and found that the root view inside the child (there's a whole `UIViewRepresentable` structure in there) is getting deallocated/removed from memory. Is there a way to either
a) Prevent a descendant view from being destroyed when it is out of view?
b) Force a child view to be re-initialized when a view comes back into view?
See less
1 reply

WWDC Bot
APP  
@Curt C (Apple)
 answered:
Thanks for the question!
Directly doing a) or b) isn't possible. I'd suggest trying to lift your state above the lazy container so you're just passing a reference or a binding to it into the parent and child views. This allows the grandparent to keep the state alive even as the deeper views come and go.

----

@Jan
 asked:
If I fetch a large dataset (using `@FetchRequest` or `@Query`) into List or LazyVStack, should I consider batching in the fetch request, or will performance be good because these load data lazily?
2 replies

WWDC Bot
APP  
@Kyle M (Apple)
 answered:
In general, List and LazyVStack should do work proportional to the view port, so even if the dataset is very large they should be able to handle it. Check out https://developer.apple.com/videos/play/wwdc2023/10160/ tomorrow for more info on this.
At the point, when the dataset is so large keeping all the model objects in memory... that's the time to start looking at batching.


----

@Vendula
 asked:
When using the `searchable` view modifier and tapping the search bar the navigation bar is hidden with an animation, is it possible to keep it shown? I think the `hidesNavigationBarDuringPresentation` property is used for this behaviour in UIKit. Is there an equivalent in SwiftUI?
:heavy_plus_sign:

WWDC Bot
APP  
@Harry L (Apple)
 answered:
There is no equivalent API in SwiftUI for that property, but a feedback requesting this would be appreciated!

----

@Sam
 asked:
How should we handle situations where a segmented control is appropriate but the actual design of a system segmented control makes it unfeasible (either there are either too many options or the labels are too long)?
5 replies

WWDC Bot
APP  
@Franck N (Apple)
 answered:
Hey Sam, Thanks for the question.
Assuming iOS, in cases where you there are too many options AND the labels are very long, we typically recommend using the wheel picker style or the `navigationLink` picker style if contained within a List.
However for most cases many options or medium length labels the menu style is always preferred.
Do you mind expanding on your use case? It seems like you would prefer a segmented style but or system design poses a barrier to you. Could you go deeper on the specifics?
See less


Sam
  
@Franck N (Apple)
 Thanks for your reply! It's similar to Twitter's profile tabs, switching a profile view between different types of posts: "Posts", "Posts + Replies", "Media", and "About"
Right now, it's just those four segments but I can easily see this snowballing into many more. I don't think the standard system segmented control is sustainable here with those labels — five items will make it extremely tight esp with internationalization


Sam
  
On Android/Material, they've got these scrolling tab bars but there's not a clean parallel to that on iOS. I've seen something like that for the chip filter row in Maps (when you tap a pre-defined category like "Restaurants" but that seems more like a filtering thing rather than a view mode switcher)


Klayton
  
Would this be a good use case for TipKit--adding text popovers to Image labels?


Franck N (Apple)
  
I see. So ideally you'd would want a "minimally styled" segmented picker that could scroll horizontally in certain cases? A bit like the new `palette` picker works within menus? This is an interesting use-case.
Could you please file a radar describing your specific use-case? That would be highly appreciated.

----

@Fredric Michael
 asked:
Hi,
To remove the translucent background of the sidebar on macOS when using a NavigationView I was recommended to set `.scrollContentBackground(.hidden)`. But I have not been able to get this to work. What is the correct way to implement this?
Thanks!
12 replies

WWDC Bot
APP  
@Nick T (Apple)
 answered:
Hi Fredric, a great question. Can you give a little more detail what you're trying to achieve here? Is the intent to have a solid background color for the sidebar? Or just the system background color, only non-translucent?


Fredric Michael
  
@Nick T (Apple)
 I basically want to set my own solid background color for the entire sidebar. Right now I am forced to use introspect to set the background color of the List combined with setting the background for the entire sidebar.


Nick T (Apple)
  
@Fredric Michael
 we're chatting about this kind of use case and have a few more questions:
What background color do you want? A system material, or solid "plain old" color?
Do you still want sidebar list styling, just on top of this solid background? Or are you trying to get the content and detail columns without sidebar?


Nick T (Apple)
  
Also, could you give this code a run and see if it is close?
```
@main
struct TestApp: App {
    var body: some Scene {
        WindowGroup {
            NavigationSplitView {
                ZStack {
                    Color.green.ignoresSafeArea()
                    List {
                        ForEach(0..<100) { int in
                            NavigationLink("Option \(int)", value: int)
                        }
                    }
                }
            } detail: {
                Color.yellow
            }

        }
        .windowStyle(.titleBar)
        .windowToolbarStyle(.unified(showsTitle: true))
    }
}
```

Nick T (Apple)
  
(Note that those are broken navigation links given no .navigationDestination or List selection, but they'll work for the proof-of-concept)


Fredric Michael
  
I want a solid "plain old" color and I am using sidebar list styling. I am not using NavigationLinks but instead change the state of a selected row (folder) that in turn  updates the content view. I will attach a screenshot so that you can see what it looks like.


Fredric Michael
  
Screenshot 2023-06-07 at 23.16.22.png

Nick T (Apple)
  
I think what I've recommended above should work for your use case, but if you're trying to use a material there in the sidebar, you might run into some trouble.


Fredric Michael
  
Interesting solution. Yes that seems to work. Seems a little "hacky". So there is no other way to turn off the translucency?


Fredric Michael
  
@Nick T (Apple)


Nick T (Apple)
  
No there isn't. This was a very useful discussion though, we'd appreciate a feedback to track this and come up with the best way to handle this

----

@Jonathan
 asked:
How can I implement a confirmation, when the user pulls down a sheet? Exactly how adding an event in the Calendar app asks if you want to discard or keep editing the new event.
There is View.interactiveDismissDisabled() to prevent it being pulled down, but I can't find a way to react to the user trying to pull it down.
In UIKit it would be https://developer.apple.com/documentation/uikit/uiadaptivepresentationcontrollerdelegate/3229888-presentationcontrollerdidattempt
See less
Apple Developer DocumentationApple Developer Documentation
presentationControllerDidAttemptToDismiss(_:) | Apple Developer Documentation
Notifies the delegate that a user-initiated attempt to dismiss a view was prevented. (22 kB)
https://developer.apple.com/documentation/uikit/uiadaptivepresentationcontrollerdelegate/3229888-presentationcontrollerdidattempt

5 replies

WWDC Bot
APP  
@Curt C (Apple)
 answered:
Thanks for the question!
`interactiveDismissDisabled()` is the best we have with a pure SwiftUI solution today.


Curt C (Apple)
  
If you have a chance to file a Feedback asking for parity with the UIKit API, I’d really appreciate it. A Feedback for this doesn’t need a lot of detail.

External Feedback helps us prioritize the work.

----

@John
 asked:
Any guidance on creating an adjustable vertical split view?  In portrait orientation, I want a view on the top half and bottom half of the screen, and I want the user to be able to tap and hold a divider line  between these 2 views to increase/decrease how much of each view is show by dragging it up and down
See less
:+1:

7 replies

WWDC Bot
APP  
@Nick T (Apple)
 answered:
Hi John, which platform are you asking about?


John
  
@Nick T (Apple)
 iOS/iPadOS, thanks!


Nick T (Apple)
  
Ah, on macOS there is VSplitView but on iOS, this should be doable via the Layout protocol
:heart:


Nick T (Apple)
  
and some gesture recognizers to boot


John
  
ah ok, yea I saw VSplitView was macOS only and was confused why that would be, i’ll take a look at Layout


Nick T (Apple)
  
A feedback here would be a good thing to have if you can file :slightly_smiling_face: It'd be good to know what other capabilities you might expect from a system view like this

----

@Jan
 asked:
Is it possible to implement a full text search in TextEditor? Where after entering a few letters, it would scroll to the first matching string, and offer buttons to move to other instances of matching string
1 reply

WWDC Bot
APP  
@Harry L (Apple)
 answered:
TextEditor has built in support for find and replace as of iOS 16.0. You can control the presentation of this UI with the findNavigator() modifier:
https://developer.apple.com/documentation/swiftui/view/findnavigator(ispresented:)

----

@Fredric Michael
 asked:
Hi,
Is there any way to enable the same behavior in SwiftUI as usesPredominantAxisScrolling in NSScrollView. Most use cases of graphical nature such as a drawing canvas should scroll freely in both axes.
Thanks
2 replies

WWDC Bot
APP  
@Harry L (Apple)
 answered:
There is no equivalent to that API in SwiftUI though a feedback detailing your use case would be appreciated!

----

@Nicholas
 asked:
I'm trying to use a SwiftUI button in a list without using a `NavigationLink`.
I'm trying to replicate the effect you get when a NavigationLink is tapped, where the row background changes to systemGray4.
Using a default button, the list row highlights if you tap and hold, but not after a quick tap like you see when using a NavigationLink.
Is this a known issue- or is there a better way to achieve this?
See less
9 replies

WWDC Bot
APP  
@Curt C (Apple)
 answered:
It's a bit of a hack, but you could use a custom button style and change the background if the button is pressed OR if the value that the navigation path contains the associated value.


Curt C (Apple)
  
Essentially, your style would show the “pressed” state if the button was pressed or if its associated view was presented.


Nicholas
  
Gotcha…  I will try that - thanks!
Is this behavior by design that it only highlights on long tap? Or should I enter a FB?


Nicholas
  
I also noticed that the tap state of the button doesn’t always seem to trigger consistently when in a list (more noticeable when using a prominent button style)


Curt C (Apple)
  
I think the delay is because the containing scroll view has to reject the gesture first, but I’d need to investigate a particular code sample to be sure.


Curt C (Apple)
  
One thing to watch for with a custom button style in a list is that you may need to set `.contentShape(.rect)` so the background is part of the tap target.
:+1:


Nicholas
  
If it is down to the scroll view rejecting the gesture, is there a way around that?


Curt C (Apple)
  
If it is the scroll view, then the delay would be consistent with other apps. If you’re noticing more than that, it’s probably something else and worth a Feedback.

----

@Mohammad
 asked:
What happens when an observable property changes, but the view observing it is out of visible bounds? Does the view get updated or update happens when the view becomes visible?
1 reply

WWDC Bot
APP  
@Daniel D (Apple)
 answered:
The view does get updates regardless of visibility of the element displaying the object's property.

----

@Nicholas
 asked:
Is it valid to use nested buttons in SwiftUI? If so, are there recommendations for how to solve the associated accessibility issues (where the inner button is cannot be focused by Voiceover)? If not, what is the recommended approach?
2 replies

WWDC Bot
APP  
@Jason S (Apple)
 answered:
You’re right that only the outermost `Button` can be focused by VoiceOver, but the actions of the nested buttons can still be performed by VoiceOver users!
A nested `Button` will be added to VoiceOver’s custom actions menu that displays other types of actions an element can perform, such as the equivalent of a right-click to show a menu. (On macOS, that can be activated by pressing the VO keys + Command + Space.)
For example, you might have a `Button` or `NavigationLink` that represents an item in a grid, each with a “favorite” button within it. SwiftUI will automatically add the “favorite” button to VoiceOver’s custom actions menu.
Alternatively, you could consider using a `ZStack` to instead position one button above the other, instead of nesting them.

----

@Willie
 asked:
Will there be additions to SwiftUI for keyboard navigation instead of what we currently use NSEvent local monitoring for?
5 replies

WWDC Bot
APP  
@Cody B (Apple)
 answered:
As far as keyboard input goes, there's a new `onKeyPress()` view modifier that lets you react to keyboard input directed at the focused view. That may get you what you need, though of course it depends on what you're currently using event monitors for.
If you have specific cases where you still need to fall back on AppKit event monitors, please consider filing feedback reports describing them!
See less
:+1:


Willie
  
I’m using arrow keys for keyboard navigation in a LazyVGrid on a Mac SwiftUI app


Willie
  
I think `onKeyPress` will be what I need, I hope! :-)


Cody B (Apple)
  
One thing to note is that `onKeyPress` is sensitive to focus—if you put it on your lazy grid but the grid itself and its contents aren't focusable, you won't get any calls.
And FYI, for Arrow Keys specifically there's a higher-level `onMoveCommand` modifier you can use in place of `onKeyPress`. One benefit: it back-deploys. Same note about focus applies.


Willie
  
Thanks for the caveat and the info about `onMoveCommand`, I’ll check them out!

----

@Jason
 asked:
Just want to confirm, among all the new animation updates in SwiftUI this year, there's nothing that lets us customize the presentation of sheets  - either size, position, or custom transition animations, right? In other words, there's not really a bridge to the UIViewControllerTransitioningDelegate for presenting sheets or pushing new views onto the navigation stack?
:heavy_plus_sign:

WWDC Bot
APP  
@Curt C (Apple)
 answered:
That's correct. We'd love a feedback request to better understand the effects you're trying to achieve.

----

@Valentin
 asked:
In "Design with SwiftUI," Will showcased a "ticker" animation for the walking radius label at 8:26. I once tried to create a similar animation by adding transitions to separate Text views for each digit in an HStack. Unfortunately, this caused the font's kerning to be incorrect. It seemed to me like the implementation displayed in the aforementioned session had correct kerning. Can you talk about how something like that could be implemented?

WWDC Bot
APP  
@Curt C (Apple)
 answered:
Thanks for the question.
I don't have access to the specific code that Will was demo'ing. My guess is that he used the `monospaceDigit` modifier on the `Font` to get the font variant that kerns digits without shifting as they change.

You might also look at the `contentTransition` modifier which has some built-in options for transitioning between Text values that include numbers.


Valentin
  
Thanks! `.contentTransition(.numericText())` is what I was trying to replicate, because I wasn’t able to get it to work outside of Live Activities. I was under the impression that was intentional — is this meant to be used anywhere?


Curt C (Apple)
  
`contentTransition` should work in general. You need to be careful to not invalidate the identity of the surrounding view through the transition, otherwise the entire Text will be replaced and the `contentTransition` won’t fire.

----

@Tien
 asked:
UITraitCollection has a trait called userInterfaceLevel. Do we have something similar in SwiftUI? I want to be able to tell when my view is in an elevated context.
1 reply

WWDC Bot
APP  
@Anil K (Apple)
 answered:
You can use the new UITraitBridgedEnvironmentKey protocol to bridge the user interface level from UIKit to SwiftUI. However, depending on what your goal is, it may be simpler to use the builtin SwiftUI views and colors that automatically adapt to the current level.

----

@Bogdan
 asked:
Most of my apps are MVVM, will the SwiftData `@Quary` work if it’s inside an `@Observable` viewModel instead of a View?
:-1:

3 replies

WWDC Bot
APP  
@Julia V (Apple)
 answered:
`@Query` uses the underlying `ModelContext` of the `View` that is provided via `Environment`.
Because of that, `@Query` works only when it can access the `Environment`.


Bogdan
  
Thanks 
@Julia V (Apple)
,  so how do we go about using SwiftData from where it cannot access the `Environment`? eg. viewModel or some kind of manager?
:-1:


Julia V (Apple)
  
You'll have to do all the same operations manually: pass the `ModelContext` to the model, ask it to provide you the models, etc.

----

@Yannik
 asked:
Is there a multi-platform way to just have a single Window in SwiftUI?
There is "Window" for macOS and the "UIApplicationSupportsMultipleScenes" Info.plist key set to false for iOS, but doing a single thing instead of having to do both would be nice.
2 replies

WWDC Bot
APP  
@Jeff R (Apple)
 answered:
Hi - thanks for the question! You are correct that Window is only available on macOS. We'd certainly appreciate a feedback for this, though. If you can include any information about your specific use case, that also helps. Thanks!

----

@Michael
 asked:
Do the new additions to ScrollView provide hooks into the pan gesture that powers the view's scrolling? I'd like to build a custom pull-to-refresh control for my content, but I'm not sure if this is possible without understanding when the pan gesture is in progress and when a user has lifted their finger.
See less
2 replies

WWDC Bot
APP  
@Harry L (Apple)
 answered:
There are no new APIs around hooking into the state of the scroll view's pan gesture.
However, there is a new scrollTransition() and visualEffect() API that lets you transform a view based on its relative position within the scroll view's visible region.
You could use potentially use this to create a custom refresh indicator without needing to interact with the pan gesture. I'm thinking put a refresh indicator at the top of your scroll view that reveals itself as you scroll up.

----

@David
 asked:
With current `ObservableObject` conformance in iOS 16 we were able to use @published vars with the $ prefix to get access to the publisher and then could set up combine chains to react to updates to the data. With moving to `@Observable`, is there a new recommendation on how to react to changes to the v…
See more
1 reply

WWDC Bot
APP  
@Kyle M (Apple)
 answered:
I recommend checking out and even participating in the active open source Swift Evolution review of @Observable: https://forums.swift.org/t/second-review-se-0395-observability/65261
In the "Future Directions" section of the proposal it says: "An earlier version of this proposal included asynchronous sequences of coalesced transactions and individual property changes, named values(for:) and changes(for:)."
However even without values(for:) to track individual property changes, I'd recommend keeping as much logic in your model as possible. Computed properties on your model can come in really handy for this. SwiftUI will intelligently invalidate when any of the stored properties accessed from the computed property change.

----

@Rhys
 asked:
Is there a way in SwiftUI to remove the shadow from a Navigation bar?
For example, in UIKit, I would use:
`UINavigationBarAppearance().shadowImage = nil` and/or `UINavigationBarAppearance().shadowColor = .clear`
Using .toolbarBackground(.someColor, for: .navigationBar) still displays a shadow.
Is there a way to remove that shadow?
See less
:heavy_plus_sign:

WWDC Bot
APP  
@Harry L (Apple)
 answered:
There is currently no equivalent to that in SwiftUI but a feedback detailing your request would be appreciated!

----

@Rafal
 asked:
With Swift Charts, when I update the underlying data model object,, my charts do update, but there is no animation. Do I have to write code or modifiers to enable animated transitions? I was under the impression that changes would be animated by default.
4 replies

WWDC Bot
APP  
@Robert M (Apple)
 answered:
Are you using `withAnimation`, for example `withAnimation(.easeInOut) { ... change the data ... }`?


Rafal
  
no, should I? I have not seen this code in the sample code (SwiftChartsExample), where the transitions do animate. Hence my impression...


Robert M (Apple)
  
Please, try it and file a feedback in case of a mismatch between what you expect based on talks/docs and what behavior you see. You can file a request using Feedback Assistant by visiting https://developer.apple.com/bug-reporting/

----

@Pyry
 asked:
Suppose I was using `@SceneStorage` to persost some bits of pertinent scene state for later restoration. But `SceneStorage` can only accommodate some basic types and the docs recommend not to store anything large there. (A JSON string could of course store virtually anything.) So all is well, I can make a UUID string and store it in `@SceneStorage` and put the big data elsewhere so I can restore the whole scene.
But when the user or OS eventually deletes a scene which was once persisted, how will the app come to know it's no longer there so it could reclaim disk storage?

WWDC Bot
APP  
@Jeff R (Apple)
 answered:
Hi - thanks for your question! You are correct around the current API for `SceneStorage`. I don't believe we have anything that could help with your specific case here, but we'd love a feedback with these details so that we can improve on the API.

----

@Naftali
 asked:
Is there a way to know when a compact date picker is open or closed? For example I have a compact date picker and when the date is changed I want to save the date to CoreData. Currently I’m using `onChange` but I feel like it’s not so efficient, I wish it would support .focused unless there’s a better solution
See less
:eyes:
1

1 reply

WWDC Bot
APP  
@Sam S (Apple)
 answered:
Currently, there is no support for detecting when a `DatePicker` is opened or closed, but this does seem like it could be useful. Please file a feedback with some more information about your use case since we use feedbacks to help inform our future design direction. Thanks!

----

@Tien
 asked:
I create a custom SwiftUI component and follow the style approach that we use for native buttons to customize this component from the call site if need. My style is just a protocol. Is there a way to mimic the behavior of native styles like `ButtonStyle` where we can access the environment directly and allow automatic body invalidation even though it’s not a view?
See less
1 reply

WWDC Bot
APP  
@Max O (Apple)
 answered:
There is! You can create types that receive updates to SwiftUI properties just like View by conforming them to the DynamicProperty protocol. Whenever SwiftUI encounters a property conforming to DynamicProperty inside a View, it will also update any SwiftUI properties stored inside it.
You should be get started with the following example:
```
import SwiftUI

struct ContentView: View {
    var body: some View {
        MyCustomControl(state: .constant(true), style: MyStyle())
    }
}

struct ContentView_Previews: PreviewProvider {
    static var previews: some View {
        ContentView()
    }
}


struct MyCustomControl<Style: MyCustomControlStyle> : View {
    @Binding var state: Bool
    
    var style: Style
    
    var body: some View {
        style.makeBody(configuration: .init(isOn: $state))
    }
}

struct MyCustomControlConfiguration {
    @Binding var isOn: Bool
}

protocol MyCustomControlStyle<Body> : DynamicProperty {
    associatedtype Body : View
    
    typealias Configuration = MyCustomControlConfiguration
    
    
    @ViewBuilder func makeBody(configuration: Self.Configuration) -> Self.Body
}

struct MyStyle : MyCustomControlStyle {
    @Environment(\.colorScheme) var colorScheme
    
    func makeBody(configuration: MyCustomControlConfiguration) -> some View {
        Group {
            colorScheme == .light ? Color.yellow : Color.red
        }
        .onTapGesture {
            configuration.isOn.toggle()
        }
    }
}
```

----

@Abdel Ali
 asked:
I have a question about making text clickable with hashtags and mentions. What is the most effective way to detect clicks on hashtags and mentions within text? I would appreciate any guidance or suggestions you can provide. Thank you
2 replies

WWDC Bot
APP  
@Jacob R (Apple)
 answered:
Have you tried using markdown or `AttributedString` with `.link` attribute? These links should be handled through the environments `\.openURL` action.

----

@Trevor
 asked:
With the new Observable macros, will this change the suggested app architecture away from MVVM? To me it seems like we almost no longer need the view model?  Thanks!
:+1:
1
:man-facepalming:

1 reply

WWDC Bot
APP  
@Matt R (Apple)
 answered:
Our goal is always to make it so that SwiftUI views can be as simple as they need to be, and that the most intuitive code to write also provides great performance.
The new `@Observable` macro will definitely help! Unlike with `ObservableObject`, views track dependencies to `@Observable` objects based on the specific properties of those objects that they actually use within `body`. This means that there are a number of cases where apps may have previously needed to restructure their `ObservableObject` model code to break models into smaller pieces just to achieve good performance, where `@Observable` will provide better performance without needing to do similar tricks. This may eliminate the need to create bespoke, per-view models in some cases.
Please try it out and share your feedback with us! If there are places in your code where `@Observable` is not an improvement over using `ObservableObject`, we'd love to hear from you.

----

@Jiayou
 asked:
`onKeyPress` does not work inside a `ScrollView`. Do you know if this a known bug or feature?
```
struct ContentView: View {

    @State var text = ""

    var body: some View {
        ScrollView {
            TextField("placeholder", text: $text)
                .onKeyPress("a") {
                    print("Pressed a!")
                    return .ignored
                }
        }
    }
}
```
3 replies

WWDC Bot
APP  
@Cody B (Apple)
 answered:
This is a known issue in the seed. In general, onKeyPress has trouble interoperating with bridged AppKit views that also try to handle key events.

Also true for bridged UIKit views.

----

@Aviel
 asked:
If I want to add a SwiftUI view to a UIView, do I also have to reach the view controller and add the Hosting Controller as a child view controller? Or can my leaf UIView just retain the Hosting Controller, and just add the main view of that hosting controller as subview to my UIView?
1 reply

WWDC Bot
APP  
@Anil K (Apple)
 answered:
The `UIHostingController` should be added as a child view controller, otherwise some functionality may not behave correctly.

----

@Jan
 asked:
First of all let me say the new modifiers for Scrollview are excellent, well done :v:
Now - can we tell ScrollView to start/end scrolling programmatically?
I want to build an app with a daily timeline that scrolls and may have an event placed on top of it. This event can be moved around and stretched/contracted, as in the iOS Calendar app. When the event is stretched to the edge of the visible area, the scrollview should start scrolling automatically to reveal more of the timeline. In the iOS Calendar app, moving or stretching an event beyond the visible timeline causes the entire timeline to smoothly auto-scroll and reveal the area into which the user wants to move or stretch the event.
Unfortunately I was unable to implement this. Already submitted FB11955036 in the past. There should be a method to start/stop scrolling of Scrollview or List, along with a setting for scrolling speed. Something like: ‘.scroll(isOn: Binding<Bool>, direction: .beginning/.end, speed: CGFloat? = nil)’
See less
5 replies

WWDC Bot
APP  
@Curt C (Apple)
 answered:
The new API doesn't support this use case.
You might experiment with rendering your timeline using a `ZStack` or `Canvas` rather than a `ScrollView`. This would let you maintain an offset manually and adjust the contained views when the user's gesture reaches the edges of the containers bounds.


Curt C (Apple)
  
Admittedly, this approach has other drawbacks, since it loses the usual scroll affordances.


Curt C (Apple)
  
Thanks very much for the Feedback!


Jan
  
Thank you 
@Curt C (Apple)
! I will try your suggested approach for the time being. Hopefully we will get this in the future!


Curt C (Apple)
  
Good luck. It’s a really cool UI!

----

@Charles
 asked:
Are there any changes to TextField in iOS 17? In particular, can we have a way to clear the text?
5 replies

WWDC Bot
APP  
@Harry L (Apple)
 answered:
What do you mean by "clear the text"? You can write to the binding of the text field to be an empty string which will "clear the text".


Matthew
  
Like a button to clear the text?  (I asked something similar above)


Charles
  
Yes, mine was the same as Matthew’s question. I’d prefer that a “clear” function was built-in to TextField in similar fashion to AppKit and UIKit.


Harry L (Apple)
  
Ah, no there is no API for that currently in SwiftUI though a feedback requesting this would be appreciated.

----

@Sean
 asked:
Is it possible to use a List within a ScrollView without having to manually set the .height() of the List to make its cells visible within the ScrollView? Thanks!
6 replies

WWDC Bot
APP  
@Haotian Z (Apple)
 answered:
Hi! We want to know more about your intended usage - could you explain more on the 'visible' part? Is it that you are trying to make List itself non-scrollable and let the outer ScrollView to drive the scrolling? We would also appreciate a feedback so that we could keep track of it and communicate with you on this topic, thank you!
See less


Sean
  
@Haotian Z (Apple)
 Yeah of course! So I made this in Swift Playgrounds. I want to have an image on top and then list rows below. If I use .scaledToFit(), the list appears, but the bottom one (without any frame modifier) doesn’t.
Screenshot 2023-06-07 at 22.37.32

Then, if I add .scaledToFit() to the second list, it appears, but at the bottom of the ScrollView because the topmost list’s background takes up all the available space
Screenshot 2023-06-07 at 22.38.23
 
So, is it possible to restrict the amount of space to just the space needed for the cells, or should I just create custom cells that match the style of the insetGrouped .listStyle()?

Thanks!

Haotian Z (Apple)
  
Got it, thanks Sean for the detailed explanation! For now I would suggest using one `List` and put the image within `List` as a custom cell. However, a feedback on making `List` self-sizing to the total height of its cells would be great for us to keep track and evaluate the idea, thank you! (edited) 

----

@Naftali
 asked:
When using a number pad with a TextField there is no done button, is there any plans to add one? If not what’s expected to happen once the user is done inputting (other than manually implementing a done button)
1 reply

WWDC Bot
APP  
@Jason S (Apple)
 answered:
We recommend adding your own "Done" button in your UI.

----

@Andrew
 asked:
Do the new (or existing) ScrollView APIs allow access to the content offset or away to apply effects to a view based on its position? E.g. an scroll view of album art views that rotate or have a transform applied as the list scrolls.
2 replies

WWDC Bot
APP  
@Harry L (Apple)
 answered:
Yes! There are two new APIs you should check out.
scrollTransition():
https://developer.apple.com/documentation/swiftui/view/scrolltransition(_:axis:transition:)
visualEffect():
https://developer.apple.com/documentation/swiftui/view/visualeffect(_:)

----

@Fredric Michael
 asked:
Hi,
Any advice on how to get matchedGeometryEffect to work with View content that is inside a NSScrollView implemented by NSViewRepresentable?
Thanks
3 replies

WWDC Bot
APP  
@Tim D (Apple)
 answered:
As you've found, `matchedGeometryEffect` does have limitations when SwiftUI views are split between different hosting views/controllers. We would really appreciate a Feedback with specifics of your use case to help prioritize improvements here.

----

Kiril
 asked:
I was trying to customize the colors inside the Picker, but it seems this is currently not possible. For example with the code below I see all items as black and white, not colorized (and as you see  tried different color properties - none of them helped):
```
enum Flavor: String, CaseIterable, Identifiable {
    case chocolate, vanilla, strawberry
    var id: Self { self }
    var color: Color {
        switch self {
        case .chocolate:
            return .brown
        case .strawberry:
            return .red
        case .vanilla:
            return .yellow
        }
    }
}


struct TestView: View {
     
    @State var selectedFlavor: Flavor = .chocolate
    
    var body: some View {
        List {
            Picker("Flavor", selection: $selectedFlavor) {
                ForEach(Flavor.allCases) { flavor in
                    flavorView(flavor)
                }
            }
        }
    }
    
    func flavorView(_ flavor: Flavor) -> some View {
        HStack {
            Text(flavor.rawValue.capitalized)
                .foregroundColor(flavor.color)
            Spacer()
            Image(systemName: "birthday.cake.fill")
                .tint(flavor.color)
                .foregroundColor(flavor.color)
                .accentColor(flavor.color)
        }
    }
}
```
Is this by design or a bug? If by design, what would you recommend as an alternative UI element for this kind of situation (i.e. when we need to provide a single item selection, but also have items use images / text with colors)
Thanks in advance.
See less
2 replies

WWDC Bot
APP  
@Jason S (Apple)
 answered:
This is a known limitation, but we would appreciate feedback via Feedback Assistant. As a workaround, you can use ImageRenderer to create a rasterized version of your Image with the tint applied, and use that in your labels.
```
@MainActor
func image(for flavor: Flavor) -> Image? {
    let renderer = ImageRenderer(content: Image(systemName: "birthday.cake.fill").foregroundStyle(flavor.color))
    guard let cgImage = renderer.cgImage else { return nil }
    return Image(decorative: cgImage, scale: renderer.scale)
}

@MainActor
func flavorView(_ flavor: Flavor) -> some View {
    HStack {
        Text(flavor.rawValue.capitalized)
            .foregroundColor(flavor.color)
        Spacer()
        if let image = image(for: flavor) {
            image
        }
    }
}
```

----

@Axel
 asked:
How can we use BGProcessingTask with SwiftUI? For BGAppRefreshTask, we can use the viewModifier `backgroundTask(_:action:)` on a Scene, but it does not seem to support long processing task.
1 reply

WWDC Bot
APP  
@John G (Apple)
 answered:
Similar to the BackgroundTask framework's BGTaskSchedule, registering for a background processing app refresh task using the SwiftUI background task SceneModifier is the same as registering for a plain app refresh task. For both, using the .appRefresh("Identifier") task type as the property to the `.backgroundTask` modifier will give you your desired callbacks and runtime.

----

@Axel
 asked:
In UIKit, we have access to semantic colors for standard and grouped content background colors (like systemBackground). These colors are not in SwiftUI and we need to use the uiColor initializer in Color to get these colors. Is that intentional?
1 reply

WWDC Bot
APP  
@Daniel D (Apple)
 answered:
The `.background()` view modifier gives your view the appropriate default background style. If you are missing other semantic colors, please file feedbacks with your use cases.

----

@Tien
 asked:
Question for PreferenceKey: does reduce get called with default value? According to this article https://www.fivestars.blog/articles/preferencekey-reduce/, it shouldn't. But I notice often that my reduce method is triggered with a default value of 0 even though none of my views set the preference key to 0.
See less
FIVE STARSFIVE STARS
How SwiftUI's Preference Keys are propagated | FIVE STARS
Demystifiying how PreferenceKey's reduce method works (39 kB)
https://www.fivestars.blog/articles/preferencekey-reduce/

1 reply

WWDC Bot
APP  
@Anil K (Apple)
 answered:
The reducer does not get called with the default value, but there could be another view in your app that's setting the preference to 0.

----

@Axel
 asked:
How can we create a HUD in SwiftUI, that should appear above the top View (root stack or modal etc.). In UIKit, the solution was to use a UIWindow on top of the main app UIWindow.
1 reply

WWDC Bot
APP  
@Curt C (Apple)
 answered:
An `overlay` anchored to the root view is the usual approach in SwiftUI. You might also experiment with the `zIndex(_:)` modifier.

----

@Jonathan
 asked:
Thanks for all the help so far :) Whats the best way the best way to move focus to the next textfield when the user taps the return key on the keyboard?
I use onSubmit like (with two textfields in a VStack for email and password and corresponding FocusState)
```
onSubmit {
  if focus == .email {
     focus = .password
  else if focus == .password {
    // do the login
  }
}
```
But this causes the keyboard and the UI (that has moved due to keyboard avoidance) to jump down and up. It's quite jarring for the user. I guess the textfield has already started dismissing the keyboard before onSubmit is called.
See less
2 replies

WWDC Bot
APP  
@Cody B (Apple)
 answered:
You're using the API correctly, and are seeing a bug with how we manage the keyboard. We're tracking the issue already internally.

----

@Elijah
 asked:
How does SwiftUI locate metal shaders in a project, and how might you debug a shader not being applied to a view?
1 reply

WWDC Bot
APP  
@Haotian Z (Apple)
 answered:
Hi! Xcode compiles `.metal` files into a default library called `default.metallib`. Though if you want to customize the process of SwiftUI looking up for a shader, you could use a custom ShaderLibrary using its constructors that accept `Data` / `URL` types. As for debugging the shader not being displayed, I would suggest first make a simple shader that only displays a simple color - make sure it shows up first. It could be that the signature does not match with what `colorEffect`, `distortionEffect`, or `layerEffect` requires, so please check with documentation.

----

@Valentin
 asked:
SwiftUI shaders are mind blowing stuff. I've been working with MTKViews to achieve similar visual effects in the past. One thing I haven't been able to achieve that way with SwiftUI was to create effects that work on the background of a view, for example to create a custom backdrop (gradient) blur effect. Do these new shader-related APIs support that use case?
See less
1 reply

WWDC Bot
APP  
@Jeff R (Apple)
 answered:
Hi - thanks for the question. We don't believe this particular use case is supported by the new Shader APIs, but we'd love a feedback with these details, to help inform future updates to the API. Thanks!

----

@Aviel
 asked:
I have 2 questions about offset: Say I have a button with `offset(y: 200)`
1. Does the touch area of the button moves along with the visual effect of moving the button?
2. Does the offset somehow affecting the layout of the parent view?

WWDC Bot
APP  
@Tim D (Apple)
 answered:
1. Yes, the touch area does move to match what you see on screen.
2. No, the offset effect is layout neutral and only impacts the presentation of the view. This also explains why `offset` is available through the new `.visualEffect` modifier, which only supports effects that are layout neutral.

----

@Valentin
 asked:
The new layerEffect modifier requires a parameter of type SwiftUI::Layer for its Metal function. However, I'm not sure how to import the SwiftUI namespace in my Metal files. Is there an #include <SwiftUI/metal> directive I should use? I couldn't find any mention of this in the documentation.
2 replies

WWDC Bot
APP  
@Haotian Z (Apple)
 answered:
Hi! Please add this line #include <SwiftUI/SwiftUI.h> at the top of your Metal files to have proper reference to `SwiftUI::Layer`. Thanks!

----

@Juan Andres
 asked:
Does ToolbarItem support wrapping up a Menu object instead of a plain Button? I’m getting mixed results when used in a DocumentGroup app on SwiftUI.
3 replies

WWDC Bot
APP  
@Harry L (Apple)
 answered:
You can put any kind of view inside of a toolbar item. Could you file a feedback detailing the bug you are seeing?

----

@Zhiwen
 asked:
I would like to integrate SwiftData with my SwiftUI app. I would like to ask that will `.modelContainer(for: [Class.self])` init brand new database instant every time? Or it can detect the database creation status automatically? What if I have multiple modelContainer mods with the same instance class? I am wondering this because I my mind, the database should be initialised by a dedicated code, and I want to know that why use the modifier methods to create database structure.
See less




2 replies

WWDC Bot
APP  
@Curt C (Apple)
 answered:
The overloads taking model types are conveniences for quickly adding a container.
If you'd like to take full control over instantiating containers, there is an overload that takes a ModelContainer instance instead.


Matt R (Apple)
  
As Curt said, the modifiers are a convenience and you can create `ModelContainer` instances directly for full control. A few more answers to your questions:
* The modifier will not create a new database every time, it will connect to the same persistent backing store.
* If you use multiple `modelContainer()` modifiers with the same configuration, that's equivalent to creating and using multiple `ModelContainer` instances with the same configuration: they will resolve to the same, shared, persistent backing store, but will vend different model contexts and use separate in-memory caching state.
One example of where `modelContainer()` is helpful even in apps that create their own containers is in `#Preview` code, where you can conveniently configure a preview to have it's own, isolated, in-memory container.

----

@William
 asked:
I’ve been having trouble with a common animation scenario and wondering if there’s a way to solve this in Xcode 15. The scenario is that I’m animating a container view onto the screen using a .move transition while simultaneously downloading the container’s data from the network. After the data is download, we display it in the container using an .opacity transition. The problem occurs when the data’s transition starts before the container’s transition is completed. One would expect the data to be moving along with the container while it fades in. However, the data fades in with its final geometry, completely disconnected from the container’s moving geometry.
We could wait to display the data until the animation is completed, but the app feels so much more responsive when the data appears as soon as possible. Is there a way to accomplish this in SwiftUI? I have feedback with a sample playground – FB12260978


Tim D (Apple)
  
Hi William – this occurs because animations occur at the level of the children within the container by default. When a new child appears mid-transition, it comes in at its destination position.
As a workaround, you can apply the transform(.identity) modifier to the container, which will cause layout animations to occur at the level of the container. This should give you the behavior that you are after.


William
  
The playground code

``` 
import Combine
import SwiftUI
import PlaygroundSupport
​
// This view illustrates an animation bug by simulating the
// presentation of a dialog-type view that displays an async message.
// The bug occurs when the async message's transition begins while the dialog's
// transition is still in progress. The message should move with the dialog and
// fade in. However, what actually happens is the message fades in at the final
// position, not moving with the dialog at all.
struct DialogPresentingView: View {
​
    /// Set to `true` to present dialog
    @State var isPresented = false
​
    /// Async message to be displayed in the dialog
    @State var asyncMessage: String?
​
    var body: some View {
        ZStack {
            // A button that presents the dialog and sets the async message.
            VStack {
                Button("Present Dialog") {
                    isPresented.toggle()
                    DispatchQueue.main.asyncAfter(deadline: .now() + 0.1) {
                        asyncMessage = "WORLD"
                    }
                }
                .padding(40)
                Spacer()
            }
            // Present a dialog that loads and displays data
            if isPresented {
                VStack(spacing: 0) {
                    // Static content
                    Text("HELLO")
                        .background(.white)
                    // Async message
                    if let asyncMessage {
                        Text(asyncMessage)
                            .background(.yellow)
                            .transition(.opacity)
                    }
                }
                .frame(width: 200, height: 200)
                .background(.black.opacity(0.1))
                .transition(.move(edge: .bottom))
            }
        }
        .frame(width: 400, height: 400)
        .animation(.easeInOut(duration: 2), value: isPresented)
        .animation(.easeInOut(duration: 2), value: asyncMessage != nil)
    }
}
​
PlaygroundPage.current.setLiveView(DialogPresentingView())
​
```


William
  
@Tim D (Apple)
 did you mean transformEffect(.identity)? I couldn’t find a transform(.identity). I tried the former and it almost worked, but the views within the container still move around within the container. (edited) 


William
  
With transformEffect(.identity)

``` 
import Combine
import SwiftUI
import PlaygroundSupport
​
// This view illustrates an animation bug by simulating the
// presentation of a dialog-type view that displays an async message.
// The bug occurs when the async message's transition begins while the dialog's
// transition is still in progress. The message should move with the dialog and
// fade in. However, what actually happens is the message fades in at the final
// position, not moving with the dialog at all.
struct DialogPresentingView: View {
​
    /// Set to `true` to present dialog
    @State var isPresented = false
​
    /// Async message to be displayed in the dialog
    @State var asyncMessage: String?
​
    var body: some View {
        ZStack {
            // A button that presents the dialog and sets the async message.
            VStack {
                Button("Present Dialog") {
                    isPresented.toggle()
                    DispatchQueue.main.asyncAfter(deadline: .now() + 0.1) {
                        asyncMessage = "WORLD"
                    }
                }
                .padding(40)
                Spacer()
            }
            // Present a dialog that loads and displays data
            if isPresented {
                VStack(spacing: 0) {
                    // Static content
                    Text("HELLO")
                        .background(.white)
                    // Async message
                    if let asyncMessage {
                        Text(asyncMessage)
                            .background(.yellow)
                            .transition(.opacity)
                    }
                }
                .transformEffect(.identity)
                .frame(width: 200, height: 200)
                .background(.black.opacity(0.1))
                .transition(.move(edge: .bottom))
            }
        }
        .frame(width: 400, height: 400)
        .animation(.easeInOut(duration: 2), value: isPresented)
        .animation(.easeInOut(duration: 2), value: asyncMessage != nil)
    }
}
​
PlaygroundPage.current.setLiveView(DialogPresentingView())
​
```

Tim D (Apple)
  
Yes, `transformEffect`. It looks like you just need to put that modifier a bit further down: it works when I put it here:
```
.background(.black.opacity(0.1))
.transformEffect(.identity)
.transition(.move(edge: .bottom))
```

This causes layout animations to happen at the outermost level (the visual moving box)

----

@Jiayou
 asked:
How can I make @Observable work with the UIViewReprestable API? Thanks, again!
The following UILabel does not update as textObject changes:
```
@Observable
class TextObject {
    var text = ""
}

struct UIText: UIViewRepresentable {

    var textObject: TextObject

    func makeUIView(context: Context) -> UILabel {
        UILabel()
    }

    func updateUIView(_ uiView: UILabel, context: Context) {
        uiView.text = textObject.text
    }
}

struct ContentView: View {

    @Bindable var textObject = TextObject()

    var body: some View {
        VStack {
            UIText(textObject: textObject)
            TextField("placeholder", text: $textObject.text)
        }
    }
}
```
:+1:
1

1 reply

WWDC Bot
APP  
@Matt R (Apple)
 answered:
Hi Jiayou, thanks for asking about this — this is currently a known issue.

----

@Charles
 asked:
Is there any app delegate behavior that we should/or can move to App? App delegate behavior seems tacked on in SwiftUI (for example @NSApplicationDelegateAdaptor). Or are app delegates here to stay? Also, thanks for being here!
9 replies

WWDC Bot
APP  
@Jeff R (Apple)
 answered:
Hi - thanks for your question. Is there anything specific you had in mind? We do have some scene level modifiers for some of what app delegates do currently.


Charles
  
@Jeff R (Apple)
 state restoration and push notification handling immediately come to mind.


Jeff R (Apple)
  
Ah, ok. For state restoration, is there something beyond what @SceneStorage offers that you are looking for? I'll also say that we'd appreciate any feedbacks in this area as well as the app delegate functionality.


Charles
  
@Jeff R (Apple)
 TIL about SceneStorage; is this about restoring UI state?


Jeff R (Apple)
  
Yep. Its a dynamic property for use in your views.


Charles
  
@Jeff R (Apple)
 also thinking about drag and drop on macOS with application(application:open:)


Kyle
  
What about Notifications?


Charles
  
rn I just make AppDelegate an ObservableObject which seems brute force.


Jeff R (Apple)
  
We have some support for external events, via handlesExternalEvents, though I think there may still be some setup that needs to be done in the delegate (feedbacks welcome as always).

----

@Juan Andres
 asked:
Is it possible to separate buttons (like with a Divider()) in the ellipsis menu that builds automatically on toolbars with customizable content? I can’t find the right way to do it. Thx!
5 replies

WWDC Bot
APP  
@Harry L (Apple)
 answered:
Yes! You should be able to wrap your views in a ControlGroup which will render individual toolbar items but put them in a section in the overflow menu.

----

@Charles
 asked:
Is there a way to configure TextEditor to have controls for formatted text? At current I have to wrap NSTextView to get that.
1 reply

WWDC Bot
APP  
@Jason S (Apple)
 answered:
There's no support for formatting controls with TextEditor. But we'd appreciate your feedback detailing your expectations via the Feedback Assistant app.

----

@Rich
 asked:
I have a SwiftUI ScrollView with a VStack that has a lot of vertical content. Toward the bottom is a simple Map with no annoations. For whatever reason having the map in the hierarchy causes the first load of my ScrollView to be slow. I can resolve this by using a LazyVStack, but then I lose the abi…
See more
2 replies

WWDC Bot
APP  
@Harry L (Apple)
 answered:
If you have a lot of content to scroll vertically, you can improve scrolling / loading performance by using a LazyVStack. ScrollViewReader does support scrolling to offscreen views but the id of the view has to be visible to the stack.
```
LazyVStack {
  ForEach(items) { item in 
    // ...
  }
}
```
For example, you can scroll to views with an id of any of the items id's in the above example.

----

@Matthew
 asked:
Is it possible for the Table to present data in Sections?  I have a Bill Tracking/Budgeting app that organizes data for the user in sections like ‘Overdue’, ‘Pending’, ‘Next Pay Period’, etc., and I think a Table view would be excellent on the wider devices, but I don’t think there’s a way to maintain the Sectioned layout (FB12272254)
See less
4 replies

WWDC Bot
APP  
@Sam L (Apple)
 answered:
Hi! If you use the rows parameter of the Table initializer to build your content, you can create sectioned rows!
An example would look something like:
```
Table(for: Person.self) {
    TableColumn("Given Name", value: \.givenName)
    TableColumn("Family Name", value: \.familyName)
} rows: {
    Section("Favorites") {
        TableRow(sam)
        TableRow(matthew)
    }
    Section("Contacts") {
        ForEach(contacts) { person in
            TableRow(person)
        }
    }
}
```

----

@Chris
 asked:
In the docs there's an issue about views not updating when a dependency is inside a content closure. See also this post: https://m.objc.io/@lickability@mastodon.social/110503839533962620. It seems to work fine though. Are the docs outdated or is this behavior going to work as described in the docs?
MastodonMastodon
Lickability (@lickability@mastodon.social)
Attached: 1 image
:brain: The new Observable type greatly simplifies data flow and improves performance in SwiftUI, and our engineers are so excited to use it! #WWDC23
However, there’s a critical gotcha that Apple mentions briefly in their docs—if you use SwiftUI, you’ll NEED to know this. #iOSDevTips
https://developer.apple.com/documentation/swiftui/managing-model-data-in-your-app
Jun 7th
https://m.objc.io/@lickability@mastodon.social/110503839533962620

3 replies

WWDC Bot
APP  
@Matt R (Apple)
 answered:
Hi Chris. Those views will indeed update correctly. We will be updating the documentation to reflect that soon.
:raised_hands:
7
:100:

Chris
  
Thank you, that's great news. The new APIs are then very easy to understand and explain indeed!

Matt R (Apple)
  
To be more specific, in that case the view will behave as if you had factored out that BookView yourself, but automatically, giving you the best performance by default.
Specifically, the List's closure is an escaping closure, so not executed within LibraryView's body property. This means LibraryView will not form any dependencies to book properties in that example. Instead, SwiftUI runs the list content closure internally when creating internal wrapper views for each individual list row, so those list rows will form their own independent dependencies to their respective book.title properties.
This means that if a specific book's title updates, the only part of the UI that will be reevaluated for that change will be the specific row view that displays that book, not the whole list! (edited) 

----

@Austen
 asked:
What kind of architecture does the team use/prefer? MVVM or other? Is SwiftUI designed to work with any specific architecture?
:eyes:
1
:man-facepalming:
1

27 replies

WWDC Bot
APP  
@Kyle M (Apple)
 answered:
SwiftUI is designed to work with a variety of architectures. Different members of the team use different architectures. I've recently enjoyed using SwiftData to build the model layer in my hobby apps.


Mohammad
  
But, did you have any kind of considerations for any specific arch or, did you have any specific arch in mind when designing SwiftUI features?
UIKit had MVC in mind, that's why this question is important to me. And Furthermore, I have seen a few articles about how MVVM can be counterproductive to the core concepts of SwiftUI. (edited) 


Mohammad
  
Any thoughts 
@Kyle M (Apple)
?


Kyle M (Apple)
  
There are a lot of opinions about app architecture, so we want to support as wide a variety as is reasonable. We don't want someone with a strong opinion about app architecture to feel like they can't use SwiftUI and have to revert to using a non-native UI toolkit.


Kyle M (Apple)
  
At the same time we want to provide a turnkey solution for people that, among other things, makes it as easy as possible to get started. That's why we're so happy that we're finally able to share SwiftData (edited) 

----

@Kristjan
 asked:
Does the Swift charts team plan to support building custom chart views? Also what is the performance of swift charts when rendering data at scale?
29 replies

WWDC Bot
APP  
@Robert M (Apple)
 answered:
What exactly do you refer to with "custom chart views"? Please explain or link.
Regarding performance, it's best to do a preliminary testing with the approximate quantity of data, and the type of interaction and device floor you plan to support, before iterating a lot on the design.


Kristjan
  
Hey, sorry I posted a new question thread because I originally couldn’t reply to this thread


Kristjan
  
This is our app, where you can take a look at the screenshots to understand what type of custom chart views we would want to make https://apps.apple.com/us/app/crm-analytics/id916982402
App StoreApp Store
‎CRM Analytics
‎CRM Analytics lets Salesforce users take their data with them everywhere. CRM Analytics transforms the way your company uses data, making every employee more productive so you can grow your business faster. And with the CRM Analytics app, any Sales Cloud or Service Cloud user can instantly get relev…
https://apps.apple.com/us/app/crm-analytics/id916982402



Kristjan
  
@Robert M (Apple)


Kristjan
  
The current charts are driven mostly by metadata, where you give an array of data elements and configure some visual elements like color, font, etc. but afaik, there’s no way for us to do rendering ourselves where we can draw a bar at position or I’m not sure if you can the corner rounding bar for bar rec stuff


Kristjan
  
Or adjust the axis and legend designs and layout


Kristjan
  
Taking a look at our screenshots, is that something that can be achieved with current swift charts, that is our product design system


Robert M (Apple)
  
For example, you can use the RectangleMark to render rectangles, and you can position them arbitrarily. You can also directly specify the data domain you currently want to visualize. RectangleMark supports corner rounding. You can also experiment with, for example, the horizontal positioning of vertical bars, using BarMark


Kristjan
  
Ok will look at that thanks


Kristjan
  
Would something like a Sankey chart or ratings chart be achievable?


Kristjan
  
IMG_4283
 
IMG_4283


Kristjan
  
IMG_4284
 
IMG_4284


Kristjan
  
@Robert M (Apple)


Robert M (Apple)
  
Regarding the dashboard question: you can use SwiftUI to create a layout of your choice that accommodates multiple charts. Each chart would be a Chart view instance.


Robert M (Apple)
  
I don't think it's easy to create a Sankey chart with current Swift Charts. While you may be able to place RectangleMark and smooth, interpolated LineMark to achieve, or at least approximate the look of a Sankey, you'd be on your own, computing the exact position of the rectangles and line starts/ends. Most Sankey implementations use the Sugiyama approach for an iterative layout generation (layered graph drawing)


Kristjan
  
Thanks, and is there clipping support? Where inside a chart view we would want a clip so shapes beyond the clipped area won’t be visible until the user scrolls through them


Kristjan
  
And do you support Bézier curves?


Kristjan
  
which is what we’ll need for sankeys


Robert M (Apple)
  
You can use RectangleMark(...).clipShape(...)
:+1:
1



Robert M (Apple)
  
It uses AnyShapehttps://developer.apple.com/documentation/swiftui/anyshape?changes=_1
Apple Developer DocumentationApple Developer Documentation
AnyShape | Apple Developer Documentation
A type-erased shape value. (22 kB)
https://developer.apple.com/documentation/swiftui/anyshape?changes=_1



Kristjan
  
And for the ratings chart image?


Kristjan
  
I would assume same issue?


Robert M (Apple)
  
What's the Ratings chart image? I see a "New Ratings Dashboard" with three panes, each of them has a lot of stars in them, so I don't understand the context or the question


Kristjan
  
Right, would something this be supported?
IMG_4285
 
IMG_4285




Kristjan
  
asking about the chart itself, not the dashboard container


Kristjan
  
and just so I understand you linked ‘AnyShape’ as a response to Bézier curves right?


Robert M (Apple)
  
Ah OK now I see real content in the image (five circles, stars etc. with full or split color according to some value). There is no direct support for this chart type. Since you'd need to compute the clipping and the shapes, you may as well use general SwiftUI, because Swift Charts doesn't add much to the solution in this case
:+1:
1



Robert M (Apple)
  
Yes, AnyShape is what you supply to .clipShape


Kristjan
  
Cool thank so much for your help Robert. I appreciate your patience and expertise!

----

@Austen
 asked:
What's the proper way of implementing multi-select in a List with value-based navigation?
4 replies

WWDC Bot
APP  
@Curt C (Apple)
 answered:
In a NavigationSplitView you can put a List in the sidebar. Bind a Set as the selection to get multi-select. Then make the root of the detail column use that selection to decide what to show.

When the NavigationSplitView collapses to a single column, as on iPhone or iPad in Slide Over, it will push based on single selection. That’s the platform convention. You can add an EditButton to your toolbar to enable multi-select in that case.


Ratnesh
  
Is there a way to collapse the NavigationSplitView’s sidebar to TabBarView?
I.e. on compactSizeClass it can be in TabBar and in regular it can be in Side bar?


Curt C (Apple)
  
You can use the horizontalSizeClass environment value to switch between a NavigationSplitView in regular width and TabView in compact width.

----

@Anton
 asked:
Is there a way to always show the sidebar and remove the collapse button in the NavigationSplitView in macOS?
Just like in the Apple Music app
2 replies

WWDC Bot
APP  
@Nick T (Apple)
 answered:
There have been many requests for this feature in this year's Slack channels. We appreciate the desire for this feature. There is no way to do this at present. But the requests have been heard and are very useful!
:+1:

Cristian-Mihai
  
I think a workaround could be to hide the toolbar using the .toolbar() modifier:
```
struct SomeView: View {
  var body: some View {
    NavigationSplitView(
      columnVisibility: .constant(.all),
      sidebar: { 
          Text("Sidebar")
               .toolbar(.hidden, for: .navigationBar)
      },
      detail: { Text("Detail") }
    )
    .navigationSplitViewStyle(.balanced)
  }
}
```

----

@Andrew
 asked:
Can the new Metal shaders API for SwiftUI be used with SwiftCharts?
1 reply

WWDC Bot
APP  
@Anil K (Apple)
 answered:
The foregroundStyle modifier on ChartContent accepts anything that conforms to ShapeStyle, so you should be able to provide a Shader.

----

@Matthew
 asked:
For Pie Charts is it possible for a segment to have additional dimensions of data?  For example, if your Segments are primarily based around expense categories (‘Home’ expenses, ‘Auto’ expenses, etc), could each of the pie segment be further split into ‘Overdue’, ‘Due This week’, ‘Due later’, etc?
5 replies

WWDC Bot
APP  
@Robert M (Apple)
 answered:
How would the second layer of breakdown (partitioning) work in your specific case? Can you describe how it'd look? Do you refer to sunburst charts?


Matthew
  
I think each segment would be broken into multiple colors -- for example, the outer portion representing Overdue bills would be colored red, the next section in, representing Paid, but Uncleared bills would be colored yellow, and the internal portion of the segment, representing future bills would be colored green.  I use this today with the BarMark, and it works well -- I believe it could also work well in a Pie Chart.


Matthew
  
I'm not familiar with a sunburst chart - is that something SwiftUI supports?


Robert M (Apple)
  
You could indeed do it with coloring, which you described, because you can use foregroundStyle to specify colors directly. But bear in mind that it's generally not good practice to show a lot of sectors, because it's then next to impossible for the viewer of your charts to associate the sectors with the meaning (category).
We don't have direct support for sunburst charts. In theory, two charts with different innerRadius / outerRadius can be stacked, but it may or may not work out for you, for example, legends won't work.
Consider a main pie chart for the top level, and a list/grid of pie charts for the breakdown of each category, or let users interact with the main pie chart, where a tap would break down the tapped category in a secondary pie chart (or other visualization)

----

@Casey
 asked:
I'm overjoyed to see the addition of `isPresenting: @Binding` to `.searchable()`. Is there any way to back-deploy that (or fake it) for iOS 16? The hacks I'm doing right now are... :nauseated_face:
:squirrel:
1
:heart:
1

2 replies

WWDC Bot
APP  
@Harry L (Apple)
 answered:
No, its implementation uses new logic that can't be back deployed.

----

@Jack
 asked:
When working with @ObservableObjects, you can declare @Published var properties with initial values specified in the initialiser. Is it possible to achieve the same using the @Observable macro? It seems to result in compiler errors requiring an initial value:
```
// Compiler Error
@Observable final class Foo {
    var bar: String // @Observable requires property 'bar' to have an initial value (from macro 'Observable')

    init(bar: String = "") {
        self.bar = bar
    }
}

// No compiler Error
@Observable final class Foo {
    var bar: String = "" // <- The initial value is required here

    init(bar: String) {
        self.bar = bar
    }
}
```
1 reply

WWDC Bot
APP  
@Luca B (Apple)
 answered:
This is actively discussed in the Swift community: https://forums.swift.org/t/pitch-init-accessors/64881


----

@Naftali
 asked:
When creating a label if I want 2 lines of text and the icon centered to the text, I couldn’t find a way to do it without making a custom label, is there a better way to do it? (You can see that Apple does it settings for example in Settings > Notifications for each app it has the app name and un…
See more
5 replies

WWDC Bot
APP  
@Anil K (Apple)
 answered:
You can write a custom LabelStyle implementation that places the icon and content in a center-aligned HStack.


Naftali
  
So does that mean the labels in settings is done with UIKit?


Anil K (Apple)
  
There's no need to use UIKit, you can write your own label style that center-aligns the labels components:
```
struct MyLabelStyle: LabelStyle {
    func makeBody(configuration: Configuration) -> some View {
        HStack(alignment: .center) {
            configuration.icon
            configuration.title
        }
    }
}
``` 


Naftali
  
Yes but that causes it to lose all default styling
Screenshot 2023-06-08 at 1.30.53 PM.png


Anil K (Apple)
  
You can apply the same tint color in your own style, along with the image scale and size to match the appearance

----

@Nayan
 asked:
I've only seen `@Observable` macro used with classes. Can it be used with structs or does this not result in anything?
For example, changing a property in a struct will cause the whole struct to change, therefore reloading any views that are dependent on the object, regardless of what properties it rea…
See more
2 replies

WWDC Bot
APP  
@Luca B (Apple)
 answered:
The `@Observable` macro is only used for reference type.

To be more precise you should only use `@Observable` on classes.

----

@Casey
 asked:
I am working on an iOS app that shows user ratings for movies/TV shows/etc. It is a perfect use case for `CircularProgressViewStyle`, but that isn't really supported on iOS. I know you can't speak about future plans, but could you give any insight why that is? Or am I holding it wrong? For what it's worth, FB12272960.
See less
2 replies

WWDC Bot
APP  
@Matt R (Apple)
 answered:
Hi Casey, thanks for submitting that feature request! You are correct that `CircularProgressViewStyle` does not currently support showing determinate progress in iOS apps, only indeterminate progress. I can't speak to future plans but this is definitely a request we've received before and that we are tracking.

----

@Miguel
 asked:
Hi! :)
I have an `@Observable` class from which I use multiple properties in my views. In the same class, I'm also updating multiple variables that I don't use in any View. Should I somehow decouple the architecture or is this OK?
The variables I'm updating come from some motion data (like gyroscope), so it's really a lot of updates, however I'm just updating the variables that my Views use once in a while.
Thank you :)
See less
2 replies

WWDC Bot
APP  
@Daniel D (Apple)
 answered:
Variables not displayed in view bodies don't cause view view rendering. Your approach of selectively updating view-rendered variables sounds great!

----

@Juan Andres
 asked:
I just watched the video on building better Document-based apps, and I'm curious to know if the "Document-picker presentation" feature is available on SwiftUI and how to accomplish it... I've tried changing the Info.plist as in the video but I'm not sure I'm doing it right.


WWDC Bot
APP  
@Julia V (Apple)
 answered:
The same presentation is available both for document and non-document applications. Check out https://developer.apple.com/documentation/appintents/shortcutslink/fileimporter(ispresented:allowedcontenttypes:allowsmultipleselection:oncompletion:oncancellation:) and other fileImporter variants.
Does thi…
See more
Apple Developer DocumentationApple Developer Documentation
fileImporter(isPresented:allowedContentTypes:allowsMultipleSelection:onCompletion:onCancellation:) | Apple Developer Documentation
Presents a system dialog for allowing the user to import multiple files. (22 kB)
https://developer.apple.com/documentation/appintents/shortcutslink/fileimporter(ispresented:allowedcontenttypes:allowsmultipleselection:oncompletion:oncancellation:)


Juan Andres
  
Oh, not really. Actually what I’m looking for is to have an EmptyState instead of a Document Browser shown when the user first launches my Document-based app on iPadOS… In that way I’m able to provide Onboarding and perhaps access to general app settings. I filed a feedback for this kind of possibility earlier this year: FB11983271


Julia V (Apple)
  
I see. Thanks for filing the feedback!
:slightly_smiling_face:


Juan Andres
  
So perhaps it’s a UIKit feature only, right? Since it refers to the UIDocumentClass…


Michael O (Apple)
  
Presenting a document group with no document is not supported right now. Thanks for filing the feedback though!


Juan Andres
  
Thanks Michael!


Michael O (Apple)
  
Also maybe worth pointing out: the nil-document makes the most sense when you can not use a document browser as the root view controller of your app for some reason. That’s what this mode is for. Ideally, according to the HIG, if your app is document based, you should have a document browser on the root level. So what SwiftUI provides is really the expected set up for a document based app.


Juan Andres
  
Oh, I see… and then Onboarding should come after the user opens / creates the first document?


Michael O (Apple)
  
I guess that depends a bit on your onboarding. You could also first have full screen onboarding screens like when you set up an iPhone for the first time, and then switch to the document group.


Juan Andres
  
But then not in SwiftUI, right? I can’t mix a WindowGroup with a DocumentGroup…


Michael O (Apple)
  
oh that is something someone else might want to chime in on. I’m on UIKit, I only worked on the document view controller and integrating it with DocumentGroup.
:+1:

Julia V (Apple)
  
I can’t mix a WindowGroup with a DocumentGroup…
Sure, you can! This is supported
:astonished:


Juan Andres
  
Oh, is that new or have I missed it? Are there any sample projects combining both WindowGroup and DocumentGroup I could look into?


Juan Andres
  
I mean how do I move from a WindowGroup into a DocumentGroup for instance?


Julia V (Apple)
  
When the app launches, you get DocumentGroup behavior (open dialog). To open a window from a WindowGroup, assign an id to a window and call openWindow(id:) as shown here: https://developer.apple.com/documentation/swiftui/environmentvalues/openwindow 

----

@Ratnesh
 asked:
Basic SwiftUI question:
How to programming invoke refresh control inside a list otherwise shown using refreshable modifier?
1 reply

WWDC Bot
APP  
@Luca B (Apple)
 answered:
This is currently not possible but it would be a great feedback to file.

----

@Axel
 asked:
Thanks for the amazing work and for taking so much time to answer our questions!
My question is about view modifiers added to items in lazy containers: should sheet/confirmationDialog/alert modifiers be directly added to items in a lazy container (List or ScrollView + LazyVStack) or added to the lazy container itself? For example, the swipeActions are directly added to the rows, and not to the List, which make me question about performance issues.
Use case: tap a row to edit an item in a modal sheet. We can imagine having a subview for the row that contains the sheet to present, or to attach the sheet to the List/ScrollView and pass the action to present the sheet to the row.
Is there any performance impact in replicating these modifiers for every row?


WWDC Bot
APP  
@Curt C (Apple)
 answered:
Thanks for the question!
In general it's best to apply the presentation modifier outside the lazy container.
If you apply it to an item in the container it won't trigger when the item is scroll out of view. Worse, it may trigger just because the user scrolled the item into view, depending on the associated state.

Interestingly, this issue is why we recommend that developers apply `navigationDestination` outside a lazy container, and why we deprecated `NavigationLink(isActive:destination:)` last year.

----

@Klayton
 asked:
Sorry if this is already being addressed.  Somebody named Eric seems to have somehow added a question to the 2022 WWDC channel when this Q&A opened:
What is the best way to scroll/focus an item in a horizontal list on tvOS after online data loading? In my code, it doesn't work 100% of the time and…
See more
1 reply

WWDC Bot
APP  
@Parry P (Apple)
 answered:
Depending on whether you're using a lazy layout or not the solution could be different.
to change focus to a specific view you should use FocusState. This will work for non-lazy layouts, like HStack, etc.
you will have to scroll the view to be focused to visible first though if you're using a lazy layout, like LazyHStack, etc.

----

@Łukasz
 asked:
Could you shed some light how Apple team achieved the background "area" mark that connect each sleep stage health's app sleep chart? I mean this one:
https://support.apple.com/library/content/dam/edam/applecare/images/en_US/Health/ios-16-iphone-13-pro-health-browse-sleep-history.png


2 replies

WWDC Bot
APP  
@Richard W (Apple)
 answered:
This is not possible with Swift Charts today. Feel free to file Feedback with your use case!

----

@Jacob
 asked:
An scenario I’ve always found awkward and would like some assistance with in SwiftUI.
Assume I have some header text centered horizontally on the screen. But now I want to add a button or something else to the leading edge of the screen but I don’t want it to shift the header text off center.
I know I could use a ZStack so that they don’t affect each other but then if the header text gets long it can overlap and go under or over the leading button.
I know I could also put a translucent view on the trailing edge with the same size as the leading button but that feels hacky, and also prevents that trailing space from being used by the header if it gets long.
The best I’ve come up with so far is to wrap the leading button in a frame with infinite max width and leading alignment, put a spacer on the other side of the header also with infinite max width, and then give the header text layoutPriority(1). This seems to be the most robust and handles long and short headers and keeps it centered when it can be, but it also feels a little janky.
Just curious if there is a recommended way to handle this type of scenario. Maybe a custom Layout? But is there a more built-in way?
See less
:+1:

1 reply

WWDC Bot
APP  
@Jacob R (Apple)
 answered:
This feels like good place to use a custom layout since there's a bunch of special case rules (like using all remaining space when the header is long).

----

@Sam
 asked:
Can shaders be applied to views meant for interaction, such as a form? Such as a CRT effect shader applied to a form for the settings in a game.
2 replies

WWDC Bot
APP  
@Sam L (Apple)
 answered:
You can apply a shader in any place you can apply a `foregroundStyle` / `tint`! That said, certain controls which have a system-styled appearance may not respect the applied shader (a good example of this would be Toggle). To work around this, for controls that have a style protocol (such as ToggleStyle) if you implement a custom style fully in SwiftUI, it will support the shader!
So with some combination of custom control styles and the new shaders, you should be able to achieve something like this!

----

@Jack
 asked:
Is it possible to style a `.searchable` search bar without resorting to global `.appearance()` modifications?
1 reply

WWDC Bot
APP  
@Harry L (Apple)
 answered:
SwiftUI has no APIs to explicitly customize the appearance of the configured search field. I'd love a feedback detailing some of the things you'd like to customize though!

----

@Fredric Michael
 asked:
Hi,
Is it possible to use keyboardShortcut with a menu picker?
Thanks
:+1:

WWDC Bot
APP  
@Franck N (Apple)
 answered:
Hi Fredric. Unfortunately, keyboardShortcut is currently unsupported by menu pickers, but we’d appreciate a feedback request with more detail about why you’d like this capability!

----

@Axel
 asked:
I want to have an option in my app to change the tint of the whole app. What should I use? I tried to update the tint color in the root view but it is not working for places where I use the .accentColor (like a Text, a Shape, etc). Is there a way to update the .accentColor in the app or should I specify the color in every View that needs to use it?
See less
:+1:

WWDC Bot
APP  
@Max O (Apple)
 answered:
You can specify the accent color for the entire app as an asset in your Xcode project as documented here: https://developer.apple.com/documentation/xcode/specifying-your-apps-color-scheme/
Apple Developer DocumentationApple Developer Documentation
Specifying your app’s color scheme | Apple Developer Documentation
Set a global accent color for your app by using asset catalogs. (22 kB)
https://developer.apple.com/documentation/xcode/specifying-your-apps-color-scheme/


Axel
  
Thanks 
@Max O (Apple)
 but I want the user to have the choice to choose the app tint from the app settings. Let's say red, blue, green or indigo. And when the user chooses a color, I want to override the accentColor from the asset catalog by the color they want.


Naftali
  
I have done this in my app in the root view `.accentColor(someVariableThatIsAColor)` and it works

----

@Austin
 asked:
Do you have any tips on using OutlineGroup on macOS? I have performance issues when creating an OutlineGroup with nested data of more than a few hundred tree items. It seems like there is a cost for items even if they are collapsed.
:+1:

WWDC Bot
APP  
@Haotian Z (Apple)
 answered:
Hi Austin! Before going into details, I want to make sure if you were referring to OutlineGroup in List, or Table, or just OutlineGroup in a stack, and they have different implementation details so it is important to know which one we are talking. We have also published a video on optimizing perform…
See more
Apple DeveloperApple Developer
Demystify SwiftUI performance - WWDC23 - Videos - Apple Developer
Learn how you can build a mental model for performance in SwiftUI and write faster, more efficient code. We'll share some of the common... (19 kB)
https://developer.apple.com/videos/play/wwdc2023/10160/


Austin
  
Sorry, I was referring to an OutlineGroup contained in a List

The "Demystify SwiftUI performance" session looks great, I'll take a look at that. If you have anything specific about the OutlineGroup/List combo I'm having trouble with I'd still love to hear it too though!


Haotian Z (Apple)
  
Thanks for letting me know the details! I think the session covers most topics I could think of, and still, if after the video you still find OutlineGroup in a List to be not as fast as you want, we would appreciate a feedback with your use case and sample code so we could keep track of the issue!

----

@Abhishek
 asked:
Hej! I really love custom view controller transitions (via UIViewControllerAnimatedTransitioning) - how do I approach building these with SwiftUI? For example, I'm looking to add a shared element transition between a view and the sheet it presents. Thank you!
:eyes:
2 replies

WWDC Bot
APP  
@Curt C (Apple)
 answered:
Thanks for the question!
SwiftUI doesn't currently support the full power of `UIViewControllerAnimatedTransitioning`. I'd love a Feedback with your particular use case to help prioritize future work.

----

@Simon
 asked:
In the session on interactivity in Swift Charts you have an interactive chart that shows the sales the last 30 days so you set chartXVisibleDomain(3600 * 24 * 30) and snap to the first day of the month when swiping using the chartScrollTargetBehavior() modifier.
How would we go about showing all data for an entire month, no matter if that month is 28, 30, or 31 days? It seems that’s what the Health app does when scrolling the chart while viewing data for a month.
I’m thinking we can’t use chartXVisibleDomain(3600 * 24 * 30) since the domain is variable. So what can we do instead to replicate the behavior of the Health app?
See less
1 reply

WWDC Bot
APP  
@Robert M (Apple)
 answered:
Assuming a fixed screen range for the plot area (in points/pixels) and snapping to the beginning of the month, it is not possible to vary the number of bars within the visible plot area, because the bar count would vary between 28 and 31. So it'd require some kind of autozooming to adjust the "zoom magnification".  It is a bit questionable interaction-wise, because the user would scroll, and as the chart settles, it'd change the zoom level.
You could perhaps achieve what you want by using a 31-day window, and coloring months differently, or using gridlines at month boundaries

----

@Axel
 asked:
With SwiftData and Observation, what is the difference between @Bindable and @Binding? Should we only use @Bindable when we pass a @State to a subview that needs right access to the variable (like a Bool, Date, or String for simple types)?
:eyes:

WWDC Bot
APP  
@Matt R (Apple)
 answered:
Hi Axel! @Bindable is a utility to allow arbitrary @Observable objects to be able to create bindings to any of its properties.
You can store an @Observable object in @State, and create bindings to it using the state $ prefix operator syntax, without needing to use @Bindable at all.
However, there are some cases where you need to create a binding to an object's property, but you only have a reference to the object, and it's not appropriate to store it in @State. @Bindable is there to provide that capability.
See less
:pray:

Axel
  
Thanks 
@Matt R (Apple)
. So @Bindable works more or less like @Binding, and we should use @Bindable instead of @Binding, even for simple data types like a Bool? For example, a subView that contains a toggle, and the State is in the parent view. Should I use @Binding var isEnabled: Bool or @Bindable var isEnabled: Bool?

Bindable and Binding are separate concepts, serving different purposes:
* Binding is a read-write connection to a particular piece of data.
* Bindable is a utility for creating Bindings from @Observable object references, and does not apply to non-object data types.

Axel
  
Thanks 
@Matt R (Apple)

Matt R (Apple)
  
To compare to `ObservableObject`, SwiftUI has the `@ObservedObject` property wrapper to declaring dependencies to ObservableObject instances in your view. `@ObservedObject` also provides a way to get bindings to properties of those objects.
With `@Observable`, you no longer need to use a property wrapper at all for views to track changes to those object's properties. However, there is still a need to create bindings to object properties. `@Bindable` provides that specific capability for `@Observable` objects, replacing `@ObservedObject`.

----

@Landon
 asked:
What is the recommended way to have a view that updates when it's parent ScrollView's offset changes?
Two approaches we've tried are
* Grabbing the underlying UIScrollView/NSScrollView and reading contentOffset
* Putting a zero-height view at the top of the ScrollView and reading it's minY with a Geome…
See more
2 replies

WWDC Bot
APP  
@Harry L (Apple)
 answered:
It depends what you are trying to do. If you'd like to visually alter a view based on its relative position within the scroll view's visible region, you have a few new APIs available to you.
scrollTransition() good for common operations like opacity, rotation, offset. specific to ScrollView:
https://developer.apple.com/documentation/swiftui/view/scrolltransition(_:axis:transition:)
visualEffect() good for deeper customizations based on a GeometryProxy, useful in ScrollView and other contexts:
https://developer.apple.com/documentation/swiftui/view/visualeffect(_:)

----

@Kevin
 asked:
Is there a way to change the background color of the searchable modifier that is in navigationBar? I have my toolbarBackground set up as a color but my searchable blends.
2 replies

WWDC Bot
APP  
@Harry L (Apple)
 answered:
There are no APIs to configure the appearance of the search field configured by a searchable modifier but feedbacks detailing your use case would be appreciated!

----

@Fredric Michael
 asked:
Hi,
I’ve discovered a use case where the width of the sidebar is not persisted when quitting the app.
This seems to happen for both NavigationSplitView and NavigationView when the detail view has a frame set.
If the app is restarted via Xcode the width is persisted as expected.
This has been reported in FB12196768.
Is this a potential bug or am I doing something wrong?
Thanks
Michael
See less
13 replies

WWDC Bot
APP  
@Nick T (Apple)
 answered:
Hi Michael,
This is a bug. I've screened your feedback and we'll do our best to address this quickly.


Nick T (Apple)
  
While it may not fit your use case, as a temporary workaround, omitting the binding argument altogether allows proper persistence. (edited) 


Fredric Michael
  
@Nick T (Apple)
 Good to know. Which binding are you referring to? Will this be fixed in Ventura?


Nick T (Apple)
  
The `columnVisibility` argument to `NavigationSplitView`. Omit that and it behaves better. We can't provide that kind of information, sorry.


Fredric Michael
  
Understood. I'm seeing the same bug with NavigationView as well. There is no workaround for that one?


Nick T (Apple)
  
Unfortunately, no


Fredric Michael
  
I would love to use `NavigationSplitView` but I want the sidebar toggle button to appear in the sidebar toolbar instead.


Fredric Michael
  
Also I noticed that when using `NavigationSplitView` my `matchedGeometryEffect` stopped working. Any idea what change could cause that?


Nick T (Apple)
  
No, a feedback for that would be interesting too!


Fredric Michael
  
I actually created a demo app and the effect worked in that case. So there must be something else in my app setup that is hindering the effect when using NavigationSplitView. I will have to debug further...


Nick T (Apple)
  
There was a bug temporarily in Ventura where animations in `NavigationSplitView`  were almost entirely disabled


Fredric Michael
  
Ah, maybe that could be it. In which version was this?


Nick T (Apple)
  
13.3 IIRC. See here for more discussion: https://developer.apple.com/forums/thread/728132

----

@John
 asked:
AppKit apps have the ability to indicate that a window has been edited (by putting a dot in the close button and adding "— Edited" to the title), and also asking for confirmation if the user attempts to close the window while there are unsaved changes.
Is it possible to do this with SwiftUI and/or Ma…
See more
2 replies

WWDC Bot
APP  
@Jake S (Apple)
 answered:
That behavior can be achieved by using a DocumentGroup scene type in a SwiftUI app.
You can see more about that, including sample-code, here: https://developer.apple.com/documentation/swiftui/documents
If there is additional behavior or functionality that would be useful in this area, a Feedback report would be appreciated!

----

@Chris
 asked:
Hey! I was wondering whether I'm using the keyframe animation API correctly. I wanted to move between two states. State A has offset (0,0) and state B has offset (150,150). It animates fine when I go from A to B, but tapping again has a weird glitch. Am I using the API wrong or is this a bug? Here's the code I tried out: https://gist.github.com/chriseidhof/f2506dc54363e7204deeef9837b90dad
3 replies

WWDC Bot
APP  
@Curt C (Apple)
 answered:
Thanks for the question!
I wonder if the initialValue in your example needs to be predicated on the value of active?
:+1:

Chris
  
I tried that but that really breaks things!


Curt C (Apple)
  
Would love a feedback then. Thanks for kicking the tires!


Curt C (Apple)
  
Would love a feedback then. Thanks for kicking the tires!

----

@Chris
 asked:
When I have a frame modifier that's not scoped to be within an animation, it often still animates. For example in this gist: https://gist.github.com/chriseidhof/b7aacc4dd971ea81e9973fa0b4ab0c77. Is this considered a bug? I have tested the new .animation(_:body:) API which works great as a workaround, thank you for that. (And also for custom transaction keys, very helpful).
See less
2 replies

WWDC Bot
APP  
@Jacob X (Apple)
 answered:
This is expected behavior. Sizing and positioning in SwiftUI happen at leaf elements (i.e. the Color in this example). This means that when a layout modifier like .frame is applied, its effect isn't applied until the leaf views it affects. This means that the transaction at the leaf view will be what determines how the layout change animates.

----

@Andrew
 asked:
Can SwiftCharts be brought into RealityKit? Would it be possible to have a 3 axis, 3D in RealityKit (let’s say X and Y represent lat and longitude while Z represents temperature)
1 reply

WWDC Bot
APP  
@Richard W (Apple)
 answered:
You can use Swift Charts to create 2D charts and use it like any SwiftUI View when it comes to AR. But we don’t have the right expert on RealityKit available to answer this. Please ask on https://developer.apple.com/forums/

----

@Harry
 asked:
When using @Observable, if one of the properties is a struct, how will that behave when changing the properties inside (for instance title inside Item below?
e.g.
```
@Observable class Model {
  var item: Item
}
struct Item {
  var title: String
  var description: String
}
```
3 replies

WWDC Bot
APP  
@Luca B (Apple)
 answered:
Because struct have value semantic, when you mutate a property of the struct you're effectively mutating the whole struct.
In your example if you mutate the item's title, `Model` will publish a change for it's item.

----

@James
 asked:
Is there a way to "remember" the position of a ScrollView in a View to scroll to that position after the user dismisses the View, with @SceneStorage maybe? I'd like to scroll to that position on the user's next time in that View.
5 replies

WWDC Bot
APP  
@Harry L (Apple)
 answered:
You can use the new scrollPosition() modifier along with the scrollTargetLayout() modifier to accomplish this. Just ensure the binding you provide has a non-nil initial value and the ScrollView will ensure the view with that id (within the layout configured with scrollTargetLayout()) is visible when the scroll view appears.
See the docs for more information here:
https://developer.apple.com/documentation/swiftui/view/scrollposition(id:)
Apple Developer Documentation Apple Developer Documentation
scrollPosition(id:) | Apple Developer Documentation
Associates a binding to be updated when a scroll view within this view scrolls.

James
  
New…iOS 17. Nothing for iOS 16?


Harry L (Apple)
  
In iOS 16.0 you can keep track of the views that appear and disappear yourself and manually scroll to the last visible view using a ScrollViewReader + onAppear.

----

@Ratnesh
 asked:
SwiftUI transition question:
Is it possible to apply `matchGeometryEffect` between sheet presentation or push/pop navigation?
If not, what should be the approach to achieve such effect?
2 replies

WWDC Bot
APP  
@Curt C (Apple)
 answered:
Thanks for the question!
This is not currently supported. Please file a Feedback with your specific use case. That really helps us prioritize future work.
:+1:

Curt C (Apple)
  
Some people have had luck using an overlay plus `matchedGeometryEffect` for these sorts of transitions. That’s challenging to get right. (But fun to play with!)

----

@Luis
 asked:
`#Preview` macro doesn't work if the project targets previous iOS versions, is this the final behaviour or will you add support to make it version agnostic?
2 replies

WWDC Bot
APP  
@Richard W (Apple)
 answered:
The `#Preview` macro is not backdeployable in Seed 1. Feel free to file Feedback!

----

@Simon
 asked:
Is there any way to have the Y-axis labels update in a scrolling chart as the chart is scrolled or when the scrolling settles after snapping? This behavior would be similar to what the Health app does where the Y-axis reflects the minimum and maximum of the currently visible area.
1 reply

WWDC Bot
APP  
@Robert M (Apple)
 answered:
You would currently need to update (with animation) the Y-axis yourself by setting the  Y domain according to the temporal window the user settled on, presumably with a debounce, so it activates some hundreds of milliseconds after the scroll interaction ended

----

@Hoang Nam
 asked:
Hi. Is there any possible way to detect when user finishes scrolling a ScrollView? (Similar to UIKit UiscrollView delegate scrollViewWillEndDragging).
6 replies

WWDC Bot
APP  
@Harry L (Apple)
 answered:
There is no SwiftUI API that provides this functionality though I'd love a feedback detailing your use case.

Hoang Nam
  
When user finishes scrolling I want to fix the scroll position to always show the whole item at top of the screen (but only for some item, depends on the type)

Abhishek
  
Quickly jumping in to say that I'd really love for something like this to exist as well :slightly_smiling_face: I have a list of cards in a ScrollView - each one of them have a custom drag gesture which I'd like to disable when user is scrolling so that I don't accidentally trigger any action via the gesture.

Harry L (Apple)
  
If influencing where a scroll view chooses to scroll is what you want to do, the ScrollTargetBehavior protocol and modifier might be useful:
https://developer.apple.com/documentation/swiftui/view/scrolltargetbehavior(_:)
This protocol will give you the content offset the scroll view will scroll to and you can change it to scroll somewhere else.
Apple Developer Documentation Apple Developer Documentation
`scrollTargetBehavior(_:)`

This won’t help the “disable while scrolling” case.

----

@Jan
 asked:
What is the recommended way to pass data from child to parent view in SwiftUI? I underderstand this could be done using PreferenceKey, but I wonder what alternatives there are and what is recommended
2 replies

WWDC Bot
APP  
@Curt C (Apple)
 answered:
Thanks for the question!
A classic engineering answer: it depends!
Preferences are a reasonable mechanism in some use cases, though you need to be careful to avoid cycles.
Creating some state in the parent view and passing a binding to the state down to the child is also a good technique sometimes.
:raised_hands:

Curt C (Apple)
  
A bit more on cycles with preferences: Suppose the preference comes up and changes the parent. Then suppose the parent changes the child. If that causes the child to send a different preference value on the next update, then a cycle will occur.

----

@Miguel
 asked:
Hi,
```
struct ContentView: View {
    @Bindable private var viewModel = ViewModel()
    @Bindable var viewManager: ViewManager
    
    init(viewModel: ViewModel = ViewModel(), viewManager: ViewManager) {
        self.viewModel = viewModel
        // Can I now instatiate my ViewManager object?
    }
    
    var body: some View {
        VStack {
            Text(viewModel.username)
        }
    }
}

@Observable class ViewModel {
    var username: String = ""
}


@Observable class ViewManager {
    var viewModel: ViewModel
    
    init(viewModel: ViewModel) {
        self.viewModel = viewModel
    }
}
```
I would like to know if it is appropriate (and how to achieve it) communication between @Observable objects with other objects (being them also Observable or not). Thank you!
1 reply

WWDC Bot
APP  
@Daniel D (Apple)
 answered:
Your observable objects are normal Swift references. You can manage relationship of their instances as normal reference relations. Memory management best practices apply as well. To your example, if you want your view to store a source of truth of your view model, you should use a `@State` as opposed to `@Bindable`.

----

@Jack
 asked:
I really love the look of the new ScrollView enhancements this year! Is there a way to get them to work with Lists as well?
:eyes:
1

1 reply

WWDC Bot
APP  
@Harry L (Apple)
 answered:
APIs like scrollPosition() and scrollTransition() currently only work with ScrollView though I'd love a feedback detailing your use case with List.

----

@Anton
 asked:
Can you explain, when we should use @State with an @observable class?
4 replies

WWDC Bot
APP  
@Matt R (Apple)
 answered:
You can store @Observable objects within @State properties, when the view needs to create and own the lifetime of a particular object instance.
ObservableObject required the use of @StateObject to handle dependencies to object-based state correctly. With @Observable, you can just use @State for the same purpose.
See less
:+1:

Jan Luca
  
So we use @State instead of @StateObject, and just a plain reference instead of @ObservableObject?

Matt R (Apple)
  
Correct!
With @Observable, the experience is much closer to what you write today when just struct-based state.

----

@Fredric Michael
 asked:
Hi,
I've been trying to implement .focusable in my Ventura macOS app. Unfortunately something seems broken as I can't get the focus ring to appear when I apply .focusable on any view. Do I need to apply something else to get it to work?
Thanks
Michael
4 replies

WWDC Bot
APP  
@Cody B (Apple)
 answered:
Prior to macOS Sonoma, the `focusable()` modifier only has an effect when the user enables keyboard navigation in System Settings. There's no way to configure this to be otherwise.
We changed this in Sonoma to be more flexile:
* `focusable()` now makes your view focusable whether or not keyboard navigation is enabled.
* To get the old behavior, you can specify that your control only wants focus for "activation" interactions. The system will treat the view more or less like a button for focus—never focusable on click, only focusable on tab with keyboard navigation enabled.
TapGestureView()
    .focusable(interactions: .activate)
We're releasing a talk tomorrow that covers this (and some other common focus questions) in a little more detail: The SwiftUI Cookbook for Focus.
See less


Fredric Michael
  
@Cody B (Apple)
 Thanks. That explains it. But if I understand it correctly even with keyboard navigation off the tab will still work for lists, textfields etc in Ventura?


Cody B (Apple)
  
Yes, that's right. The built-in text controls and collection views always accept keyboard focus. Prior to this year's releases there were no public APIs for getting the same behavior in custom views; now you get it automatically when you specify `.focusable()` with no arguments.

----

@Sam
 asked:
Regarding San Francisco's “high legibility” alternate style set: since there isn’t a proper font modifier for this, is it safe to assume using the stylistic set key is shelf-stable? (I think it was kStylisticSetSix or something)
2 replies

WWDC Bot
APP  
@Jacob R (Apple)
 answered:
Please file a Feedback. Meanwhile you can create a CTFont requesting this font feature and then use that to create a Font.

----

@Noam
 asked:
How can I make a reversed scroll view?
I’ve seen some hacks like rotating the view but it messes with the scroll indicators.
Would it be possible to use some of the new APIs to implement that?
:+1:

WWDC Bot
APP  
@Harry L (Apple)
 answered:
What do you mean by a "reversed scroll view"?

Noam
  
Like in a messaging app,
You start scrolling from the bottom rather than the top
:eyes:

Harry L (Apple)
  
The scrollPosition() API might help you here:
https://developer.apple.com/documentation/swiftui/view/scrollposition(initialanchor:)
There’s an initialAnchor variant that you can use if you don’t care about observing changes to what view is currently visible.
```
ScrollView { ... }
  .scrollPosition(initialAnchor: .bottom)
```
This will tell the scroll view to start scrolled to the bottom and try to stay scrolled to the bottom until the user scrolls.
If you do care about the state changes, you can use the id version and ensure the binding has an initial value to the last element in the lazy stack.
```
ScrollView {
  LazyVStack {
    ForEach(items) { ... }
  }
  .scrollTargetLayout()
}
.scrollPosition(id: $position)
```

----

@Sam
 asked:
It seems like decorative gradients are a big trend this year — any specific pointers on creating nice, tasteful gradients?
2 replies

WWDC Bot
APP  
@Anil K (Apple)
 answered:
The builtin color gradients (e.g., Color.blue.gradient) all look great. There's also a Meet the Presenter for the Design with SwiftUI session, and they may be able to provide further guidance.
:raised_hands:

Sam L (Apple)
  
To add on a little to this: It’s hard to give generally applicable advice here aside from those system provided gradients because it’s all about your design sensibilities here. Areas like this are where you can bring your own special touch to your apps, so play around with different color combinations and stops and see what looks great to you! (edited) 

----

@Chris
 asked:
Is there a way to get the transaction within an onChange(of:...) callback? This would be useful for when I'm animating something there (and use the same animation for my new change).
2 replies

WWDC Bot
APP  
@Jacob X (Apple)
 answered:
There's currently no way to do this, but it does sound like it would be useful. Could you file a feedback with a request for this?

----

@Luis
 asked:
Apparently, we can use the @Observable macro together with ObservableObject, but it throws an error when trying to compile: "'ObservationRegistrar' is only available in iOS 17.0 or newer"
This is the chunk of code:
```
@Observable class Test: ObservableObject {
    var name: String? = nil
}

struct ContentView: View {
    @ObservedObject var test: Test
    
    var body: some View {
       Text("hola")
    }
}
```
6 replies

WWDC Bot
APP  
@Matt R (Apple)
 answered:
Hi Luis, @Observable is only available for use starting in iOS 17.

Luis
  
I saw you took down the docs saying otherwise… bummer :disappointed: https://developer.apple.com/documentation/swiftui/migrating-from-the-observable-object-protocol-to-the-observable-macro (edited) 
image.png

I will file a feedback :pray:


Jonathan
  
That doc has been up and down a few times now, maybe its just a problem with the website?


Matt R (Apple)
  
We're currently revising that article to provide more clarity, it will come back

----

@Nayan
 asked:
When using a NavigationSplitView is it possible two have either a two-column or three-column layout depending on a condition? For example, in Notes it's three columns when viewing in a list and two columns when viewing as a grid.
2 replies

WWDC Bot
APP  
@Curt C (Apple)
 answered:
Thanks for the question!
The best way to do this today is to switch between a two- and three-column NavigationSplitView inside an if.


Curt C (Apple)
  
If you do this, be sure to lift any state above the view containing the if, as changing the condition changes the view’s identity.

----

@Colin
 asked:
When using `@Query` to fetch from SwiftData in a SwiftUI view, are the sorting and predicates dynamic like they are with @FetchRequest for CoreData? I'm having a hard time setting them programmatically in a view.
1 reply

WWDC Bot
APP  
@Daniel D (Apple)
 answered:
If you need to update your predicate or sorting criteria, pass them into the view that your query resides. Let's say you need to change the sort order of recipes, the related code might look something like:
```
struct ContentView: View {
    @Query var recipes: [Recipe]
    init(order: SortOrder) {
        _recipes = Query(sort: \.name, order: order)
    }

    var body: some View { ... }
}
```

----

@Bruno
 asked:
Elaborating my previous question, I have several UIKit views that I use in SwiftUI through UIViewRrepresentable.
When I use those views out of a ScrollView, they size correctly.
When I place them inside a ScrollView, they vanish, unless I set a height for them.
Is this a known issue? How can I fix this?

6 replies

WWDC Bot
APP  
@Parry P (Apple)
 answered:
You can implement sizeThatFits in your representable view to provide sizing information.


Parry P (Apple)
  
https://developer.apple.com/documentation/swiftui/uiviewrepresentable/sizethatfits(_:uiview:context:)-9ojeu
Apple Developer DocumentationApple Developer Documentation
`sizeThatFits(_:uiView:context:)` | Apple Developer Documentation
Given a proposed size, returns the preferred size of the composite view.

Bruno
  
But to which size should I refer to? The purpose of auto-sizing is that I don’t have to calculate manually. Also it works fine when it’s not inside a ScrollView so, are we talking about a bug?


Parry P (Apple)
  
sizeThatFits gets passed in a proposed size that can be used to return an expected size. What seems like is happening is that the representable is not able to return an expected size based on the proposed size, which is something that definitely makes a difference within a ScrollView.
I'd suggest to use sizeThatFits to see if that can get you unblocked, but to also file a bug report for this for it to be tracked, where this issue will get more eyes.
:+1:

Parry P (Apple)
  
Also, I was wondering why do you have to use UIViewRepresentable instead of creating views directly in SwiftUI…could you share your use-case please?

Bruno
  
So this is about a project from my employer (not my personal project), and it’s a big company, many complex components that would take time to convert to SwiftUI, so… in short term or we use UIViewRepresentable for now or we don’t use SwiftUI at all hahaha I think you know my position :sweat_smile:

----

@Jan Luca
 asked:
I noticed that a lot of sample code uses var instead of let for read only properties of Views. Is there any difference? I usually use let in my structs unless I explicitly need mutability.
:eyes:

WWDC Bot
APP  
@Robert M (Apple)
 answered:
Prefer to use let unless a var is needed, for example for mutation, or computed properties

----

@David
 asked:
Hello! A question about my project. l'm sorry if it was answered already.
Is it possible to bind directly to CoreData objects? Publishing them from a VM.. I remember running into issues with all of the optionals.

WWDC Bot
APP  
@Anil K (Apple)
 answered:
`NSManagedObject` conforms to `ObservableObject`, so you can use one in an `@ObservedObject` property and from there get bindings to its properties.

----

@Brandon
 asked:
When moving to `@Observable` from `@ObservableObject`, can you still use `@Published`, such as in cases where you want the `@Observable` type to have Combine-related functionalities in addition to updating SwiftUI views?
6 replies

WWDC Bot
APP  
@Luca B (Apple)
 answered:
No, `@Published` only works when applied to properties contained in a type conforming to the ObservableObject protocol
:+1:

Brandon
  
Understood, thank you!

Tomas
  
Someone asked yesterday if we can have ObservableObject with @Published properties to keep the viewmodel working in older iOS versions, but add @Observable to make things more performant in iOS 17+, but this question got lost in a thread, would it work like this?


Matt R (Apple)
  
To add some additional detail, it's possible to apply the `@Observable` macro to a type that simultaneously conforms to ObservableObject, and both observation systems can work in parallel.
However, it's currently not possible to apply `@Observable` to a class that also uses @Published on any of its properties. We understand the practical impediment of that, and we're tracking that issue. Manually triggering ObservableObject notifications via the objectWillChange publisher will work, however.

----

@Artyom
 asked:
Are there any recommendations to display list's with large datasets with SwiftUI?
For example, I'm using now:
1. CoreData classes which load their properties lazily
2. These classes are wrapped in 'descriptors' classes that generate  all fields required to split content into different sections(like id, startDate) while the rest of the content is loaded on demand
3. UITableView prefetching mechanism is used to trigger this load
4. Dataset is around 20k elements

9 replies

WWDC Bot
APP  
@Max O (Apple)
 answered:
We just published a talk on SwiftUI performance that discusses a lot of important concepts around making large lists performant. I hope this will help, but feel free to come back with more specific questions! https://developer.apple.com/videos/play/wwdc2023/10160/


Artyom
  
Thanks 
@Max O (Apple)
! Yes, I’ve checked that one out already. If my assumptions are correct, as far as identity is provided correctly essentially the same approach as I’m using now should work.
I’m curious if I should be concerned in any way with prefetching, though. Would SwiftUI generate row content quickly enough to achieve the same as UITableView prefetching does?


Artyom
  
Or rather, would SwiftUI trigger row content generation at the same time as UITableView prefetching allows us to or would it be similar to cell dequeuing?


Raj R (Apple)
  
SwiftUI does not offer a data prefetching facility like you'd find in UITableViewDataSourcePrefetching, but it will construct views for cells in advance, at its discretion
:bow:

Artyom
  
Thanks 
@Raj R (Apple)
!
Okay, then the final questions for this thread are:
Does it mean that SwiftUI’s List’s and ForEach are not necessarily constrained by dataset sizes? Meaning, they’d support smooth scrolling and presentation both for 100 items as well as for 10000 items and if they do not — it means I’m modelling my data in a wrong way?
If the first is true, does this also apply to rows re-ordering and deletion? Meaning, if I model my data correctly I still get fast updates and responsive UI with all of the good stuff of SwiftUI? I wonder about this point specifically since I’ve noticed long updates, for example when my data collection changes — I’ve concluded that the bottleneck is the diffing between old and new snapshots


Raj R (Apple)
  
Correct, but note the IDs have to be generated for all rows. It's actually quite quick to retrieve a large set of IDs though.
Not really— diffing is currently somewhat expensive as SwiftUI creates a copy of the IDs, which costs. But we're always looking to improve this, so if you have a concrete slow case, a feedback is most welcome.
:pray:

Artyom
  
I see. Did I get it right, that it is possible to display large datasets with relatively no issues, but subsequent updates would be a problem? (edited) 

Raj R (Apple)
  
Right.

----

@Nayan
 asked:
Just wanted to ask if this behaviour was intentional or not: disabled buttons in alerts don't show at all. Say you had a text field in the alert and only wanted the "Done" button to be enabled when the text field was not empty, the done button wouldn't show, only a cancel button.
I filed a feedback report anyway, FB11558215, because I'm pretty sure this used to work before.
See less
1 reply

WWDC Bot
APP  
@Curt C (Apple)
 answered:
Thanks for the feedback!
This was not an intentional change and we're aware of the issue.

----

@Chris
 asked:
I was wondering why the .preference-based APIs use a default value for the key (key: K.Type = K.self). Is there any case in which this is used? I have always found myself specifying the key explicitly. OTOH, for the new @Environment initializer this could be a useful thing to add, so you can do `@Environment private var book: Book` instead of `@Environment(Book.self) private var book`.

2 replies

WWDC Bot
APP  
@Matt R (Apple)
 answered:
Hi Chris, thanks for this feedback. We're always looking at ways to make the syntax nicer and more intuitive.
One thing to point out for environment objects w/ @Observable, is that you can get both optional and non-optional properties, and also define your own properties with custom keys:
```
@Environment(Book.self) private var book: Book // default
@Environment(Book.self) private var book: Book?

@Environment(\.myFavoriteBook) private var book: Book?
```
One benefit of specifying `Book.self` explicitly is that it helps clarify that it's a key, like `\.myFavoriteBook`. So the first two properties above will access the same underlying storage, because they share the same `Book.self` key, whereas the third property is accessing different underlying storage, even though it has the same return type, because it uses a different key.

----

@Tomas
 asked:
I have built my iOS app with widgets in XCode 15, and suddenly all homescreen widgets have huge padding (I previously used to draw them myself, edge to edge, but now whole my ‘edge to edge’ view is inside the widget’s safe area, including the background). How can I tell SwiftUI not to do this? .ignoresSafeArea(.all) on a widget doesn’t help.

WWDC Bot
APP  
@Jacob R (Apple)
 answered:
By default widgets now receive a default padding; you can disable that using .contentMarginsDisabled() modifier on your widget configuration. But there's probably some cases where you want to use the default content margin that we provide. For that you can combine it with \.widgetContentMargins. For much more detail about this you can watch https://developer.apple.com/videos/play/wwdc2023/10027/.

----

@Axel
 asked:
As far as I know, Pickers does not load their content lazily. When I have a picker with 100 items, all the elements are accessed right away. In UIKit, UIPickerView were lazy so we could have many items. Is that a bug or a feature?
1 reply

WWDC Bot
APP  
@Franck N (Apple)
 answered:
Hey Axel, thanks for the feedback. This is a known issue and we are working on fixing it, but we’d appreciate a feedback request with more detail about why you’d like this capability!
You can learn how to file a request using Feedback Assistant by visiting https://developer.apple.com/bug-reporting/.

----

@Jacob
 asked:
What is the best way to animate a parent when a child view’s frame changes in an animated way due to a change in the child view. Today the parent will not animate in this case, it just jumps to accommodate the new size of the child even though the child is animating its size change. I came up with this answer but I’m wondering if there is a built in way to do this: https://stackoverflow.com/a/76195425/9156195
See less
Stack Overflow
Parent view not animating child view's frame changes in SwiftUI
So I'm finishing up work on a SwiftUI based iOS App. I'm in the process of refining animations and visual layout stuff. My issue is as follows:
I have a parent view (A) which contains a child view ...
1 reply

WWDC Bot
APP  
@Anil K (Apple)
 answered:
It seems like you want withAnimation(_:_:), which will animate all of the changes that are triggered by its body without needing the .animation modifier.

----

@Axel
 asked:
What is the cleanest alternative to multi columns pickers? Like to choose hour/minutes/seconds to configure a picker for example, or to choose the decimal and float part of a number (distance for a running app, height, weight, etc).
3 replies

WWDC Bot
APP  
@Franck N (Apple)
 answered:
Hey Axel, thanks for the question.
I think this really depends on your specific use-case and design language. navigationLink styles could be a nice alternative. Is there any specific reason why multi-column pickers aren't a good fit for your use-case?

Axel
  
Thanks 
@Franck N (Apple)
. Yes, the use cases I described. How to you select two or three values in a NavigationLink? Like my height in meters, and centimeters. My weight in kilograms and grams. A duration in hours, minutes, seconds. I remind you that we don’t have the UIDatePicker.Mode.countDownTimer in SwiftUI.


Franck N (Apple)
  
In the navigationLink case you'll need one picker per unit. If you are requesting a control to pick multiple items, please could you file a feedback?
A workaround could be to use a menu of toggles. That would render as a menu picker with multiple selection enabled. Would that help your case?

----

@Harry
 asked:
In SwiftData, can I create a custom query using custom SQL or similar? Some of the use cases may include UNION or CTE queries

WWDC Bot
APP  
@Matt R (Apple)
 answered:
Not using SQL, no. The full span of customization for a query is represented by the configuration properties of the `FetchDescriptor` type in SwiftData.

----

@Axel
 asked:
It's currently not possible to present a sheet, fullScreen, confirmation dialog, alert, etc. from a Menu. It this a bug or a feature?

WWDC Bot
APP  
@Anil K (Apple)
 answered:
You can trigger sheet/etc. presentation from a Button within a Menu. Make sure you're attaching the .sheet modifier outside of the Menu content. If that's not working as expected, please file a feedback with more information about your use case!

----

@Jack
 asked:
Is there a way to increase the total height of a `.wheel` Picker style? It doesn't seem possible to achieve a like-for-like swap from a `UIPickerView` in UIKit. The UIKit version displays more items on the wheel than the SwiftUI version; even when their parent views are the same size.
1 reply

WWDC Bot
APP  
@Franck N (Apple)
 answered:
Hey Jack, thanks for the feedback, unfortunately, there currently isn't an API to set the height of a wheel picker item  but we’d appreciate a feedback request with more detail about why you’d like this capability! Having parity on both frameworks is very important to us.

----

@Arkadiusz
 asked:
I'm building an app that shows a List on iPhone and a LazyVGrid with two columns on iPad and on Mac. Is there any documentation that lists differences between these two containers, for example I know that swipeActions aren't available outside of List. Any other gotchas I should know?
5 replies

WWDC Bot
APP  
@Cody B (Apple)
 answered:
`LazyVGrid` is a layout primitive, while `List` is a featureful collection view, so most things you get "for free" with List need to be re-implemented in one way or another when building with a lazy grid/stack.
In other words, it's like comparing `VStack` and `List`—they both present a vertical array of views, but List has behavior and semantics that VStack lacks.


David
  
Does `List` load views lazily? I’ve used `LazyVGrid` for a single-column list with a large number of items for its lazy loading.


Cody B (Apple)
  
Yes, List loads its row content lazily.


Arkadiusz
  
@Cody B (Apple)
 Thanks! Are there any other views in SwiftUI that are internally backed by `UICollectionView`  and come with additional features?


Cody B (Apple)
  
Behavior that isn't documented in the SwiftUI API docs is likely an implementation detail that could change from one release to the next. So, you might find some UIKit collection views if you poke around in the view debugger, but there isn't anything you can safely do with that knowledge besides filing feedback reports and enhancement requests.

----

@Naftali
 asked:
Is there a way to resize the columns in a navigationstack on iPad?
1 reply

WWDC Bot
APP  
@Curt C (Apple)
 answered:
Thanks for the question!
iPad doesn't support user-driven column resizing. You can use the navigationSplitViewColumnWidth modifier to set a custom width.

----

@James
 asked:
I'm trying to use
 `.containerBackground(for: .navigation)`
on a form. I get the compile-time error "'navigation' is unavailable in iOS". Same for all ContainerBackgroundPlacements, like .widget or .tabView.
What containers does this work with?
8 replies

WWDC Bot
APP  
@Jacob R (Apple)
 answered:
`.navigation` and `.tabView` are only available on watchOS 10.0. For iOS/macOS/watchOS there's `.widget` when used with WidgetKit. There's also a few other container background placements related to some of the new StoreKit APIs.


Jacob R (Apple)
  
If there's additional use case where you could use it please file a Feedback.


James
  
Is this modifier not expected to be used outside of StoreKit Views?

I thought it would be useful with any container.


Jacob R (Apple)
  
It works outside of StoreKit views -- specifically for widgets.

A Feedback would be good to help us prioritize that we support all the use cases on all platforms.


James
  
So if the view is not in a StoreKit view and not in a Widget, I can’t use it?


Jacob R (Apple)
  
That's correct.

----

Nayan
 asked:
Are expanding sections only available with the sidebar list style?

WWDC Bot
APP  
@Haotian Z (Apple)
 answered:
Hi! You could control the expand state with the new programmatic expandable Section support with any list style, detailed here https://developer.apple.com/documentation/swiftui/section/init(isexpanded:content:header:)-561d7?changes=latest_minor

----

@Naftali
 asked:
Using a compact date picker and trying to present an alert on the parent view, the alert will not show if the datepicker is still open, is there a way around this? or is this intended behavior?
1 reply

WWDC Bot
APP  
@Franck N (Apple)
 answered:
Hey Naftali, this seems like it could be a bug, we’d appreciate a feedback request with more detail about why you’d like this capability!

----

@Alperen
 asked:
What is the best way to use State and Binding mixed in a child view? For example, if I pass a binding value in initializer, the view must use it. If binding value is nil, the view must create a state and use it. I created a property wrapper for this, and it works. But actually I didn't like it at all. I want to know if there is a better solution for this.
See less
1 reply

WWDC Bot
APP  
@Matt R (Apple)
 answered:
Hi Alperen, that's the general solution we'd recommend for that. If you have ideas for making that workflow easier, please file feedback and let us know!

----

@Kazushi
 asked:
Is there a way to achieve floating view in ScrollView, like supplementary view in UICollectionView?

WWDC Bot
APP  
@Harry L (Apple)
 answered:
Yes, the new visualEffect() API can accomplish this:
https://developer.apple.com/documentation/swiftui/view/visualeffect(_:)

----

@Patrick
 asked:
Does the new `@Bindable` in SwiftUI support nested changes? For example, I understand that `$friend.firstName` would update if `firstName` changes, but would `$friend.pet.name` also update if `name` changes?
Also, what about nullables, such as `$friend.pet?.name`? This has historically been a huge pain.
1 reply

WWDC Bot
APP  
@Matt R (Apple)
 answered:
Yes, all of that works!

----

@Matthew
 asked:
When using NavigationStack w/ .navigationDestions, when I want to push a navigation value that represents "editing an object", where should ownership of that model-being-edited lie? I've found that if I push it onto the navigation stack, that works well. But if I push a description of it onto the stack, and then have the view instantiate the model during initialization, that view-owned model is re-instantiated many times (on each render). It seems odd to push the model-being-edited onto the Navigation stack. What am I missing? This has been my top issue/question with Navigation Stack. Thank you!

WWDC Bot
APP  
@Curt C (Apple)
 answered:
Thanks for the question!
I'm not 100% sure I understand. This might be a good question for a lab session.
In general, either approach should work. Another approach is to use a data store and look up the new model by ID, creating a new instance of the ID doesn't exist.

----

@Andrew
 asked:
With SwiftData is there a way to create a sectioned fetch request in the view?
1 reply

WWDC Bot
APP  
@Matt R (Apple)
 answered:
This is not currently supported.

----

@James
 asked:
I have a Text with paragraphs of strings. It seems the user can select only the entirety of the Strings in the Text view. Is it possible to let the user select a portion of the contents of the Text view?

2 replies

WWDC Bot
APP  
@Jacob R (Apple)
 answered:
This is not currently possible on iOS / iPadOS. Please file a Feedback.

----

@Brandon
 asked:
Using `@Observable`, what is a use case where one might still need to use `@State` to reference a view model, as opposed to directly referencing the view model itself?  The sample video says that `@State` is still practical in a case where only the view needs to reference a property, but I was kind of unclear on why someone would ever use `@State` instead of the view model handling all of the logic?

2 replies

WWDC Bot
APP  
@Daniel D (Apple)
 answered:
This depends on how you would like to arrange the ownership of your data. Without `@State`, your `Observable` model instance must come from somewhere else, the view would not be the owner of this instance. Sometimes you want your view to be the logical owner of some data (such as a boolean for whether a sheet should bet shown, but realistically likely more complex to warrant a class). `@State` helps you achieve that.

----

@Andrew
 asked:
I see from an earlier response that you can adjust a predicate for a @Query from the init() is this the recommended way to handle searching where as you type your updating a child view  init() with the updated value for the predicate
3 replies

WWDC Bot
APP  
@Daniel D (Apple)
 answered:
Correct!


Yona
  
Can you provide an example?


Daniel D (Apple)
  
Here's an small example that updates the sort order of the query I used to answer a similiar question. You should be able to adapt it for updating parts of a predicate:
```
struct ContentView: View {
    @Query var recipes: [Recipe]
    init(order: SortOrder) {
        _recipes = Query(sort: \.name, order: order)
    }
    var body: some View { ... }
}
```

----

@Naftali
 asked:
I have IAP in my app, and I limit certain features based on if the user purchased premium. How can I make it so that while I'm working on the app I can use those features in the simulator/preview while ensuring that it wont by mistake end up in production?

WWDC Bot
APP  
@Sam L (Apple)
 answered:
This should be supported by StoreKit, but we're not the right people to help you further. StoreKit and In App Purchase is running a Q&A tomorrow at noon in the 
in-app-purchase channel! They're the real experts in that area and they'll likely be able to answer your questions more precisely.

----

@Chris
 asked:
With the new containerRelativeFrame API, what are the actual containers that are supported? The documentation is a bit unclear ("including"). Can we modify our own views to become containers?

WWDC Bot
APP  
@Harry L (Apple)
 answered:
You can use the components listed in the docs as the supported "containers". Its not possible to make an arbitrary view a "container" with respect to container relative frame though I'd love a feedback detailing your use case.

----

@Jack
 asked:
Do `LazyVStacks` leverage cell reuse behind the scenes? I really want to use the new ScrollView APIs (that aren't compatible with `List`) but am worried about performance when using `ScrollView` with `LazyVStack` in the context of infinite scrolling (eg. such as a social media feed).

WWDC Bot
APP  
@Curt C (Apple)
 answered:
Thanks for the question!
`LazyVStack` does not currently have cell reuse. In general you'll want to make the data associated with items in a `LazyVStack` as lightweight as possible.
Often this can be accomplished by minimizing the state created by the item and having it reference a separate data model.

----

@John
 asked:
Is it possible to add multiple shortcuts to a single user interface elements? If I have multiple `.keyboardShortcut()`s attached to a single view, only one of them fires. (Also related, is it possible to attach keyboard shortcuts directly to an action, rather than requiring, say, an invisible button?)
See less
3 replies

WWDC Bot
APP  
@Cody B (Apple)
 answered:
Controls can only support one shortcut currently, as you've found. I'm track a request to add that support internally, and I'll consider this another vote of support.
Buttonless shortcuts also aren't possible, but there are a few other possible work-arounds:
* First, should there be a menu item for this function? If so, you can use the Commands API to add a menu item with a shortcut.
* Second, if you have focusable content in your scene, you can react to key presses using the `onKeyPress` modifier, new this year.

----

@Austen
 asked:
What's the proper way to open a ScrollView at the bottom (like in Messages)?
1 reply

WWDC Bot
APP  
@Anil K (Apple)
 answered:
You can use the new scrollPosition modifiers to set the scroll position!

----

@Rhys
 asked:
Is it possible to set the alert style with SwiftUI for macOS?
I want to present a Critical style alert, but I can’t find any way to configure this. I don’t want to drop back to using NSAlert if it can be avoided!

@Jeff R (Apple)
 answered:
Hi - thanks for the question. You can use the new dialogSeverity() modifier for this. For example:
```
.alert(...) {
    ...
}
.dialogSeverity(.critical)
```
This modifier is available on macOS 13+

Rhys
  
Fantastic, thank you!!!
:pray:

Jeff R (Apple)
  
There were a few new dialog customization modifiers added. See Whats New in SwiftUI for some more: https://developer.apple.com/videos/play/wwdc2023/10148/

----

@Brandon
 asked:
I am working on building a sample app that uses the new SwiftData framework and the new `@Bindable` type.  In the past, one could use .constant when compiling a preview that injected a mock property to make the preview usable.  Is there something similar for `@Bindable`?
4 replies

WWDC Bot
APP  
@Anil K (Apple)
 answered:
You can construct one of your model objects in the preview and pass it into the view and to use to initialize the Bindable property.

Brandon
  
Thanks, 
@Anil K (Apple)
So, for previews, I no longer need to use .constant() to inject a mock Bindable property?


Anil K (Apple)
  
That's correct. A `@Bindable` property is akin to a regular property without a property wrapper, you can initialize it to a regular value.

----

@Matthew
 asked:
Follow up to a previous question -- when I use a Table with the `Table(of:columns:rows)` initializer, and I define Sections with custom headers in the rows section, the custom header is ignored for the first section, while it is properly displayed for all subsequent Sections.  Is that expected?  Is there a way to provide a custom View for the header of the first Section?

WWDC Bot
APP  
@Haotian Z (Apple)
 answered:
Hi! We need more detail on this - did you see the header for the first section in the table header region? Is there a screenshot for us to better understand what it is now vs what is your wanted behavior?


Matthew
  
Yep -- screenshot here of what I get:
simulator_screenshot_A91786AE-6F4D-4EEE-98E3-30C2CCDFCD93.png

Matthew
  
But I have a similar header on the first section -- on iPhone, it looks like this (note the 'Overdue' header:
simulator_screenshot_7238DEEE-C3E1-49A8-9232-72D13DCAD639.png

(You can ignore the 'This is some inconveniently wide text' -- I wanted to see if it would overlay over the column ehadings when scolled up)


Haotian Z (Apple)
  
Got it, could you file a feedback for this? Table on iOS has a behavior of merging section header into first column header when scrolling, but we cannot decide if this is the case here - a feedback with the code used here to produce the image will be great!

Matthew
  
No problem -- I already have FB12272254, but this was from prior to this session.  I will update it with my latest code. Thanks!
:+1:

Haotian Z (Apple)
  
Thank you! We will look into it

----

@Dachary
 asked:
I have struggled with NavigationSplitView in my macOS apps, and safely sharing state/nullifying state with content/detail columns and child views.
Aside from last year's SwiftUI Cookbook for Navigation, are there other good examples or learning resources?


WWDC Bot
APP  
@Nick T (Apple)
 answered:
Hi @Dachary, I need a little more context around what you mean by nullifying state, or sharing state. The cookbook is a great resource, but if it doesn't speak to your use case, or you require some more nuance to managing selection, I'd recommend asking on the developer forums. I'd be happy to try to clarify any questions you might have right now though

Dachary
  
I tend to have a lot of crashes relating to deleting things in content/detail view while still holding a reference to them elsewhere in the view stack. Another thing I can't quite wrap my head around is how to mix navigation types in my "sidebar" (first column) - i.e. value-driven navigation + some other navigation destination. I would find it helpful to see - in general - more approaches to NavigationSplitView on macOS to try to get a better conceptual state for what I'm doing incorrectly.


Nick T (Apple)
  
The crashes around deleting are interesting and I've definitely seen less of those kinds of bugs. For the second question, are you talking about a construction like:
```
struct SomeView: View {
  var body: some View {
    NavigationSplitView {
        List {
            NavigationLink("Value-based", value: 4)
            NavigationLink("View-based") {
                Text("A more, single use destination")
            }
        }
        .navigationDestination(for: Int.self) { int in
            Text("View for \(int)")
        }
    } detail: {
        Text("Detail")
    }
  }
}
```

Nick T (Apple)
  
This example makes use of a concept we call "root-replacement".
Both of those NavigationLinks above will target the detail column (if I had a content column they would target that, not the detail column)


Nick T (Apple)
  
This means that the presented view will be shown in the detail column. Generally, we almost always advise using the value-based NavigationLinks


Nick T (Apple)
  
This is because view-based NavigationLinks by nature provide less programmatic control, and if you have a lot of them, state restoration depends on them being scrolled into view, so will not work properly if your navigation links are off screen


Dachary
  
Ahh, that in itself is helpful! :bulb:  One of my WIP apps has a List where I can select a value and navigate based on that, but I also have some views that don't have associated values and I wasn't sure how best to mix and match those in the same column. I thought. the List could only contain homogenous value-type destinations.
:sunglasses:


Nick T (Apple)
  
Another tip: In general right now, we do not recommend mixing List selection and .navigationDestination. In a construction like this
```
struct SomeView: View {
    
    @State var selection: Int? = nil
    var body: some View {
        NavigationSplitView {
            List(selection: $selection) {
                NavigationLink("Value-based", value: 4)
                NavigationLink("Value-based", value: 5)
            }
        } detail: {
            Text("Detail")
        }
    }
}
```
The links' values will drive that selection state. If you tried to use a navigationDestination  in this case, that leads to some ambiguity. So, for the time being, use one or the other.


Nick T (Apple)
  
I love that particular use case because that is when the navigation system can really shine.


Nick T (Apple)
  
There are a few ways to spell that, allow me to suggest another


Dachary
  
I was doing something like this:
```
NavigationSplitView{ 
...      
NavigationLink(
              destination: MainDashboardView(userProfile: profile)
              .environment(\.realmConfiguration, realmConfiguration)
            ) {
              HStack(alignment: .center) {
                Image(systemName: "rectangle.on.rectangle")
                Text("All Repositories")
            }.buttonStyle(.borderless)
            List(profile.watchedRepositories, selection: $navModel.selectedRepository) { repository in
              let badgeCount = getRepositoryBadgeCount(forRepository: repository)
              NavigationLink(value: repository, label: {
                Label(repository.customName ?? repository.name, systemImage: "rectangle")
                  .badge(badgeCount)
              })
            }
            .frame(maxHeight: 250)
            .navigationTitle("Repositories")
            NavigationLink(
              destination: IgnoredPullRequestsView()
            ) {
              HStack(alignment: .center) {
                Image(systemName: "eye.slash")
                Text("Ignored PRs")
                Spacer()
              }.padding()
            }.buttonStyle(.borderless)
```
Which unsurprisingly doesn't work great. Your example is quite helpful!
:eyes:


Dachary
  
So in my case, I had a List where I could drive navigation by selection of values, but also other views where there isn't an associated value


Nick T (Apple)
  
That is a totally reasonable use case. You can even do stuff like
```
enum MySelection: Hashable {
    case value(Int)
    case settings

    var description: String {
        switch self {
        case .value(let int):
            "\(int)"
        case .settings:
            "Settings"
        }
    }
}

struct SomeView: View {
    @State var selection: MySelection? = nil
    var body: some View {
        NavigationSplitView {
            List(selection: $selection) {
                NavigationLink("Value-based", value: MySelection.value(4))
                NavigationLink("Value-based", value: MySelection.value(5))
                Text("Settings")
                    .tag(MySelection.settings)
            }
        } detail: {
            if let selection {
                Text(selection.description)
            } else {
                Text("make a selection")
            }

        }
    }
}
```

Nick T (Apple)
  
For some sidebar list items that just make more sense to be "hardcoded"


Nick T (Apple)
  
I want to reiterate that even if a view doesn't necessarily have a "value", it's worth doing something like the above with the .settings case because there isn't another way to get full programmatic control over that selection


Dachary
  
That's perfect, I think that will exactly solve my use case (for this app, anyway!)
Thank you so much for the time and info! This is super helpful :slightly_smiling_face:
:heart:

Nick T (Apple)
  
One last sharp edge that we're working on. Links that do root-replacement should generally not contain a NavigationStack. If you need a NavigationStack in a detail column, it's best to have that like this:
NavigationSplitView {
  Sidebar()
} detail: {
  NavigationStack(path: $path) { ... }
}
and either
A) root replace that NavigationStack with non-NavigationStack views or
B) Use that $path binding to programmtically push views onto that stack

----
Welcome to Q&A: SwiftUI Previews
----

@Noam
 asked:
Does the new Previews macro still allow for using @State?
As in declaring a @State or @Bindable property for testing Views that use them.

WWDC Bot
APP  
@Eliza B (Apple)
 answered:
There are a couple patterns for doing this. Details in thread.
:eyes:

Eliza B (Apple)
  
Intuitively, what you need to do is create a wrapper view for your preview that has state (because state has to be declared on a view) and then preview your wrapper view.

Supposing you were trying to preview a Toggle, for example, you could do this:
```
#Preview {
   struct WrapperView {
      @State var toggled = false
      var body: some View {
         Toggle("my toggle", isOn: $toggled)
      }
   }
   return WrapperView()
}
```
But that's pretty annoying to type every time you need state.

There's a convenience you can add to your project to make this easier if you find you're having to do it a lot.
```
struct WithPreviewState<S>: View {
    @State var state: S
    var makeBody: (Binding<S>) -> any View
    
    init(
        _ state: S,
        @ViewBuilder makeBody: @escaping (Binding<S>) -> some View
    ) {
        self._state = State(initialValue: state)
        self.makeBody = makeBody
    }
    
    var body: some View {
        AnyView(makeBody($state))
    }
}
```

If you define that somewhere in your app, you can then do this:
```
#Preview {
    WithPreviewState(false) { // makes false the initial state value 
        Toggle("toggle", isOn: $0)
    }
}
```

Alperen
  
I like this solution a lot! Also it seems you can use new parameter packs in generics for unlimited state parameters.
https://wwdc23.slack.com/archives/C058BFXG291/p1686251328052729?thread_ts=1686251115.945039&cid=C058BFXG291


Eliza B (Apple)
```
struct WithPreviewState<S>: View {
    @State var state: S
    var makeBody: (Binding<S>) -> any View
    
    init(
        _ state: S,
        @ViewBuilder makeBody: @escaping (Binding<S>) -> some View
    ) {
        self._state = State(initialValue: state)
        self.makeBody = makeBody
    }
    
    var body: some View {
        AnyView(makeBody($state))
    }
}
```
From a thread in swiftui | Jun 8th | View reply

Noam
  
That’s awesome! tysm!

----

@Bill
 asked:
First, I love being able to use previews on my UIKit code. It makes UI iteration so much more productive! I have one question: in my UIKit preview block, I need to initialize a singleton class that the view controller I'm previewing expects to use. The singleton class is marked @MainActor.
When I try to run the preview I get " error: main actor-isolated static property 'shared' can not be referenced from a non-isolated context".
This was surprising to me, since I thought preview code was already on the main actor since it's working with other @MainActor UI classes, like UIView and UIViewController.
How do you recommend resolving this? I was able to launch a Task to init the singleton on the main actor, but that means it happens in parallel to the loading of the view controller.
Can I make this happen sequentially?

WWDC Bot
APP  
@Charles A (Apple)
 answered:
This is a known issue.

Charles A (Apple)
  
Fortunately there is an easy workaround, which is to use this new function on MainActor: https://developer.apple.com/documentation/swift/mainactor/assumeisolated(_:file:line:)-swift.type.method
Apple Developer DocumentationApple Developer Documentation
assumeIsolated(_:file:line:) | Apple Developer Documentation
A safe way to synchronously assume that the current execution context belongs to the MainActor. (22 kB)


Charles A (Apple)
  
There was a similar question yesterday with some code examples, which you can review here:
https://wwdc23.slack.com/archives/C058BFXG291/p1686166750220869

WWDC Bot
@Dennis
 asked:
The new #Preview doesn't seem to work when you inject an @MainActor @Observable object into the environment of a View.
```
@Observable @MainActor final class ViewModel {}

#Preview {
    ContentView()
        .environment(ViewModel())
}
```
The preview crashes with the error: "Compiling failed: call to main actor-isolated initializer 'init()' in a synchronous nonisolated context".
Is that intentional or a bug?
Also I wanted to say: I love the new previews, it makes everything so much easier and more convenient :)
View original message
Show less
Thread in swiftui | Jun 7th | View message

Bill
  
Ooh perfect - thanks!
Does it help to file another feedback request?

Charles A (Apple)
  
While it never hurts, in this case we are well aware of the issue so there is probably no need. :slightly_smiling_face:

----

@Fredric Michael
 asked:
Hi,
When using a sheet on macOS, is there anyway to discard the keyboard shortcuts that affect the underlying view or entire app?
Since I want the user to be fully focused on the sheet content it seems odd that I can use keyboard shortcuts that affect the underlying content. Right now the workaround is to monitor NSEvents and basically discard non relevant events when the sheet is displayed.
Thanks
Michael

WWDC Bot
APP  
@Cody B (Apple)
 answered:
SwiftUI doesn't have anything automatic to help with this right now, so a feedback report describing your use cases would be welcome.

----

@Fabian
 asked:
We are using a very special setup at work where we are using cocoapods and each team develops their own pod (library) which then gets glued together into a single app. Previews never worked for us really with different error messages. Are there any changes in the preview generation with the new #Preview macro which might fix this?

WWDC Bot
APP  
@Kevin C (Apple)
 answered:
We're sorry you're having issues! The Preview macro is specifically improving the ergonomics around specifying what you want to preview. The mechanics for how this integrates with your project are part of the underlying preview engine. While we are constantly making improvements and bug fixes to the engine, and we know there are many cases surrounding the use of package management systems that still need to be addressed. For your issue, please file a Feedback request and attach the previews diagnostics Xcode generates (I'll post the details in a second on how to do that). We really love looking at the various usages of previews and try to account for all of the various project structures that exist. Post the feedback number back here and we can try to take a look during this Slack activity.

----

@Gleb
 asked:
Hi! I have a question about the availability. When I tried to use previews in projects with minimum deployment target lower than iOS 17, it gives me an error. How can I use it with lower minimum deployment target?
1 reply

WWDC Bot
APP  
@Charles A (Apple)
 answered:
This is a known issue. At the moment you have to set a deployment target of iOS 17 to use the new macro syntax.

----

@Naresh
 asked:
Do we have a know issue on SwiftUI previews where the keyboard does not show up when you click on a text field ? I never got it to work starting from Xcode 13. Sometimes it looks like the keyboard is hidden because if I click on the area where the keyboard is supposed to show up, it recognizes the characters in that area( only sometimes though)
See less
5 replies

WWDC Bot
APP  
@Eliza B (Apple)
 answered:
Yes, this is a known issue. Details in thread.

The keyboard unfortunately can't appear in the live preview in the Xcode canvas. However if you tap in the text field, you can type on your mac and the characters will be routed to the preview (as if your mac's keyboard is an external BT keyboard connected to the simulated iPhone)

To see what your app looks like with the software keyboard showing, you can either build & run and use the simulator, or preview on-device.


Naresh
  
Thanks for the response and confirmation. I think I am going to sleep well tonight knowing that it is not my Mac. :joy: :joy:


Eliza B (Apple)
  
Ha, sorry we made you doubt yourself!

----

@Christoph
 asked:
Is there a recommended way to pass a new SwiftData `@Model` into `#Preview`?

@Charles A (Apple)
 answered:
In your `#Preview` macro body you can create SwiftData model objects as needed that you can then pass in to your View init.

Depending on how your view interacts with SwiftData, you may also want to setup an in memory only ModelContainer with the modelContainer view modifier.

You can find some more details about that in the answer to this question from yesterday: https://wwdc23.slack.com/archives/C058BFXG291/p1686161829378699

```
#Preview {
    ContentView()
        .modelContainer(for: [MyModel.self], inMemory: true)
}
```
WWDC Bot
@Jan
 asked:
What is the best way to make SwiftData work in Previews? In Core Data I used a separate persistenceController.preview instance that was in memory using  "/dev/null"
View original message
Thread in swiftui | Jun 7th | View message

Christoph
  
ahh .... that sound good. I guess quite like the CoreData inMemory container init

Charles A (Apple)
  
Yes, just like that!

It is a little easier with SwiftData to create your objects for previews though, because they do not need a context for initialization.

----

@Alperen
 asked:
Is "#if DEBUG" thing in previews has a visible effect to size, performance, or any other thing in production? I personally don't prefer to use it, and not sure I should.
5 replies

WWDC Bot
APP  
@Jonathan P (Apple)
 answered:
This is a standard compiler conditional to hide code from release builds. You can use it for anything, including Previews code. Ideally, during a release build the optimization levels set for the compiler and linker end up stripping unused non-public code out. Often, relying on this is fine. But if…
See more


Jonathan P (Apple)
  
One benefit of using #if DEBUG is that it really does force you to make sure you don't expose anything public that you wanted stripped out. It's a compiler failure to reference anything in #if DEBUG if you try to build in release.
:raised_hands::raised_hands::skin-tone-2:

Jaanus
  
I use #if DEBUG to provide mock resources of various things, and use those in Previous.
```
#if DEBUG
extension SomeModelObject {
  static var forPreview: SomeModelObject { ... }
}
#endif
```

Noam
  
Isn’t it possible to put the sample data under Development Assets? Wouldn’t that remove it from the final release build as well?

Jaanus
  
You can put data in Preview Assets catalog and that works fine. One downside of that is that Preview Assets catalog is available only in toplevel app target, but not in SPM modules, I asked about it the other day here.

----

@Cristian-Mihai
 asked:
I saw that there's a known issue in which `#Preview` isn't compatible with targets earlier than iOS 17 in another question. This got me wondering: what's different? Shouldn't it work the same, since it should be same code except it uses macros?
6 replies

WWDC Bot
@Kevin C (Apple)
 answered:
It's not the same code :)  The preview macro is implemented with an entirely new preview API under the hood.

Christoph
  
:astonished:

Jonathan P (Apple)
  
To be clear, your code under preview is the same. :slightly_smiling_face: It's the registration of it that we rewrote and hide all the ugly bits behind the awesome macro.

----

@Jun
 asked:
How can I preview a view without being contained in an iPhone bezel nor a Mac Window?
I want to preview a single view component in its natural compact size but from Xcode 14 and above previews it in a iPhone bezel or a Mac Window, fitting edges to it.
8 replies

WWDC Bot
APP  
@Chris J (Apple)
 answered:
you can instruct the preview to show you a size-that-fits version in the "Selectable" mode in the preview canvas.


Chris J (Apple)
  
An example would be (using the new #Preview syntax):
```
#Preview(traits: .sizeThatFitsLayout) {
    ContentView()
}
```

Chris J (Apple)
  
With the older syntax it would be:
```
    static var previews: some View {
        ContentView()
            .previewLayout(.sizeThatFits)
    }
```

Jun
  
@Chris J (Apple)
 Thanks for the answer and codes! but I unable to do that.
How can I resolve these?
A: My content view still try to fit to screen edges. Xcode 13 Previews renders more compact, not being affected by iPhone bezel size
B: iPhone bezel is still drawn. it’s space consuming in vertical axis. I could see more view components at a time by stacking them vertically, If bezel were not drawn. (Xcode 13 Previews can do that…)

Chris J (Apple)
  
just to make sure we are on the same page, are you changing the canvas mode to the selectable like I did in these screenshots?
2 files
 
Screenshot 2023-06-08 at 13.29.45.png

Jun
  
@Chris J (Apple)
 Thank you for your detailed instructions! It can be improved with the combination of sizeThatFits layout & selectable mode:+1::skin-tone-2:
VStack-ing them still be affected by the iPhone screen height for now in my case but I will try with the options in your answers. thank you!

Chris J (Apple)
  
Please file feedbacks if you have suggestions for improvements we can make that would enable your use cases

----

@Antoine
 asked:
I'm finding it difficult to make SwiftUI previews work consistently.  My MacOS app uses about a dozen models.  However, the backbone of the interaction is with a centralized join table: a list of entries, each of which points to a record on one of the other 11 tables.  To make the view's Preview code work, I need to instantiate all these tables and pass them into the preview provider.  It seems that inevitably I end up giving up on making the preview actually work.  Are there any best-practices for this type of situation?  It's a bit of a drag to instantiate 12 @StateObjects and then use the same .environmentObject() modifiers, in each preview of 100+ different views (and I'm less than halfway done).

WWDC Bot
@Kevin C (Apple)
 answered:
Good question. I think you have the right intuition that trying to recreate all of these data models and State objects might not be worth the effort to make the preview work. My hunch here is that there's probably a way to break up your views into smaller pieces that don't require all of the data models. For example your "root view" (used by your app) could do all of this heavy lifting to load the tables and data. But then, your root view could quickly turn that loaded data into smaller sub-objects / value types that get passed into individual views. This would allow you to preview not the root view but rather the layer of views immediately below that.

----

@Cristina
 asked:
Hello! In order to make previews the most useful I use to write mock data but I'm also concerned about that mock data not ending up in the production build, so I developed the practice of putting sample files and sample code inside the Preview Content folder and then surrounding every Preview code with #if DEBUG. Do you think this is a good practice or have a better solution? If you think this is good, can I extend the new #Preview macro to be always only available in debug builds so that I don't have to put the same preprocessor macro in every view as I do now? Thanks!

WWDC Bot
@Kevin C (Apple)
 answered:
Yes, that is currently the best practice if you want to guarantee for sure that it does not show up in your production version. In general the linker does "dead code" stripping and will remove all of your preview providers, but the #if compiler conditional will provide the guarantee.
Currently the Preview macro is not expandable to automatically include the #if DEBUG, but this area is something that we're still thinking about, so please file a feedback request so we can gather some more use cases. Thanks!

----

@Witold
 asked:
Would it be possible for sizeThatFitsLayout to create a more fitting preview for the UIKit views? Right now it matches the screen size.
E.g. infer the size with Auto Layout (given the device width) or allow to define the size. Something like:
```
#Preview("Cell", traits: .sizeThatFitsLayout) {
    let cell = UITableViewCell()
    var grouped = UIListContentConfiguration.groupedFooter()
    grouped.text  = "hello hello"
    cell.contentConfiguration = grouped

    cell.frame.size = cell.systemLayoutSizeFitting(
        CGSize(width: UIScreen.main.bounds.width, height: 0),
        withHorizontalFittingPriority: .required,
        verticalFittingPriority: .fittingSizeLevel
    )

    return cell
}
```

@Kevin C (Apple)
 answered:
Ah, look like there is a bug with .sizeThatFitsLayout for UIKit, it should be a smaller size when in the Selectable / Static mode of the canvas, but we're seeing it's not too. Please file a Feedback request for us!

----


