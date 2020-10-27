In this workshop, we will be creating a Pac-Man maze game, where the user moves around a maze eating all the dots, while being pursued by four ghosts. 
We will use Freegame library. 

## Setup
Start a new Python file. Using your favourite text editor or go on [repl.it/languages/python3](https://www.repl.it/languages/python3) to start new coding environment of Python 3 to quick start our workshop.

Start your new `main.py` with a new modular imports:
```python3
from random import choice
from turtle import *
from freegames import floor, vector
```
The above code says, import choice function form random module. Import every functions form turtle module. [floor](http://www.grantjenks.com/docs/freegames/api.html#freegames.floor), [vector](http://www.grantjenks.com/docs/freegames/api.html#freegames.vector) form freegames module.

Next, we’ll want to declare and initialise all of our global variables:
```
state = {'score': 0}
path = Turtle(visible=False)
writer = Turtle(visible=False)
aim = vector(5, 0)
pacman = vector(-40, -80)
ghosts = [
    [vector(-180, 160), vector(5, 0)],
    [vector(-180, -160), vector(0, 5)],
    [vector(100, 160), vector(0, -5)],
    [vector(100, -160), vector(-5, 0)],
]
```
.__state__ variable will display Score of game which start on zero.__aim__ sets the movement direction of Pacman with (5,0).By default we are moving the Pacman at speed of 5 toward positive x-axis. We are also creating a couple of Turtles which we’ll use later:__path__ for drawing the game world and __writer__ for writing the score.

## Draw the Maze
Lets desgine the maze layout with an list called _tiles_ that contains our maze. In the array, a 0 represents a black wall, and a 1 represents a collectable dot or floor space. Our game world is 20X20, so it may be easier for future hacking the Python list by creating a new line every 20th value.
```python3
tiles = [
    0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
    0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 0, 0, 0,
    0, 1, 0, 0, 1, 0, 0, 1, 0, 1, 0, 0, 1, 0, 0, 1, 0, 0, 0, 0,
    0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 0, 0, 0,
    0, 1, 0, 0, 1, 0, 1, 0, 0, 0, 1, 0, 1, 0, 0, 1, 0, 0, 0, 0,
    0, 1, 1, 1, 1, 0, 1, 1, 0, 1, 1, 0, 1, 1, 1, 1, 0, 0, 0, 0,
    0, 1, 0, 0, 1, 0, 0, 1, 0, 1, 0, 0, 1, 0, 0, 1, 0, 0, 0, 0,
    0, 1, 0, 0, 1, 0, 1, 1, 1, 1, 1, 0, 1, 0, 0, 1, 0, 0, 0, 0,
    0, 1, 1, 1, 1, 1, 1, 0, 0, 0, 1, 1, 1, 1, 1, 1, 0, 0, 0, 0,
    0, 1, 0, 0, 1, 0, 1, 1, 1, 1, 1, 0, 1, 0, 0, 1, 0, 0, 0, 0,
    0, 1, 0, 0, 1, 0, 1, 0, 0, 0, 1, 0, 1, 0, 0, 1, 0, 0, 0, 0,
    0, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 1, 1, 0, 0, 0, 0,
    0, 1, 0, 0, 1, 0, 0, 1, 0, 1, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0,
    0, 1, 1, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 0, 0, 0, 0,
    0, 1, 1, 0, 1, 0, 1, 0, 0, 0, 1, 0, 1, 0, 1, 1, 0, 0, 0, 0,
    0, 1, 1, 1, 1, 0, 1, 1, 0, 1, 1, 0, 1, 1, 1, 1, 0, 0, 0, 0,
    0, 1, 0, 0, 0, 0, 0, 1, 0, 1, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0,
    0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 0, 0, 0,
    0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
    0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
    
]
```

## Creating the Walls
Now it's time to put turtles to work. We will make a function __brick__ to draw squares,which will add up to create walls. 
```python3
def brick(x, y):
    "Draw square using path at (x, y)."
    path.up()
    path.goto(x, y)
    path.down()
    path.begin_fill()

    for count in range(4):
        path.forward(20)
        path.left(90)

    path.end_fill()
```
    
## Follow the Rules!
Every one should follow the rules.So our Pacman also need to follow basic physics rules. We'll need a function __offset__ for collision detection and __valid__ to ensure our Pacman doesn't wall through the walls and out in real world :)
```python3
def offset(point):
    "Return offset of point in tiles."
    x = (floor(point.x, 20) + 200) / 20
    y = (180 - floor(point.y, 20)) / 20
    index = int(x + y * 20)
    return index
```


```python3
def valid(point):
    "Return True if point is valid in tiles."
    index = offset(point)

    if tiles[index] == 0:
        return False

    index = offset(point + 19)

    if tiles[index] == 0:
        return False

    return point.x % 20 == 0 or point.y % 20 == 0
```

## Building the Game World

__world__ function will use to build game world. Of course you can choose your own colours, but to start with we’re using the traditional black and blue environment of the 1980 version of Pac-man. That final colour (path.dot) is assigned to our dots on the path. You’ll notice we’re taking advantage of the brick function we set up previously.. Remember to be very careful about the indentation of each line - for example, make sure the path.up() is lined up exactly under the if syntax.
```python3
def world():
    Screen().bgcolor('black')
    path.color('blue')

    for index in range(len(tiles)):
        tile = tiles[index]

        if tile > 0:
            x = (index % 20) * 20 - 200
            y = 180 - (index // 20) * 20
            brick(x, y)

            if tile == 1:
                path.up()
                path.goto(x + 10, y + 10)
                path.dot(2, 'white')
    
    update()
```
This function sets the background of the screen to black, then uses the path turtle to draw a 20 pixel blue square for every tile with a value greater than zero. 
If a tile value equals 1, then we draw a white dot in the middle of the square.

## Moving in the Game World
.Our one major function, move, will be used to move both our Pac-man and our ghosts around the game
Then create a new function called move().
```python3
def move():
    "Move pacman and all ghosts."
    writer.undo()
    writer.write(state['score'])
    clear()
```

We need to move Pac-Man on every screen update. Change the move() and add following code:
    
```python3
    if valid(pacman + aim):
        pacman.move(aim)

    index = offset(pacman)

    if tiles[index] == 1:
        tiles[index] = 2
        state['score'] += 1
        x = (index % 20) * 20 - 200
        y = 180 - (index // 20) * 20
        wall(x, y)

    up()
    goto(pacman.x + 10, pacman.y + 10)
    dot(20, 'yellow')

    for point, course in ghosts:
        if valid(point + course):
            point.move(course)
        else:
            options = [
                vector(5, 0),
                vector(-5, 0),
                vector(0, 5),
                vector(0, -5),
            ]
            plan = choice(options)
            course.x = plan.x
            course.y = plan.y

        up()
        goto(point.x + 10, point.y + 10)
        dot(20, 'red')

    update()

    for point, course in ghosts:
        if abs(pacman - point) < 20:
            return

```

##
We need to create a new function to change the position of Pac-Man when we press a keys like up, down, left, right.
```python3
def change(x, y):
    "Change pacman aim if valid."
    if valid(pacman + vector(x, y)):
        aim.x = x
        aim.y = y
```

## abc
This will cause the program to listen for arrow keypresses, and update Pac-Man’s direction.
```python3
setup(420, 420, 370, 0)
hideturtle()
tracer(False)
writer.goto(160, 160)
writer.color('white')
writer.write(state['score'])
listen()
onkey(lambda: change(5, 0), 'Right')
onkey(lambda: change(-5, 0), 'Left')
onkey(lambda: change(0, 5), 'Up')
onkey(lambda: change(0, -5), 'Down')
world()
move()
done()
```

