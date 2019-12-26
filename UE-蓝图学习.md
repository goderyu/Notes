# Getting Started with Unreal Blueprints

## Blueprint Structure

- GAME
    - MAP
        - ACTOR
            - BP
                - NODE
                    - DATA - VARIABLE

## Blueprint Classes

 the most common Parent Classes: 

- Actor: It is an object that can be placed or spawned in the world
- Pawn: It is an Actor that can be possessed and it receives input from a Controller (which can be a user or an Artificial Intelligence)
- Character: It is a Pawn that includes the ability to walk, run, jump, and so on
- PlayerController: It is an Actor that is responsible for controlling a Pawn
- Game Mode: It defines the game rules, scores, and any aspect of a game type


> Due to this harmony between Blueprint and code, if you choose to create a project from the Blueprint section you can, at any time, generate its C++ project: the engine will create the Visual Studio solution as soon as you add your first code class from the editor (File | Add Code to Project

|Can be created in Unreal Editor: |Needs to be created using an external software:|
|---|---|
| Game Levels | Static Meshes |
| Particle Systems | Skeletal Meshes |
| Cinematic Sequences | Skeletal Animations |
| Blueprint Scripts | Textures |
| AI Navigation Meshes | Sounds (WAVs) |
| Pre-calculated Light Maps | IES Light Profiles |
| Level Lights | Nvidia APEX files (APB and APX) |
| Materials| |

## Graph editor

`Click + Drag + C` - on the empty space Adds a comment box containing the selected nodes

## Custom Data Type

These types can be regrouped in five categories, as follows:

- Structure: Struct (value) types. A structure is a container of custom variables. It is used to group related variables in a single entity in order to simplify data combining and data management.
- References to objects or Actors: As the name suggests, these data types are references of any object/actor in the game. They are useful when we want to communicate between two different Blueprint classes.
- References to interfaces: They are the same as object pointers; however, they are referred to as interfaces objects.
- References to classes: Similar to object references, this type of variable contains references to a class. The main difference is that this type points to the default class, while the object reference points to a single instance of this particular class in the game.
- Enumeration: An enumerator is basically a byte variable that, instead of numbers, has a human-readable list of names. It can be used to store any kind of object state or type (Game States, tree types, weapon types, player states, and so on)

## Blueprint debugger tab

From the Window tab, in the menu bar, you can open Blueprint Debugger. This panel shows all the watched variables or Breakpoint assigned. You can add multiple Blueprint debugger tabs by holding Shift and clicking on Actor in your scene.

## Project Folder Description

Let's have a brief look at these folders, as shown in the following:

- Binaries: It contains executable or other files that are created during compiling.
- Config: Configuration files are used to set values that control engine and game default behavior.
- Content: It holds all the content of the game, including asset packages and maps.
- Intermediate: It contains temporary files that are generated during the 
building of the engine or game.
- Saved: It contains autosaves, configuration (same *.ini of Config folder) files, and log files. Here, you can find crash logs, hardware information, and swarm options and data.
- Source: It contains all the source files for the game divided in to object class definitions (.h files) and object class implementation (.cpp files).

# Tic-Tac-Toe

In order to create this game, we need the following:
- A static camera that is always pointing to the grid
- A 3 x 3 grid that is made by nine individual square Static Mesh
- Two Symbols: O and X
- A user interface showing which player can make his move and the state of the game
- A game logic: a controller for the grid state and a turn handler

BuildString