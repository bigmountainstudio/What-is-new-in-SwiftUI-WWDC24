# Slack data-frameworks channel
WWDC Bot
APP  6 days ago
@Steven asked:
Does SwiftData have a way to perform data migrations?

@Rishi V (Apple)
Yes it does via the SchemaMigrationPlan and VersionedSchemas for those that are Lightweight or require a Custom Migration Stage

Patrick K (Apple)
This will get covered at length in Rishi’s video named Model your schema with SwiftData tomorrow morning! Make sure you check that out once it’s available
We think the data migration pattern in SwiftData is pretty slick!

@Amy asked:
Is it possible for CoreData and SwiftData to coexist in an app if support for older iOS / macOS versions is required?

Patrick K (Apple)
  6 days ago
You’ll be able to migrate from Core Data to a full SwiftData implementation or have both co-exist in the same project! Check out Luvena’s Migrate to SwiftData video Thursday morning for more information about both of those options!


Patrick K (Apple)
  6 days ago
Xcode even has support for generating SwiftData models in code from your existing Core Data managed object models!
:the_horns:



Ben T (Apple)
  6 days ago
I think the challenge here is that SwiftData is not available prior to iOS17.


Patrick K (Apple)
  6 days ago
And our other releases this year like macOS Sonoma and watchOS 10!


Patrick K (Apple)
  6 days ago
You’ll need to use some if available() logic in your app to make sure you don’t interact with SwiftData from previous versions. And, if you plan on supporting previous versions when iOS 17, macOS Sonoma, and others are released, you’ll need to support Core Data alongside SwiftData until you can update your app targets to only support the newest OSes.

@Jonathan
 asked:
With CoreData, you cannot use constraints with CloudKit.
With SwiftData there is the “@ Attribute(.unique)” property wrapper (?, if that’s what it’s called lol). Does this work with CloudKit?
:+1:
3
:brain:
1

4 replies

WWDC Bot
APP  6 days ago
@Nick G (Apple)
 answered:
No, you should file an enhancement request for unique constraint support when working with SwiftData + CloudKit.


Patrick K (Apple)
  6 days ago
If you use feedbackassistant.apple.com to file an enhancement request, we’d love if you could provide the Feedback ID here! We rely on awesome feedback from developers like you to drive a lot of future functionality, especially for brand new frameworks like SwiftData!


Jonathan
  6 days ago
Will do :+1::skin-tone-3:

----

@Matthew
 asked:
Does SwiftData have the capability to run fetch on updates in the View Model of a MVVM architecture?  Most VM objects provide data prepping for the view/swiftUI struct view and swiftData might not be able to support MVVM?  As a conversation topic SwiftData might eliminate View Models? Maybe MVVM archi…
See more
:heart:
1
:-1:
1

3 replies

WWDC Bot
APP  6 days ago
@Rishi V (Apple)
 answered:
SwiftData harnesses Observation to update the SwiftUI views and so the PersistentModels can be your VM objects and work great!


Patrick K (Apple)
  6 days ago
There’s an awesome video on observation named Discover Observation in SwiftUI that’s available now! (edited) 




Matthew
  6 days ago
Just to clarify, SwiftData is not intended to run outside of the SwiftUI Observation.  I’m kind-of thinking View Models are obviously not needed.  Which makes sense since “struct-view” is refreshed and reloaded on change.

-----
@Simon
 asked:
Hi, we have multiple apps that have severel 100k of database entries. Our first tests, with your WWDC23 demo project, show that writing as well as reading takes very long, at least some 6-7 seconds and consumes lots of memory on an iPhone 11 Pro (our reference device). Is this the performance we hav…
See more
:eyes:
2

3 replies

WWDC Bot
APP  6 days ago
@Ben T (Apple)
 answered:
Hi Simon.  There are some known issues in seed 1.  You can file a bug report to get updates


Patrick K (Apple)
  6 days ago
Use feedbackassistant.apple.com to let us know what you run into, and any enhancement requests you may have! We’ll be looking at your feedback during the beta period to make SwiftData even better!


Patrick K (Apple)
  6 days ago
If you file a feedback, post the Feedback ID here!

----

@Matthew asked:
In the past Core Data has needed attention concerning the managed object context.  When two threads access and try to save to the same managed context an exception happen infrequently/aperiodically.  Now this is with the iOS SDK 12, 13, 14.  Meaning the app can create an exception when two threads try…


@Ben T (Apple)
 answered:
@Model and ModelContext are not sendable.  You can't pass them to other actors.  You can create other ModelContexts using the ModelContainer (which is sendable) on other actors.

Matthew
  6 days ago
I’m still not fully understanding the answer.  Lets say I have a background thread running out of AppDelegate (maybe Core Data) and an update, because a communication came in,  the managed object (e.g. Boat).  Now if the user is tapping the boat manage view , swiftUI with @Model , and there is a update to Boat entity manage object is this answer still the right path to stop thread resource contention?


Ben T (Apple)
  6 days ago
In Swift, you'd use a detached Task or a custom Actor instead of an operation queue.  The compiler won't let you pass Model or ModelContext around since they are not sendable


Ben T (Apple)
  6 days ago
so you can pass the objectID and the ModelContainer and rehydrate them in the other actor

----

@Konrad asked:
Is it possible to use public, private and shared CloudKit databases with SwiftData ?
2 replies

WWDC Bot
APP  6 days ago
@Nick G (Apple)
 answered:
Yes, but only via co-existence. ModelConfiguration doesn't know how to express the capabilities of NSPersistentCloudKitConfiguration. You should file an enhancement request for that.
Today you will need an NSPersistentCloudKitContainer on the side to manage the store descriptions, mirroring, and shar…
See more
Apple DeveloperApple Developer
Migrate to SwiftData - WWDC23 - Videos - Apple Developer
Discover how you can start using SwiftData in your apps. We'll show you how to use Xcode to generate model classes from your existing... (13 kB)
https://developer.apple.com/videos/play/wwdc2023/10189

Apple Developer DocumentationApple Developer Documentation
Adopting SwiftData for a Core Data app | Apple Developer Documentation
Persist data in your app intuitively with the Swift native persistence framework. (22 kB)
https://developer.apple.com/documentation/coredata/adopting_swiftdata_for_a_core_data_app

----

@Brendan
 asked:
What's the recommendation if my app is mixed Objective-C and Swift using Core Data? My model layer is all in Swift. But I have many views and controllers that are written in Objective-C that still need to access Core Data entities. Can I use SwiftData models from Objective-C?




8 replies

WWDC Bot
APP  6 days ago
@Rishi V (Apple)
 answered:
SwiftData uses Swift native classes and cannot be used with Objective-C.  However you can coexist with Core Data and use new views with SwiftUI to start your SwiftData adoption!


David S (Apple)
  6 days ago
In this case, you may want to consider having two different stacks in your app, a SwiftData stack and a CoreData stack. There are some considerations for this such as making sure the Schema and the managed object model are entity version hash equivalent. Also, so that the classes can co-exist, you’d need to rename one of them but have it point to the same entity name.
:+1:
1



Brendan
  6 days ago
So does that mean that the underlying SQLite schema will be identical between CoreData entities and SwiftData entities? I.e. i can fetch for example a CDTag object in my database from Objective-C and a SDTag object in SwiftUI and they are both pointing to the same row in the database? How does that affect uniquing of objects in memory? (edited) 


David S (Apple)
  6 days ago
Yes, that is correct, they must be identical


David S (Apple)
  6 days ago
You can verify by checking entityVersionHashesByName


David S (Apple)
  6 days ago
When you generate your Schema you can make a MOM out of that then call entityVersionHashesByName


Brendan
  6 days ago
But if I have some code that modifies the CoreData object, will the SwiftData version of that object hear about the update? And vice versa?


David S (Apple)
  6 days ago
You would need to re-fetch the object but yes, the changes are written to the same store

Patrick K (Apple)
For the enhancement request, check out feedbackassistant.apple.com and reply with your Feedback ID once you have one!

----

@Axel asked:
Are the limitation from Core Data + CloudKit the same for when SwiftData is used with CloudKit? For example, can we use @Attribute(.unique) for a ModelContainer configured with CloudKit?
1 reply

@Rishi V (Apple)
 answered:
The same limitations apply, please file feedback for any enhancements!

----

@Marco asked:
Hi! I would like to know if it's possibile to use SwiftData to save on the cloud with CloudKit easily, in a similar way as it is possible with CoreData (using a CloudKit Persistent Cointainer).
1 reply


@Luvena H (Apple) answered:
Yes, SwiftData works with CloudKit as long as you have the correct CloudKit entitlements for your app enabled. For a more complex configuration, it is always an option to coexist with CoreData and SwiftData to still use an NSPersistentCloudKitContainer

----

@Dean asked:
Does SwiftData allow for optional Int/Double types? For CoreData those values are always defaulted to 0 in fetched objects, seemingly because of their bridging to Objective-C.
1 reply

@Rishi V (Apple) answered:
Indeed SwiftData does allow for optionals of scalar types.

----

@Aidan asked:
I see that BackingData is declared as a protocol, with CoreDataBackingData being a concrete implementation. Does this mean we can insert a custom persistence mechanism into this system?

APP  6 days ago
@Ben T (Apple)
 answered:
Not at this time

----

@Jeannine asked:
I love the idea of automatic syncing to iCloud, but it seems like there are a lot of edge cases and I’d like to know if we have to handle them ourselves or if they’re handled by SwiftData. For example:
What happens for new users that are not logged-in to iCloud? Is that something I have to manually check for and handle by creating a local-only configuration?
What happens when an existing user logs-out of iCloud? Do I have to manually cleanup anything? Is their data wiped from the device?
In the Notes app, there’s something like an “On this Mac” folder for non-iCloud data. Is that the design pattern we should use when designing for these edge kinds of cases?

@Nick G (Apple)
 answered:
What happens for new users that are not logged-in to iCloud? Is that something I have to manually check for and handle by creating a local-only configuration?
This is automatically handled.
What happens when an existing user logs-out of iCloud? Do I have to manually cleanup anything? Is their data wiped from the device?
This is also handled automatically. Data is tied to the iCloud account signed in to the device, so it will be deleted on sign out.
In the Notes app, there’s something like an “On this Mac” folder for non-iCloud data. Is that the design pattern we should use when designing for these edge kinds of cases?
You a free to maintain multiple stores, some that sync and others that don’t. Your “on this mac” store would be a store without iCloud support.

----

@Bryan asked:
How do we maintain backwards compatibility in the same code base with a Core Data app? Not everyone updates on day one.


@Rishi V (Apple) answered:
SwiftData is iOS 17+ and can coexist with Core Data for backwards compatibility.  Developers can explore using SwiftData to implement new features for future releases

----

@Bryan asked:
Cored Data optionals are not Swift optionals. Are the optionals in SwiftData Swift optionals?

@Amit G (Apple) answered:
Yes, optional attribute of your Model are Swift Optional.

----

