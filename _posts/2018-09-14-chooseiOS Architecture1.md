---
title: "iOS Architectures (I)"
image: /assets/posts/chooseiOSArchitecture1/arch1.jpg
categories:
- Auto Layout
tags:
- UI
- Tips & Tricks
- Performance
layout: post
actions:
---

### How to choose the appropriate iOS Architecture?


Note: You can read the original Medium article [`here`](https://medium.com/@tiagoflores_23976/how-choose-the-appropriate-ios-architecture-mvc-mvp-mvvm-viper-or-clean-architecture-2d1e9b87d48).


In this post series, I will talk about some metrics for choosing the appropriate architecture, about each individual architecture and some of the differences between them. In the end, I will talk about Reactive Programming. Reactive Programming isn’t an architecture pattern but because often it is used together with MVVM it is important to talk about it.
The title is how to choose the appropriate iOS Architecture and not the right iOS Architecture because there is not Architecture that fits right in all apps independently of context that surrounds each specific iOS app.
There are two general metrics, related to the app dimension and the surrounding environment, that apply to any project that I personally consider important to choose the appropriate architecture:

 * Evaluate the essential complexity of the app requirements to avoid accidental complexity
 * Evaluate the Team around the app to create an effective communication channel
 
### Evaluate the essential complexity of the app requirements to avoid accidental complexity

>“Keep it simple and straightforward” -  [`KISS`](https://en.wikipedia.org/wiki/KISS_principle) design principle noted by the U.S Navy

MVC is at the first level. There are apps that are done in two/three weeks, and MVC is enough for them.
In the last level of complexity, there is VIPER or VIP (Clean Architecture). 
These architectures have so many layers and require a lot of boilerplate, that the iOS some of the developers who use them also use Code Template generators.

I read that UBER uses a VIPER variant but we have to contextualise that UBER has a team of 100 iOS developers. In VIPER and VIP, there are more layers than in the other architectures, and this can help fulfil the Single Responsibility Principle better and make the code more modular. Without any doubt when you have an app with millionaire profits and high complexity you should separate the responsibilities of the app as much as possible and test all components individually.

In VIPER and VIP, there is a flow of execution where things are going through each layer in a certain order. If 80% of the time things are going through extra layers where we do not add anything useful, we may be using the wrong architecture for that specific app.

VIPER and VIP, when used in the right context and right way, is like an exchange of development speed by maintenance and testability of the app. We lose development speed because to make a small change we have to go through all the layers. It maybe makes sense if are talking about an inner product with enough complexity that is going to be kept in the House or by other words if it is a worthy long-term investment.
There are two main mistakes you can make with an architecture:

* The absence of it that creates an overall lack of organization and causes astonishment violating the Principle of least astonishment [`astonishment`](https://en.wikipedia.org/wiki/Principle_of_least_astonishm)
* The excess use of it that is usually called overengineering or overdesign

To avoid the first error we have to be organized not only ourselves but also for the other team members. Or even for ourselves, if we take the same project a few months later.
Lack of architecture can be bad but architecture in excess or architecture out of context too: the called overdesign or overengineering errors.

Overdesign errors happens when we read about good architectural / design patterns useful in a given context but we haven’t yet sufficiently developed our contextual sense to know how to apply patterns to the right context where they are useful. It is as if we caught a phrase in the middle of a half-way conversation and tried to understand it without having the context in which it occurred.

Each design pattern solves specific problem in a very specific context and each design decision always implies trade-offs but sometimes when have hammer everything looks like a nail and we try overuse design patterns independently of the context.


### Evaluate the team around the app to create an effective communication channel

>“organizations which design systems … are constrained to produce designs which are copies of the communication structures of these organizations.” - [`Conway’s law`](https://en.wikipedia.org/wiki/Conway%27s_law)

By the team around the app I mean mainly:
The software team, the project managers and the customer that pays the app development. One of the main functions of a good architecture is to help in communication with juniors, business managers and other team members.
If a programmer is working alone on a personal project he may become accustomed to his lack of organization.
However, when the project is accomplished by a team the establishment of a common organization becomes important. To communicate well we need to create standards and make everybody speak the same language.

It is always necessary to have a minimal architecture to give an organizational structure and maintain the conceptual integrity of the system. This way the project code base doesn’t take a chaotic form and each developer has a rough idea of how other developers who are adopting the same architecture have structured their code. So architecture is like a glue that forces people to speak the same language.

One of the important factors is the experience of the people in the project. 
If we have someone on the team who started programming iOS 1 or 2 weeks ago, then maybe it’s not worth using anything other than MVC.

Apple uses MVC in some of its code examples because the goal is to not complicate the demonstration but just keep it simple and demonstrate the working of the framework and nothing more.
 The main target audience when Apple makes project examples are all people who are starting to learn the framework.

Another important thing to keep in mind is that different elements of the team members may have different perspectives.
The business manager and customers want to deliver the features as quickly as possible, while software engineers want to create code that is easy-to-change and maintainable.

Unit testing and Integration Tests are specially important to maintain quality and avoid regressions of colossus like UBER that will last many years and where by scale reasons one single bug can affect million of costumers. 
However if we are talking about apps that are made in less than a month like a proof of concept to test technical limitations or to test the market, maybe it’s not so that important because the integration between the components and each individual unit of the app aren’t so complex. So maybe in some contexts of some small apps, the customers/managers are right about wanting an ASAP app because keeping a software team is expensive.

As the size of the team increases, communication difficulties simultaneously increase. What is the usual size of a team working on a project?
 [`The Code Complete`](https://www.amazon.com/Code-Complete-Practical-Handbook-Construction/dp/0735619670/ref=sr_1_1?s=books&ie=UTF8&qid=1536741129&sr=1-1&keywords=code+complete)  book presents this list:

```
Team Size Approximate Percentage of Projects
1–3 Persons, 25%
4–10 Persons, 30%
11–25 Persons, 20%
26–50 15%
50+ 10%
Source: Adapted from “A Survey of Software Engineering Practice: Tools, Methods, and Results “(Beck and Perkins 1983), Agile Software Development Ecosystems (Highsmith 2002), and Balancing Agility and Discipline (Boehm and Turner 2003).
```
If we take into account that mobile apps are usually less complex than other software projects, the team’s medium size is still smaller. The size of the team depends on the essential complexity of the app that was designed.
In some teams, the project is so complex that there may be developers or teams of developers who are solely dedicated to the maintenance of specific parts of the app. 

It is possible to organize and divide the team into teams responsible by different layers, components or features. 
For example, if the app has 3 components and there are 9 people allocated, it is possible to divide the team into 3 subteams of 3 people.

Integration begins to become a serious problem when you have to integrate code developed by a big team because the scale of integrations problems is directly associated with the size of the team.
So there are practices that are almost essential when working with a large team such as Continuous Integration and Modular development of components simultaneous (Independent Developability )
 but in the cases of very small teams they have a low positive impact or are even harmful.

Architectures like MVP and MVP allows a better separation of roles. 
They allows frontend/UX engineers to focus on UX needs rather than programming of business logic/ backend logic. 

These architectures also make each layer more decoupled from the others allowing an easier creation of unit tests allowing the developer to test the layer without even having the layers with which it will interact.
So Team size can also affect the overall decisions about the app. The point is that Team size matters.
Ants and bees live in large numbers and also have a large social organization that create together great engineering works such as anthills or hives in a very structured way. 
Think about this a bit.

PS: I did some updates to the article that made it grow a lot, so it was necessary to split it into three parts. You can read part two here.
Part one ends here. All feedback is welcome. Part two will talk about MVC, Apple MVC, MVC vs. MVP and MVP vs. MVVM.


Thank you for reading! ✨
