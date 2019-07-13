---
title: "Fruit Vision"
image:
  path: /assets/appstore/fruitglasses/cover.png
  thumbnail: /assets/appstore/fruitglasses/logo.png
  caption: ""
comments: false
---

I began to develop interest in Augmented Reality and Computer Vision especially when I [read rumors](https://www.businessinsider.com/apple-smart-glasses-rumors-everything-we-know-2019-3) that Apple was developing a smart glasses Technology
 that would be released in the future.
So I decided to develop a small project to test and learn some of the new  Apple's Machine Learning APIs and CreateML tool that allows train ML models.
The result of my experiences was Fruit Vision, an app that represents my first steps in the field of machine learning. 

## Video Demo
[Show Video](https://youtu.be/Y1IuyVyK4bc)

## Test Flight

(Will be added soon)

## App Features

The app lets you point the camera to a piece of fruit and show off a balloon with nutritional information.
Note: The model is currently only trained to detect bananas and oranges. Some will be added later.

## Technology

The Machine Learning model was created using CreateML.
The app uses CoreML, Vision Framework, ARKit and SpriteKit to renderer the Views.
The Fruit Nutrition information is obtained from the API:
https://developer.edamam.com