@Dachary asked:
How many levels deep can a cascading delete - delete? i.e. if a Person has a relationship to Dog, and if Dog has a relationship to Toy, can a cascading delete delete from Person delete Toy or just Dog?

@David S (Apple) answered:
As long as the relationship is set up as cascade all the way down, it will continue to cascade.

----

@Donny asked:
In Core Data I often used child contexts to allow my SwiftUI views to manipulate a copy of a managed object that I could either discard or save depending on the user's action. Is there a similar mechanism in SwiftData or can we only have 1 instance of a given underlying model object due to SwiftData not having child or background model contexts?

@Ben T (Apple) answered:
SwiftData doesn't currently offer child contexts, but you can create background ModelContexts as much as you like.

Donny
Thanks! Could you elaborate a little bit on how that's done? I haven't seen an option for that in the documentation. Or at least not one that I interpreted as background context (edited) 

Christopher
In the Meet Swift Data talk they mentioned creating new contexts using ModelContext(container), I assumed this was a solution for this use-case. Maybe the docs around this will be more detailed soon, https://developer.apple.com/documentation/swiftdata/modelcontext/init(_:)

----

@Axel asked:
It does not seem possible to group results from a Query (like it is possible with NSFetchRequest, NSFetchedResultsController or SectionedFetchRequest). Do you confirm this is not supported? Here is my FB12236319, because I really think this should be in the framework (in both Query and FetchDescriptor)

@Ben T (Apple) answered:
thanks

----

@James asked:
Is SwiftData deployable to older OS versions?

@David S (Apple) answered:
No.

----

@Marin asked:
I've recently started transitioning my unreleased app core data. It previously used a JSON file to save and load data, which is not ideal because the entire file was loaded each time. Would it be better to pause my transition to core data and wait for Swift Data, or should I continue the transition to core data and then to Swift Data later on?

@Rishi V (Apple) answered:
Depending on your backwards compatibility goals, SwiftData is iOS 17+ and Core Data goes back many releases.  You will most likely want to coexist for a few releases

----

@Patrice asked:
Do we need to maintain a core data model?

@David S (Apple) answered:
Only if you want to co-exist.

----

@Axel asked:
How should we handle large data import that used to be made on a background context associated with a background queue? Do we also need another Core Data stack (with a NSPersistentContainer) that coexist with the ModelContainer used in the UI?

@Ben T (Apple) answered:
Make a modelContext on a separate actor.

----

@Matt asked:
After your data store is converted to SwiftData, can you read that store with existing UIKit CoreData and through SwiftData?

@David S (Apple) answered:
Yes, using co-existence. The MOM and the Schema must be entity version hash equivalent

Brendan
  6 days ago
How does one ensure they’re entity version hash equivalent? Sorry if that’s a dumb question. Is it just maintaining the exact same set of attributes, relationships, and properties in the model for an entity?

David S (Apple)
Yes, you can verify by checking entityVersionHashesByName

When you generate your Schema you can make a MOM out of that then call entityVersionHashesByName

----

@Amy asked:
If SwiftData and CoreData exist in the same app, do they both use the same underlying store?

@Luvena H (Apple)
 answered:
Yes, the two stacks are talking to the same persistent store

Patrick K (Apple)
Check out Luvena’s video Migrate to SwiftData to learn more about using Core Data and SwiftData in the same app!

----

@Duncan asked:
Does SwiftData offer a complete replacement for Core Data at this time? In other words, in a fully Swift codebase, could the entirety of an existing CoreData + NSPersistentCloudKitContainer setup be switched over to SwiftData, with all existing capabilities?

@Rishi V (Apple) answered:
There are a few aspects that are not possible such as Abstract Entities but please let us know via Feedback of any enhancements that block your replacement

----

@Tom asked:
Does SwiftData support context merging? And if not, what is the standard way to create and edit entities without affecting the main context?

@Ben T (Apple) answered:
It does, although there are some known issues in seed 1.  You can create additional ModelContexts with its init method.

----

@Charles asked:
Does SwiftData support extensions? For example, can changes to persisted storage from an extension be told to the main app?

@Dave N (Apple) answered:
Yes, using a shared app container, they extension & your main app can write to the same database.

----

@Michael asked:
Does a Model that has a unique attribute automatically conform to Identifiable?

@Rishi V (Apple) answered:
PersistentModel is Identifiable but it does not use the unique attributes for this, please file a feedback with an enhancement request with your use case!

----

@ILDAR asked:
Are there any ways to organize mutation of SwiftData store in transactions similar to sqlite?

@Nick G (Apple) answered:
Mutations to model objects are captured in a ModelContext until save is called. Save necessarily creates a transaction.

----

@Marco asked:
If I've understood correctly, if I want to save my SwiftData data into a CloudKit  private/shared database, I have to manually mirror the SwiftData models to cloudkit entities / NSPersistentCloudKitContainer managed objects, so what you mean is that SwiftData can coexist with CloudKit, but SwiftData doesn't knows anything about cloudkit and can't automatically mirror data on it like CoreData can do instead, right ?

@Rishi V (Apple) answered:
There is no need to manually mirror, setting up a ModelContainer with a ModelConfiguration noting your CK Entitlement will setup a persistent backend with CK syncing enabled!

Marco
Ok, there is a session where do you explain how to do that that i can see ?


Rishi V (Apple)
I do believe that may be noted in the Dive deeper into SwiftData

----

@ILDAR asked:
Are there any ways to encrypt data on db level? How I can request to fully encrypt the database?

@Amit G (Apple) answered:
you can define your Model Attribute with option @Attribute (.encrypt) to encrypt your property.

----

@Donny asked:
Does SwiftData provide support for CloudKit record sharing in a similar way as NSPersistentCloudKitContainer does?

@Nick G (Apple) answered:
SwiftData supports synchronization with the CloudKit private database via the cloudKitContainerIdentifier (or just having one in your entitlements) property on ModelConfiguration. https://developer.apple.com/documentation/swiftdata/modelconfiguration/
If you want to use the Shared or Public databases you will need to use NSPersistentCloudKitContainer.

----

@Brandon asked:
Is there a recommended SwiftData analog for the "Loading and Displaying a Large Data Feed" sample Core Data project? My app inserts lots of non-deterministic entities one-by-one over short time periods

@Dave N (Apple) answered:
Implement an object that conforms to the ModelActor protocol to allow you to do your work on a background queue.  Use your ModelContainer to create the ModelContext that is used to initialize an instance of DefaultModelExecutor.  Initialize this object on your background queue and do your work on a function on this actor using it's context.

----

@Patrice asked:
How does transformable attribute, like UIColor, works in swift data ?

@David S (Apple) answered:
I would recommend using NSCompositeAttributeDescription for this.

----

@Dachary asked:
Is binary data a supported data type?

@Rishi V (Apple) answered:
Yes it is!

----

@Charles
 asked:
Are batch/bulk operations supported in SwiftData?
1 reply

WWDC Bot
APP  6 days ago
@Amit G (Apple)
 answered:
Yes, you can perform batch operations.

----

@Luc-Olivier
 asked:
Does SwiftData support the Core Data launch argument debug tools? (i.e. -com.apple.CoreData.SQLDebug 4 and friends)
1 reply

WWDC Bot
APP  6 days ago
@Amit G (Apple)
 answered:
Yes, it does support.

----

@Kevin
 asked:
From the demos it looks like using persistent history to coordinate between apps and their extensions is not longer required or internally handled. Is that the case?

@Luvena H (Apple)
 answered:
Yes, SwiftData automatically turns on persistent history tracking

Kevin
  6 days ago
So I assume that means changes in extension are merged into the main application and vice-versa and transaction history are intelligently deleted.

Rishi V (Apple)
  6 days ago
Indeed the remote changes will be used by the main application to consume the changes

----

@Brendan
 asked:
Does SwiftData support SUBQUERY()? And how about derived attributes?

@Jeremy S (Apple)
 answered:
Yes! You can use #Predicate to create predicates to use for your fetch queries which can include operations like calling .filter(:) on collections, which is supported with SwiftData. If there are additional operations you'd like to support in your predicates with SwiftData that you don't see supported, feel free to send us a feedback request!
However, derived attributes like in CoreData are not supported. You can write a computed property and label it with the @Transient attribute instead which may help but not quite the same as CoreData's derived attributes.

----

@Konrad asked:
Do we have the equivalent of CoreData abstract classes ?

@David S (Apple)
 answered:
No, if this is something you'd like to request support for, please file a Feedback Report.

----

@Naftali asked:
How does model versioning work in SwiftData?

@David S (Apple) answered:
We recommend using enums to version your models.

Bryan
  6 days ago
Can you explain that in more detail?


David S (Apple)
  6 days ago
For example,
```
enum ModelVersion1 {
  @Model public class Entity
  ...
}

enum ModelVersion2 {
  @Model public class Entity
}
```

David S (Apple)
You can use typealias to always point to the most recent one.

----

@Aidan asked:
Are SwiftData models thread restricted in the same way as NSManagedObject models? That is, can you only use them on the thread in which they were created? If so, what is the best practice when using these models in a system that utilizes swift concurrency?
1 reply

WWDC Bot
APP  6 days ago
@Ben T (Apple)
 answered:
@Model and ModelContexts are not sendable.  You can create new model contexts on other actors with the sendable ModelContainer

----

@Aidan
 asked:
Are SwiftData models thread restricted in the same way as NSManagedObject models? That is, can you only use them on the thread in which they were created? If so, what is the best practice when using these models in a system that utilizes swift concurrency?
1 reply

WWDC Bot
APP  6 days ago
@Ben T (Apple)
 answered:
@Model and ModelContexts are not sendable.  You can create new model contexts on other actors with the sendable ModelContainer

----

@Joel
 asked:
What do we need to know about the multithreading story? Is it functionally different from CoreData in any way?
1 reply

WWDC Bot
APP  6 days ago
@Ben T (Apple)
 answered:
the compiler will enforce sendability

----

@Heiko
 asked:
We are using view model controllers to prepare model data for our views (sorting, grouping, augmenting, transforming…) to make the logic testable. How can SwiftData fetches be performed except for the @Query property wrapper that only works in views I suppose?
1 reply

WWDC Bot
APP  6 days ago
@Matt R (Apple)
 answered:
You can perform fetches directly on a ModelContext instance for full control, which is the same API that @Query uses under-the-hood.

----

@Bryan
 asked:
When dealing with larger datasets, how do we designate the thread to run the context on?
1 reply

WWDC Bot
APP  6 days ago
@Ben T (Apple)
 answered:
Make a ModelContext on a separate actor or Task

----

@James
 asked:
I see there is an externalStorage property option, does Swift Data handle the location of the object and hold a reference?
If I wanted to store images would this be what I should be using and should I use the SwiftUI Image type or raw data?
1 reply

WWDC Bot
APP  6 days ago
@David S (Apple)
 answered:
Yes, SwiftData will manage the location. Use Data

----

@Brennon
 asked:
