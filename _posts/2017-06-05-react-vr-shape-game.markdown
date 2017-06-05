---
layout: post
title: "React-VR Shape Game Explained"
date: 2017-05-08 11:36:06 -0400
comments: true
categories:
---
![alt text](../images/react-vr-shape-game/09-shape-game.png)

# Build a BADASS SHAPE GAME with React

![alt text](../images/react-vr-shape-game/05-import-index.png)


First I need to **import elements** from react, react-vr, and a shape component I'll make a little bit later.

### The rest probably seems familiar, but what are these components we're importing from React VR?

#### 1. View

The most fundamental component for building a UI, **View** is a **container** that supports layout with flexbox, style, some touch handling, and accessibility controls.

#### 2. Text

A React **component** for displaying **text**.

Text supports nesting and styling, in a future release styling will be inherited from parent Text elements.

#### 3. AppRegistry

**AppRegistry** is the JS **entry point** to running all React Native apps.

App root components should register themselves with **AppRegistry.registerComponent**.

#### 4. AsyncStorage

**AsyncStorage** is a simple, unencrypted, asynchronous, persistent, key-value **storage system** that is global to the app.

It should be used instead of **LocalStorage**. I will use this to save the score.

# Anatomy of the Shape Game

I need to **render** a heading and a score to the scene as well as 4 shapes. Every time the player **chooses** a shape the game **resets** and the shapes are changed.

Every new round 3 shapes are the same and 1 is **special**.

I want the user to be able to look at a shape and get a point if it's the special shape.

## Define ShapeGame Class

![alt text](../images/react-vr-shape-game/06-shape-class.png)

I begin by defining the **class** and it's **constructor** function.  Inside I
call **super** to inherit **props** from the window since index.vr.js is the **root component**.

Next I set the **constructor's state**.  I want to keep track of 4 shape array slots, the player's score, and the index within gameShapes that is different than the rest.

## Register it with AppRegistry

![alt text](../images/react-vr-shape-game/07-app-registry.png)

Since I have now defined my **ShapeGame class component** I need to **register** it
with **AppRegistry** since **index.vr.js** is my **root** file.

## Load the score and Trigger new Shape Set on first render

![alt text](../images/react-vr-shape-game/03-componentDidMount.png)

When the **gameShape component** first renders onto the page and is in it's **componentDidMount** phase it loads the score from **AsyncStorage**, sets gameShape's score **state** equal to that value, and then triggers a **newGameSet**.

## Create styles

![alt text](../images/react-vr-shape-game/08-styles.png)

Before I create a render function to display the game I need to
create some **styles** to use.

First note how I've **nested css key value pairs** within my styles **jsx object**. This will allow me to keep my related styles for this page in the same place.

I can call them with **styles.game** and **styles.text** respectively.

The **transform property** uses a **translate property** and **3 integer coordinates** to dynamically alter the element's inline style. This property **positions** the **element** into the scene **within 3D space**.

## Render the game

![alt text](../images/react-vr-shape-game/00-index-render.png)

Just like with **vanilla react** I can only **export** *one container at a time*. To take care of this I'll nest all the scene's components inside a **react-vr View component**.

To **display** the *Game heading* and *score* **text** I have used a **react-vr Text component**.

In order to **display the 4 shapes** I need to iterate through the **gameShapes array** in my **state** and *render* them into the scene.

Inside my **map function** I'm returning another **View component** which will consist of 1 shape. I give it a **unique key** using index and define an **onEnter event**.

This is a **new event type** *specific* to **VR** that enables me to **trigger logic** when the user's *gaze crosshair* **enters the shape**.

When I **enter the shape with my gaze crosshair** I want to pass it the **map's index** within my **gameShapes array**, designating which of the **4 shapes** it is to my **pickShape
method**, which will **set the props to pass to the Shape component** which I will soon create in another file.

You'll notice I'm passing it a **transform property** with some **subtraction** and **multiplication** going on.  These **calculations** will make sure *each consecutive shape* spaces and **aligns itself** so the shapes **don't generate** on top of each other.

### Create a Shape Class Component in a new file

![alt text](../images/react-vr-shape-game/04-imported-shape-component.png)

Ok. First I **import** vanilla react/component, but you'll notice I'm also
importing several **components** from **react-vr**.

#### 1. Box

Box constructs a box-type 3D primitive in your scene.

It can be sized through dimWidth, dimHeight, and dimDepth properties, which take numeric values measured in meters. If one of these dimensions is not specified, it defaults to 1.

#### 2. Sphere

Sphere constructs a sphere-type 3D primitive in your scene.

It can be sized through a radius property, which takes numeric values measured in meters. You can also specify the number of width and height segments with the widthSegments and heightSegments props.

#### 3. Cylinder

Cylinder constructs a cylinder-type 3D primitive in your scene.

It can be sized through dimHeight, radiusTop, and radiusBottom properties, which take numeric values measured in meters.

You can also specify the number of segments in the cylinder with the segments property.

## Export an array containing all 3 react-vr shape components

I do that by defining an array of react-vr components, saving it to a const named shapes and then simply exporting it.

#### Render the Shape

I let my shape component become whatever shape corresponds the shape array index number passed into the component when it's being created. I also define an array of color codes that I also want to be set by props.

At the end I return this nameless Component and it will in turn render out to become whatever shape the game's logic dictates. The shape's color and position properties are then being set by the color and transform props.


## Back in index.vr.js Create a New Game Set

![alt text](../images/react-vr-shape-game/01-new-game-set.png)

#### game logic

Now for the logic that creates a single set of the game.  

On Enter with crosshair gaze if it is the correct shape you get a point.

If it is not the special shape you lose a point. Reset the game with a new assortment of shapes.

#### baseShapeId

I calculate our baseShapeId by generating a random index number that we will use to pass to the Shape Component to tell it what to become.

I then instantiate the specialShapeId and set it equal to the baseShapeId I just generated.

#### set the id of the specialShape

These shapes now pass into a while loop that checks to see if they are identical.

When the game is first set they are both the same so the conditional is true. This then tells the specialShape to generate a new random id.  

This makes sure that the special shape id is always different from the base id.

#### create this set's new shapes array state

I instantiate a newGameShapes array that I will use to store the new shape id values for the set.  

I iterate through the ShapeGame class's gameShapes state setting all the shapes in the new array equal to the new baseShapeId.

#### use index to replace

Now I randomly generate the new special index number, use it to replace the index in newGameShapes that is at the special index that it is equal to the specialShapeId, and finally update ShapeGame's state to have the latest array of game shapes as well as the special index.

## Pick shape

![alt text](../images/react-vr-shape-game/02-pick-shape.png)

1. I start by setting the local variable score to this.state.score. Any time an index is passed to pickShape it will check if this.state.specialIndex is the same as the shapeIndex passed in.  If it is then the ternary will flip true and return the score + 1 and - 1 if false.

2. After modifying the score I update the state with this new score.

3. After score is saved in state I save the score to AsyncStorage so that if I close my browser or refresh the page the score will stay.

4. After saving the score to AsyncStorage trigger a new game set.

# SHAPE GAME YAY!

Now you see how to make your very own shape game.

Go forth and create!
