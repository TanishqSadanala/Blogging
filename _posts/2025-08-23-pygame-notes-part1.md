---
layout: post
title: "Pygame Notes - 1"
date: 2025-08-23
categories: pygame
---

![Author](https://img.shields.io/badge/author-Tanishq_Sadanala-blue)

---

## 1. Basics - State Change

**Key Concepts:**

- Variables are used as **states** in Pygame
- Start with a default color and use `global` to control color changes
- `clicked` and `m_pos` are two None variables used for passing events from the game loop to functions
- Simple yet confusing approach: we use `clicked` to determine if a button is pressed from the event queue

---

## 2. Basics - Moving Rectangles

### Event Handling Fundamentals

Events are the basic building blocks for interactivity in Pygame.

**Getting Events:**

```python
pygame.event.get()  # Gets all events constantly restacked by game loop
```

**Basic Event Loop:**

```python
for event in pygame.event.get():
    if event.type == pygame.QUIT:
        pygame.quit()
        sys.exit()
    elif event.type == pygame.KEYDOWN:
        if event.key == pygame.K_LEFT:
            x -= 5
        elif event.key == pygame.K_RIGHT:
            x += 5
```

### Why Event Keys (`KEYDOWN`) Feel Choppy

**The Problem:**

- `KEYDOWN` only fires **once** at the instant the key is pressed
- When you press → it adds `+5` **just once**
- Nothing happens until you release and press again
- Movement feels like "jumping" in steps, not continuous

### When to Use Which Approach

**Use `KEYDOWN/KEYUP` (events) for:**

- *One-off actions* that should only happen once per press
- Examples: shooting a bullet, opening a menu, toggling pause, typing text
- When you care about **"the moment"** the key was pressed or released

**Use `get_pressed()` (state polling) for:**

- *Continuous actions* while a key is held down
- Examples: moving a character, accelerating a car, moving a cursor
- When you care about **"is this key still held down right now?"**

### Blended Approach (Common in Games)

**Best Practice: Combine Both Methods**

- **Movement:** use `get_pressed()` for smooth motion
- **Actions:** use events for precise triggers

```python
# Handle one-time actions with events
for event in pygame.event.get():
    if event.type == pygame.KEYDOWN:
        if event.key == pygame.K_SPACE:
            shoot_bullet()

# Handle continuous movement with state polling
keys = pygame.key.get_pressed()
if keys[pygame.K_LEFT]:
    player.x -= speed
if keys[pygame.K_RIGHT]:
    player.x += speed
```

**Bottom Line:**

- **Event loop** = step-by-step triggers
- **`get_pressed()`** = smooth, continuous states
- Use both depending on whether you want an **instant action** or a **held action**

---

## 3. Bouncing Ball - Physics and Delta Time

### Frame Rate (FPS)

**Understanding FPS:**

- Each computer has its own CPU/GPU capacity
- The infinite game loop runs at different speeds on different machines

**Measuring Loop Speed:**

```python
import time

count = 0
start = time.time()

while True:
    count += 1
    now = time.time()
    if now - start >= 1:  # one second passed
        print("Loops per second:", count)
        count = 0
        start = now
```

**Results:**

- Raw loop: ~8.4M - 8.5M loops per second
- With Pygame operations (`event.get()` + `draw` + `flip()`): slows to real frame rate

**Pygame FPS Measurement:**

```python
clock = pygame.time.Clock()
while True:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            pygame.quit()
            sys.exit()
    
    fps = clock.get_fps()
    pygame.display.flip()
    clock.tick()
    print(f"FPS after pygame loop: {fps:.2f}")
# Output: FPS after pygame loop: 5000.00
```

### Delta Time

**Purpose:**

- Delta time provides **frame-rate independence**
- Movement smoothness shouldn't depend on CPU capacity
- Normalizes framerate across all computers for consistent performance

| ![Case 1: px/loop]({{ '/assets/1755884581778.png' | relative_url }}) | ![Case 1: px/sec]({{ '/assets/1755890798982.png' | relative_url }}) |
|:--:|:--:|
| Case 1: 10 px/loop | Case 1: 10 px/sec |


### Case 1: Frame-Dependent

**Problem:**

- Movement = 10 pixels **every frame**
- `x_pos = x_pos + 10`
- At 60 FPS → **10 × 60 = 600 px per second**
- At 30 FPS → **10 × 30 = 300 px per second**
- **Motion speed depends on frame rate** → inconsistent across machines

### Case 2: Frame-Independent

**Solution:**

- Object moves 10 pixels **per second**, regardless of FPS
- **Formula:** `x_pos = x_pos + speed * dt`

**At 60 FPS:**

- dt ≈ 1/60 ≈ 0.0167 seconds
- Δx = 10 × 0.0167 ≈ 0.167 pixels per frame
- Over 60 frames → about 10 px/sec

**At 30 FPS:**

- dt ≈ 1/30 ≈ 0.033 seconds
- Δx = 10 × 0.033 ≈ 0.33 pixels per frame
- Over 30 frames → about 10 px/sec

**Result:** Motion speed is consistent regardless of FPS

### Physics of Motion and Bounce

### Level 1: Basic Bouncing

- Simple downward movement with velocity reversal at ground contact
- Fixed velocity approach

```python
y += velocity * dt
if y + radius >= window_height:
    velocity = -velocity
```

*Seems unrealistic*

### Level 2: Adding Gravity

- Gravity constantly attracts the ball downward
- Ball constantly accelerates toward ground

```python
velocity += gravity * dt
y += velocity * dt
```

*Ball sticks to ground constantly*

### Level 3: Energy Loss on Bounce

- Add damping factor to reduce acceleration after bounce
- 0.8 is the damping factor (energy loss)

```python
velocity += gravity * dt
y += velocity * dt
if y + 20 >= window_height:
    velocity = -velocity * 0.8
```

Full Code : 

```python
import pygame, sys, time

pygame.init()
Window_size = 300
WindowSurface = pygame.display.set_mode([Window_size]*2)
pygame.display.set_caption('Bouncing Ball')

#colors
Colors = {'red': (255,0,0), 'blue':(0,0,255)}

velocity = 203
gravity = 400
x,y = (10,10)

prev_time = time.time() 
while True:

    now = time.time()
    dt = now - prev_time
    prev_time = now

    velocity += gravity * dt
    y += velocity * dt  
    if y + 20 >= Window_size:
        y = Window_size - 20
        velocity = -velocity *0.8
    x += 30 *dt

    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            pygame.quit()
            sys.exit()
    
    WindowSurface.fill((100,200,100))
    pygame.draw.circle(WindowSurface,Colors['blue'],(x,y),20,3)
    pygame.display.flip()

```

![Output for Bouncing Ball]({{ '/assets/bouncing_ball.gif' | relative_url }}){: width="400" }

Output for Bouncing Ball