Can you briefly talk about persisting encrypted data using SwiftData?  Does SwiftData offer any form of encrypted storage (beyond the traditional data protection levels in iOS) or would I need to encrypt/decrypt the data myself before storing and after fetching?
5 replies

WWDC Bot
APP  6 days ago
@David S (Apple)
 answered:
You should be using the traditional data protection levels for encryption at rest on device. You can use .encrypt for encryption in CloudKit.

Tales
  6 days ago
the .encrypt attribute (cited on another answer as well) is just for CloudKit encryption?


Brendan
  6 days ago
I don’t think the standard iOS protection levels work on Mac though, do they? Would be great to be able to specify an encryption key for CoreData / SwiftData to separately encrypt the entire store.

David S (Apple)
  6 days ago
@Tales
 Correct.
@Brendan
 I believe they do

----

@Charles
 asked:
Following up on versioning, will SwiftData let you control when a lightweight migration is performed instead of doing it automatically on instantiation as it is in CoreData?
1 reply

WWDC Bot
APP  6 days ago
@David S (Apple)
 answered:
To open a store, SwiftData must migrate the store to the version you indicated is the most recent. There is however VersionedSchema and SchemaMigrationPlan to give you more fine-grained migration control.

----

@ILDAR
 asked:
Are there any limitations in concurrent access from different processes, let's say App and SSO Extension?
1 reply

WWDC Bot
APP  6 days ago
@Rishi V (Apple)
 answered:
There are no limitations in the concurrent access as long as the two use the Shared App Group

----

@David
 asked:
I understand that I need to use context.save to confirm transactions. What is the best practice on how often this can be done? is it very demanding doing soon after each change or it depends on whether we use CloudKit vs local storage?
1 reply

WWDC Bot
APP  6 days ago
@David S (Apple)
 answered:
You should batch save transactions according to your requirements, but a save for each change is aggressive. CloudKit has a different event loop for mirroring those changes.

----

@Michael
 asked:
Is there a way of viewing the datastore behind SwiftData so as to verify data is being stored as expected? For example, when using the GRDB library with Swift, I can view the SQLite database file in a SQLite client to ensure data is being persisted as expected.
2 replies

WWDC Bot
APP  6 days ago
@Rishi V (Apple)
 answered:
The same can be done with your underlying persistent back end with SwiftData


Ben T (Apple)
  6 days ago
you can view the underlying Core Data (sqlite) file.  Mutating it is not supported

----

@Duncan
 asked:
As I understand it, SwiftData model objects are not thread-safe, just like NSManagedObjects. Are there any additional mechanisms to make managing this easier for us than it was in traditional Core Data? e.g., compiler warnings before passing an object out of its context?
2 replies

WWDC Bot
APP  6 days ago
@Dave N (Apple)
 answered:
We have provided the ModelActor protocol & the DefaultModelExecutor to make SwiftData work with Swift Concurrency.  Use your ModelContainer to initialize a ModelContext in the initializer for your ModelActor conforming actor object and use that context to initialize a DefaultModelExecutor.  This will allow you to use that context with async functions on your actor.

Ben T (Apple)
  6 days ago
Swift will enforce sendability requirements

----

@Simon
 asked:
When we fetch data, is the whole data then directly read and written to memory? Or is the fetch result a descriptor and data will only be read when it is actually used?
2 replies

WWDC Bot
APP  6 days ago
@Dave N (Apple)
 answered:
FetchDescriptor has an option to set fetchLimit & fetchOffset so you can page the data in chunks.


Scott P (Apple)
  6 days ago
Note that using fetchOffset will still cause the database to (re)process all records before the offset. SwiftData does include faults as a concept, but if you find you need them somewhere you're not getting them today you can DIY using FetchIdentifiers (edited) 

WWDC Bot
@Ben T (Apple)
 answered:
SwiftData supports internal Futures like CoreData, but at different granularities since value type properties can't support side effects.

----

@Duncan
 asked:
Do SwiftData model objects return faults, just like NSManagedObjects, where only the specifically fetched properties are safe to access?
1 reply

WWDC Bot
APP  6 days ago
@Ben T (Apple)
 answered:
SwiftData supports internal Futures like CoreData, but at different granularities since value type properties can't support side effects.

----

@Yuhan
 asked:
Currently I’m using a custom store that conforms NSIncrementalStore protocol as the underlying storage instead of the default SQLite. However, I browsed the SwiftData API documentation, noticed that there doesn't seem to be an option can be used to specify the store type.
I’m wondering if there’re any plans to support other store types in SwiftData?
Is it possible that I could use NSIncrementalStore and NSAtomicStore etc. with SwiftData in the future?


WWDC Bot
APP  6 days ago
@Ben T (Apple)
 answered:
Please file a feedback report.

----

@Ricky
 asked:
Will SwiftData render my app obsolete if I don't migrate?
2 replies

WWDC Bot
APP  6 days ago
@Ben T (Apple)
 answered:
No

Patrick K (Apple)
  6 days ago
SwiftData is an awesome persistence framework, but using Core Data is still a great option as well! Check out the What’s new in Core Data video to learn about some new Core Data functionality this year!

----

@Matthew
 asked:
Can a swiftData model be searilized or stored in some format that i can just restore when the user loads the app for the first time?


WWDC Bot
APP  6 days ago
@Rishi V (Apple)
 answered:
Yes a file can be saved as a Resource in the application bundle to be used as the seed data for the first launch experience.  Much like it was done in Core Data, the same approach can be used in SwiftData for the canned data in the resources.

----

@Renan
 asked:
Does SwiftData support CloudKit sharing with ShareLink and Transferable. If not, can we still achieve this by having some sort of CoreData code on the side? If so, are there any sample code for this?
2 replies

WWDC Bot
APP  6 days ago
@Nick G (Apple)
 answered:
SwiftData doesn’t know about the shared CloudKit database. So you will necessarily need to have an instance of NSPersistentCloudKitContainer on the side to manage that.
That said, your SwiftData model can conform to Transferable and assemble a suitable transfer representation for you to identify it in NSPersistentCloudKitContainer.
ShareLink is orthogonal (independent) to NSPersistentCloudKitContainer, NSPersistentCloudKitContainer will be invoked at the point that you resolve your transfer item to a CKShareMetadata.
For more information on CKShareTransferRepresentation see this document, which also links a number of pieces of sample code including one that uses NSPersistentCloudKitContainer: https://developer.apple.com/documentation/cloudkit/cksharetransferrepresentation?changes=_1

---

@Yuhao
 asked:
Is it possible to specify how to sort optional properties for nil values using @Query?
2 replies

WWDC Bot
APP  6 days ago
@Scott P (Apple)
 answered:
Please file a feedback report
(No)

----

@Bryan
 asked:
Is SwiftData faulted like CoreData?
2 replies

WWDC Bot
APP  6 days ago
@David S (Apple)
 answered:
SwiftData supports internal Futures like CoreData, but at different granularities since value type properties can’t support side effects.

----

@Charles
 asked:
With CoreData you can define an index for performance. Does a similar thing exist for SwiftData?
1 reply

WWDC Bot
APP  6 days ago
@David S (Apple)
 answered:
Yes, you can use .index on the @Attribute macro

----

@Christopher asked:
Can the @Query property wrapper handle nested/sub queries? If I need to fetch some subset of Model A as part of my query for Model B what would be the best way to do that?
4 replies

WWDC Bot
APP  6 days ago
@Jeremy S (Apple)
 answered:
You can write traditional CoreData SUBQUERYs using operations like .filter(_:) in your #Predicate in order to compose complex operations. For example, if your Model A had a one-to-many relationship to Model B you can use filter(_:) in your predicate to get a subset of model B's to perform an operation on in your query.


Jeremy S (Apple)
  6 days ago
If that's not quite the type of subquery you're trying to do, feel free to provide an example of your model relationships or an example of the query you're trying to perform and we can help you figure out the best way to write it!


Christopher
  6 days ago
For example, if I have a 1:M relationship between Model A and Model B and I want to fetch all objects of A that has no related Model B. Would I first fetch all unique values of Model B and use that to fetch Model A objects that are not in the first result?


Jeremy S (Apple)
  6 days ago
So if you have some
@Model
class ModelA {
    let modelBs: [ModelB]
}
You can write
#Predicate<ModelA> { modelA in
    modelA.modelBs.isEmpty
}
and provide it to a fetch request or @Query and it will filter the results for all ModelAs that don't have any ModelBs (edited) 

----

@Heiko asked:
Will a synced SwiftData be updated when the app is not running? If yes, can we schedule a background task in this event for a timely update of the app state? E.g. sync a timer and use the event to update widgets or schedule user notifications.
1 reply

WWDC Bot
APP  6 days ago
@Nick G (Apple)
 answered:
You should enable the background activity capability in your application, the system may choose to launch your application for networking activities and when that happens you can initialize your ModelContainer to do work with CloudKit.

----

@Tales
 asked:
Can a @Model contains a property of the same type, in a recursive way? Like a @Model Person having a 'father: Person' and 'children: [Person]'?
2 replies

WWDC Bot
APP  6 days ago
@Scott P (Apple)
 answered:
Yes! Modeled types that refer to each other are manifested in the schema as relationships.

Scott P (Apple)
  6 days ago
Where the type being the same makes a difference is that you may have to indicate the relationship's inverse explicitly because they won't be something the runtime can infer.

----

@Yuhan asked:
Does SwiftData define index in a different way? I couldn't see anything like @index or any other APIs includes index, is it possible to define index in SwiftData?
2 replies

WWDC Bot
APP  6 days ago
@Rishi V (Apple)
 answered:
Yes in a future seed!

Patrick K (Apple)
  6 days ago
SwiftData will continue to evolve throughout the beta window! If there are enhancements you’d like to see, please use feedbackassistant.apple.com to let us know!

----

@Kevin
 asked:
How are dynamic predicates handled with @Query? Is that covered in later sessions?

WWDC Bot
APP  6 days ago
@Matt R (Apple)
 answered:
Hi Kevin! If you're referring to dynamically changing the predicate passed to a @Query property, the current recommended approach is to manually instantiate the Query in your view's initializer. For example, this view uses a boolean value passed into the view's initializer to construct the query:
```
struct ContentView: View {
    @Query private var favoritePeople: [Person]
    
    init(showFavoritesOnly: Bool) {
        _people = Query(filter: #Predicate { $0.isFavorite || !showFavoritesOnly })
    }
    
    var body: some View {
        List(favoritePeople) { person in
            Text(person.name)
        }
    }
}
```
A view that contains ContentView in this example can drive the showFavoritesOnly parameter in any way it needs to, such as from a @State property or other source.

Scott P (Apple)
  6 days ago
You can also find this pattern in this year's Backyard Birds sample app, which should be available soon

----

@Patrice
 asked:
is the composite attributes, compatible with swiftdata ?
1 reply

WWDC Bot
APP  4 days ago
@Rishi V (Apple)
 answered:
Indeed it is, that is how a struct or enum that is Codable  in SwiftData will be represented in a coexistence experience with Core Data.

----

