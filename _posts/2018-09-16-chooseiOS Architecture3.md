---
title: "iOS Architectures (III) "
image: /assets/posts/chooseiOSArchitecture3/arch3.jpg
categories:
- Auto Layout
tags:
- UI
- Tips & Tricks
- Performance
layout: post
actions:
---

## VIP and VIPER

I have limited experience with this Architecture because I almost never used it because I did not come across an application with enough complexity and requirements that would justify it. I consider that this architecture may have too many unnecessary layers for most of the iOS apps.

The Clean Architecture was popularised mainly by the Uncle Bob. VIPER is an implementation variant of the classic Clean Architecture. Another implementation of Clean Architecture is VIP popularised by Raymond Law in his [`Clean Swift blog`](https://clean-swift.com/blog/) where he writes extensively about how to apply this architecture to the iOS.

The primary layers of VIP and VIPER are:

View, Presenter, Interactor, Model(Entity)

They also use another layer called Router (the R in the VIPER) that plays the role of a Flow Coordinator that manages the navigation of the app. Each Architecture has a different flux order. The layer order of VIP is:

![](https://github.com/TiagoMJFlores/tiagomjflores.github.io/blob/master/assets/posts/chooseiOSArchitecture3/VIP.png?raw=true)

Of course in the Interactor layer, you often access the model layer. In the VIPER the model layer is represented by the E in the VIPER word that means Entity. The layer order of VIPER is:


![](https://github.com/TiagoMJFlores/tiagomjflores.github.io/blob/master/assets/posts/chooseiOSArchitecture3/VIPER.png?raw=true)
Begin -> View/VC -> Presenter -> Interactor -> Presenter -> View/VC


The limitation/restriction of the flow always following strictly the same predefined sequence and direction using the architecture layers is a good point in favour of VIPER and VIP. This prevents each developer in the team from following a different flow sequence and direction between layers.

According to my limited experience, the main difference between VIPER and VIP is that in the VIPER the Presenter has a central role and in the VIP the flow sequence between layers is more linear.

I have read some posts about Android app architecture, and often they seem to call the MVP of VIPER.

In this post about MVP for Android, Antonio Leiva seems to suggest an architecture that looks more like VIPER (Correct me if am wrong ). And he gives an example of code that he calls“MVP”. In that example there is a Login View (Login Activity) that fires a click event:

```
@Override public void onClick(View v) 
{ 
	presenter.validateCredentials(username.getText().toString(), password.getText().toString()); 
}
```

That views calls presenter.


```
Override public void validateCredentials(String username, String password) {
  if (loginView != null) { 
      loginView.showProgress();
   } 
  loginInteractor.login(username, password, this); }
```

That calls interactor.

```
@Override
    public void login(final String username, final String password, final OnLoginFinishedListener listener) {
        // Mock login. I'm creating a handler to delay the answer a couple of seconds
        new Handler().postDelayed(new Runnable() {
            @Override public void run() {
                if (TextUtils.isEmpty(username)) {
                    listener.onUsernameError();
                    return;
                }
                if (TextUtils.isEmpty(password)) {
                    listener.onPasswordError();
                    return;
                }
                listener.onSuccess();
            }
        }, 2000);
    }
```

That calls presenter again

```
@Override public void onSuccess() {
         if (loginView != null) {
             loginView.navigateToHome();
          }
 }
```

That calls View again.

In my limited experience, VIPER still adds more complexity than VIP. In the VIPER sometimes instead of accessing the Interactor layer directly from the View Layer / View Controller, you have to pass in the Presenter before without adding anything useful and you still stop in the Presenter before come back the View Layer again.

That is one more extra step than VIP, and often an unnecessary extra step where you only pass in the present layer one time in each cycle.

You can read this good article that talks about some of the VIPER pitfalls.

### Reactive Programming (RxSwift and Reactive Cocoa)

Reactive Programming is usually used together with an MVVM architecture, but also can be used with others architectures. However it is not mandatory to use Reactive Programming with MVVM, and it isn’t very difficult to implement MVVM data binding observers using generics and closures.

With Rx, everything is a stream of values that arrive over time (even a click of a button is a stream) that are manipulated by operators.

Some of the main changes of the reactive programming paradigm are the change from an imperative paradigm to a declarative paradigm, less mutability and the “simplified” way of dealing with asynchronous and reactive code. Used in the right context, Reactive Programming can help.

Of course, this varies according to the perspective, because for those who are starting and have little experience with Reactive Programming undoubtedly this means that the code is more difficult to read, and it is harder to deal with the asynchronous things of this new form than the traditional one.

It takes some discipline and effort to really understand the Reactive Programming paradigm and all the operators. And this discipline doesn’t just fall on you, but also on all people who will maintain the project in the future. Some of these people aren’t even in the company yet. Software engineers like to consider how much their software tolerates change, but they should also think about how it tolerates changes in the team.

Reactive Programming can be canon for a kill a fly in many apps. If the app complexity is simple, have few reactivities and few/simple asynchronous code add a Reactive Programming framework can add more complexity than removes. There is who uses RxSwift/Reactive Cocoa with VIPER or VIP and that increased complexity has to be justified.

Do not use Reactive Programming or Clean Architecture just because all the cool dudes are using it. If you’re going to use a Reactive Programming framework then you should have at least sense of one set of apps that makes sense to use it and another set of apps that no longer makes sense to use it. You need contextual sense.

Now let’s evaluate some of the concepts used in the article to choose the right architecture for a specific project and give them the specific names that identify them.

### Trade-offs

So one of the main things to take into account when choosing the architecture are the trade-offs. The trade-offs mean that there is no right architecture for all situations.


A [`trade-off`](https://en.wikipedia.org/wiki/Trade-off) is a situational decision that involves diminishing or losing one quality, quantity or property of a set or design in return for gains in other aspects.

To understand the importance of trade-offs we can make a comparison with the theory of evolution.
Darwin found that finches had different beaks depending on what they ate.

For example, those with smaller beaks were specialised in eating insects, while those with more extensive beaks were able to split seeds. A large beak is not suitable for eating small insects, and a short beak is not ideal for cracking seeds. Each species adapted to the environment. What is good in one context is not useful in another.


### Constraints

The constraints are things that limit the scope of development. For example, when we are in a process of we move to a new house each mobile must occupy a specific position within the new house.
The new position of the mobile is limited by spatial constraints as the size of the divisions and aesthetic/experiencing constraints because, for example, a refrigerator can’t stay inside the room where we sleep.

There are many significant constraints to consider like the team experience, customer expectations and time to market.

Business constraints are defined by the business model and business needs. For example, it does not matter to create a highly adaptable app to change if the extra time spent on facilitating the app maintenance makes the company lose a business opportunity to a competitor. In certain contexts, it may make sense to get some technical debt because of the Time to Market or another business considerations.

Some people may say that constraints are one of the things that sets the theoretical ideal world apart from the real world. Knowing how to work with constraints is to be practical.

However, business managers are not always prepared to understand and evaluate the importance of architecture and software design, so it also must be the responsibility of the software team to communicate the possible consequences of haste.

Another type of constraint appears is when we enter a project with an already semi-defined architecture. So, the constraints are also defined by the context for where we jump in.

The very choice of architecture imposes constraints on how code should be implemented. Architectures restrict how layers should communicate with each other and in what order. And those constraints can be beneficial or harmful depending on the context of the project.

### Contextual Sense

Each project is framed in the specific context, defined by the environment around the developer (eg: the team around the projects and the app requirements). A trade-off may be advantageous in one context and disadvantageous in another. In an ideal world without limitations, many desirable objectives would be achievable, but in practice, we are restricted by the constraints.

In a simplified way, this is what the choice comes down to: After analysing the constraints and the specific context of the environment in which we are immersed, a weighted architectural solution using the right trade-offs is outlined. So, develop a strong contextual sense is an important step to choose the right architecture.

Part three ends here. All feedback is welcome.

Thank you for reading!  ✨

