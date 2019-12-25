# Blueprint Structure

- GAME
    - MAP
        - ACTOR
            - BP
                - NODE
                    - DATA - VARIABLE

# Blueprint Classes

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

- [ ] P31