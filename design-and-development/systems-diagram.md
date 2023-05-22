# 2.1 Design Frame

## Systems Diagram

<figure><img src="../.gitbook/assets/Screenshot 2023-05-22 at 09.13.21.png" alt=""><figcaption></figcaption></figure>

This diagram shows the different parts of the weather monitoring and prediction system that I will be focusing on. I have split each section into smaller sub-sections, using [computional methods ](../analysis/1.4b-computational-methods.md)such as decomposition. Throughout the development stage, I will pick one or two of these sections to focus on at a time to gradually build up and piece the personal weather station together. Blue sections represent features that I must include for my solution to be functional, while the purple section includes features that a stakeholder may want to include but are not essential to completing the project. Each of these components corresponds roughly to a section in [1.5 Sucess Criteria](../analysis/1.5-success-criteria.md).

## Usability Features

Usability features will ensure that any stakeholder is able to use the weather monitoring and prediction system. Therefore, I will use the following key points to make my project accessible and provide the best user experience. I will make my solution effective, efficient, engaging, error tolerant and easy to learn. These criteria will help me design a program that is usable by a stakeholder.

### Effective

I need to make sure that all processes carried out by the personal weather system are effective and are carried out automatically. This means a stakeholder will be able to use my solution efficiently without any additional input. The personal weather station must be able to take readings from the outdoor environment and transmit data to the internet to be recorded on a database. The accompanying app must then be able to pull this data and display it in a way that can be used effectively by a stakeholder.

As well as the weather station communicating with the app, communication between the app and stakeholder must be effective to ensure they get the best possible user experience. I will use tables and graphs to present data so that it can be easily understood. The ways I aim to make my project effective are shown below.

#### Aims

* The personal weather station will need to automatically take readings from the environment at regular intervals&#x20;
* It must then be able to communicate with the app wirelessly without input from the user
* The user interface will then need to display weather monitoring and forecasting information visually

### Efficiency

Making my project efficient involves making the weather station easy to set up and use so that the stakeholder can quickly monitor their outdoor environment. The app must be easy to navigate and connect to the weather station, and then be updated automatically with accurate weather information. To simplify the setting up process, the user should only have to follow basic instructions before setting up the device. Once this is done, they should quickly start recieving usable weather data from the app.

All previous measurements must be recorded so that trends tracked by the user. This will also allow the device to detect sudden changes in weather conditions and send notifications to the user. Another important feature of my solution is therefore how data is stored. Previous weather information should be recorded on a online database, so that it can be quickly and efficiently accessed by a user upon request.

#### Aims

* It should be easy to set up the app and connect to the weather station
* A stakeholder should be able to navigate through the app easily and understand all displayed data
* All previous data readings should be uploaded to a database so that they can be easily accessed by the user at a later date

### Engaging

Making the solution for the user will ensure that the device is interesting and easy to use. While the main purpose of this project is to provide a stakeholder usable information about their local environment, my solution must still be engaging and interesting to use. I must also ensure my solution prefferable to other traditional weather applications.



The solution is engaging for the user to use. To do this, I will create 5 levels and an online multiplayer mode to keep the players engaged and allow them to have fun while playing the game. Using vector style art will also make the game nicer to look at than blocks, so will draw more people in, keeping them engaged.

#### Aims

* Create a series of levels to work through
* Create a multiplayer mode to play
* Incorporate a style of game art the suits the game

### Error Tolerant

The solution should have as few errors as possible and if one does occur, it should be able to correct itself. To do this, I will write my code to manage as many different game scenarios as possible so that it will not crash when someone is playing it.

#### Aims

* The game doesn't crash
* The game does not contain any bugs that damage the user experience

### Easy To Learn

The solution should be easy to use and not be over complicated. To do this, I will create simple controls for the game. I will make sure that no more controls are added than are needed in order to keep them as simple as possible for the players.

#### Aims

* Create a list of controls for the game
* Create an in-level guide that helps players learn how to play the game

## Pseudocode for the Game

### Pseudocode for game

This is the basic layout of the object to store the details of the game. This will be what is rendered as it will inherit all important code for the scenes.

```
object Game
    type: Phaser
    parent: id of HTML element
    width: width
    height: height
    physics: set up for physics
    scenes: add all menus, levels and other scenes
end object

render Game to HTML web page
```

### Pseudocode for a level

This shows the basic layout of code for a Phaser scene. It shows where each task will be executed.

```
class Level extends Phaser Scene

    procedure preload
        load all sprites and music
    end procedure
    
    procedure create
        start music
        draw background
        create players
        create platforms
        create puzzle elements
        create enemies
        create obstacles
        create finishing position
        create key bindings
    end procedure
    
    procedure update
        handle key presses
        move player
        move interactable objects
        update animations
        check if player at exit
    end procedure
    
end class
```
