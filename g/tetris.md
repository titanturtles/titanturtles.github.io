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
4. Create a Figure class

    * Create a class
  
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
6. 
