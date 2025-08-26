---
layout: post
title: "Pygame Notes - 2"
date: 2025-08-26
categories: pygame
---

# Board games, placements

----------

## 1. Grid

I’m not good at math, so I need to _see_ things.

 **First rule**: Pygame’s window isn’t standard Cartesian — it’s flipped (Y-axis) 
 -   Because displays are literally drawn **row by row from the top-left corner** of the screen memory buffer.
 -   It’s more efficient to let `y` increase downward since video memory is laid out in scanlines (row 0, row 1, row 2, …).

----------

| ![norm_cart]({{ '/assets/norm_cart.png' | relative_url }}){: width="300" } | ![pygame_cart]({{ '/assets/pygame_cart.png' | relative_url }}){: width="300" } |
|:--:|:--:|
| Normal Cartesian | Pygame Cartesian |

### **Board Games**

> I took examples from [Pygame Docs](https://pygame.readthedocs.io/en/latest/9_board/board.html) since they have no code.


----------

### 1. Checkered Board

### Step 1: Nested Loop

-   Initial step is to render a grid

```python
for y in range(rows):
    for x in range(cols):
        rect = pygame.Rect(y*cell_size, x*cell_size, cell_size, cell_size)
        pygame.draw.rect(WindowSurface, Colors['black'], rect, 1)

```

-   Loop over rows & cols
-   Multiply by `cell_size`
-   Draw rectangles 

![Recording 2025-08-25 152354.gif]({{ '/assets/Recording_2025-08-25_152354.gif' | relative_url }}){: width="300" }
----------

> Tip: If nested loops confuse you → watch this quick [Nested loops](https://youtu.be/qwvswinpu2s) video

----------

### Step 2: Watch out for window size

-   `300x300 / 8 = 37.5` → rounding messes up (1-pixel overlaps).
-   Better → `320x320 / 8 = 40`.

```python
cell_size = 40
rows,cols = 8,8

for row in range(rows):
    for col in range(cols):
        rect = pygame.Rect(col*cell_size, row*cell_size, cell_size, cell_size)
        pygame.draw.rect(WindowSurface, Colors['blue'], rect, 1)

```

----------

### Step 3: Alternate colors

Just check `(row + col) % 2`. Even/odd decides the fill.

```python
if (row + col) % 2 == 0:
    pygame.draw.rect(WindowSurface, Colors['blue'], rect)
else:
    pygame.draw.rect(WindowSurface, Colors['red'], rect)


```

**Result**:

----------

![Screenshot 2025-08-25 153442.png]({{ '/assets/Screenshot_2025-08-25_153442.png' | relative_url }}){: width="300" }

**Slight problem here!**

> We are drawing the squares on the screen for each loop.
> 
> -   This might not be a load on CPU for such a small code., but if we scale it we might encounter efficiency problems.
> -   better to make change now itself.

Right now:

-   Every frame → recalculating rects.
-   Okay for 8×8, but scales badly.

Better idea:

-   Pre-store rects in an array.
-   Draw them in the loop.
-   In this way we will precompute the values once and use them in loop.

Optimization time :

```python
rects = []
for row in range(rows):
    for col in range(cols):
        rect = pygame.Rect(col*cell_size, row*cell_size, cell_size, cell_size)
        rects.append(rect)
While(True)...

```

Loop → only `draw.rect` each tick.

----------

### Step 4: Add state (clicked / color)

Now, we make a list of dicts., in which we store our states.

-   store `rect`
-   condition `clicked`
-   variable `color`

```python
rects.append({
    'rect': rect,
    'clicked': False,
    'color': None
})


```

Handle clicks:

```python
elif event.type == pygame.MOUSEBUTTONDOWN:
    pos = event.pos
    for val in rects:
        if val['rect'].collidepoint(pos):
            val['color'] = random.choice(list(Colors.values()))
            val['clicked'] = not val['clicked']


```

Draw loop:

```python
for val in rects:
    if val['clicked']:
        pygame.draw.rect(WindowSurface, val['color'], val['rect'])
    pygame.draw.rect(WindowSurface, (0,0,0), val['rect'], 1)


```

----------

> Full Code :

```python
import inint(), colors etc..
rects = []
for row in range(rows):  
        for col in range(cols): 
            rect = pygame.Rect(col*cell_size, row*cell_size, cell_size, cell_size)
            rects.append({
            'rect': rect,
            'clicked': False,
            'color': None})

while True:
    
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            pygame.quit()
            sys.exit()  
        elif event.type == pygame.MOUSEBUTTONDOWN:
            pos = event.pos
            for val in rects:
                if val['rect'].collidepoint(pos):
                    val['color'] = random.choice(list(Colors.values()))
                    val['clicked'] = not val['clicked']
        
    WindowSurface.fill((255,255,255))
    for val in rects:
        if val['clicked'] == True:
            pygame.draw.rect(WindowSurface,val['color'], val['rect'])
        pygame.draw.rect(WindowSurface, (0,0,0), val['rect'], 1)

    pygame.display.flip()

```

----------

----------
![Screenshot 2025-08-25 203638.png]({{ '/assets/Screenshot_2025-08-25_203638.png' | relative_url }}){: width="300" }
### Quick recap

-   Grid basics (just rect loops)
-   Fixed window size problem
-   Checkerboard coloring
-   Optimized rect storage
-   Mouse clicks toggle color

We will see collision detection with a small game in the next part…
