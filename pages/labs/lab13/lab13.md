---
title: "Lab 13: Transitioning, Saving and Building"
parent: Labs
layout: home
nav_order: 13
nav_exclude: true
---

# Lab 13: Transitioning, Saving and Building
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

## Lab Overview
This lab will be going through how to transition between scenes and how to create save data in a game. Much of this lab will be topics that are final components in building a game, such as transitioning through scenes, creating and loading save data, and building an .exe file. In all, this will cover: 

1. Transitioning between different scene by pressing a button or key 
2. Resetting/reloading a level 
3. Setting up pausing in game 
4. Using DontDestroyOnLoad 
5. Saving and loading a game 
6. Building an executable 

This project file is set up with multiple scenes, some simple sprites and some scripts. The simple game is just an endless runner to collect candy, where the number of candies is saved between scenes, and can be saved between opening and closing the game. Most of the code that is currently provided in the project is just to make the game function and does not affect anything else in the lab. 

## Lab Instructions
Scene Transitions 
Scenes can be controlled in code using Unity.SceneManagement. In the Scenes folder, there should be 3 scenes (Menu, level1, and level2). In Menu, there are a few objects under the Canvas game object (Figure 1) in the Hierarchy. This is where all UI elements are. There is a UI lab for how to work with UI, but that is not necessary for this lab. 

Figure 1


![](images\figure1.png)


The goal is to make scene transitions happen by pressing a button. To do this, we will edit the SceneController script. Some parts are already there, but we will be adding to it. To be able to use SceneManagement, ”using UnityEngine.SceneManagement” must be added so that the top looks like Figure 2. 

Figure 2


![](images\figure2.png)


The first thing we will be doing is creating a way to reload the level by pressing ”r”. The simplest way to do this is to reload the current scene. One way to reload a scene is by using its name. 

1. Create a new string variable called p_SceneName (the p_ denotes this being a private variable. It is simply a convention and can be ignored) with the purpose of storing the name of the current scene. 
2. Then, in the Start method, retrieve the current scene’s name and store it in our p_SceneName variable. See Figure 3.

The first thing we will be doing is creating a way to reload the level by pressing ”r”. The simplest way to do this is to reload the current scene. One way to reload a scene is by using its name. 
Create a new string variable called p_SceneName (the p_ denotes this being a private variable. It is simply a convention and can be ignored) with the purpose of storing the name of the current scene. 
Then, in the Start method, retrieve the current scene’s name and store it in our p_SceneName variable. See Figure 3.

Figure 3

![](images\figure3.png)


To reset the scene, a new function will be created that will be called in Update() whenever ”r” is pressed. In the new function, called ReloadScene(), we can just callSceneManager.LoadSceneAsync(p_SceneName) and it will reload our scene. LoadSceneAsync(name) loads the new scene separately from the scene currently loaded and will show the new scene as soon as it is finished loading. It will also automatically unload the current scene, so there is no need to unload a scene. The exception to this is if the mode of LoadSceneAsync is changed. By default, the mode is set to Single, so that only a single scene is loaded at once. However, if the mode is set to Additive, then the scenes will add on top of each other, so that scenes do have to be manually unloaded. Our game only needs to use Single mode. Once this part of the code is finished, Update() and ReloadScene() should look like Figure 4: 


Figure 4

![](images\figure4.png)

Figure 5

![](images\figure5.png)

Figure 6

![](images\figure6.png)

Figure 7

![](images\figure7.png)

Figure 7

![](images\figure7.png)

Figure 8

![](images\figure8.png)

Figure 9

![](images\figure9.png)


Figure 10

![](images\figure10.png)

Figure 11

![](images\figure11.png)

Figure 12

![](images\figure12.png)

Figure 13

![](images\figure13.png)

Figure 14

![](images\figure14.png)

Figure 15

![](images\figure15_1.png)


![](images\figure15_2.png)

Figure 16

![](images\figure16.png)

Figure 17

![](images\figure17.png)

Figure 18

![](images\figure18.png)

Figure 19

![](images\figur19.png)

Figure 20

![](images\figure20.png)

Figure 21

![](images\figure21.png)

Figure 22

![](images\figure22.png)

Figure 23

![](images\figure23.png)


Figure 24

![](images\figure24.png)

Figure 25

![](images\figure25.png)

## Bug Reports
If you experience any bugs or typos within the lab itself, please report it [here!]

[here!]: https://forms.gle/1C2GPHGDHCQo3WWe7 