@Bastian
 asked:
In the sessions @Model seems to be always used in conjunction with classes. Can it also be used with structs to get easy Equatable conformances for e.g. testing?
1 reply

WWDC Bot
APP  4 days ago
@Scott P (Apple)
 answered:
No. The nature of @Model types requires reference sematics. Consider Self-referential relationships and how the context would keep track of (and propagate) changes to the objects that it's responsible for.

----

@Martin
 asked:
Hey all! I'd like to ask what would you recommend for gracefully handling `NSInternalInconsistencyException
This NSPersistentStoreCoordinator has no persistent stores (disk full). It cannot perform a save operation. ` ? Thanks!
2 replies

WWDC Bot
APP  4 days ago
@Ben T (Apple)
 answered:
You really need to handle the error when the store fails to load and we give you the error in addPersistentStore or loadPersistentStores


Ben T (Apple)
  4 days ago
It is a programmatic failure to write to a database you never opened

----

@Christopher
 asked:
How would we create new instances of models from a view model? I see that we have access to the model context via @Environment(\.modelContext) but if we want to move our logic to a view model how would we insert/delete objects?
1 reply

WWDC Bot
APP  4 days ago
@Rishi V (Apple)
 answered:
A ModelContext can also be obtained from the ModelContainer via mainContext or you can create a ModelContext with a given container

----

@Bryan
 asked:
Is the staged migration backwards compatible, or is it 17+ only?
1 reply

WWDC Bot
APP  4 days ago
@David S (Apple)
 answered:
Great question! It is available in iOS 17 and macOS Sonoma and later

----

@Alessandro
 asked:
Does SwiftData support batch processing?
1 reply

WWDC Bot
APP  4 days ago
@Rishi V (Apple)
 answered:
The ModelContext has API for batch processing, check it out!

----

@Bryan
 asked:
What is the process in SwiftData for downloading data from a URLSession into a model? Is it as simple as decoding with the model?
5 replies

WWDC Bot
APP  4 days ago
@Rishi V (Apple)
 answered:
Decode the model and insert into a ModelContext!


Bryan
  4 days ago
What is the syntax for that?
The insert into ModelContext part?


Rishi V (Apple)
  4 days ago
context.insert(myinstance)

----

@David
 asked:
Is there documentation and/or sample code on syncing SwiftData via iCloud?
2 replies

WWDC Bot
APP  4 days ago
@Rishi V (Apple)
 answered:
Keep an eye on https://developer.apple.com/documentation/swiftdata !

----

@Bryan
 asked:
I have inherited a CoreData store that reflects a model downloaded from a server, but only has some of the parameters as attributes. The entire object was then also stored into an attribute on the entity of type Data. One of my summer projects is to unwind this mess, and make all the parameters into attributes on the model entity through a migration(s). What would be the best way to go about this for an iOS app supporting iOS 16+?

7 replies

WWDC Bot
APP  4 days ago
@David S (Apple)
 answered:
That's a good question!

Bryan
  4 days ago
I wish it wasn't mine!

David S (Apple)
  4 days ago
What you could do is something similar to what I described in Evolve Your App’s Schema in WWDC22

David S (Apple)
  4 days ago
Basically you would build your own “staged migration”

Bryan
  4 days ago
Somehow I missed that in '22

David S (Apple)
  4 days ago
Yeah, I kind of describe the process in that session

David S (Apple)
  4 days ago
Essentially you’re building a state machine to iterate through all your models and you’d perform the transformations you want prior to or after each migration

----

@Natanel
 asked:
Is there a way to fetch models from SwiftData outside of SwiftUI views like in a view model instead?
:man-facepalming:
1

4 replies

WWDC Bot
APP  4 days ago
@Rishi V (Apple)
 answered:
Indeed there is with fetch() on the ModelContext


Natanel
  4 days ago
Cool thanks! Curious, why is that method not async if the models might need to be fetched from iCloud when CloudKit is enabled? 
@Rishi V (Apple)
  (edited) 


Rishi V (Apple)
  4 days ago
That is a good question,  the fetch() only checks for data that has already synced to the local data store and does not query the CK Container directly

----

@Viennarz
 asked:
What are the general tips and practices to avoid data loss when performing data mode migration?
7 replies

WWDC Bot
APP  4 days ago
@David S (Apple)
 answered:
Great question!
You definitely need to be careful of the changes you make in your model
For example, don’t truncate values in columns that have larger types, like going from Int64 to Int16
I highly recommend building tests that perform the migration from each version to every other version to make sure they’ll always migrate without data loss

The key here is to have actual data in the store when you do it!

Otherwise you won’t detect incompatible changes.

----

@Brendan
 asked:
How do you model many to many relationships in SwiftData?
:heart:
2

2 replies

WWDC Bot
APP  3 days ago
@Ben T (Apple)
 answered:
You can make the relationship and its inverse both collection types

----

@Manuel
 asked:
How does SwiftData integrates with CloudKit?
4 replies

WWDC Bot
APP  3 days ago
@Amit G (Apple)
 answered:
To Integrate CloudKit, you can enable "CloudKit" option while creating the project Or For your target, Capability add support for iCloud & CloudKit with a container

Manuel
  3 days ago
I did that, but the records from my public database isn't downloaded. Do I have to write code to fetch from CloudKit?


Luvena H (Apple)
  3 days ago
If you want to use Public databases, you will need to coexist with Core Data and use the NSPersistentCloudKitContainer (edited) 

----

@Yannik
 asked:
I have the following two entities referring each other:
```
@Model class Item {
   ...
   @Relationship(inverse: \Category.items) var categories: [Category]?
}
@Model class Category {
   ...
   @Relationship(inverse: \Item.categories) var items: [Item]?
}
```
But in both relationship lines I get the error: "Circular reference resolving attached macro 'Relationship'". What am I doing wrong?
This was automatically generate from the old CoreData scheme by the way.
See less
:heart:
2
:raised_hands:
2

3 replies

WWDC Bot
APP  3 days ago
@Scott P (Apple)
 answered:
Setting the inverse on only one of the models (or neither, and letting SwiftData infer their association) should get this working. Could you file a feedback report against the Xcode migrator with your model please?

----

@David
 asked:
How does autosave work with validations? What happens if the timer goes to save the context but there is an invalid object?

WWDC Bot
APP  3 days ago
@Ben T (Apple)
 answered:
The pending changes will remain in the context until you call save explicitly, or fix the validation issue and get picked up by the next autosave.

----

@Nico
 asked:
Is there a way to use SwiftData without automatic iCloud sync? I’d like to do that manually using my own CloudKit solution or CKSyncEngine. SwiftData automatically picks up any CloudKit containers though and I have not seen an option to disable this behavior. Setting cloudKitContainerIdentifier to n…
See more
:eyes:
1

4 replies

WWDC Bot
APP  3 days ago
@David S (Apple)
 answered:
This sounds like a bug. Please file a feedback report.

----

@Ashley
 asked:
If I save a custom ModelContext for a Container, will the other ModelContext’s derived from that Container (both custom and ‘main’) automatically be updated?
:eyes:
1

1 reply

WWDC Bot
APP  3 days ago
@Ben T (Apple)
 answered:
They will, although there are some known issues in seed 1.

----

@William
 asked:
Does SwiftData handle the URLSession, CoreData, and persistence?  I guess I’m confused about what all SwiftData can handle.    I look at theLoadingAndDisplayingALargeDataFeed  and do not consider that large data.  I need to handle JSON data from several sites and wondered if SwiftData is able to handle t…
See more
2 replies

WWDC Bot
APP  3 days ago
@Rishi V (Apple)
 answered:
For SwiftData, you would need to decode your models and then` insert()` them in the ModelContext , there is no direct`URLSession` support but please file a feedback request with your use case

---

@Duncan
 asked:
Core Data has added new capabilities this year, such as new ways for handling migrations. Are these new capabilities also all available in SwiftData? (I realise this may be covered in a video—I have not been able to view all of them yet.)
1 reply

WWDC Bot
APP  3 days ago
@David S (Apple)
 answered:
Yes. In SwiftData, you’ll use VersionedSchema and SchemaMigrationPlan.

----

@Frank
 asked:
This isn't super clear to me; does SwiftData take care of our local persistence store? Does this mean we don't need to use CoreData?
1 reply

WWDC Bot
APP  3 days ago
@Ben T (Apple)
 answered:
You can use either, or both, as makes sense for your app

---

@Keith
 asked:
What happens when I have a view observing a model object that is deleted elsewhere? Is there a way to detect the object has been deleted? The trips sample code crashes in this situation with two windows open on an iPad (see FB12272702). One window showing list of trips, other window showing trip det…
See more
:eyes:
1

6 replies

WWDC Bot
APP  3 days ago
@Rishi V (Apple)
 answered:
as a workaround, can you try calling save() after the delete() to see if that resolves the crash?





Keith
  3 days ago
Adding a save() did not resolve the crash


Rishi V (Apple)
  3 days ago
Thanks for the update, we will add that to the feedback report!
:raised_hands:
1



Keith
  3 days ago
But the real question is what should happen in this case? It would be nice to have some way to dismiss the detail view once the object has been deleted


Rishi V (Apple)
  3 days ago
In the view that is observing the model, could check if the instance isDeleted()


Keith
  3 days ago
OK, that could work, Will give that a go as a workaround and update the feedback with results.

----

@Keith
 asked:
Do predicates support complex expressions? For example, in Core Data I often use NSExpression to fetch min and max values of a field.
:eyes:
1

6 replies

WWDC Bot
APP  3 days ago
@Jeremy S (Apple)
 answered:
I think the answer to this will depend on which expressions you're looking to support, so feel free to clarify or ask in the thread about specific expressions. But predicates with SwiftData do not currently have explicit support for some of the mathematical/statistical values such as min/max - feel…
See more


Jeremy S (Apple)
  3 days ago
In particular, filing feedbacks with the content of any #Predicates that don’t seem to be supported is very helpful so that we can see how you’re representing your query in Swift code


Keith
  3 days ago
min and max are the ones I use the most. Will file feedback


Jeremy S (Apple)
  3 days ago
Sounds great, thanks! Feel free to leave the feedback number here once filed so we can help make sure it gets to the right place :slightly_smiling_face:

----

@Frank
 asked:
Does SwiftData allow me to sync to iCloud? How does that process work exactly? What do I need, and where can I find some more resources regarding this?
2 replies

WWDC Bot
APP  3 days ago
@Luvena H (Apple)
 answered:
SwiftData supports synchronization with CK private database using the cloudKitContainerIdentifier property on ModelConfiguration or setting it up in your entitlements. If you want to use shared / public databases, you'll need to coexist with Core Data and use the NSPersistentCloudKitContainer container
I'd suggest going through the SwiftData documentation for more details. For ModelConfiguration: https://developer.apple.com/documentation/swiftdata/modelconfiguration/

----

@Axel
 asked:
Does SwiftData work with multiple stores? For example, when we want to use CloudKit sharing, the previous recommandation was to use a dedicated store for the private data and another one for the shared data. Both were loaded by the same container, and could use a single NSManagedObjectContext.
3 replies

WWDC Bot
APP  3 days ago
@Ben T (Apple)
 answered:
Yes, you can create a ModelContainer using multiple ModelConfigurations similarly to NSPersistentContainer and store descriptions.

----

@Matthew
 asked:
Coexisting scenarios:  The ‘Migrate to Swift Data’ presentation indicates that I need to be sure to used Schema Versioning for using both CoreData and SwiftData, and it refers to the Modeling with Swift Data presentation.  This makes sense from the SwiftData perspective, but I do I need to also make s…
See more
2 replies

WWDC Bot
APP  3 days ago
@David S (Apple)
 answered:
That’s correct. If you choose to co-exist the Schema and Core Data model must be kept in sync and be Entity version hash equivalent

----

@Douglas
 asked:
What's the best way to create sample data with relationships in a SwiftUI preview? It appears that I need to pass a ModelContext and insert records manually before setting relationships but maybe I'm missing something.
:heart:
1
:eyes:
1

3 replies

WWDC Bot
APP  3 days ago
@Nick G (Apple)
 answered:
There are a few options:
1. Manually create an object graph, as you say
2. Use an in-project store file to hold a representative data graph you want to work with in your preview
3. Interact with your previews to configure data in an in-memory store

Douglas
  3 days ago
For option 1, does manually creating the object graph require passing around the ModelContext or is there a way to just treat models as plain old swift objects? (edited) 


Nick G (Apple)
  3 days ago
Models can exist outside (not bound to) a context, but they are not persistent.

----

@Michael
 asked:
How do you prevent the use of CloudKit — e.g., if you want to use CKSyncEngine instead of SwiftData's own sync engine?
1 reply

WWDC Bot
APP  3 days ago
@David S (Apple)
 answered:
Simply do not use a CloudKit container identifier when configuring the SwiftData stack. There is currently a bug with this, so please file a feedback report.

----

@Axel
 asked:
You explained Deferred migration in the video What’s new in Core Data. In this case, it means that the app will use a version of the store that is not up to date. How can the app handle this? Parts of the app that uses the new schema can’t work properly if the model is not up to date.
5 replies

WWDC Bot
APP  3 days ago
@Ben T (Apple)
 answered:
No, that's not what it means.  After a migration, the current model is always completely represented in the schema and the app can use all its model features.  However clean up, deletions, and dropping old indices, and some other work, is deferred.

Bryan
  3 days ago
For how long is this deferred, and what conditions cause the work to be done?

Axel
  3 days ago
@Ben T (Apple)
 oh sorry, I probably misunderstood what deferred migrations are then. I’ll check again the video to have a better idea of what work a deferred migration will do.

Ben T (Apple)
  3 days ago
it is deferred until you finish it

Ben T (Apple)
  3 days ago
// Finish deferred work from lightweight migration
- (BOOL)finishDeferredLightweightMigration:(NSError **)error API_AVAILABLE(macosx(10.16),ios(14.0),tvos(14.0),watchos(7.0));

----

@Christiano
 asked:
Is there an equivalent in SwiftData for the CoreData NSPersistentHistory classes, such as NSPersistentHistoryChangeRequest, NSPersistentHistoryTransaction, NSPersistentHistoryToken, and so on, please?
2 replies

WWDC Bot
APP  3 days ago
@Ben T (Apple)
 answered:
Not at this time.  Please file a feedback request.

----

@Aidan
 asked:
Is it silly to do something like this?
Predicate { _ in true }
I know you don't need this for FetchDescriptor, but I wonder would there ever be a legitimate use case for this? Maybe if you wanted to have an array pf Predicates that you could choose from? This currently raises a runtime exception:
Exc…
See more
:eyes:
1

2 replies

WWDC Bot
APP  3 days ago
@Jeremy S (Apple)
 answered:
You can indeed write a predicate like that to indicate an always-true condition if that helps with your app's logic. The crash that you're seeing is noted in the release notes for the released beta at https://developer.apple.com/documentation/ios-ipados-release-notes/ios-ipados-17-release-notes:
SwiftData queries will not work with #Predicate { true } and #Predicate { false } will cause a crash. (109425342)
Workaround: Use a logically true expression like { 1 == 1 }.

----

@Michael
 asked:
Can we create indexes on property(s) in our Models or is this handled automatically for us? Are indexes automatically created on unique properties and properties marked as relationships?
:eyes:
1

5 replies

WWDC Bot
APP  3 days ago
@David S (Apple)
 answered:
You can add indicies on your attributes by using .index with the @Attribute macro

Michael
  3 days ago
As a follow up, does this mean we can’t have a compound index?

Gayle
  3 days ago
@Attribute(.index) var name: String?
Produces a compile error:
   Type ‘PropertyOptions’ has no member ‘index’ (edited) 


David S (Apple)
  3 days ago
Please file a feedback report for this.

----

@Keith
 asked:
If I have a CloudKit container entitlement in my app, can I disable the SwiftData container from using it for previews and unit testing? For example, in Core Data I can delete the CloudKit options in the persistent store description before loading.
2 replies

WWDC Bot
APP  3 days ago
@Luvena H (Apple)
 answered:
There are some known issues in seed 1 of SwiftData. Please file a feedback report for this issue and we will take a look!

----

@David
 asked:
Did I hear it right that inserting an object with a unique constraint will upsert it automatically? What if you have a partially populated model? For instance my API returns a profile with an id and name, but I already have a version of that profile with all of its other properties like bio. With th…
See more
1 reply

WWDC Bot
APP  3 days ago
@Rishi V (Apple)
 answered:
Only the properties that were updated will be included in the UPSERT. Please file a feedback report with your sample project if that is not what you experience.

----

@Michel
 asked:
I notice retrieving the ModelContext (ModelContainer.mainContext) is @MainActor . However, the model itself is not. Is it only the retrieval that needs to be done in the Main thread, or everything inside?
1 reply

WWDC Bot
APP  3 days ago
@Ben T (Apple)
 answered:
Models and ModelContext are not @sendable, so you can't smuggle them to other actors.  They are bound to the actor you create them upon.

----

@Matthew
 asked:
I used the ‘Create SwiftData Code’ action in Xcode, and it generated Model objects with bi-direction relationships (which is correct — my Core Data model includes these).  All of these, however, are marked with the error Circular reference resolving attached macro 'Relationship'
I later noticed that the Sample 'Trip' app addresses this by only having the @Relationship annotation on one side of the relationship, but both of the 'child' entities refer back to an Optional Trip, and use a 'Nullifies' cascading strategy.  
Is it possible for these child entities to refer back with a non-Optional field, and for it to use a 'Deletes' cascading strategy?  I'm considering a co-existing scenario, and I want to make sure that my two models can properly be in sync with one another
See less




5 replies

WWDC Bot
APP  3 days ago
@David S (Apple)
 answered:
Please file a feedback report for this, it is a bug in code generation. Thank you!

Matthew
  3 days ago
Thanks, will do -- to my second question, does the reference back to the 'root' entity (the one annotated with @Relationship ) need to be optional?

Matthew
  3 days ago
@David S (Apple)
 FB12292422

David S (Apple)
  3 days ago
Yes I believe so!
Thank you!

----

@Michel
 asked:
The idea of having newBackgroundContext seems to be out. I shouldn't care? What if I want to do a background task?
3 replies

WWDC Bot
APP  3 days ago
@Ben T (Apple)
 answered:
Just create one.  `ModelContext(mycontainer)`


Michel
  3 days ago
But then, based on mainContext being a @MainActor and that shouldn’t be used concurrently (see https://wwdc23.slack.com/archives/C058Q99P81F/p1686345330166609 ), does that mean my new ModelContext().mainContext cannot do any operation outside the Main actor at the moment?

----

@David
 asked:
Are compound unique keys supported and if so, how do they work? I have several objects that are unique based on an id and a relationship to a login. When I generate SwiftData models from that, only the id is marked unique. Can relationships be a unique constraint?
2 replies

WWDC Bot
APP  3 days ago
@Rishi V (Apple)
 answered:
Please file a feedback request for Compound Unique Constraints.  A To-One Relationship is eligible to be considered for a unique constraint, if that is not the behavior you are experiencing let us know!

----

@Andreas
 asked:
In my app, I have sensitive data, that I need to encrypt locally on the iPhone. Does SwiftData have a build-in feature, that offers encryption (full database or fields)? And, if so, how can I use it?

WWDC Bot
APP  3 days ago
@David S (Apple)
 answered:
You should use the built in data protection mechanisms (NSFileProtection) for iOS. This would be accomplished by setting the attribute on the file.

Andreas
  3 days ago
Thanks, I’ll try


Matthew
  3 days ago
@David S (Apple)
, hope I can have a follow on question on this thread.


David S (Apple)
  3 days ago
Sure

Matthew
  3 days ago
“data protection mechanisms (NSFileProtection) for iOS. This would be accomplished by setting the attribute on the file.”
Is this encryption unlocked when the user device passcode is satisfied OR when each request in swiftData is running CRUD operations?


Matthew
  3 days ago
which file … the .sqlite?


David S (Apple)
  3 days ago
When the device is unlocked. Yes the sqlite file!


Matthew
  3 days ago
Ok, its un-encrypted with the rest of the bundle and all that is in the documents folder when device is unlocked.   I ask because of my security audits when using Core Data come often in the Health Care Industry.
I have actually started R&D with CryptoKit to make sure certain data points inserted are encrypted.
Thanks so much for all the support during WWDC.


David S (Apple)
  3 days ago
When the device is locked, it’ll be re-encrypted.

----

@Brandon
 asked:
Currently with CoreData and CloudKit I have to do a bunch of complicated stuff to track the persistent history using the PersistentHistoryToken so I can deduplicate records since CloudKit doesn't support the unique attribute of CoreData, is there an equivalent process to this with SwiftData?


4 replies

WWDC Bot
APP  3 days ago
@Rishi V (Apple)
 answered:
Coexistence is available and for this approach you would continue to use the CoreData stack to process the Persistent History

Brandon
  3 days ago
So the idea would be to do the deduping with coredata in the background and possibly use SwiftData for  driving the UI?


Rishi V (Apple)
  3 days ago
That is correct

----

@Kelly
 asked:
Is there an equivalent of NSManagedObject's isInserted property?
1 reply

WWDC Bot
APP  3 days ago
@Ben T (Apple)
 answered:
Please file a feedback request. You can ask the ModelContext if its in the collection of inserted objects.

----

@Douglas
 asked:
Is the @Relationship property wrapper required for any property whose type is a SwiftData @model or is it just needed when you want to specify @Relationship-specific behavior like .cascade?

1 reply

WWDC Bot
APP  3 days ago
@David S (Apple)
 answered:
It’s only needed when you have a non-default configuration.

----

@Michael
 asked:
CoreData has the ability to hold a context at a specific version of the data, and update to the latest version only on request. Does — or will — SwiftData have something similar? (For many things you want to be working with a stable snapshot over a period of time.)
1 reply

WWDC Bot
APP  3 days ago
@Rishi V (Apple)
 answered:
Please file a feedback request with your use case

----

@Brendan
 asked:
When storing a Date property, does SwiftData also store time zone information for that date such that if the user's device time zone changes, the data in the database still respects the original time zone the date value was stored in?
2 replies

WWDC Bot
APP  3 days ago
@Debbie G (Apple)
 answered:
Date objects are always in GMT. Time zones are applied when converting to or from textual or calendrical representations. If you want to store a time zone you can store its identifier separately from the Date.

----

@Kenneth
 asked:
If we sync our SwiftData models through iCloud, will they be end-to-end encrypted automatically if the user has enabled Advanced Data Protection on their account?
4 replies

WWDC Bot
APP  3 days ago
@David S (Apple)
 answered:
You’d want to use the .encrypt property on the Attribute macro to encrypt them at rest on the iCloud server.

Brendan
  3 days ago
Follow-up question for this. Does the .encrypt property work with the Shared CloudKit database in Core Data? If so, how does that work if you’re sharing with different iCloud users?


David S (Apple)
  3 days ago
It’s for the private database only I believe

----

@Aidan
 asked:
Is it possible to combine multiple ModelConfigurations into a single ModelContainer where one is in memory and one is not? I would love it if you can have a single object graph where some objects are persistent and others are not.
```
@Model
final class Person {
  var name: String
}

