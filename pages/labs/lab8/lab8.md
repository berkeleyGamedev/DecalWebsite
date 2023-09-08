---
title: "Lab 8: Animator and Blend Trees"
parent: Labs
layout: home
nav_order: 8
---

# Lab 8: Animator and Blend Trees
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

## Overview

The **Animator** is an element with an associated Component and Window (similar to Inspector, Project, Animation, etc.) 

![](images\image1.png)

This window is where you connect your code to the visuals by defining the animation transitions. 

**Blend Trees** are used inside of the Animator to organize and smooth the transition between similar Animations. 
Use cases include transitioning between walk and run animations depending on how far you’re pushing your joystick or simply transitioning between up/down/left/right movement animations in a top-down 2D game (think retro Legend of Zelda). 

In this lab, we will go through the process of implementing an Animator component for a player GameObject using predefined animations (either through the provided frames or through your own .anim files). Then we will communicate between the code handling the player movement and the animation transitions to finish implementing a simple 2D platformer player with idle, run and jump animation states. 
After that, we will implement a blend tree to transition between eight different animation states for a top-down 2D game. 
**By the end of this lab, you should be comfortable with creating and customizing Animator mappings as well as recognizing how to simplify mappings using blend trees to support whatever your future games will require.**


## Setup

Open your Animator scene by going to “Window” tab in the top bar and navigating to `Animation>Animator`. Once it’s open, double click on the `Animator scene`, or make sure the `Animator scene` is open, load in the pre-setup. You have two options to set up the rest of this lab in your `Animator`: load your animation frames or load your .anim files if you have pre-made animations.

### Load Your Animation Frames

1. For each folder within `Animator/Animations/Frames`, select all the images, click and drag them over the Player GameObject in the `Hierarchy View`. 
2. This will create animations using each set of images you selected. Name them Idle_Anim, Jump_Anim and Run_Anim respectively and create them inside the `Assets/Animator/Animations` folder. 
3. Be sure to preview the animations to make sure they’re playing at the right speed, review the Animation lab for details on how to use the `Animation View`. 
4. Notice that inside the `Animations` folder in the `Project View`, there is a new item called “Player.” 
5. Select this item to reveal that it is a Component of type Animator. 
6. Select the Player GameObject in the `Hierarchy`, note that this Component has automatically been added as a result of your dragging and dropping. 
7. Double-click the Player Animator in the `Project View` to open the `Animator View`. Alternatively, you can open the Animator by navigating to `Window>Animation>Animator`.

### Load Your .anim Files into States

1. If you have your own .anim files you want to use for this tutorial, add the Animator component to the Player GameObject by selecting “Add Component” and typing in “Animator” and title it “Player.” Save it to the `Assets/Animator/Animations` folder.
2. Double-click the Player Animator in the `Project View` to open the `Animator View`. Alternatively, you can open the Animator by navigating to `Window>Animation>Animator`.
3. Right click the empty space and select “New State.”
4. Select this new state so that its properties appear in the `Inspector`.
5. Drag your .anim file from the `Project` into the `Inspector` over the Motion field to load your animation.
6. Rename your new state(s) accordingly.


## The Animator

Your Animator should look something like this, with an Entry state, an Any State, three _anim states, and one Exit state.

![](images\image11.png)

### Transitions

Upon Entry, we will go directly to the Idle animation. Think for a moment about how you would want to be able to transition from one state to another, sketch out or visualize what that would look like in this mapping, then continue reading. 4 From Idle, we want to be able to transition into Run, and from either Idle or Run we want to be able to transition into a Jump. After we finish Jumping, we want to be able to stand Idle or continue Running, and of course we also want to be able to stop Running and stay Idle. To create a transition, right-click a state and select “Make Transition” and left-click the destination state. 
Create transitions according to the following Animator mapping:

![](images\image10.png)

How will we determine when to execute a transition? We need to define parameters that will only activate when our specified conditions are met.

**At a high level:**
- We will be idle if we are not moving 
- We will run when the magnitude of our velocity (our speed) is anything other than 0
- We know we will jump when the jump button is pushed. 

On the left half of the `Animator View`, observe the `Parameters` tab. Use the + button to the right of the search field to create a new float parameter called “Speed” and a new Trigger parameter called “Jump.” Note that the capitalization of these variable names do matter, so try to stay consistent in your naming conventions. 
Now we will implement these parameters into the State Transitions we defined above. 

Select the transition arrow from Idle to Run. In the `Inspector`, click the + button under the Conditions tab and define it to react only when Speed is greater than 0.5. We just need a small number; technically, a number like 0.00001 would suffice, but 0.5 just looks cleaner and functions the same for the purposes of this tutorial so we’ll stick with that.

![](images\image10.png)

Now make the following changes:
- Select the transition from Run to Idle and set it to activate only when Speed is less than 0.5 
- Select the transition from Jump to Idle and set it to activate only when Speed is less than 0.5 
- Select the transition from Any State to Jump and set it to activate only when the Jump Trigger is activated.

You may be wondering why we can’t use the Any State to transition to Idle and Run the same way we did for Jump. It’s not that you can’t, there are many ways to implement an Animator, this is just one of them. You could use a Boolean to check isMoving, instead of having a Speed float variable, or have an isJumping Boolean to check for whether a foot collider is touching the ground or not. Be sure to explore, expand or redesign this implementation as part of the extra challenges part of this lab

### Connecting to Scripts

Our Animator logic is all set up, everything makes sense in theory but none of it has truly been implemented yet. To do this, we’ll have to communicate our movement to the Animator Component through our movement script. 
Create a Movement.cs script and attach it to the Player GameObject. 
In order to communicate with the Animator, we’ll need to create a reference to it in our script. 

You can either (1) create a private reference and use GetComponent to retrieve the Animator or (2) create a public reference and drag/drop the Animator in from the `Inspector View`.

## Bug Reports
If you experience any bugs or typos within the lab itself, please report it [here!]

[here!]: https://forms.gle/1C2GPHGDHCQo3WWe7 