*Game Programmer and Tool Developer*

#### Getting Started:

| Requirements | Installation | Installation | Projects |
| - GNU Compiler at least 17 version.<br> - Visual Studio Code is recommended. | - Download the engine source code from [itch.io](https://abhijit-biswas.itch.io/hazard-game-engine) or [github](https://github.com/gamdevAbhi/Hazard-Game-Engine.git).<br> - Extract the file to your location. | - Make sure to change the compiler path in “.vscode/tasks.json“ (if using vs code). | Cosmic Mayhem - [Source Code](https://github.com/gamdevAbhi/Cosmic-Mayhem) (Github)|

## Documentation

#### GameLoop

```cpp
void Engine::GameLoop::initialize(std::string title)
```
***title** - it’s the game window title.  
It’s called before calling any engine features. It initialize the engine and it’s necessary components like collider manager, rendering manager, window, inputs etc.

```cpp
void Engine::GameLoop::begin()
```
It’s start the games loop.

```cpp
void Engine::GameLoop::endGameLoop()
```
It makes the games loop to break. Uses for exiting the application.

```cpp
glm::ivec2 Engine::GameLoop::getCurrentWindowSize()
```
**Return** - the window size.
x = window width
y = window height.
It returns the current window size of the application.

