---
title: "Lab 2: Basic Scripting"
parent: Labs
layout: home
nav_order: 2
---

# Lab 2: Basic Scripting
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

## Lab Introduction

In this lab, you will gain a surface level introduction to programming by interpreting and editing elements of pre-written scripts. You will not be required to create any scripts for this lab. <span style="color:blue">**Information only relevant to artists will appear in blue**</span>. <span style="color:red">**Information only relevant to programmers will appear in red.**</span>

You will be adding a new enemy type to a simple minigame, converting both enemy types into Unity Prefabs, and then spawning your enemy Prefabs into the minigame.

Upon completion of this lab, you should have a basic understanding of what variables are and of potential ways to modify them. You will also learn how to create and edit Prefabs in Unity. <span style="color:red">Programmers will learn about variable protection levels and gain a surface level introduction to how different types of scripts interact with each other.</span>

## Lab Instructions
### Editing Scripts
1. Select the Main scene from the Assets folder in the Project tab. The image on your screen should match the one seen in Figure 1.
    - 
    Notice that the Player GameObject under Main in the *Hierarchy* tab corresponds to the purple square, while the SimpleEnemy GameObject corresponds to the red circle. 

![](images\image5.png)

2. Press play and observe how the game currently plays. Use WASD or the arrow keys to move the Player around. a. If the SimpleEnemy object touches the Player, the game should end.


In order to make this rather dull minigame more interesting, we will add multiple enemies and ensure that the player can survive more than one hit before dying.

3. Open the Scripts folder, located under Assets in the Project tab. Navigate to the Player folder, find the script called **Health**, and double click to open it.

4. You should see several lines of code similar to the ones transcribed below. Read the section indicated below.

<span style="color:red">

        public class Health: MonoBehaviour {

        /// Non-programmers only need to look between >>>>> and <<<<<

        /// >>>>>

        // Most scripts will have a collection of Variables at the top. 
        Similar to their function in mathematical equations, a variable is a 
        name that is associated with a value. In this example, the variable 
        startingHealth is set to a value of 1. Following the variable is 
        a comment that explains its purpose. Variables preceded by the keyword 
        public will show up in the Inspector tab when you attach this 
        script to a GameObject in the Hierarchy tab.

        public int startingHealth = 1; 
        // This is how much health you have before you die 
        // <<<<< Non-programmers may stop reading here 

        int currentHealth;


In addition to the **public** keyword used in the code block above, several other **variable protection levels** exist in programming:
- **Private** — The default setting, meaning that no other script may access this variable (including child scripts). 
- **Public** — All scripts may access this variable. Additionally, it will show up as a field in the Inspector tab when this script is attached to a GameObject.
    - If you require a variable to be public but **not** show up in the Inspector, type **HideInInspector** before the variable declaration or on the line above it.
- **Protected** — Only child scripts may access this variable. No other scripts can.

Each GameObject with the Health script attached to it will have its own copy of the variables declared within the script (unless the variables are preceded by the keyword **static**). Public variables will remain set to their default values unless set to another one via the Inspector. **Inspector changes to public variables will override values set in scripts.**
</span>

We will now modify the **startingHealth** variable in the Inspector in order to allow our Player to survive more than one hit.

![](images\image8.png)

5. Exit out of this script and return to Unity, then select the Player GameObject in the Hierarchy. In the Inspector, displayed in Figure 2, locate the component titled **Health (Script).** The component should have only one modifiable field, titled **Starting Health.** Change this value to any number greater than 1.

***Checkoff Requirement:*** The Player should be able to survive more than one hit from an Enemy.

## Prefabs
A Prefab is a predefined GameObject that is saved as an Asset, in a manner similar to how one would save a script or an art asset. Prefabs are created by dragging an existing GameObject from the Hierarchy into the Project tab. Once a Prefab has been created, you can repeatedly drag it from the Project tab into the Scene in order to create multiple copies of the object.