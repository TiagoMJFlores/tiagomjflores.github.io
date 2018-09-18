---
title: "iOS Architectures (II) "
image: /assets/posts/chooseiOSArchitecture2/arch2.jpg
categories:
- Auto Layout
tags:
- UI
- Tips & Tricks
- Performance
layout: post
actions:
---

## The Main iOS Architectures


### MVC


![](https://github.com/TiagoMJFlores/tiagomjflores.github.io/blob/master/assets/posts/chooseiOSArchitecture2/mvc.png?raw=true)


The MVC layers are as follows:

M : Business Logic, Network Layer and Data Access Layer

V : UI Layer (UIKit things, Storyboards, Xibs)

C: Coordinates mediation between Model and View.

To understand MVC we must understand the context in which it was invented. MVC was invented in the old Web development days, where Views has no state. In the old times each time we need a visual change in the website, the browser reload the whole HTML again. At the time there was no concept of the view state being maintained and saved.

There were, for example, some developers that mixed within the same HTML file, PHP and database access. So the main motivation of MVC was to separate the View layer from the Model layer. This increased the testability of the Model layer. Supposedly in MVC, the View and Model layer should know nothing about each other. For make this to be possible, an intermediary layer named Controller was invented. This was the SRP that was applied.

An example of the MVC cycle:

.1 A user action/event in the View Layer (eg: Refresh Action) is fired and that action is communicated to Controller
.2 The Controller that asks data to the Model Layer
.3 Model the returns data to Controller
.4 Controller says for the View update his state with the new Data
.5 View update his state

### Apple MVC

In iOS, the View Controller is coupled to the UIKit and the lifecycle view, so it is not pure MVC. However, in the MVC definition, there is nothing to say that the Controller can’t know the View or Model specific implementation. His main purpose is to separate responsibilities the Model layer from the View layer so we can reuse it and test the Model layer in isolation.


![](https://github.com/TiagoMJFlores/tiagomjflores.github.io/blob/master/assets/posts/chooseiOSArchitecture2/appleMVC.png?raw=true)


The ViewController contains the View and owns the Model. The problem is we used to write the controller code as well as the view code in the ViewController.

MVC often creates the called Massive View Controller problem, but that only happens and become a serious thing in apps with enough complexity.

There are some methods that the developer can use to make the View Controller more manageable. Some examples:

* Extracting the VC logic for other classes like the table view methods data source and delegate for other files using the delegate design pattern.
* Create a more distinct separation of responsibilities with composition (e.g. Split the VC into child view controllers).
* Use the coordinator design pattern to remove the responsibility of implementing the navigation logic in the VC
* Use a DataPresenter wrapper class that encapsulates the logic and transforms the data model into a data output representing the data presented to the end user.

### MVC vs MVP

![](https://github.com/TiagoMJFlores/tiagomjflores.github.io/blob/master/assets/posts/chooseiOSArchitecture2/MVP.png?raw=true)

The MVC was a step forward, but it still was marked by absence or silence about some things.

Meanwhile, the World Wide Web grew and many things in the developers’ community evolved. By example, the programmers began using Ajax and only load parts of pages instead of the whole HTML page at once.

In MVC I think there is nothing to indicate that the Controller should not know the specific implementation of View (absence).

HTML was part of the View layer and many cases were dumb as fuck. In some cases, it only receives events from the user and displays the visual content of the GUI.

As parts of the web pages began to be loaded into parts, this segmentation led in the direction of maintaining View state and a greater need for a presentation logic responsibility separation.

Presentation logic is the logic that controls how UI should be displayed and how UI elements interact together. An example is the control logic of when a loading Indicator should start showing/animate and when it should stop showing/animate.

In MVP and MVVM the View Layer should be dumb as fuck without any logic or intelligence in it, and in the iOS, the View Controller should be part of the View Layer. The fact of View being dumb means that even the presentation logic stays out of the View layer.

One of the problems of MVC is that it not clear about where the presentation logic should stay. He is simply silent about that. Should presentation logic be in the View layer or in the Model Layer?

If the Model’s role is to just provide the “raw” data, it means that the code in the View would be:

Consider the following example: we have a User, with first name and last name. In the View, we need to display the username as “Lastname, Firstname” (e.g. “Flores, Tiago”).

If the Model’s role is to provide the “raw” data, it means that the code in the View would be:


```
let firstName = userModel.getFirstName()
let lastName = userModel.getLastName()
nameLabel.text = lastName + “, “ + firstName
```

So this means that it would be the View’s responsibility of handling the UI logic. But this makes the UI logic impossible to unit test.

The other approach is to have the Model expose only the data that needs to be displayed, hiding any business logic from the View. But then, we end up with Models that handle both business and UI logic. It would be unit testable, but then the Model ends up, implicitly being dependent on the View.

```
let name = userModel.getDisplayName()
nameLabel.text = name
```

The MVP is clear about that and the presentation logic stays in the Presenter Layer. This increases the testability of the Presenter layer. Now the Model and Presenter Layer are easily testable.

Normally in the MVP implementations, the View is hidden behind an interface/protocol and there should be no references to the UIKit in the Presenter.

Another thing to keep in mind is the transitive dependencies.

If the Controller has a Business Layer as a dependency and the Business Layer has Data Access Layer as a dependency, then the Controller has a transitive dependency for the Data Access Layer. Since the MVP implementations normally use a contract (protocol) between all layers it hasn’t transitive dependencies.

The different layers also change for different reasons and at different rates. So when you change a layer you do not want this to cause secondary effects/problems in the other layers.

Protocols are more stable than classes. The protocols haven’t implementation details and with the contracts, so it is possible to change the implementation details of a layer without affecting the other layers.

So the contracts (protocols) create a decoupling between the layers.

### MVP vs MVVM

![](https://github.com/TiagoMJFlores/tiagomjflores.github.io/blob/master/assets/posts/chooseiOSArchitecture2/MVVM.png?raw=true)

One of the main differences between MVP and MVVM is that in MVP the Presenter communicates with the View through interfaces, and in the MVVM the View is oriented to data and events changes.

In The MVP we make manual binding between Presenter and View using Interfaces/Protocols.
In The MVVM we make automatic data binding using something like RxSwift, KVO or use a mechanism with generics and closures as I did in this project.

In the MVVM we even not need a contract (eg: java interface/ iOS protocol) between ViewModel and View because we usually communicate through the Observer Design Pattern.

The MVP uses the Delegate Pattern because the Presenter Layer Delegates orders to the View Layer, so it needs to know something about the View even, it is only the interface/protocol signature. Think of the difference between Notification Center and TableView Delegates. The Notification Center doesn’t need interfaces to create a communication channel, but TableView Delegates uses a protocol that the classes should implement.

Think of the presentation logic of a loading indicator. In MVP the presenter does ViewProtocol.showLoadingIndicator. In MVVM there may be an isLoading property in the ViewModel. The View layer through an automatic data binding detects when this property changes and refreshes itself. MVP is more imperative than MVVM because the Presenter gives orders.

MVVM is more about data changes than direct orders, and we make associations between data changes and view updates. If use RxSwift and functional reactive programming paradigm together with MVVM we have made the code even less imperative and more declarative.

MVVM is easier to test than MVP because MVVM uses the Observer Design Pattern that transfers data between components in a decoupled way.
So we can test just by looking at changes in data just by comparing the two objects rather than creating mocks the methods calls to test the communication between View and Presenter.

Part two ends here. All feedback is welcome. Part three will talk about VIPER, VIP, Reactive Programming, Trade-Offs, Constraints and Contextual Sense.

Thank you for reading!  ✨
