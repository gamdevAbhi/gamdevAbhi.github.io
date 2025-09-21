*Game Programmer and Tool Developer*

## Contents
- [Getting Started](#getting-started)
- [Core Classes](#core-classes)
  - [GameLoop](#gameloop)
  - [Input](#input)
  - [Time](#time)
  - [UI](#ui)
- [Management Classes](#management-classes)
  - [RendererManager](#renderermanager)
  - [ColliderManager](#collidermanager)
- [Actor and Component Classes](#actor-and-component-classes)
  - [Actor](#actor)
  - [Component](#component)
  - [Collider](#collider)
  - [Renderer](#renderer)
- [Resource Classes](#resource-classes)
  - [Sprite](#sprite)
  - [Font](#font)
- [Component Classes](#component-classes)
  - [Audio](#audio)
  - [Button](#button)
  - [Camera](#camera)
  - [Transform](#transform)
  - [RectTransform](#recttransform)
- [Renderer Classes](#renderer-classes)
  - [SpriteRenderer](#spriterenderer)
  - [BillboardRenderer](#billboardrenderer)
  - [Text](#text)
- [Collider Classes](#collider-classes)
  - [BoxCollider](#boxcollider)
  - [CircleCollider](#circlecollider)


### Getting Started

| Requirements | Installation | Installation | Projects |
| - GNU Compiler at least 17 version.<br> - Visual Studio Code is recommended. | - Download the engine source code from [itch.io](https://abhijit-biswas.itch.io/hazard-game-engine) or [github](https://github.com/gamdevAbhi/Hazard-Game-Engine.git).<br> - Extract the file to your location. | - Make sure to change the compiler path in “.vscode/tasks.json“ (if using vs code). | Cosmic Mayhem - [Source Code](https://github.com/gamdevAbhi/Cosmic-Mayhem) (Github)|

## Core Classes

### GameLoop

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

### Input

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

### Time

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

### UI

```cpp
glm::vec2 Engine::UI::getResolution()
```
**returns** - the current UI resolution size.

```cpp
void Engine::UI::setResolution(glm::vec2 size)
```
**size** - set the current UI resolution. default value is the window size.

## Management Classes

### RendererManager

```cpp
int Engine::RendererManager::getLastRendererCount()
```
**returns** - the last total number of renderer needed to drawn.

### ColliderManager

```cpp
int Engine::ColliderManager::getTag(std::string tagName)
```
**returns** - the tag value of the tag.

```cpp
int Engine::ColliderManager::addTag(std::string tagName, bool selfRelation)
```
**returns** - the tag value of the added tag.  
this function create a tag based on the tag name and assign a value. if **selfRelation** is true then two collider with same tag will interact.  

```cpp
void Engine::ColliderManager::addRelation(int tag1, int tag2)
```
this function create a relation between two tags. if relation between two tags exist then colliders with tags will interact.

## Actor and Component Classes

<img src="../assets/images/hz_acr.jpg" alt="Screenshot">
<img src="../assets/images/hz_cs.jpg" alt="Screenshot">

### Actor

```cpp
Engine::Actor* Engine::Actor::createActor(std::string name)
```
**returns** - a pointer to a new actor with the name.  
It creates a new actor with the name. Every actor will have **Transform** component if creates in this way.
```cpp
Engine::Actor* Engine::Actor::createUIActor(std::string name)
```
**returns** - a pointer to a new UI actor with the name.  
It creates a new UI actor with the name. Every actor will have **RectTransform** component if creates in this way.
```cpp
Engine::Actor* Engine::Actor::getActor(std::string name)
```
**returns** - a pointer to the actor with the name.  
It finds actor with the name and return it. If no actor found with that name it will return **nullptr**.

```cpp
std::vector<Engine::Actor*> Engine::Actor::getActors(std::string name)
```
**returns** - a vector of pointers of the actors with the name.  
It finds all the actors with the name and return it.

```cpp
int Engine::Actor::getActorCount()
```
**returns** - the number of actors exist in the game.

```cpp
void Engine::Actor::clearActors()
```
It destroy all the actors which **manualDestroy** set to false before the next frame.

```cpp
std::string this->getName()
```
**returns** - the actor’s name.

```cpp
void this->setName(std::string name)
```
**name** - set the actor’s name.

```cpp
bool this->getActive()
```
**returns** - the actor’s active status.

```cpp
void this->setActive(bool status)
```
**status** - set the actor’s active status.  
**Note** - if actor’s active set to false then the **update()**, **lateUpdate()**, **fixedUpdate()** will not be called and other things like collision detection, rendering will also be ignored.

```cpp
bool this->getManualDestroyStatus()
```
**returns** - the actor’s manual destroy status.

```cpp
void this->setManualDestroyStatus(bool status)
```
**status** - set the actor’s manual destroy status.  
**Note** - if actor’s manual destroy status set to true then the actor will only be destroyed by **setDestroy()** call or after game loop is break.

```cpp
void this->setDestroy(bool childIncluded)
```
the actor will be destroyed before the next frame start.  
**Note** - if the **childIncluded** is false then only the actor will be destroyed and it’s child’s parent will **nullptr** else all childs also be be destroyed.

```cpp
// T must be derived from Engine::Component class
T* this->addComponent<T>() 
```
**returns** - a pointer of the created component.  
It creates the component and attach it to the actor. After adding the component **Start()** function is called.

```cpp
// T must be derived from Engine::Component class
T* this->getComponent<T>()
```
**returns** - a pointer of the added component from the actor.  
It finds the component added to the actor. if no component is found then **nullptr** is returned.

```cpp
// T must be derived from Engine::Component class
std::vector<T*> this->getComponents<T>() 
```
**returns** - a vector of pointers of the added components from the actor.  
It finds the components added to the actor.

### Component

**Component** - It’s an abstract class and base class for all classes which can be added to actor. An actor can have multiple components. Components are special because they have some special functions that directly called in games loop.

```cpp
Engine::Actor* this->getActor()
```
**returns** - the actor this component attached to.

```cpp
void this->setDestroy()
```
set the component to destroy before next frame.

**Special virtual functions which called directly from games loop:**

```cpp
void Engine::Component::start()
```
**Call Time** - when the component is added to an actor.

```cpp
void Engine::Component::update()
```
**Call Time** - this function is called in every frame.

```cpp
void Engine::Component::lateUpdate()
```
**Call Time** - this function is called after all update functions are executed.

```cpp
void Engine::Component::fixedUpdate()
```
**Call Time** - this function is called every fixed time (default 0.2 seconds).

```cpp
void Engine::Component::onTransformChanged()
```
**Call Time** - this function is called if transform matrix is changed (if used wrongly then endless loop may occur).

```cpp
void Engine::Component::onCollisionEnter(Engine::Collider* collider)
```
**Call Time** - this function is called when collision happend.

```cpp
void Engine::Component::onCollisionStay(Engine::Collider* collider)
```
**Call Time** - this function is called if collision stay for more than one frame.

```cpp
void Engine::Component::onCollisionExit(Engine::Collider* collider)
```
**Call Time** - this function is called if two objects collision is finished.

```cpp
void Engine::Component::onTriggerEnter(Engine::Collider* collider)
```
**Call Time** - this function is called when collision happend & trigger event can applicable.

```cpp
void Engine::Component::onTriggerStay(Engine::Collider* collider)
```
**Call Time** - this function is called if collision stay for more than one frame & trigger event can applicable.

```cpp
void Engine::Component::onTriggerExit(Engine::Collider* collider)
```
**Call Time** - this function is called if two objects collision is finished & trigger event can applicable.

```cpp
void Engine::Component::onDestroy()
```
**Call Time** - this function is called when the component is going to destroy.

### Collider

```cpp
int this->getTag()
```
**returns** - the current tag of the collider.

```cpp
int this->setTag(int value)
```
**value** - set the current tag of the collider.

```cpp
tuple<bool, bool> this->getFixed()
```
**returns** - the current status of the collision effect respective to x axis and y axis.

```cpp
void this->setFixed(bool x, bool y)
```
**x, y** - set the current status of the collision effect respective to x axis and y axis. If x axis and y axis set to true then the collider will behave like a static object.

```cpp
void this->setTriiger(bool isTrigger)
```
**isTrigger** - set the current trigger status of the collider. If trigger is true then trigger event will be called else collision event.

### Renderer

```cpp
int this->getOrder()
```
**returns** - the order of the renderer.

```cpp
void this->setOrder(int order)
```
**order** - set the order of the renderer. Higher ordered renderer will be drawn first then the lower ordered renderer will be drawn.

## Resource Classes

### Sprite

**Sprite** - sprites are the images which are shown the game. The Sprite class constructor take relative path of the desired image.

```cpp
Sprite(std::string relative_path) // Constructor
```

```cpp
void this->destroy()
```
this functions destroy the sprite and free the memory. Make sure to remove the sprite from all renderers which are using it.

### Font

**Font** -  fonts are used for rendering text in the game. The Font class constructor take relative path of the desired font. Font must be in **Truetype format (.ttf)**.

```cpp
Font(std::string relative_path) // Constructor
```

```cpp
void this->destroy()
```
this functions destroy the font and free the memory. Make sure to remove the font from all text renderers which are using it.

## Component Classes

### Audio

```cpp
enum AudioStatus {NONE, READY, PLAYING, PAUSE}
```
**NONE** - it returns if the audio component has no loaded audio.  
**READY** - it returns if the audio component has loaded audio but playing.  
**PLAYING** - it returns if the audio is playing.  
**PAUSE** - it returns if the audio is paused.  

```cpp
enum AudioFile {NO_TYPE, MP3, WAV}
```
**NO_TYPE** - default value.  
**MP3** - for .mp3 audio files.  
**WAV** - for .wav audio files.  

```cpp
void this->load(std::string relative_path, AudioFile fileType)
```
load the audio file from the specified path and file type, to check if the file is loaded try **getStatus()** return value should be **AudioStatus::READY**.

```cpp
void this->unload()
```
unload the current audio file.

```cpp
void this->play(bool shouldLoop)
```
play the current loaded audio file.

```cpp
void this->pause()
```
pause the current loaded audio file.

```cpp
void this->stop()
```
stop the current loaded audio file.

```cpp
AudioStatus this->getStatus()
```
**returns** - get the current loaded audio file status.

```cpp
std::string this->getAudioFilePath()
```
**returns** - get the current loaded audio file path.

```cpp
AudioFile this->getAudioFileType()
```
**returns** - get the current loaded audio file type.

```cpp
int this->getVolume()
```
**returns** - get the current loaded audio volume.

```cpp
void this->setVolume(int volume)
```
set the current loaded audio volume.

```cpp
bool this->isLooping()
```
**returns** - get if two loaded audio is looping.

### Button

```cpp
enum HoverStatus {HOVER_NONE, HOVER_ENTER, HOVER_STAY, HOVER_EXIT}
```
**HOVER_NONE** - it returns if the mouse is hovering on the button.  
**HOVER_ENTER** - it returns if the mouse is start hovering from this frame.  
**HOVER_STAY** - it returns if the mouse is hovering for more than one frame.  
**HOVER_EXIT** - it returns if the mouse is stop hovering from this frame.  

```cpp
enum ClickStatus {BUTTON_NONE, BUTTON_CLICK, BUTTON_HOLD, BUTTON_RELEASE}
```
**BUTTON_NONE** - it returns if the mouse is not clicking the button.  
**BUTTON_CLICK** - it returns if the mouse is clicking the button from this frame.  
**BUTTON_HOLD** - it returns if the mouse is clicking the button for more than one frame.  
**BUTTON_RELEASE** - it returns if the mouse is stop clicking from this frame.  

```cpp
void this->setSize(float left, float right, float up, float down)
```
set the area of the button respective to the scaling.

```cpp
HoverStatus this->getHoverStatus()
```
**returns** - the hover status of the button.

```cpp
ClickStatus this->getClickStatus()
```
**returns** - the click status of the button.

```cpp
glm::vec2 this->getWidth()
```
**returns** - get the button width.

```cpp
glm::vec2 this->getHeight()
```
**returns** - get the button height.

```cpp
int this->getOrder()
```
**returns** - get the button order.

```cpp
void this->setOrder(int order)
```
set the button order.

```cpp
bool this->getClickableStatus()
```
**returns** - get the button clickable status.

```cpp
void this->setClickableStatus(bool status)
```
set the button clickable status.

```cpp
bool this->getTransparentStatus()
```
**returns** - get the button transparent status.

```cpp
void this->setTransparentStatus(bool status)
```
set the button transparent status.

### Camera

```cpp
Engine::Camera* Engine::Camera::getRenderCamera()
```
**returns** - get the game camera.

```cpp
glm::vec2 this->getBoundary()
```
**returns** - get the camera boundary.

```cpp
glm::vec3 this->getScreenToWorldPosition(glm::vec2 screenPosition)
```
**returns** - get the world position from the screen position.

```cpp
glm::vec2 this->getWorldToScreenPosition(glm::vec3 worldPosition)
```
**returns** - get the screen position from the world position.

```cpp
glm::vec3 this->getBackgroundColor()
```
**returns** - get the camera background color.

```cpp
void this->setBackgroundColor(glm::vec3 color)
```
set the camera background color.

```cpp
float this->getOrthographicSize()
```
**returns** - get the camera orthographical size.

```cpp
void this->setOrthographicSize(float size)
```
set the camera orthographical size.

### Transform

```cpp
glm::vec3 this->getLocalUp()
```
**returns** - the normalized local up vector.

```cpp
glm::vec3 this->getLocalRight()
```
**returns** - the normalized local right vector.

```cpp
glm::vec3 this->getLocalForward()
```
**returns** - the normalized local forward vector.

```cpp
glm::vec3 this->getWorldUp()
```
**returns** - the normalized world up vector.

```cpp
glm::vec3 this->getWorldRight()
```
**returns** - the normalized world right vector.

```cpp
glm::vec3 this->getWorldForward()
```
**returns** - the normalized world forward vector.

```cpp
glm::vec3 this->getWorldPosAt(glm::vec3 localOffset)
```
**returns** - the world position of the local offset.

```cpp
glm::vec3 this->getLocalPosition()
```
**returns** - the local position of the Transform.

```cpp
void this->setLocalPosition(glm::vec3 localPosition)
```
set the local position of the Transform.

```cpp
glm::vec3 this->getLocalRotattion()
```
**returns** - the local rotation of the Transform.

```cpp
void this->setLocalRotation(glm::vec3 localRotation)
```
set the local rotation of the Transform.

```cpp
glm::vec3 this->getLocalScale()
```
**returns** - the local scale of the Transform.

```cpp
void this->setLocalScale(glm::vec3 localScale)
```
set the local scale of the Transform.

```cpp
glm::vec3 this->getWorldPosition()
```
**returns** - the worldposition of the Transform.

```cpp
void this->setWorldPosition(glm::vec3 localPosition)
```
set the world position of the Transform.

```cpp
glm::vec3 this->getWorldRotattion()
```
**returns** - the world rotation of the Transform.

```cpp
void this->setWorldRotation(glm::vec3 localRotation)
```
set the world rotation of the Transform.

```cpp
glm::vec3 this->getWorldScale()
```
**returns** - the world scale of the Transform.

```cpp
void this->setWorldScale(glm::vec3 localScale)
```
set the world scale of the Transform.

```cpp
Transform* this->getParent()
```
**returns** - the **Transform** parent.

```cpp
void this->setParent(Transform* parent)
```
set the **Transform** parent.

```cpp
int this->getChildsSize()
```
**returns** - the **Transform's** childs size.

```cpp
Transform* this->getChild(int index)
```
**returns** - the **Transform's** child at index position.

### RectTransform

```cpp
glm::vec3 this->getAnchorUp()
```
**returns** - the normalized anchor up vector.

```cpp
glm::vec3 this->getAnchorRight()
```
**returns** - the normalized anchor right vector.

```cpp
glm::vec3 this->getAnchorForward()
```
**returns** - the normalized anchor forward vector.

```cpp
glm::vec3 this->getRectUp()
```
**returns** - the normalized rectangle up vector.

```cpp
glm::vec3 this->getRectRight()
```
**returns** - the normalized rectangle right vector.

```cpp
glm::vec3 this->getRectForward()
```
**returns** - the normalized rectangle forward vector.

```cpp
glm::vec2 this->getAnchorAt(glm::vec2 screnPosition)
```
**returns** - the anchor origin position from the screen position.

```cpp
glm::vec3 this->getScreenPosAt(glm::vec2 anchor)
```
**returns** - the screen position based on the anchor origin position.

```cpp
glm::vec3 this->getScreenPosAt(glm::vec3 localOffset)
```
**returns** - the screen position of the local offset.

```cpp
glm::vec2 this->getAnchor()
```
**returns** - the anchor origin position.

```cpp
void this->setAnchor(glm::vec2 anchor)
```
set the anchor origin position.

```cpp
glm::vec3 this->getAnchorRotattion()
```
**returns** - the anchor rotation.

```cpp
void this->setAnchorRotation(glm::vec3 anchorRotation)
```
set the anchor rotation.

```cpp
glm::vec2 this->getAnchorSize()
```
**returns** - the anchor size.

```cpp
void this->setAnchorSize(glm::vec2 anchorSize)
```
set the anchor size.

```cpp
glm::vec3 this->getRectPosition()
```
**returns** - the rectangle position in the screen.

```cpp
glm::vec3 this->getRectRotattion()
```
**returns** - the rectangle rotation in the screen.

```cpp
glm::vec3 this->getRectScale()
```
**returns** - the rectangle scale in the screen.

```cpp
RectTransform* this->getParent()
```
**returns** - the **RectTransform** parent.

```cpp
void this->setParent(RectTransform* parent)
```
set the **RectTransform** parent.

```cpp
int this->getChildsSize()
```
**returns** - the **RectTransform's** childs size.

```cpp
RectTransform* this->getChild(int index)
```
**returns** - the **RectTransform's** child at index position.

## Renderer Classes

### SpriteRenderer

**Note** - **SpriteRenderer** only works with **Transform** Component.

```cpp
Sprite* this->getSprite()
```
**returns** - the assign **Sprite** to the renderer.

```cpp
void this->setSprite(Sprite* sprite)
```
set the **Sprite** to the renderer. All **SpriteRenderer** have an initialize default sprite.

```cpp
Sprite* this->getColor()
```
**returns** - the assign color to the renderer.

```cpp
void this->setColor(Sprite* sprite)
```
set the color to the renderer.

### BillboardRenderer

**Note** - **Billboard** only works with **RectTransform** Component.

```cpp
Sprite* this->getSprite()
```
**returns** - the assign **Sprite** to the renderer.

```cpp
void this->setSprite(Sprite* sprite)
```
set the **Sprite** to the renderer. All **Billboard** have an initialize default sprite.

```cpp
Sprite* this->getColor()
```
**returns** - the assign color to the renderer.

```cpp
void this->setColor(Sprite* sprite)
```
set the color to the renderer.

### Text

```cpp
enum Alignment {RIGHT, MIDDLE, LEFT}
```
**RIGHT** - the text will start from the right of the position.  
**MIDDLE** - the text middle will be the position.  
**LEFT** - the text will start from the left of the position.  

```cpp
Font* this->getFont()
```
**returns** - get the font of the **Text**. All **Text** renderer have a default **Font**.

```cpp
void this->setFont(Font* font)
```
set the font to the **Text**.

```cpp
Alignment this->getAlignment()
```
**returns** - get the alignment of the **Text**.

```cpp
void this->setAlignment(Alignment alignment)
```
set the alignment to the **Text**.

```cpp
std::string this->getText()
```
**returns** - get the text of the **Text**.

```cpp
void this->setText(std::string text)
```
set the text to the **Text**.

```cpp
glm::vec4 this->getTextColor()
```
**returns** - get the text color of the **Text**.

```cpp
void this->setTextColor(glm::vec4 color)
```
set the text color to the **Text**.

```cpp
float this->getScale()
```
**returns** - get the text scale of the **Text**.

```cpp
void this->setScale(float scale)
```
set the text scale to the **Text**.

```cpp
unsigned int this->getCharPerLine()
```
**returns** - get the character per line of the **Text**. default value is 0 it means all character will be in the same line.

```cpp
void this->setCharPerLine(unsigned int chars)
```
set the character per line of the **Text**.

## Collider Classes

### BoxCollider

```cpp
void this->setBoundary(float left, float right, float up, float down)
```
set the area of the **BoxCollider**.

```cpp
glm::vec2 this->getWidth()
```
**returns** - get the BoxCollider width.
```cpp
glm::vec2 this->getHeight()
```
**returns** - get the BoxCollider height.

### CircleCollider

```cpp
float this->getRadius()
```
**returns** - get the radius of the **CircleCollider**.
```cpp
void this->setRadius(float radius)
```
set the radius of the **CircleCollider**.