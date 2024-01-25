---
title: "Lab 15: Raycasting"
parent: Labs
layout: home
nav_order: 15
nav_exclude: true
---

# Lab 15: Raycasting
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

## Introduction

This lab expects you to understand physics layers and colliders. With that said, let’s ask the question: what is raycasting? In Unity, when someone talks about raycasting, they are referring to a form of collision detection that uses rays or one of the other potential shapes. Unlike colliders, raycasting is performed in a script by calling a Physics or Physics2D function. This lab will be covering:

- 2D vs 3D raycasting
- How to raycast
- The difference between casting and overlapping
- The difference between all and not all
- The various types of shapes that can be cast in 2D

## 2D vs 3D Raycasting

In order to perform raycasting in 2D, you must use the Physics2D class while performing raycasting in 3D requires you to use the Physics class. The main difference between 2D and 3D is that you are expected to be using the 2D version in a 2D game and the 3D version in a 3D game. Below are a portion of the signatures:

//Figure 1

As we can see, the 2D version uses Vector2’s while the 3D version uses Vector3’s. Beyond the different types, using these two is nearly the same. You choose a starting position for your raycast, a direction for it to move in, a distance, and that is about it. One key difference of note is the retrieval of information from a collision. In 2D, the Raycast directly returns the information while in 3D it only returns whether there was a collision. In order to retrieve the collision information from a Raycast in 3D, you must use do something like the following:

//Figure 2

That’s it! If you now add an “EnemySpawner” prefab into the scene and press play, you will be able to fire the normal laser and kill enemies to gain points.

## Minor Differences and a Mega Laser

There are three options available to you when choosing how to raycast. You have the normal “Raycast” that we just learned about, but you also have “RaycastAll” and “RaycastNonAlloc”. When using “RaycastAll” or “RaycastNonAlloc”, the ray will travel to the end of its range regardless of whether it hit something or not. What ends up happening is that it captures all of the colliders in front of it instead of solely the first. “RaycastAll” is a slightly less efficient version of “RaycastNonAlloc” so the signature is different but otherwise, they can be treated identically. The image on the right shows an example of a “RaycastAll”

//Figure 3

In order to proceed to the Mega Laser, we need to learn about some of the other types of “rays” that we can cast. When casting a ray, you are casting a single line. However, it is possible to use other shapes for different circumstances! Two of the other shapes available to us are circle and box. When casting a box or a circle, you can treat it the same as a ray. Provide it with a starting point, a direction, and a range, then it will move from the provided starting point to the ending point described by its direction and range. A circle cast would form this type of trail and the first thing in its path will be hit. Quick note: in the function signature, you must define the radius of the circle you are casting.

//Figure 4

Now, what if we want to model an explosion? We can do that using an overlap instead of a cast. If we take an “OverlapCircleAll” for example, this will place a circle with its center and radius defined by us. Anything with a collider within our circle will be returned. The partial signatures for “OverlapCircle” and “OverlapCircleAll” are:

//Figure 5

We are finally ready to implement the Mega Laser! Start by copy-pasting everything from your “ShootLaser” method into your “ShootMegaLaser” method. Now they should be identical. The one modification to make is to replace every instance of “m_LaserPrefab” with “m_MegaLaserPrefab” so that the correct graphics are displayed.


The first thing we want to do is create an explosion so that the mega laser can destroy multiple enemies with a single shot. We will take advantage of “OverlapCircleAll” here. Add the following line after “AddScore()”:

//Figure 6

Essentially, we are going to check for anything within a circle of radius two, with its origin at the hit point of the laser. We need an array of “Collider2D” because of the possibility that there is more than one enemy blob in the specified region.

Next, we will simply iterate over each of the items that were in our region, check if they are an enemy and if they are, hitting them and giving ourselves a point.


//Figure 7

//Figure 8

If you go into the game and try it out now, shooting the mega laser into a group should kill the group (assuming you hit an enemy inside the group)! To make the explosion slightly more clear, let’s add an accompanying effect. After the first enemy was hit and before the first point is given, add a call to “PlayMegaLaserExplosion(...)” and use “hit.point” as the point the explosion should originate from.

Your code should look like:

//Figure 9

Sweet! There is now an explosion! Unfortunately, you may notice that, while the mega laser is large, it sometimes does not explode when clearly hitting an enemy. That is because we are still using a ray - which is a single thin line. To fix this, we will switch to casting a box instead. Go ahead and modify our initial raycast from

//Figure 10

to

//Figure 11

As before, the origin is at the start, however instead of just using a direction and range, we now also have a size and an angle. The size is to define the size of our box. If we choose a very small size, it may be preferable to just switch back to using a ray instead. As for the angle, we just ensured that it is facing the direction of our laser. In the instance to the right, we want the box angled appropriately. That means we do not want a box with the angles shown in the below images.

//Figure 12
//Figure 13

With that, we are just about done. Unfortunately, if you play the game now and aim down, you will experience some weird behavior. The behavior is being caused by the box cast colliding with the collider on the right of the screen (used to know when an enemy cannot be killed anymore).

Luckily for you, we can take advantage of the fact that the enemies are on a different physics layer. All we have to do is tell our box cast exactly which layers it is allowed to hit. We will be using something called a layer mask to achieve this. As you know, if you go into Tags and Layers (under Edit → Project Settings), you will see all of the available layers. Now imagine that we had a binary representation of these layers, with each bit initially set to 0. If you want the box to collide with a particular layer, you must set its corresponding bit to a 1. For example, the Default layer is the first layer so we can assume it corresponds to the first bit. Furthermore, for the sake of this example, assume we have a total of five layers (equating to five bits). If we want to only affect the Default layer, we use the binary number 00001. If we want to affect every layer except for the Default layer, we use the binary number 11110.

For our project, we only want the box to collide with objects found on the Enemy and AntiExplosionEnemy layers. As we do not immediately know which bit to turn on, we must perform a roundabout series of operations to create our binary number. First, we take the number 1, then we shift it by the layer’s “number” (thus enabling collisions with that layer). To retrieve a layer’s “number”, we do “LayerMask.NameToLayer(...)”. After performing this on our two layers, we perform a bitwise or operation to combine the layers we had enabled into a single LayerMask. The box cast should now look like:

//Figure 14

## Lab Summary

Raycasting is a very powerful tool that allows us to create and detect collisions in the behavior that we want. They are a fast and simple way to produce collisions without needing to create separate GameObjects with collider components. Hopefully from this lab, you can start to understand its many use cases and the common game mechanic patterns that it solves. Raycasting comes in all sorts of shapes and behaviors, definitely check out the Unity documentation if you’d like to learn more: https://docs.unity3d.com/ScriptReference/Physics.html

## Check Off

- Make the explosion of the mega laser only affect enemies on the Enemy layer
- Run the game and show the following (feel free to add more spawners in):
    - Aiming down and shooting the mega laser shoots all the way down
    - Hitting an enemy with the mega laser will kill enemies in the surrounding region
    - The normal baby laser can kill enemies too
- Explain what raycasting means.
- Provide an example of raycasting (unrelated to this lab).




## Bug Reports
If you experience any bugs or typos within the lab itself, please report it [here!]

[here!]: https://forms.gle/oiyM6iu3MinHfmNc7 