@Model
final class Post {
  var title: String
  var author: Person?
}

let fullSchema = Schema([Person.self, Post.self])
let people = ModelConfiguration(for: Person.self, inMemory: false)
let posts = ModelConfiguration(for: Post.self, inMemory: true)
let container = ModelContainer(for: fullSchema, configurations: [posts, people])
```

7 replies

WWDC Bot
APP  3 days ago
@Rishi V (Apple)
 answered:
Indeed you can setup the ModelContainer with multiple ModelConfigurations, however in this example, your Post and Person are related, and thus will be part of the same Schema in all the configurations.


Aidan
  3 days ago
yeah. So you can’t create relationships between models that span boundaries between stores?


Rishi V (Apple)
  3 days ago
That is correct


Aidan
  3 days ago
Ok thanks. While that makes sense, it’s kind of a bummer.


Aidan
  3 days ago
Thank you for your response!


Leon
  3 days ago
Is this likely to be a feature for a later release?
I suppose the only way to achieve inMemory ‘Only’ entities is mark all attributes as Transient? (somehow this is a question :))


Rishi V (Apple)
  3 days ago
The in-memory entity could not be related to other entities, then it could exist in an configuration by itself

----

@John
 asked:
I want to build a collection of SwiftUI apps that share access to the same CoreData (and CloudKit) stack. I want to ensure the CoreData Schema is consistent across the apps, so I plan to break out the CoreData Schema into a swift framework that is shared among the apps. Would I be able to use SwiftData with this use case, and are there any limitations?

1 reply

WWDC Bot
APP  3 days ago
@Nick G (Apple)
 answered:
Yes, you can use Schema’s makeManagedObjectModelMethod method to generate the model for the CoreData stack.
You won't be able to use you Model objects with CoreData though, that work will need to be in NSManagedObject or your existing subclasses.

---

@Ronan
 asked:
What is the best way to support saving one off information like user preferences with swift data? Does everything need to be a collection of records?
1 reply

WWDC Bot
APP  3 days ago
@David S (Apple)
 answered:
Anything saved in SwiftData needs to be modeled, so you can simply create one record and fetch it later.

----

@Keith
 asked:
Is loading a SwiftData container async or can it potentially block the main UI thread?
1 reply

WWDC Bot
APP  3 days ago
@Rishi V (Apple)
 answered:
There should be no blockage on the mainUI thread, please file a feedback report with a sample project if this a scenario you have experienced.

----

@Nico
 asked:
What’s the recommended approach to use SwiftData in UIKit if I previously have used NSFetchedResultsController or should I continue using Core Data for now?
:thinking_face:
1

3 replies

WWDC Bot
APP  3 days ago
@Rishi V (Apple)
 answered:
Use SwiftData with UIKit can be used with the ModelContext's .fetch() and tailoring the results with a FetchDescriptor.

----

@David
 asked:
It looks like SwiftData uses arrays instead of sets, but when I export my models I get a warning that ordered relationships aren’t supported. Are the relationships still unordered and unique like they would be with a set then?
:eyes:
1

3 replies

WWDC Bot
APP  3 days ago
@Rishi V (Apple)
 answered:
Please file a feedback report for the issue.

----

@Leon
 asked:
What is the best way to import bulk data (say thousands of network interfaces)?
Current thinking is to create a background process under 'ModelContextProvider', create a context pointing to the entities then insert/update each entity we return via external API.
Previously, using CoreData, we utilised the BatchInsertRequest functions
See less
:eyes:
1
:heart:
1

7 replies

WWDC Bot
APP  3 days ago
@Nick G (Apple)
 answered:
Please file a feedback report for batch support with some sample data if possible.
https://feedbackassistant.apple.com


Austen
  3 days ago
Batch support would indeed be awesome.


Brendan
  3 days ago
I have to chime in and say that I love that SwiftData can co-exist with Core Data so we can do some complex things like batch inserts still in Core Data and use SwiftData to drive the UI.


Leon
  3 days ago
So currently no support?
Will attempt with .insert / . update for now and report how we get on with the larger datasets.
Any ideas on the process.
Create context with autosave disabled
Obtain batch data and insert/update
Save context to persistent storage
SwiftUI/data to propagate throughout UI modelContext


Leon
  3 days ago
@Brendan
,
Are you saying we can use SwiftData for UI operations while managing batchInserts via CoreData APIs?
All in the same SQLite instance?


Brendan
  3 days ago
Yes because Core Data and SwiftData can share the same store. You’d just have to fault in the updated objects into SwiftData if you updated the store from Core Data.


Leon
  3 days ago
Looks like we can batchInsert from SwiftData (pulled from another question):
ModelContext insert(_ batch: [[String: Any]], model: any PersistentModel.Type) (edited) 

----

@Douglas
 asked:
What is the easiest way to blow away all data in dev when using the modelContainer(for:) modifier? I have been using an onAppear block where I call modelContext.container.destroy() which works but crashes the app.
1 reply

WWDC Bot
APP  3 days ago
@Rishi V (Apple)
 answered:
Setup the container in your application's initializer and then pass that container to the modelContainer modifier

----

@Ashley
 asked:
Does SwiftData have the same restrictions as Core Data when concerning CloudKit syncing? Specifically not being able to declare ordered relationships (e.g. an array @Relationship property of related items now) and empty strings?
2 replies

WWDC Bot
APP  3 days ago
@Rishi V (Apple)
 answered:
The same restrictions apply, please file a feedback report with your use case if these restrictions make your development cumbersome.

----

@Keith
 asked:
When syncing with CloudKit how do I dedupe data? Can I still register for NSPersistentStoreRemoteChange notifications and fetch the history as with Core Data?
3 replies

WWDC Bot
APP  3 days ago
@Rishi V (Apple)
 answered:
Indeed you can by setting up a parallel stack with Core Data that allow you consume Persistent History and do that processing.


Axel
  3 days ago
Thanks 
@Rishi V (Apple)
. It answers one of my question. So we still need to have a PersistenceController somewhere in the app for dedupe when using SwiftData with CloudKit? And the work is similar to what you provided in sample codes? (edited) 


Rishi V (Apple)
  3 days ago
That is correct

----

@Deeje
 asked:
I have my own coredata/cloudkit sync engine, and use coredata everywhere.  Can I migrate to SwiftData but configure its stack such that my sync engines still works?
1 reply

WWDC Bot
APP  3 days ago
@David S (Apple)
 answered:
Yes. Pass nil for CloudKit container identifier. There is currently a bug for this so file a feedback report.

----

@James
 asked:
In one of the SwiftData videos, the code showed a container configured with a CloudKit container identifier. But in an earlier Q&A period, someone asked if SwiftData can be used with CloudKit public, private, and shared containers and the answer was that you would need to use NSPersistentCloudKitContainer and CoreData co-existing with SwiftData.
What CloudKit things does SwiftData support, and what doesn't it support? And is there a document that explains this? Thank you.
See less
1 reply

WWDC Bot
APP  3 days ago
@David S (Apple)
 answered:
ModelContainer supports the private database but the public and shared database require Co-Existence currently. File a feedback report with your request.

----

@Michel
 asked:
My models are both @Model and Codable. Alas, I seem to have an issue with this, where I get ambiguous use of getValue and setValue. Known? Known workaround?


WWDC Bot
APP  3 days ago
@Rishi V (Apple)
 answered:
Please file a feedback report, you can try making your @Model not Codable to see if that is a feasible workaround.

----

@Keith
 asked:
Can a query return data grouped into sections (like NSFetchedResultsController)?

WWDC Bot
APP  3 days ago
@Rishi V (Apple)
 answered:
Please file a feedback report for this enhancement!

----

@Dave
 asked:
I cannot display the preview in SwiftDataFlashCareSample. The error stack includes the following from previewContainer while creating sample data:
“Compiling failed: main actor-isolated let 'previewContainer' can not be referenced from a non-isolated context”
I am running Xcode 15 on MacOS 13.3.1
3 replies

WWDC Bot
APP  3 days ago
@Rishi V (Apple)
 answered:
That is unexpected, please file a feedback report with your configuration included!


Bryan
  3 days ago
The issue is in the Preview macro. You can get it to work using the older PreviewProvider.

----

@Konrad
 asked:
Is it possible to use a transient property in a predicate?
:eyes:
1

2 replies

WWDC Bot
APP  3 days ago
@Jeremy S (Apple)
 answered:
Using a transient property inside of a predicate in a SwiftData Query is unsupported since the value of the property is not present in the database backing store for the query to access

----

@Timothy
 asked:
Is there batch insertion support from a background thread for SwiftData like there is for CoreData in the "Loading And Displaying A Large Data Feed" (Earthquakes) example?
1 reply

WWDC Bot
APP  3 days ago
@Rishi V (Apple)
 answered:
Indeed there is! Checkout ModelContext insert(_ batch: [[String: Any]], model: any PersistentModel.Type)

----

@Gayle
 asked:
My CoreData Model has a To Many Relationship Named "children" and an Derived Attribute "numChildren" = "children.@count" what is the best approach to recreate this in SwiftData?
Love the SwiftData class generation even with the:
warning("The property \"derived\" on Tag:numChildren is unsupported in SwiftData.")

WWDC Bot
APP  3 days ago
@Scott P (Apple)
 answered:
You could update the numChildren property in the children property's didSet!


Scott P (Apple)
  3 days ago
(If that doesn't work, please file a Feedback report! :melting_face:) (edited) 

----

@Ben
 asked:
I was wondering about inheritance in SwiftData. Unlike Core Data where entities can have super types, is there a similar structure for this where for example you can do a single fetch for all entities conforming to a protocol? (PictureMessage, TextMessage, etc. — sorted by date, etc.)
:eyes:
2
:+1:
1

12 replies

WWDC Bot
APP  3 days ago
@Rishi V (Apple)
 answered:
Please file a feedback report with your use case!


John
  3 days ago
I asked basically the same question (which hasn't been answered yet). +1 for being able to fetch all entities that conform to the same protocol, that'd be perfect for my use-case.





Konrad
  3 days ago
Would it be possible to use an enum as an property of our model, and with associated values?
enum MessageType: Codable {
  case picture(PictureOptions)
  case text(TextOptions)
}

@Model
class Message {
  var messageType: MessageType
}
(edited)


Rishi V (Apple)
  3 days ago
Yup!
:raised_hands:
1



John
  3 days ago
(That wouldn't really work for my use-case. Too many entity types, too many properties, and I'd lose the ability to be able to query on individual fields.)


Ben
  3 days ago
Yeah that seems messy. You wouldn’t be able to index those values either


Ben
  3 days ago
John since you already asked this, can you please link to your question thread?


John
  3 days ago
Also there'd likely be a perf hit from what's essentially encoding everything twice.


John
  3 days ago
https://wwdc23.slack.com/archives/C058Q99P81F/p1686348336383039


Ben
  3 days ago
@Rishi V (Apple)
 FB12292881


Ben
  3 days ago
That’s a bummer there’s no solution for this, it means we probably can’t implement this for several more OS releases unless it’s backported.


Ben
  3 days ago
Inheritance in data is very useful for fetching from large datasets, even if Swift is more about conformance.

----

@Kelly
 asked:
Does SwiftData's @Query need to be used on a View, or can it be used on a view model or anywhere? I seem to have trouble using it Anywhere but a view, but that seems to break my separation of concerns between my swiftUI view, viewModel, etc.
:man-facepalming:
1

1 reply

WWDC Bot
APP  3 days ago
@Debbie G (Apple)
 answered:
You can use ModelContext and its various fetch() functions anywhere.

----

@Bryan
 asked:
In CoreData, we get the concept of, and can create, child contexts for managing new additions to the store. Is there any equivalent concept or ability in SwiftData?
1 reply

WWDC Bot
APP  3 days ago
@Ben T (Apple)
 answered:
Please file a feedback request.  You can make new peer contexts, but not nest them at this time.

----

@Bryan
 asked:
When coexisting CoreData and SwiftData, what are the best practices, those beyond keeping the models perfectly synced? One other area is naming conflicts. Any other issues or best practices?
2 replies

WWDC Bot
APP  3 days ago
@Luvena H (Apple)
 answered:
This video goes into more depth for best practices for coexistence: https://developer.apple.com/videos/play/wwdc2023/10189
In addition to the ones you've mentioned, you would also need to keep track of the schema versions (this video might help: https://developer.apple.com/videos/play/wwdc2023/10195) . Additionally, make sure you don't forget to turn on persistent history tracking for Core Data.

----

@Ashley
 asked:
When should I be specifying the inverse key path for relationships (and when not)? I noticed that if I do it on both sides I get a circular reference compiler error, unlike in the previous Core Data editor where the inverse should always be provided.
2 replies

WWDC Bot
APP  3 days ago
@Rishi V (Apple)
 answered:
SwiftData will do implicit inverses for you.  If you would like to specify explicit inverses, then set it on only 1 side of the relationship.

----

@Tyson
 asked:
Does SwiftData work with CloudKit?
What would need to be done to this code to make it sync via CloudKit?
`@Model
struct Person {
   var name = "Bob"
}`
Does the syncing need to be done by hand or is there a more "automatic" approach? Thank you!

WWDC Bot
APP  3 days ago
@David S (Apple)
 answered:
Yes, you can pass a CloudKit container identifier to ModelConfiguration to start syncing. You’ll need to make sure your model is compatible with CloudKit as well as following some steps to create a container in CloudKit.

----

@Ashley
 asked:
NSManagedObject provided a publisher(for: …) api for observing properties internally within a model’s implementation. What’s the recommended approach for observing and responding to property changes within a @Model instance?

WWDC Bot
APP  3 days ago
@Rishi V (Apple)
 answered:
The @Model conforms to Observable and can be observed via the API provided there.

----

@Matthew
 asked:
In  what cases is in necessary to call context.instert() explicitally.
i wast adding lots of things at once yesterday to my database (not from inside a view) and then when i went to query i got errors. but after calling save the problem was resolved
4 replies

WWDC Bot
APP  3 days ago
@Ben T (Apple)
 answered:
You need to call insert() explicitly for all new Model instances, except if you relate them to a Model that has already been inserted, in which case we'll do that for you
:eyes:
2
:+1:
1



Matthew
  3 days ago
ok. im not sure i totally get that to be honest but there is no danger in just always calling save right?


Ben T (Apple)
  3 days ago
Models need to be inserted in order for them to be saved / persisted.


Ben T (Apple)
  3 days ago
But relating a Model to an existing previous Model will automatically insert it into the same context s the pre-existing object

----

@Jeannine
 asked:
When using NSPersistentCloudKitContainer to sync a public database, will my app see updates record-by-record or all at once?
For example, 200MB of public records are syncing. Will my app see these records incrementally or only after all 200MB have synced?




2 replies

WWDC Bot
APP  3 days ago
@David S (Apple)
 answered:
Records are imported as needed, in batches.


Jeannine
  3 days ago
@David S (Apple)
 So that's the latter, then? Only after all 200MB have synced do I get access to them in my app?
 
 ----
 
 @James
 asked:
For to-many relationships, can I use any type of collection or does the type need to be an Array?
1 reply

WWDC Bot
APP  3 days ago
@Scott P (Apple)
 answered:
Just Array for now!

----

@Bryan
 asked:
Can we control SwiftData's automatic saving? That is, do we have the ability to turn it off per view, and call save when we are ready, as opposed to the automatic save?
4 replies

WWDC Bot
APP  3 days ago
@Ben T (Apple)
 answered:
You can turn it off per ModelContext and call save explicitly

Bryan
  3 days ago
So, essentially, we would spin up a ModelContext for that view, turn off automatic saving, and the control it from there?
That would seem to give us the same essential control as using Child contexts. If we want to discard the data, we throw the context away. Am I correct?

Ben T (Apple)
  3 days ago
yes

----

@Duncan
 asked:
To set a default, unique value in a NSManagedObject such as assigning a UUID to an object's uuid property, this would be done in awakeFromInsert. In SwiftData I can instead just assign UUID() to the property as a default value in the initializer, like I would do with any other Swift class?
1 reply

WWDC Bot
APP  3 days ago
@Debbie G (Apple)
 answered:
Yes, you can simply provide a default value:
```
@Model
class Sample {
    @Attribute(.unique)
    var id: UUID

