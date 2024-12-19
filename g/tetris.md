Tetris

In this tutorial, we will be teaching how to make a tetris game 

0. Create a file and edit the file. Let's the file name is `tetris.py`.

1. Import packages
    
    ```python
    import pygame
    import random
    ```
    
    In the terminal, install `pygame`.
    
    ```bash
    $ pip3 install pygame
    ```

2. Add colors

   ```python
   colors = [
        (0, 0, 0),
        (120, 37, 179),
        (100, 179, 179),
        (80, 34, 22),
        (80, 134, 22),
        (180, 34, 22),
        (180, 34, 122),
   ]
   ```
4. Add a `Figure` class

    * Create the class
  
    ```python
    class Figure:
    ```
    
    * Define the variables
      
   ```python
        x = 0
        y = 0
        
        figures = [
            [[1, 5, 9, 13], [4, 5, 6, 7]],
            [[4, 5, 9, 10], [2, 6, 5, 9]],
            [[6, 7, 9, 10], [1, 5, 6, 10]],
            [[1, 2, 5, 9], [0, 4, 5, 6], [1, 5, 9, 8], [4, 5, 6, 10]],
            [[1, 2, 6, 10], [5, 6, 7, 9], [2, 6, 10, 11], [3, 5, 6, 7]],
            [[1, 4, 5, 6], [1, 4, 5, 9], [4, 5, 6, 9], [1, 5, 6, 9]],
            [[1, 2, 5, 6]],
        ]
   ```
   
    * constructor (__init__ function)
   ```python
        def __init__(self, x, y):
            self.x = x
            self.y = y
            self.type = random.randint(0, len(self.figures) - 1)
            self.color = random.randint(1, len(colors) - 1)
            self.rotation = 0
   ```
    * image display function
   ```python
        def image(self):
            return self.figures[self.type][self.rotation]
   ```
    * rotation function
      
   ```python
        def rotate(self):
            self.rotation = (self.rotation + 1) % len(self.figures[self.type])
   ```

    * Full class
      
   ```python
   class Figure:
        x = 0
        y = 0
        
        figures = [
            [[1, 5, 9, 13], [4, 5, 6, 7]],
            [[4, 5, 9, 10], [2, 6, 5, 9]],
            [[6, 7, 9, 10], [1, 5, 6, 10]],
            [[1, 2, 5, 9], [0, 4, 5, 6], [1, 5, 9, 8], [4, 5, 6, 10]],
            [[1, 2, 6, 10], [5, 6, 7, 9], [2, 6, 10, 11], [3, 5, 6, 7]],
            [[1, 4, 5, 6], [1, 4, 5, 9], [4, 5, 6, 9], [1, 5, 6, 9]],
            [[1, 2, 5, 6]],
        ]
   
        def __init__(self, x, y):
            self.x = x
            self.y = y
            self.type = random.randint(0, len(self.figures) - 1)
            self.color = random.randint(1, len(colors) - 1)
            self.rotation = 0
        
        def image(self):
            return self.figures[self.type][self.rotation]
        
        def rotate(self):
            self.rotation = (self.rotation + 1) % len(self.figures[self.type])
   ```
5. Add a `Tetris` class
   * Create the class
   * Constructor (__init__)
   * new_figure function
   * intersects function
   * break_lines function
   * go_space function
   * go_down function
   * freeze function
   * go_side function
   * rotate function
   * Full class

     ```python
     class Tetris:
        def __init__(self, height, width):
            self.level = 2
            self.score = 0
            self.state = "start"
            self.field = []
            self.height = 0
            self.width = 0
            self.x = 100
            self.y = 60
            self.zoom = 20
            self.figure = None
        
            self.height = height
            self.width = width
            self.field = []
            self.score = 0
            self.state = "start"
            for i in range(height):
                new_line = []
                for j in range(width):
                    new_line.append(0)
                self.field.append(new_line)
    
        def new_figure(self):
            self.figure = Figure(3, 0)
    
        def intersects(self):
            intersection = False
            for i in range(4):
                for j in range(4):
                    if i * 4 + j in self.figure.image():
                        if i + self.figure.y > self.height - 1 or \
                                j + self.figure.x > self.width - 1 or \
                                j + self.figure.x < 0 or \
                                self.field[i + self.figure.y][j + self.figure.x] > 0:
                            intersection = True
            return intersection
    
        def break_lines(self):
            lines = 0
            for i in range(1, self.height):
                zeros = 0
                for j in range(self.width):
                    if self.field[i][j] == 0:
                        zeros += 1
                if zeros == 0:
                    lines += 1
                    for i1 in range(i, 1, -1):
                        for j in range(self.width):
                            self.field[i1][j] = self.field[i1 - 1][j]
            self.score += lines ** 2
    
        def go_space(self):
            while not self.intersects():
                self.figure.y += 1
            self.figure.y -= 1
            self.freeze()
    
        def go_down(self):
            self.figure.y += 1
            if self.intersects():
                self.figure.y -= 1
                self.freeze()
    
        def freeze(self):
            for i in range(4):
                for j in range(4):
                    if i * 4 + j in self.figure.image():
                        self.field[i + self.figure.y][j + self.figure.x] = self.figure.color
            self.break_lines()
            self.new_figure()
            if self.intersects():
                self.state = "gameover"
    
        def go_side(self, dx):
            old_x = self.figure.x
            self.figure.x += dx
            if self.intersects():
                self.figure.x = old_x
    
        def rotate(self):
            old_rotation = self.figure.rotation
            self.figure.rotate()
            if self.intersects():
                self.figure.rotation = old_rotation

     ```
7. 
