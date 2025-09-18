*Game Programmer and Tool Developer*

#### Getting Started:

| Requirements | Installation | Installation | Projects |
| - GNU Compiler at least 17 version.<br> - Visual Studio Code is recommended. | - Download the engine source code from [itch.io](https://abhijit-biswas.itch.io/hazard-game-engine) or [github](https://github.com/gamdevAbhi/Hazard-Game-Engine.git).<br> - Extract the file to your location. | - Make sure to change the compiler path in “.vscode/tasks.json“ (if using vs code). | Cosmic Mayhem - [Source Code](https://github.com/gamdevAbhi/Cosmic-Mayhem) (Github)|

## Core Classes

#### GameLoop

```cpp
void Engine::GameLoop::initialize(std::string title)
```
**title** - it’s the game window title.  
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
**returns** - the window size.  
x = window width  
y = window height  
It returns the current window size of the application.  

#### Input

```cpp
enum KeyStatus {KEY_NONE, KEY_PRESS, KEY_HOLD, KEY_RELEASE}
```
**KEY_NONE** - if the specific key has no pressing event then this return.  
**KEY_PRESS** - if the specific key is pressed then this return.  
**KEY_HOLD** - if the specific key is hold then this return.  
**KEY_RELEASE** - if the specific key is release then this return.  

```cpp
Engine::Input::KeyStatus Engine::Input::getKeyStatus(int key)
// key value is the relative to glfw keys eg. GLFW_KEY_A value is for A key.
```
**returns** - the current status of the key in that frame.

```cpp
Engine::Input::KeyStatus Engine::Input::getMouseButtonStatus(int mouseButton)
// mouseButton value is the relative to glfw mouse buttons.
// eg. GLFW_MOUSE_BUTTON_0 value is for left mouse button key.
```
**returns** - the current status of the mouse button in that frame.

```cpp
glm::vec2 Engine::Input::getMousePos()
```
**returns** - the current position of the mouse in that frame.

#### Time

```cpp
float Engine::Time::getTimeScale()
```
**returns** - the current time scale value.

```cpp
void Engine::Time::setTimeScale(float timeScale)
```
**timeScale** - set the time scale value.

```cpp
float Engine::Time::getDeltaTime()
```
**returns** - the current delta time. Delta time is the time difference between two frames in seconds. It’s value depends on time scale.

```cpp
float Engine::Time::getFixedDeltaTime()
```
**returns** - the current unscaled delta time.

```cpp
int Engine::Time::getLastFPS()
```
**returns** - the previous frame per second.

#### UI

```cpp
glm::vec2 Engine::UI::getResolution()
```
**returns** - the current UI resolution size.

```cpp
void Engine::UI::setResolution(glm::vec2 size)
```
**size** - set the current UI resolution. default value is the window size.