    init(id: UUID = UUID()) {
      self.id = id
     }
}
```

----

@Kirkland
 asked:
Not sure if this is better asked in the CloudKit Q&A but figure I’d ask here.
Using SwiftData with the cloudKitContainerIdentifier, I can persist data across devices.
In my app, I plan on using SharePlay for group sessions. I want to include persistent sessions, & shared sessions. A la, similar to Freeform.
I need a way of storing a “session” & their respective members. Once a session ends, they can edit it after the fact, even when solo-editing, which then would propagate those changes to the group, even if they’re offline.
I imagine I’ll need a CKSyncEngine  &/or NSPersistentCloudKitContainer?
How can I keep certain sessions exclusive to certain users, within a database?
See less




2 replies

WWDC Bot
APP  3 days ago
@Nick G (Apple)
 answered:
ModelContainer manages data in the private CloudKit database. To use the shared database you need an instance of NSPersistentCloudKitContainer (or a CKSyncEngine if you want to roll your own sync) on the side.
The best approach for Sharing is to use a ModelContainer with both store files in a non-CloudKit syncing configuration (file a feedback request for this, feedbackassistant.apple.com). Then you use NSPersistentCloudKitContainer to manage the sync for both databases.
The confligration of NSPersistentCloudKitContainer, CKShare, and CKShareParticipant manage data availability and identity for different collections of participants. During a SharePlay activity data can be synced to / from the share, and the participants can remain active on the share after the SharePlay activity ends. NSPersistentCloudKitContainer will continue to sync activity to and from the share for as long as the participant is a member of it.
One thing to note is that if a group of collaborators disband, only the owner of the Share will keep a copy of the data. For that reason, and depending on your user experience and privacy goals, you may wish to copy the data to participants if they choose to leave the share before deleting it.
For more information please consult these documents and sessions:
https://developer.apple.com/documentation/cloudkit/shared_records?language=objc
https://developer.apple.com/videos/play/wwdc2021/10015/
https://developer.apple.com/videos/play/tech-talks/10874/
https://developer.apple.com/videos/play/wwdc2021/10086/
https://developer.apple.com/documentation/cloudkit/shared_records/sharing_cloudkit_data_with_other_icloud_users?language=objc
https://developer.apple.com/documentation/cloudkit/ckshare?language=objc
See less

----

@Nico
 asked:
Is there way to model a Color/UIColor in SwiftData like with Transformable in Core Data? Is storing the RGB values separately in a dictionary a good idea?
:eyes:
1

2 replies

WWDC Bot
APP  3 days ago
@David S (Apple)
 answered:
I would recommend using Composite Attributes. In Core Data, they’ll be modeled as dictionary coding and in SwiftData you can store them natively as long as it conforms to codable.

Nayan
  3 days ago
And Color is still not Codable :cry:

----

@Kelly
 asked:
Does SwiftData's @Query need to be used on a View, or can it be used on a view model or anywhere? I seem to have trouble using it Anywhere but a view, but that seems to break my separation of concerns between my swiftUI view, viewModel, etc.

1 reply

WWDC Bot
APP  3 days ago
@Debbie G (Apple)
 answered:
You don't need to use @Query; you can use ModelContext and its fetch() functions anywhere.

----

@Nicklas
 asked:
A common scenario is to have Swift model classes with enum attributes. For simplicity, assume it's a Swift enum of the type Int and no associated value.
How should we model such a NSManagedObject Swift class so that it's as easy as possible to migrate to Swift Data when we can require iOS 17? What sh…
See more
4 replies

WWDC Bot
APP  3 days ago
@David S (Apple)
 answered:
In SwiftData, have your enums confirm to codable and model them normally. In Core Data, use Composite Attributes to model the same thing

Nicklas
  3 days ago
Is Composite Attributes available prior to iOS 17?


David S (Apple)
  3 days ago
They are only in iOS 17 and macOS Sonoma

----

@Axel
 asked:
Let’s say I migrate my Core Data app to SwiftData. I have 10 versions of the store, with a mix of lightweight migrations and custom migrations. Do I have to write the stages for all the previous versions of the app?
5 replies

WWDC Bot
APP  3 days ago
@Rishi V (Apple)
 answered:
For SwiftData, it would only need the VersionedSchema for the last release to migrate the last Core Data release.


Michael
  3 days ago
If the app is on customer devices with an old Core Data version I think you’ll still need all of the migrations to get them up to the current version? (edited) 
:eyes:
1



Rishi V (Apple)
  3 days ago
Yes that is correct, and the Managed Object Model Editor has a convenient Generated SwiftData Classes to facilitate the process!


Axel
  3 days ago
Thanks.


Ben
  3 days ago
That’s nice. But Core Data generally doesn’t hop/route across multiple mappings does it though?

----

@Duncan
 asked:
In my app I have existing Core Data migration code for between previous versions, and a setup that manually manages the migration stepwise through a series of versions until the first version where NSPersistentCloudKitContainer was introduced, after which only lightweight migration is used. What advice do you have on whether I could at some point move to only/primarily using SwiftData in the app, while maintaining the possibility of a user with a very old version migrating forward to the current one?
1 reply

WWDC Bot
APP  3 days ago
@Rishi V (Apple)
 answered:
Checkout this awesome sample that shows a Core Data app that evolves to SwiftData while coexisting between the two on the way!
https://developer.apple.com/documentation/coredata/adopting_swiftdata_for_a_core_data_app

----

@Matthew
 asked:
Do you have best practices/recommendations around using identifiers for SwiftData models?  I typically don't use the objectID in my CoreData models, and instead, have a UUID field in my entities.  Do you have an opinion on this practice?
:eyes:
1

4 replies

WWDC Bot
APP  3 days ago
@David S (Apple)
 answered:
You can have an identifier var in your model with type UUID.


Bryan
  3 days ago
Is there a separate objectID that we can access in SwiftData, or do the objects need to be identifiable themselves?


David S (Apple)
  3 days ago
They need to be identifiable by themselves. There isn’t an objectID.


Matthew
  3 days ago
My models seem to have a generated objectID field of type PersistentIdentifier (edited) 

----

@Michel
 asked:
When migrating my model using the automated system, it created relationships and their inverse for everything, which is cool. But I seem to be able to create relationships only one side, or else it tells there's circular references. Anything I'm missing, or it's really meant to be single-sided?
1 reply

WWDC Bot
APP  3 days ago
@Rishi V (Apple)
 answered:
Indeed like keypaths in Property Wrappers, they are meant to be annotated only on one side.

----


WWDC Bot
APP  3 days ago
@Tim
 asked:
Is there an equivalent to Core Data’s NSFetchedResultsController in SwiftData, for use in situations like a view model where you'd want updates to data as it changes, but outside the context of a view?

2 replies

WWDC Bot
APP  3 days ago
@Debbie G (Apple)
 answered:
Please file a FB request and post the number in this thread.

Ben
  3 days ago
This seems like a huge omission :scream: 

----

@Axel
 asked:
Let’s say I start using SwiftData. I release an app with a first version of the schema. I then add new features that require to modify the schema to a v2. Do I need to specify a SchemaMigrationPlan in the ModelContainer or is it optional if the migration is lightweight?
:eyes:
1

1 reply

WWDC Bot
APP  3 days ago
@Rishi V (Apple)
 answered:
It is optional but a best practice to capture your releases incase you encounter a future migration that is not lightweight.

----

@Michael
 asked:
Do we get access to a time stamp for when data is written to the store? Or do we need to handle this via properties on our model?
:eyes:
1

3 replies

WWDC Bot
APP  3 days ago
@Scott P (Apple)
 answered:
This is best handled with properties on your own model type(s)!


Michael
  3 days ago
Do we know when data is about to be written to the store such that we can say “Update this property now before the write!” - I am concerned that setting a value on my model might mean the date/time is significantly different to the actual date/time the store writes the data?


Scott P (Apple)
  3 days ago
What are you doing where the difference between "time modified" and "time persisted" makes such a difference? Autosave triggers with the runloop so the window will be quite small when it's enabled, but you also can force the timing by calling save() manually

----

@Richard
 asked:
How do you segregate private and public objects using SwiftData?  Thank you
6 replies

WWDC Bot
APP  3 days ago
@Nick G (Apple)
 answered:
What does public and private mean in this context?


Richard
  3 days ago
In core data we can create public and private stores.   So suppose we create a quiz app,  the quiz questions would be public, the each users scores are private

Nick G (Apple)
  3 days ago
With NSPersistentCloudKitContainer? You mean store files using CKDatabaseScopePrivate and CKDatabaseScopePublic?

Richard
  3 days ago
Yes. Thank you

Nick G (Apple)
  3 days ago
ModelContainer doesn’t know about anything but the private scope. You can file a feedback request for public support using feedbackassistant.apple.com and provide the Feedback ID in this thread.
But today you do this with an NSPersistentCloudKitContainer on the side to manage syncing.

----

@Keith
 asked:
Is there a way to update the sortOrder or predicate of a query at run time similar to how @FetchRequest works?
2 replies

WWDC Bot
APP  3 days ago
@Scott P (Apple)
 answered:
Not at this time, please file a Feedback report and we'll look into it!

----

@Matthew
 asked:
If i have a huge set of static data thats not going to change is this something i should use with swift data or should i just consider loading it as a json file.
i have like 100k plus words with definitions that i want to ship with the app.
2 replies

WWDC Bot
APP  3 days ago
@David S (Apple)
 answered:
Yes you’d benefit from using SwiftData. I would recommend shipping a canned store file in your apps bundle and load it readonly.

----

@Gayle
 asked:
How do you create fetch indexes in SwiftData?
2 replies

WWDC Bot
APP  3 days ago
@Nick G (Apple)
 answered:
File a feedback using feedbackassistant.apple.com and provide the Feedback ID in this thread!

----

@Dave
 asked:
Lacking abstract entities in SwiftData, what's the best way to model a relationship to a choice of types (that would have been subclasses of the abstract entity)?  Do I need to model a separate relationship for each of the choices?

WWDC Bot
APP  3 days ago
@David S (Apple)
 answered:
Please file a feedback report with your use case for abstract entities in SwiftData!

----

@Frank
 asked:
When trying to build another hobby project I notice an error at launch: Fatal error: Unable to have an Attribute named description
There is something called an ESOAttribute, which is a Codable enum. Should I file a bug report for this, I am unsure what the error means :')

WWDC Bot
APP  3 days ago
@Rishi V (Apple)
 answered:
A property cannot be named description with SwiftData

----

@John
 asked:
Looking at the trips sample app, I don’t see a configuration for the persistence container to point to an App Group container, but the app has the App Groups entitlement. Are App Groups still necessary for accessing persisted data within widgets, and is SwiftData shared across app extensions?

WWDC Bot
APP  3 days ago
@Rishi V (Apple)
 answered:
Indeed the default experience will use the App Group entitlement if it is available.

----

@Axel
 asked:
When we add an object as a relationship to another object, we don’t have a insert the object in the context. But when I want to delete an item that is a relationship to another one, do I have to delete it from the modelContext or removing it from the relationship is enough? It seems the API is not doin the same. See here https://twitter.com/dimillian/status/1666728022068658176?s=46&t=-0NC4ZS-XDh_wH7LgEoSZA
See less
TwitterTwitter
Thomas Ricouard on Twitter
So apparently the model context in SwiftData track correctly when you append stuff. But the removeAll call is not persisted. The UI update, but when I relaunch the app, my deleted object is still there. (54 kB)
https://twitter.com/dimillian/status/1666728022068658176?s=46&t=-0NC4ZS-XDh_wH7LgEoSZA

:eyes:
1

3 replies

WWDC Bot
APP  3 days ago
@Scott P (Apple)
 answered:
That code appears to remove the objects from the Array, not the SwiftData stack. Either the relationship needs to be declared with cascade delete or the objects need to be deleted from the context directly.


Axel
  3 days ago
Thanks 
@Scott P (Apple)
. But adding an object to a relationship does not require the insertion of the object?


Scott P (Apple)
  3 days ago
Just because it wasn't explicit doesn't mean it didn't happen :melting_face:

----

@Axel
 asked:
You introduced combined attributes in Core Data (also used by Codable Enum/Struct in SwiftData). Can you give examples of where these combined attributes are better than having a dedicated entity? Why should we use one or the other?
2 replies

WWDC Bot
APP  3 days ago
@Ben T (Apple)
 answered:
In Objective-C, the composite attributes can be better than using a Transformable to flatten them into an opaque blob.  It is situationally dependent on your workflow which approach helps more.

----

@Antoine
 asked:
What would be the best way to replicate the child context from CoreData using SwiftData?
:+1:
1

1 reply

WWDC Bot
APP  3 days ago
@Amit G (Apple)
 answered:
Current Child Context are not supported but you can have peer context. Please file a feedback assistance report

----

@Dave
 asked:
Is there a way to subscribe to entity changes from within my app, so that another view can receive a notification for add, delete or update to an entity?


WWDC Bot
APP  3 days ago
@Ben T (Apple)
 answered:
Please file a feedback item

----

@Bryan
 asked:
Can you create a background context in SwiftData for potentially processing things on a background thread?

WWDC Bot
APP  3 days ago
@Debbie G (Apple)
 answered:
Please see "Concurrency Support" in the documentation for SwiftData at developer.apple.com

----

@Nayan
 asked:
If I have these models:
```
@Model class One {
    var two: Two
}

@Model class Two {
    var three: Three
}

@Model class Three {
    var name: String
}
```
when passing a list of models to be persisted to the .modelContainer modifier, can I just use [One.self] and Two and Three will be inferred automatically?

WWDC Bot
APP  3 days ago
@Rishi V (Apple)
 answered:
That is correct!

----

@Bastian
 asked:
I'd be interested in the story of testing in regards to SwiftData. Lets consider I have some service that loads data from SwiftData and I'd like to make sure that it loads the correct data in my tests. Can I just create a SwiftData Model inside my tests that I can compare to the data the service loaded from disk and the test would pass if all fields would be the same or would the test fail because equality checks would also compare some persistent identifier as well, which would be nil in case I've created the object from scratch without saving it in the database.

WWDC Bot
APP  3 days ago
@Ben T (Apple)
 answered:
Generally for model layer work, we recommend just testing actual Models to either a temp file or an in memory container.  So, yes, just use the same SwiftData APIs like the real app code.

----

@Antoine
 asked:
How can we fetch an object from another (background or main) context back to the  default main context ?
6 replies

WWDC Bot
APP  3 days ago
@Scott P (Apple)
 answered:
You can pass the object's PersistentIdentifier along and use fetchIdentifiers to manifest it!

Bryan
  3 days ago
Can we always identify the object with PersistentIdentifier?

Scott P (Apple)
  3 days ago
With one caveat; the identifier changes from a temporary to permanent id when the object is first save()d to disk.


Scott P (Apple)
  3 days ago
But… in this case the object wouldn't be accessible in another context until it had been saved anyway (edited) 

Bryan
  3 days ago
So, essentially the same system as in CoreData.

Scott P (Apple)
  3 days ago
No coincidence, that :)

----

