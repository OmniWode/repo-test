
[ROGUELIKE TUTORIALS](https://rogueliketutorials.com/)

- [Home](https://rogueliketutorials.com/)
- [TCOD Tutorial (2020)](https://rogueliketutorials.com/tutorials/tcod/v2/)
- [TCOD Tutorial (2019)](https://rogueliketutorials.com/tutorials/tcod/2019/)
- [About](https://rogueliketutorials.com/about/)

# [Part 0 - Setting Up](https://rogueliketutorials.com/tutorials/tcod/v2/part-0/)

#### Prior knowledge [](https://rogueliketutorials.com/tutorials/tcod/v2/part-0/#prior-knowledge)

This tutorial assumes some basic familiarity with programming in general, and with Python. If you’ve never used Python before, this tutorial could be a little confusing. There are many free resources online about learning programming and Python (too many to list here), and I’d recommend learning about objects and functions in Python at the very least before attempting to read this tutorial.

… Of course, there are those who have ignored this advice and done well with this tutorial anyway, so feel free to ignore that last paragraph if you’re feeling bold!

#### Installation [](https://rogueliketutorials.com/tutorials/tcod/v2/part-0/#installation)

To do this tutorial, you’ll need Python version 3.7 or higher. The latest version of Python is recommended (currently 3.8 as of June 2020). **Note: Python 2 is not compatible.**

[Download Python here](https://www.python.org/downloads/).

You’ll also want the latest version of the TCOD library, which is what this tutorial is based on.

[Installation instructions for TCOD can be found here.](https://python-tcod.readthedocs.io/en/latest/installation.html)

While you can certainly install TCOD and complete this tutorial without it, I’d highly recommend using a virtual environment. [Documentation on how to do that can be found here.](https://docs.python.org/3/library/venv.html)

Additionally, if you are going to use a virtual environment, you may want to take the time to set up a `requirements.txt` file. This will allow you to track your project dependencies if you add any in the future, and more easily install them if you need to (for example, if you pull from a remote git repository).

You can set up your `requirements.txt` file in the same directory that you plan on working in for the project. Create the file `requirements.txt` and put the following in it:

```text
tcod>=11.13
numpy>=1.18
```

Once that’s done, with your virtual environment activated, type the following command:

`pip install -r requirements.txt`

This should install the TCOD library, along with its dependency, numpy.

Depending on your computer, you might also need to install SDL2. Check the instructions for installing it based on your operating system. For example, Ubuntu can install it with the following command:

`sudo apt-get install libsdl2-dev`

#### Editors [](https://rogueliketutorials.com/tutorials/tcod/v2/part-0/#editors)

Any text editor can work for writing Python. You could even use Notepad if you really wanted to. Personally, I’m a fan of [Pycharm](https://www.jetbrains.com/pycharm/) and [Visual Studio Code](https://code.visualstudio.com/). Whatever you choose, I strongly recommend something that can help catch Python syntax errors at the very least. I’ve been working with Python for over five years, and I still make these types of mistakes all the time!

#### Making sure Python works [](https://rogueliketutorials.com/tutorials/tcod/v2/part-0/#making-sure-python-works)

To verify that your installation of both Python 3 and TCOD are working, create a new file (in whatever directory you plan on using for the tutorial) called `main.py`, and enter the following text into it:

```python
#!/usr/bin/env python3
import tcod


def main():
    print("Hello World!")


if __name__ == "__main__":
    main()
```

Run the file in your terminal (or alternatively in your editor, if possible):

`python main.py`

If you’re not using `virtualenv`, the command will probably look like this:

`python3 main.py`

You should see “Hello World!” printed out to the terminal. If you receive an error, there is probably an issue with either your Python or TCOD installation.

### Downloading the Image File [](https://rogueliketutorials.com/tutorials/tcod/v2/part-0/#downloading-the-image-file)

For this tutorial, we’ll need an image file. The default one is provided below.

![Font File](https://rogueliketutorials.com/images/dejavu10x10_gs_tc.png)

Right click the image and save it to the same directory that you’re planning on placing your code in. If the above image is not displaying for some reason, it is also [available for download here.](https://raw.githubusercontent.com/TStand90/tcod_tutorial_v2/1667c8995fb0d0fd6df98bd84c0be46cb8b78dac/dejavu10x10_gs_tc.png)

### About this site [](https://rogueliketutorials.com/tutorials/tcod/v2/part-0/#about-this-site)

Code snippets in this website are presented in a way that tries to convey exactly what the user should be adding to a file at what time. When a user is expected to create a file from scratch and enter code into it, it will be represented with standard Python code highlighting, like so:

```python
class Fighter:
    def __init__(self, hp, defense, power):
        self.max_hp = hp
        self.hp = hp
        self.defense = defense
        self.power = power
```

*_Taken from part 6_.

Most of the time, you’ll be editing a file and code that already exists. In such cases, the code will be displayed like this:

Diff Original

```diff
class Entity:
-   def __init__(self, x, y, char, color, name, blocks=False):
+   def __init__(self, x, y, char, color, name, blocks=False, fighter=None, ai=None):
       self.x = x
       self.y = y
       self.char = char
       self.color = color
       self.name = name
       self.blocks = blocks
+       self.fighter = fighter
+       self.ai = ai
+
+       if self.fighter:
+           self.fighter.owner = self
+
+       if self.ai:
+           self.ai.owner = self
```

*_Also taken from part 6._

Clicking a button above the code section changes the “style” for not just that code block, but the entire website. You can switch between these styles at any time.

In the case of the example above, you would remove the old `__init__` definition, replacing it with the new one. Then, you’d add the necessary lines at the bottom. Both styles convey the same idea.

But what’s the difference? The “Diff” style shows the code as you might find it when doing a Git diff comparison (hence the name). It shows plusses and minuses on the side to denote whether you should be adding or subtracting a line from a file. The “Original” style shows the same thing, but it crosses out the lines to remove and does not have plusses nor minuses.

The benefit of the “Diff” style is that it doesn’t rely on color to denote what to add, making it more accessible all around. The drawback is that it’s impossible to accurately display the proper indentation in some instances. The plusses and minuses take up one space, so in a code section like this one, be sure not to leave the space for the plus in your code (there should be no spaces before “from”):

Diff Original

```diff
import tcod

+from input_handlers import handle_keys
```

The “Original” style omits the + and - symbols and doesn’t have the indentation issue, making it a bit easier to copy and paste code sections.

Which style you use is a matter of personal preference. The actual code of the tutorial remains the same.

---

# [Part 1 - Drawing the '@' symbol and moving it around](https://rogueliketutorials.com/tutorials/tcod/v2/part-1/)

Welcome to part 1 of this tutorial! This series will help you create your very first roguelike game, written in Python!

This tutorial is largely based off the [one found on Roguebasin](http://www.roguebasin.com/index.php?title=Complete_Roguelike_Tutorial,_using_python%2Blibtcod). Many of the design decisions were mainly to keep this tutorial in lockstep with that one (at least in terms of chapter composition and general direction). This tutorial would not have been possible without the guidance of those who wrote that tutorial, along with all the wonderful contributors to tcod and python-tcod over the years.

This part assumes that you have either checked [Part 0](https://rogueliketutorials.com/tutorials/tcod/part-0) and are already set up and ready to go. If not, be sure to check that page, and make sure that you’ve got Python and TCOD installed, and a file called `main.py` created in the directory that you want to work in.

Assuming that you’ve done all that, let’s get started. Modify (or create, if you haven’t already) the file `main.py` to look like this:

```python
#!/usr/bin/env python3
import tcod


def main():
    print("Hello World!")


if __name__ == "__main__":
    main()
```

You can run the program like any other Python program, but for those who are brand new, you do that by typing `python main.py` in the terminal. If you have both Python 2 and 3 installed on your machine, you might have to use `python3 main.py` to run (it depends on your default python, and whether you’re using a virtualenv or not).

Alternatively, because of the first line, `#!usr/bin/env python`, you can run the program by typing `./main.py`, assuming you’ve either activated your virtual environment, or installed tcod on your base Python installation. This line is called a “shebang”.

Okay, not the most exciting program in the world, I admit, but we’ve already got our first major difference from the other tutorial. Namely, this funky looking thing here:

```python
if __name__ == "__main__":
    main()
```

So what does that do? Basically, we’re saying that we’re only going to run the “main” function when we explicitly run the script, using `python main.py`. It’s not super important that you understand this now, but if you want a more detailed explanation, [this answer on Stack Overflow](https://stackoverflow.com/a/419185) gives a pretty good overview.

Confirm that the above program runs (if not, there’s probably an issue with your tcod setup). Once that’s done, we can move on to bigger and better things. The first major step to creating any roguelike is getting an ‘@’ character on the screen and moving, so let’s get started with that.

Modify `main.py` to look like this:

```python
#!/usr/bin/env python3
import tcod


def main() -> None:
    screen_width = 80
    screen_height = 50

    tileset = tcod.tileset.load_tilesheet(
        "dejavu10x10_gs_tc.png", 32, 8, tcod.tileset.CHARMAP_TCOD
    )

    with tcod.context.new_terminal(
        screen_width,
        screen_height,
        tileset=tileset,
        title="Yet Another Roguelike Tutorial",
        vsync=True,
    ) as context:
        root_console = tcod.Console(screen_width, screen_height, order="F")
        while True:
            root_console.print(x=1, y=1, string="@")

            context.present(root_console)

            for event in tcod.event.wait():
                if event.type == "QUIT":
                    raise SystemExit()


if __name__ == "__main__":
    main()
```

Run `main.py` again, and you should see an ‘@’ symbol on the screen. Once you’ve fully soaked in the glory on the screen in front of you, you can click the “X” in the top-left corner of the program to close it.

There’s a lot going on here, so let’s break it down line by line.

```python
    screen_width = 80
    screen_height = 50
```

This is simple enough. We’re defining some variables for the screen size.

Eventually, we’ll load these values from a JSON file rather than hard coding them in the source, but we won’t worry about that until we have some more variables like this.

```python
    tileset = tcod.tileset.load_tilesheet(
        "dejavu10x10_gs_tc.png", 32, 8, tcod.tileset.CHARMAP_TCOD
    )
```

Here, we’re telling tcod which font to use. The `"dejavu10x10_gs_tc.png"` bit is the actual file we’re reading from (this should exist in your project folder).

```python
    with tcod.context.new_terminal(
        screen_width,
        screen_height,
        tileset=tileset
        title="Yet Another Roguelike Tutorial",
        vsync=True,
    ) as context:
```

This part is what actually creates the screen. We’re giving it the `screen_width` and `screen_height` values from before (80 and 50, respectively), along with a title (change this if you’ve already got your game’s name figured out). `tileset` uses the tileset we defined earlier. and `vsync` will either enable or disable vsync, which shouldn’t matter too much in our case.

```python
        root_console = tcod.Console(screen_width, screen_height, order="F")
```

This creates our “console” which is what we’ll be drawing to. We also set this console’s width and height to the same as our new terminal. The “order” argument affects the order of our x and y variables in numpy (an underlying library that tcod uses). By default, numpy accesses 2D arrays in [y, x] order, which is fairly unintuitive. By setting `order="F"`, we can change this to be [x, y] instead. This will make more sense once we start drawing the map.

```python
        while True:
```

This is what’s called our ‘game loop’. Basically, this is a loop that won’t ever end, until we close the screen. Every game has some sort of game loop or another.

```python
            root_console.print(x=1, y=1, string="@")
```

This line is what tells the program to actually put the “@” symbol on the screen in its proper place. We’re telling the `root_console` we created to `print` the “@” symbol at the given x and y coordinates. Try changing the x and y values and see what happens, if you feel so inclined.

```python
            context.present(root_console)
```

Without this line, nothing would actually print out on the screen. This is because `context.present` is what actually updates the screen with what we’ve told it to display so far.

```python
            for event in tcod.event.wait():
                if event.type == "QUIT":
                    raise SystemExit()
```

This part gives us a way to gracefully exit (i.e. not crashing) the program by hitting the `X` button in the console’s window. The line `for event in tcod.event.wait()` will wait for some sort of input from the user (mouse clicks, keyboard strokes, etc.) and loop through each event that happened. `SystemExit()` tells Python to quit the current running program.

Alright, our “@” symbol is successfully displayed on the screen, but we can’t rest just yet. We still need to get it moving around!

We need to keep track of the player’s position at all times. Since this is a 2D game, we can express this in two data points: the `x` and `y` coordinates. Let’s create two variables, `player_x` and `player_y`, to keep track of this.

Diff Original

```diff
    ...
    screen_height = 50
+
+   player_x = int(screen_width / 2)
+   player_y = int(screen_height / 2)
+
    tileset = tcod.tileset.load_tilesheet(
        "dejavu10x10_gs_tc.png", 32, 8, tcod.tileset.CHARMAP_TCOD
    )
    ...
```

_Note: Ellipses denote omitted parts of the code. I’ll include lines around the code to be inserted so that you’ll know exactly where to put new pieces of code, but I won’t be showing the entire file every time. The green lines denote code that you should be adding._

We’re placing the player right in the middle of the screen. What’s with the `int()` function though? Well, Python 3 doesn’t automatically truncate division like Python 2 does, so we have to cast the division result (a float) to an integer. If we don’t, tcod will give an error.

_Note: It’s been pointed out that you could divide with `//` instead of `/` and achieve the same effect. This is true, except in cases where, for whatever reason, one of the numbers given is a decimal. For example, `screen_width // 2.0` will give an error. That shouldn’t happen in this case, but wrapping the function in `int()` gives us certainty that this won’t ever happen._

We also have to modify the command to put the ‘@’ symbol to use these new coordinates.

Diff Original

```diff
        ...
        while True:
-           root_console.print(x=1, y=1, string="@")
+           root_console.print(x=player_x, y=player_y, string="@")

            context.present(root_console)
            ...
```

_Note: The red lines denote code that has been removed._

Run the code now and you should see the ‘@’ in the center of the screen. Let’s take care of moving it around now.

So, how do we actually capture the user’s input? TCOD makes this pretty easy, and in fact, we’re already doing it. This line takes care of it for us:

```python
            for event in tcod.event.wait():
```

It gets the “events”, which we can then process. Events range from mouse movements to keyboard strokes. Let’s start by getting some basic keyboard commands and processing them, and based on what we get, we’ll move our little “@” symbol around.

We _could_ identify which key is being pressed right here in `main.py`, but this is a good opportunity to break our project up a little bit. Sooner or later, we’re going to have quite a few potential keyboard commands, so putting them all in `main.py` would make the file longer than it needs to be. Maybe we should import what we need into `main.py` rather than writing it all there.

To handle the keyboard inputs and the actions associated with them, let’s actually create _two_ new files. One will hold the different types of “actions” our rogue can perform, and the other will bridge the gap between the keys we press and those actions.

Create two new Python files in your project’s directory, one called `input_handlers.py`, and the other called `actions.py`. Let’s fill out `actions.py` first:

```python
class Action:
    pass


class EscapeAction(Action):
    pass


class MovementAction(Action):
    def __init__(self, dx: int, dy: int):
        super().__init__()

        self.dx = dx
        self.dy = dy
```

We define three classes: `Action`, `EscapeAction`, and `MovementAction`. `EscapeAction` and `MovementAction` are subclasses of `Action`.

So what’s the plan for these classes? Basically, whenever we have an “action”, we’ll use one of the subclasses of `Action` to describe it. We’ll be able to detect which subclass we’re using, and respond accordingly. In this case, `EscapeAction` will be when we hit the `Esc` key (to exit the game), and `MovementAction` will be used to describe our player moving around.

There might be instances where we need to know more than just the “type” of action, like in the case of `MovementAction`. There, we need to know not only that we’re trying to move, but in which direction. Therefore, we can pass the `dx` and `dy` arguments to `MovementAction`, which will tell us where the player is trying to move to. Other `Action` subclasses might contain additional data as well, and others might just be subclasses with nothing else in them, like `EscapeAction`.

That’s all we need to do in `actions.py` right now. Let’s fill out `input_handlers.py`, which will use the `Action` class and subclasses we just created:

```python
from typing import Optional

import tcod.event

from actions import Action, EscapeAction, MovementAction


class EventHandler(tcod.event.EventDispatch[Action]):
    def ev_quit(self, event: tcod.event.Quit) -> Optional[Action]:
        raise SystemExit()

    def ev_keydown(self, event: tcod.event.KeyDown) -> Optional[Action]:
        action: Optional[Action] = None

        key = event.sym

        if key == tcod.event.K_UP:
            action = MovementAction(dx=0, dy=-1)
        elif key == tcod.event.K_DOWN:
            action = MovementAction(dx=0, dy=1)
        elif key == tcod.event.K_LEFT:
            action = MovementAction(dx=-1, dy=0)
        elif key == tcod.event.K_RIGHT:
            action = MovementAction(dx=1, dy=0)

        elif key == tcod.event.K_ESCAPE:
            action = EscapeAction()

        # No valid key was pressed
        return action
```

Let’s go over what we’ve added.

```python
from typing import Optional
```

This is part of Python’s type hinting system (which you don’t have to include in your project). `Optional` denotes something that could be set to `None`.

```python
import tcod.event

from actions import Action, EscapeAction, MovementAction
```

We’re importing `tcod.event` so that we can use tcod’s event system. We don’t need to import `tcod`, as we only need the contents of `event`.

The next line imports the `Action` class and its subclasses that we just created.

```python
class EventHandler(tcod.event.EventDispatch[Action]):
```

We’re creating a class called `EventHandler`, which is a subclass of tcod’s `EventDispatch` class. `EventDispatch` is a class that allows us to send an event to its proper method based on what type of event it is. Let’s take a look at the methods we’re creating for `EventHandler` to see a few examples of this.

```python
    def ev_quit(self, event: tcod.event.Quit) -> Optional[Action]:
        raise SystemExit()
```

Here’s an example of us using a method of `EventDispatch`: `ev_quit` is a method defined in `EventDispatch`, which we’re overriding in `EventHandler`. `ev_quit` is called when we receive a “quit” event, which happens when we click the “X” in the window of the program. In that case, we want to quit the program, so we raise `SystemExit()` to do so.

```python
    def ev_keydown(self, event: tcod.event.KeyDown) -> Optional[Action]:
```

This method will receive key press events, and return either an `Action` subclass, or `None`, if no valid key was pressed.

```python
        action: Optional[Action] = None

        key = event.sym
```

`action` is the variable that will hold whatever subclass of `Action` we end up assigning it to. If no valid key press is found, it will remain set to `None`. We’ll return it either way.

`key` holds the actual key we pressed. It doesn’t contain additional information about modifiers like `Shift` or `Alt`, just the actual key that was pressed. That’s all we need right now.

From there, we go down a list of possible keys pressed. For example:

```python
        if key == tcod.event.K_UP:
            action = MovementAction(dx=0, dy=-1)
```

In this case, the user pressed the up-arrow key, so we’re creating a `MovementAction`. Notice that here (and in all the other cases of `MovementAction`) we provide `dx` and `dy`. These describe which direction our character will move in.

```python
        elif key == tcod.event.K_ESCAPE:
            action = EscapeAction()
```

If the user pressed the “Escape” key, we return `EscapeAction`. We’ll use this to exit the game for now, though in the future, `EscapeAction` can be used to do things like exit menus.

```python
        return action
```

Whether `action` is assigned to an `Action` subclass or `None`, we return it.

Let’s put our new actions and input handlers to use in `main.py`. Edit `main.py` like this:

Diff Original

```diff
#!/usr/bin/env python3
import tcod

+from actions import EscapeAction, MovementAction
+from input_handlers import EventHandler


def main() -> None:
    screen_width = 80
    screen_height = 50

    player_x = int(screen_width / 2)
    player_y = int(screen_height / 2)

    tileset = tcod.tileset.load_tilesheet(
        "dejavu10x10_gs_tc.png", 32, 8, tcod.tileset.CHARMAP_TCOD
    )

+   event_handler = EventHandler()

    with tcod.context.new_terminal(
        ...

            ...
            for event in tcod.event.wait():
-               if event.type == "QUIT":
-                   raise SystemExit()

+               action = event_handler.dispatch(event)

+               if action is None:
+                   continue

+               if isinstance(action, MovementAction):
+                   player_x += action.dx
+                   player_y += action.dy

+               elif isinstance(action, EscapeAction):
+                   raise SystemExit()


if __name__ == "__main__":
    main()
```

Let’s break down the new additions a bit.

```python
from actions import EscapeAction, MovementAction
from input_handlers import EventHandler
```

We’re importing the `EscapeAction` and `MovementAction` from `actions`, and `EventHandler` from `input_handlers`. This allows us to use the functions we wrote in those files in our `main` file.

```python
    event_handler = EventHandler()
```

`event_handler` is an instance of our `EventHandler` class. We’ll use it to receive events and process them.

```python
                action = event_handler.dispatch(event)
```

We send the `event` to our `event_handler`’s “dispatch” method, which sends the event to its proper place. In this case, a keyboard event will be sent to the `ev_keydown` method we wrote. The `Action` returned from that method is assigned to our local `action` variable.

```python
                if action is None:
                    continue
```

This is pretty straightforward: If `action` is `None` (that is, no key was pressed, or the key pressed isn’t recognized), then we skip over the rest the loop. There’s no need to go any further, since the lines below are going to handle the valid key presses.

```python
                if isinstance(action, MovementAction):
                    player_x += action.dx
                    player_y += action.dy
```

Now we arrive at the interesting part. If the `action` is an instance of the class `MovementAction`, we need to move our “@” symbol. We grab the `dx` and `dy` values we gave to `MovementAction` earlier, which will move the “@” symbol in which direction we want it to move. `dx` and `dy`, as of now, will only ever be -1, 0, or 1. Regardless of what the value is, we add `dx` and `dy` to `player_x` and `player_y`, respectively. Because the console is using `player_x` and `player_y` to draw where our “@” symbol is, modifying these two variables will cause the symbol to move.

```python
                elif isinstance(action, EscapeAction):
                    raise SystemExit()
```

`raise SystemExit()` should look familiar: it’s how we’re quitting out of the program. So basically, if the user hits the `Esc` key, our program should exit.

With all that done, let’s run the program and see what happens!

Indeed, our “@” symbol does move, but… it’s perhaps not what was expected.

![Snake the Roguelike?](https://rogueliketutorials.com/images/snake_the_roguelike.png "Snake the Roguelike?")

Unless you’re making a roguelike version of “Snake” (and who knows, maybe you are), we need to fix the “@” symbol being left behind wherever we move. So why is this happening in the first place?

Turns out, we need to “clear” the console after we’ve drawn it, or we’ll get these leftovers when we draw symbols in their new places. Luckily, this is as easy as adding one line:

Diff Original

```diff
    ...
        while True:
            root_console.print(x=player_x, y=player_y, string="@")

            context.present(root_console)

+           root_console.clear()

            for event in tcod.event.wait():
                ...
```

That’s it! Run the project now, and the “@” symbol will move around, without leaving traces of itself behind.

That wraps up part one of this tutorial! If you’re using git or some other form of version control (and I recommend you do), commit your changes now.

If you want to see the code so far in its entirety, [click here](https://github.com/TStand90/tcod_tutorial_v2/tree/2020/part-1).

---

# [Part 2 - The generic Entity, the render functions, and the map](https://rogueliketutorials.com/tutorials/tcod/v2/part-2/)

Now that we can move our little ‘@’ symbol around, we need to give it something to move around _in_. But before that, let’s stop for a moment and think about the player object itself.

Right now, we just represent the player with the ‘@’ symbol, and its x and y coordinates. Shouldn’t we tie those things together in an object, along with some other data and functions that pertain to it?

Let’s create a generic class to represent not just the player, but just about _everything_ in our game world. Enemies, items, and whatever other foreign entities we can dream of will be part of this class, which we’ll call `Entity`.

Create a new file, and call it `entity.py`. In that file, put the following class:

```python
from typing import Tuple


class Entity:
    """
    A generic object to represent players, enemies, items, etc.
    """
    def __init__(self, x: int, y: int, char: str, color: Tuple[int, int, int]):
        self.x = x
        self.y = y
        self.char = char
        self.color = color

    def move(self, dx: int, dy: int) -> None:
        # Move the entity by a given amount
        self.x += dx
        self.y += dy
```

The initializer (`__init__`) takes four arguments: `x`, `y`, `char`, and `color`.

- `x` and `y` are pretty self explanatory: They represent the Entity’s “x” and “y” coordinates on the map.
- `char` is the character we’ll use to represent the entity. Our player will be an “@” symbol, whereas something like a Troll (coming in a later chapter) can be the letter “T”.
- `color` is the color we’ll use when drawing the Entity. We define `color` as a Tuple of three integers, representing the entity’s RGB values.

The other method is `move`, which takes `dx` and `dy` as arguments, and uses them to modify the Entity’s position. This should look familiar to what we did in the last chapter.

Let’s put our fancy new class into action! Modify the first part of `main.py` to look like this:

Diff Original

```diff
#!/usr/bin/env python3
import tcod

from actions import EscapeAction, MovementAction
+from entity import Entity
from input_handlers import EventHandler


def main() -> None:
    screen_width = 80
    screen_height = 50

-   player_x = int(screen_width / 2)
-   player_y = int(screen_height / 2)

    tileset = tcod.tileset.load_tilesheet(
        "dejavu10x10_gs_tc.png", 32, 8, tcod.tileset.CHARMAP_TCOD
    )

    event_handler = EventHandler()

+   player = Entity(int(screen_width / 2), int(screen_height / 2), "@", (255, 255, 255))
+   npc = Entity(int(screen_width / 2 - 5), int(screen_height / 2), "@", (255, 255, 0))
+   entities = {npc, player}

    with tcod.context.new_terminal(
        ...
```

We’re importing the `Entity` class into `main.py`, and using it to initialize the player and a new NPC. We store these two in a set, that will eventually hold all our entities on the map.

Also modify the part where we handle movement so that the Entity class handles the actual movement.

Diff Original

```diff
                if isinstance(action, MovementAction):
-                   player_x += action.dx
-                   player_y += action.dy
+                   player.move(dx=action.dx, dy=action.dy)
```

Lastly, update the drawing functions to use the new player object:

Diff Original

```diff
        while True:
-           root_console.print(x=player_x, y=player_y, string="@")
+           root_console.print(x=player.x, y=player.y, string=player.char, fg=player.color)

            context.present(root_console)
```

If you run the project now, only the player gets drawn. We’ll need to modify things to draw both entities, and eventually, draw the map we’re going to create as well.

Before doing that, it’s worth stopping and taking a moment to think about our overall design. Currently, our `main.py` file is responsible for:

- Setting up the initial variables, like screen size and the tileset.
- Creating the entities
- Drawing the screen and everything on it.
- Reacting to the player’s input.

Soon, we’re going to need to add a map as well. It’s starting to become a bit much.

One thing we can do is pass of some of these responsibilities to another class, which will be responsible for “running” our game. The `main.py` file can still set things up and tell that new class what to do, but this design should help keep the `main.py` file from getting too large over time.

Let’s create an `Engine` class, which will take the responsibilities of drawing the map and entities, as well as handling the player’s input. Create a new file, and call it `engine.py`. In that file, put the following contents:

```python
from typing import Set, Iterable, Any

from tcod.context import Context
from tcod.console import Console

from actions import EscapeAction, MovementAction
from entity import Entity
from input_handlers import EventHandler


class Engine:
    def __init__(self, entities: Set[Entity], event_handler: EventHandler, player: Entity):
        self.entities = entities
        self.event_handler = event_handler
        self.player = player

    def handle_events(self, events: Iterable[Any]) -> None:
        for event in events:
            action = self.event_handler.dispatch(event)

            if action is None:
                continue

            if isinstance(action, MovementAction):
                self.player.move(dx=action.dx, dy=action.dy)

            elif isinstance(action, EscapeAction):
                raise SystemExit()

    def render(self, console: Console, context: Context) -> None:
        for entity in self.entities:
            console.print(entity.x, entity.y, entity.char, fg=entity.color)

        context.present(console)

        console.clear()
```

Let’s walk through the class a bit, to understand what we’re trying to get at here.

```python
class Engine:
    def __init__(self, entities: Set[Entity], event_handler: EventHandler, player: Entity):
        self.entities = entities
        self.event_handler = event_handler
        self.player = player
```

The `__init__` function takes three arguments:

- `entities` is a set (of entities), which behaves kind of like a list that enforces uniqueness. That is, we can’t add an Entity to the set twice, whereas a list would allow that. In our case, having an entity in `entities` twice doesn’t make sense.
- `event_handler` is the same `event_handler` that we used in `main.py`. It will handle our events.
- `player` is the player Entity. We have a separate reference to it outside of `entities` for ease of access. We’ll need to access `player` a lot more than a random entity in `entities`.

```python
    def handle_events(self, events: Iterable[Any]) -> None:
        for event in events:
            action = self.event_handler.dispatch(event)

            if action is None:
                continue

            if isinstance(action, MovementAction):
                self.player.move(dx=action.dx, dy=action.dy)

            elif isinstance(action, EscapeAction):
                raise SystemExit()
```

This should look familiar: It’s almost identical to our event processing in `main.py`. We pass the `events` to it so it can iterate through them, and it uses `self.event_handler` to handle the events.

```python
    def render(self, console: Console, context: Context) -> None:
        for entity in self.entities:
            console.print(entity.x, entity.y, entity.char, fg=entity.color)

        context.present(console)

        console.clear()
```

This handles drawing our screen. We iterate through the `self.entities` and print them to their proper locations, then present the context, and clear the console, like we did in `main.py`.

To make use of our new `Engine` class, we’ll need to modify `main.py` quite a bit.

Diff Original

```diff
#!/usr/bin/env python3
import tcod

-from actions import EscapeAction, MovementAction
+from engine import Engine
from entity import Entity
from input_handlers import EventHandler


def main() -> None:
    screen_width = 80
    screen_height = 50

    tileset = tcod.tileset.load_tilesheet(
        "dejavu10x10_gs_tc.png", 32, 8, tcod.tileset.CHARMAP_TCOD
    )

    event_handler = EventHandler()

    player = Entity(int(screen_width / 2), int(screen_height / 2), "@", (255, 255, 255))
    npc = Entity(int(screen_width / 2 - 5), int(screen_height / 2), "@", (255, 255, 0))
    entities = {npc, player}

+   engine = Engine(entities=entities, event_handler=event_handler, player=player)

    with tcod.context.new_terminal(
        screen_width,
        screen_height,
        tileset=tileset,
        title="Yet Another Roguelike Tutorial",
        vsync=True,
    ) as context:
        root_console = tcod.Console(screen_width, screen_height, order="F")
        while True:
-           root_console.print(x=player_x, y=player_y, string="@")
+           engine.render(console=root_console, context=context)

-           context.present(root_console)
+           events = tcod.event.wait()

+           engine.handle_events(events)
-           root_console.clear()

-           for event in tcod.event.wait():
-               action = event_handler.dispatch(event)

-               if action is None:
-                   continue

-               if isinstance(action, MovementAction):
-                   player_x += action.dx
-                   player_y += action.dy

-               elif isinstance(action, EscapeAction):
-                   raise SystemExit()


if __name__ == "__main__":
    main()
```

Because we’ve moved the rendering and event handling code to the `Engine` class, we no longer need it in `main.py`. All we need to do is create the `Engine` instance, pass the needed variables to it, and use the methods we wrote for it.

Run the project now, and your screen should look like this:

![Part 2 - Both Entities](https://rogueliketutorials.com/images/part-2-drawing-both-entities.png)

Our `main.py` file is looking a lot smaller and simpler, and we’ve rendered both the player and the NPC to the screen. With that, we’ll want to move on to creating a map for our entity to move around in. We won’t do the procedural dungeon generation in this chapter (that’s next), but we’ll at least get our class that will hold that map set up.

We can represent the map with a new class, called `GameMap`. The map itself will be made up of tiles, which will contain certain data about if the tile is “walkable” (True if it’s a floor, False if its a wall), “transparency” (again, True for floors, False for walls), and how to render the tile to the screen.

We’ll create the `tiles` first. Create a new file called `tile_types.py` and fill it with the following contents:

```python
from typing import Tuple

import numpy as np  # type: ignore

# Tile graphics structured type compatible with Console.tiles_rgb.
graphic_dt = np.dtype(
    [
        ("ch", np.int32),  # Unicode codepoint.
        ("fg", "3B"),  # 3 unsigned bytes, for RGB colors.
        ("bg", "3B"),
    ]
)

# Tile struct used for statically defined tile data.
tile_dt = np.dtype(
    [
        ("walkable", np.bool),  # True if this tile can be walked over.
        ("transparent", np.bool),  # True if this tile doesn't block FOV.
        ("dark", graphic_dt),  # Graphics for when this tile is not in FOV.
    ]
)


def new_tile(
    *,  # Enforce the use of keywords, so that parameter order doesn't matter.
    walkable: int,
    transparent: int,
    dark: Tuple[int, Tuple[int, int, int], Tuple[int, int, int]],
) -> np.ndarray:
    """Helper function for defining individual tile types """
    return np.array((walkable, transparent, dark), dtype=tile_dt)


floor = new_tile(
    walkable=True, transparent=True, dark=(ord(" "), (255, 255, 255), (50, 50, 150)),
)
wall = new_tile(
    walkable=False, transparent=False, dark=(ord(" "), (255, 255, 255), (0, 0, 100)),
)
```

That’s quite a lot to take in all at once. Let’s go through it.

```python
# Tile graphics structured type compatible with Console.tiles_rgb.
graphic_dt = np.dtype(
    [
        ("ch", np.int32),  # Unicode codepoint.
        ("fg", "3B"),  # 3 unsigned bytes, for RGB colors.
        ("bg", "3B"),
    ]
)
```

`dtype` creates a data type which Numpy can use, which behaves similarly to a `struct` in a language like C. Our data type is made up of three parts:

- `ch`: The character, represented in integer format. We’ll translate it from the integer into Unicode.
- `fg`: The foreground color. “3B” means 3 unsigned bytes, which can be used for RGB color codes.
- `bg`: The background color. Similar to `fg`.

We take this new data type and use it in the next bit:

```python
# Tile struct used for statically defined tile data.
tile_dt = np.dtype(
    [
        ("walkable", np.bool),  # True if this tile can be walked over.
        ("transparent", np.bool),  # True if this tile doesn't block FOV.
        ("dark", graphic_dt),  # Graphics for when this tile is not in FOV.
    ]
)
```

This is yet another `dtype`, which we’ll use in the actual tile itself. It’s also made up of three parts:

- `walkable`: A boolean that describes if the player can walk across this tile.
- `transparent`: A boolean that describes if this tile does or does not block the field of view. Not used in this chapter, but will be in chapter 4.
- `dark`: This uses our previously defined `dtype`, which holds the character to print, the foreground color, and the background color. Why is it called `dark`? Because later on, we’ll want to differentiate between tiles that are and aren’t in the field of view. `dark` will represent tiles that are not in the current field of view. Again, we’ll cover that in part 4.

```python
def new_tile(
    *,  # Enforce the use of keywords, so that parameter order doesn't matter.
    walkable: int,
    transparent: int,
    dark: Tuple[int, Tuple[int, int, int], Tuple[int, int, int]],
) -> np.ndarray:
    """Helper function for defining individual tile types """
    return np.array((walkable, transparent, dark), dtype=tile_dt)
```

This is a helper function, that we’ll use in the next section to define our tile types. It takes the parameters `walkable`, `transparent`, and `dark`, which should look familiar, since they’re the same data points we used in `tile_dt`. It creates a Numpy array of just the one `tile_dt` element, and returns it.

```python
floor = new_tile(
    walkable=True, transparent=True, dark=(ord(" "), (255, 255, 255), (50, 50, 150)),
)
wall = new_tile(
    walkable=False, transparent=False, dark=(ord(" "), (255, 255, 255), (0, 0, 100)),
)
```

Finally, we arrive to our actual tile types. We’ve got two: `floor` and `wall`.

`floor` is both `walkable` and `transparent`. Its `dark` attribute consists of the space character (feel free to change this to something else, a lot of roguelikes use “#”) and defines its foreground color as white (won’t matter since it’s an empty space) and a background color.

`wall` is neither `walkable` nor `transparent`, and its `dark` attribute differs from `floor` slightly in its background color.

Now let’s use our newly created tiles by creating our map class. Create a file called `game_map.py` and fill it with the following:

```python
import numpy as np  # type: ignore
from tcod.console import Console

import tile_types


class GameMap:
    def __init__(self, width: int, height: int):
        self.width, self.height = width, height
        self.tiles = np.full((width, height), fill_value=tile_types.floor, order="F")

        self.tiles[30:33, 22] = tile_types.wall

    def in_bounds(self, x: int, y: int) -> bool:
        """Return True if x and y are inside of the bounds of this map."""
        return 0 <= x < self.width and 0 <= y < self.height

    def render(self, console: Console) -> None:
        console.tiles_rgb[0:self.width, 0:self.height] = self.tiles["dark"]
```

Let’s break down `GameMap` a bit:

```python
    def __init__(self, width: int, height: int):
        self.width, self.height = width, height
        self.tiles = np.full((width, height), fill_value=tile_types.floor, order="F")

        self.tiles[30:33, 22] = tile_types.wall
```

The initializer takes `width` and `height` integers and assigns them, in one line.

The `self.tiles` line might look a little strange if you’re not used to Numpy. Basically, we create a 2D array, filled with the same values, which in this case, is the `tile_types.floor` that we created earlier. This will fill `self.tiles` with floor tiles.

`self.tiles[30:33, 22] = tile_types.wall` creates a small, three tile wide wall at the specified location. We won’t normally hard-code walls like this, the wall is just for demonstration purposes. We’ll remove it in the next part.

```python
    def in_bounds(self, x: int, y: int) -> bool:
        """Return True if x and y are inside of the bounds of this map."""
        return 0 <= x < self.width and 0 <= y < self.height
```

As the docstring alludes to, this method returns `True` if the given x and y values are within the map’s boundaries. We can use this to ensure the player doesn’t move beyond the map, into the void.

```python
    def render(self, console: Console) -> None:
        console.tiles_rgb[0:self.width, 0:self.height] = self.tiles["dark"]
```

Using the `Console` class’s `tiles_rgb` method, we can quickly render the entire map. This method proves much faster than using the `console.print` method that we use for the individual entities.

With our `GameMap` class ready to go, let’s modify `main.py` to make use of it. We’ll also need to modify `Engine` to hold the map. Let’s start with `main.py` though:

Diff Original

```diff
#!/usr/bin/env python3
import tcod

from engine import Engine
from entity import Entity
+from game_map import GameMap
from input_handlers import EventHandler


def main() -> None:
    screen_width = 80
    screen_height = 50

+   map_width = 80
+   map_height = 45

    tileset = tcod.tileset.load_tilesheet(
        "dejavu10x10_gs_tc.png", 32, 8, tcod.tileset.CHARMAP_TCOD
    )

    event_handler = EventHandler()

    player = Entity(int(screen_width / 2), int(screen_height / 2), "@", (255, 255, 255))
    npc = Entity(int(screen_width / 2 - 5), int(screen_height / 2), "@", (255, 255, 0))
    entities = {npc, player}

+   game_map = GameMap(map_width, map_height)

-   engine = Engine(entities=entities, event_handler=event_handler, player=player)
+   engine = Engine(entities=entities, event_handler=event_handler, game_map=game_map, player=player)

    with tcod.context.new_terminal(
        screen_width,
        screen_height,
        tileset=tileset,
        title="Yet Another Roguelike Tutorial",
        vsync=True,
    ) as context:
```

We’ve added `map_width` and `map_height`, two integers, which we use in the `GameMap` class to describe its width and height. The `game_map` variable holds our initialized `GameMap`, and we then pass it into `engine`. The `Engine` class doesn’t yet accept a `GameMap` in its `__init__` function, so let’s fix that now.

Diff Original

```diff
from typing import Set, Iterable, Any

from tcod.context import Context
from tcod.console import Console

from actions import EscapeAction, MovementAction
from entity import Entity
+from game_map import GameMap
from input_handlers import EventHandler


class Engine:
-   def __init__(self, entities: Set[Entity], event_handler: EventHandler, player: Entity):
+   def __init__(self, entities: Set[Entity], event_handler: EventHandler, game_map: GameMap, player: Entity):
        self.entities = entities
        self.event_handler = event_handler
+       self.game_map = game_map
        self.player = player

    def handle_events(self, events: Iterable[Any]) -> None:
        for event in events:
            action = self.event_handler.dispatch(event)

            if action is None:
                continue

            if isinstance(action, MovementAction):
-               self.player.move(dx=action.dx, dy=action.dy)
+               if self.game_map.tiles["walkable"][self.player.x + action.dx, self.player.y + action.dy]:
+                   self.player.move(dx=action.dx, dy=action.dy)

            elif isinstance(action, EscapeAction):
                raise SystemExit()

    def render(self, console: Console, context: Context) -> None:
+       self.game_map.render(console)

        for entity in self.entities:
            console.print(entity.x, entity.y, entity.char, fg=entity.color)

        context.present(console)

        console.clear()
```

We’ve imported the `GameMap` class, and we’re now passing an instance of it in the `Engine` class’s initializer. From there, we utilize it in two ways:

- In `handle_events`, we use it to check if the tile is “walkable”, and only then do we move the player.
- In `render`, we call the `GameMap`’s `render` method to draw it to the screen.

If you run the project now, it should look like this:

![Part 2 - Both Entities and Map](https://rogueliketutorials.com/images/part-2-entities-and-map.png)

The darker squares represent the wall, which, if you try to move your character through, should prove to be impenetrable.

Before we finish this up, there’s one last improvement we can make, thanks to our new `Engine` class: We can expand our `Action` classes to do a bit more of the heavy lifting, rather than leaving it to the `Engine`. This is because we can pass the `Engine` to the `Action`, providing it with the context it needs to do what we want.

Here’s what that looks like:

Diff Original

```diff
+from __future__ import annotations

+from typing import TYPE_CHECKING

+if TYPE_CHECKING:
+   from engine import Engine
+   from entity import Entity


class Action:
-   pass
+   def perform(self, engine: Engine, entity: Entity) -> None:
+       """Perform this action with the objects needed to determine its scope.

+       `engine` is the scope this action is being performed in.

+       `entity` is the object performing the action.

+       This method must be overridden by Action subclasses.
+       """
+       raise NotImplementedError()


class EscapeAction(Action):
-   pass
+   def perform(self, engine: Engine, entity: Entity) -> None:
+       raise SystemExit()


class MovementAction(Action):
    def __init__(self, dx: int, dy: int):
        super().__init__()

        self.dx = dx
        self.dy = dy

+   def perform(self, engine: Engine, entity: Entity) -> None:
+       dest_x = entity.x + self.dx
+       dest_y = entity.y + self.dy

+       if not engine.game_map.in_bounds(dest_x, dest_y):
+           return  # Destination is out of bounds.
+       if not engine.game_map.tiles["walkable"][dest_x, dest_y]:
+           return  # Destination is blocked by a tile.

+       entity.move(self.dx, self.dy)
```

Now we’re passing in the `Engine` and the `Entity` performing the action to each `Action` subclass. Each subclass needs to implement its own version of the `perform` method. In the case of `EscapeAction`, we’re just raising `SystemExit`. In the case of `MovementAction`, we double check that the move is “in bounds” and on a “walkable” tile, and if either is true, we return without doing anything. If neither of those cases prove true, then we move the entity, as before.

So what does this new technique do for us? As it turns out, we can simplify the `Engine.handle_events` method like this:

Diff Original

```diff
...
-from actions import EscapeAction, MovementAction
from entity import Entity
from game_map import GameMap
from input_handlers import EventHandler


class Engine:
    ...

    def handle_events(self, events: Iterable[Any]) -> None:
        for event in events:
            action = self.event_handler.dispatch(event)

            if action is None:
                continue

+           action.perform(self, self.player)
-           if isinstance(action, MovementAction):
-               if self.game_map.tiles["walkable"][self.player.x + action.dx, self.player.y + action.dy]:
-                   self.player.move(dx=action.dx, dy=action.dy)

-           elif isinstance(action, EscapeAction):
-               raise SystemExit()
```

Much simpler! Run the project again, and it should function the same as before.

With that, Part 2 is now complete! We’ve managed to lay the groundwork for generating dungeons and moving through them, which, as it happens, is what the next part is all about.

If you want to see the code so far in its entirety, [click here](https://github.com/TStand90/tcod_tutorial_v2/tree/2020/part-2).

[Click here to move on to the next part of this tutorial.](https://rogueliketutorials.com/tutorials/tcod/v2/part-3)

---
# [Part 3 - Generating a dungeon](https://rogueliketutorials.com/tutorials/tcod/v2/part-3/)

_Note: This part of the tutorial relies on TCOD version 11.14 or higher. You might need to upgrade the library (and your requirements.txt file, if you’re using one)._

Remember how we created a wall in the last part? We won’t need that anymore. Additionally, our dungeon generator will start by filling the entire map with “wall” tiles and “carving” out rooms, so we can modify our `GameMap` class to fill in walls instead of floors.

Diff Original

```diff
class GameMap:
    def __init__(self, width: int, height: int):
        self.width, self.height = width, height
-       self.tiles = np.full((width, height), fill_value=tile_types.floor, order="F")
+       self.tiles = np.full((width, height), fill_value=tile_types.wall, order="F")

-       self.tiles[30:33, 22] = tile_types.wall
        ...
```

Now, on to our dungeon generator.

The original version of this tutorial put all of the dungeon generation in the `GameMap` class. In fact, this was my plan for this tutorial as well. But, as HexDecimal (author of the TCOD library) pointed out in a pull request, that’s not very extensible. It puts a lot of code in `GameMap` where it doesn’t necessarily belong, and the class will grow to huge proportions if you ever decide to add an alternate dungeon generator.

The better approach is to put our new code in a separate file, and utilize `GameMap` there. Let’s create a new file, called `procgen.py`, which will house our procedural generator.

Let’s start by creating a class which we’ll use to create our rooms. We can call it `RectangularRoom`:

```python
from typing import Tuple


class RectangularRoom:
    def __init__(self, x: int, y: int, width: int, height: int):
        self.x1 = x
        self.y1 = y
        self.x2 = x + width
        self.y2 = y + height

    @property
    def center(self) -> Tuple[int, int]:
        center_x = int((self.x1 + self.x2) / 2)
        center_y = int((self.y1 + self.y2) / 2)

        return center_x, center_y

    @property
    def inner(self) -> Tuple[slice, slice]:
        """Return the inner area of this room as a 2D array index."""
        return slice(self.x1 + 1, self.x2), slice(self.y1 + 1, self.y2)
```

The `__init__` function takes the x and y coordinates of the top left corner, and computes the bottom right corner based on the w and h parameters (width and height).

`center` is a “property”, which essentially acts like a read-only variable for our `RectangularRoom` class. It describes the “x” and “y” coordinates of the center of a room. It will be useful later on.

The `inner` property returns two “slices”, which represent the inner portion of our room. This is the part that we’ll be “digging out” for our room in our dungeon generator. It gives us an easy way to get the area to carve out, which we’ll demonstrate soon.

We’ll be adding more to this class shortly, but to get us started, that’s all we need.

What’s with the + 1 on `self.x1` and `self.y1`? Think about what we’re saying when we tell our program that we want a room at coordinates (1, 1) that goes to (6, 6). You might assume that would carve out a room like this one (remember that lists are 0-indexed, so (0, 0) is a wall in this case):

```fallback
  0 1 2 3 4 5 6 7
0 # # # # # # # #
1 # . . . . . . #
2 # . . . . . . #
3 # . . . . . . #
4 # . . . . . . #
5 # . . . . . . #
6 # . . . . . . #
7 # # # # # # # #
```

That’s all fine and good, but what happens if we put a room right next to it? Let’s say this room starts at (7, 1) and goes to (9, 6)

```fallback
  0 1 2 3 4 5 6 7 8 9 10
0 # # # # # # # # # # #
1 # . . . . . . . . . #
2 # . . . . . . . . . #
3 # . . . . . . . . . #
4 # . . . . . . . . . #
5 # . . . . . . . . . #
6 # . . . . . . . . . #
7 # # # # # # # # # # #
```

There’s no wall separating the two! That means that if two rooms are one right next to the other, then there won’t be a wall between them! So long story short, our function needs to take the walls into account when digging out a room. So if we have a rectangle with coordinates x1 = 1, x2 = 6, y1 = 1, and y2 = 6, then the room should actually look like this:

```fallback
  0 1 2 3 4 5 6 7
0 # # # # # # # #
1 # # # # # # # #
2 # # . . . . # #
3 # # . . . . # #
4 # # . . . . # #
5 # # . . . . # #
6 # # # # # # # #
7 # # # # # # # #
```

This ensures that we’ll always have at least a one tile wide wall between our rooms, unless we choose to create overlapping rooms. In order to accomplish this, we add + 1 to x1 and y1.

Before we dive into a truly procedurally generated dungeon, let’s begin with a simple map that consists of two rooms, connected by a tunnel. We can create a new function to create our dungeon, intuitively named `generate_dungeon`, which will return a `GameMap`. As arguments, it will take the needed width and the height to create the `GameMap`, and it will utilize the `RectangularRoom` class to create the needed rooms. Here’s what that looks like:

Diff Original

```diff
from typing import Tuple

+from game_map import GameMap
+import tile_types


class RectangularRoom:
    def __init__(self, x: int, y: int, width: int, height: int):
        self.x1 = x
        self.y1 = y
        self.x2 = x + width
        self.y2 = y + height

    @property
    def inner(self) -> Tuple[slice, slice]:
        """Return the inner area of this room as a 2D array index."""
        return slice(self.x1 + 1, self.x2), slice(self.y1 + 1, self.y2)


+def generate_dungeon(map_width, map_height) -> GameMap:
+   dungeon = GameMap(map_width, map_height)

+   room_1 = RectangularRoom(x=20, y=15, width=10, height=15)
+   room_2 = RectangularRoom(x=35, y=15, width=10, height=15)

+   dungeon.tiles[room_1.inner] = tile_types.floor
+   dungeon.tiles[room_2.inner] = tile_types.floor

+   return dungeon
```

Now we can modify `main.py` to utilize our now `generate_dungeon` function.

Diff Original

```diff
#!/usr/bin/env python3
import tcod

from engine import Engine
from entity import Entity
-from game_map import GameMap
from input_handlers import EventHandler
+from procgen import generate_dungeon


def main() -> None:
   ...

   entities = {npc, player}

-   game_map = GameMap(map_width, map_height)
+   game_map = generate_dungeon(map_width, map_height)

   engine = Engine(entities=entities, event_handler=event_handler, game_map=game_map, player=player)
   ...
```

Now is a good time to run your code and make sure everything works as expected. The changes we’ve made puts two sample rooms on the map, with our player in one of them (our poor NPC is stuck in a wall though).

![Part 3 - Two Rooms](https://rogueliketutorials.com/images/part-3-two-rooms.png)

I’m sure you’ve noticed already, but the rooms are not connected. What’s the use of creating a dungeon if we’re stuck in one room? Not to worry, let’s write some code to generate tunnels from one room to another. Add the following function to `procgen.py`:

Diff Original

```diff
+import random
-from typing import Tuple
+from typing import Iterator, Tuple

+import tcod

from game_map import GameMap
import tile_types

...

        ...
        return slice(self.x1 + 1, self.x2), slice(self.y1 + 1, self.y2)


+def tunnel_between(
+   start: Tuple[int, int], end: Tuple[int, int]
+) -> Iterator[Tuple[int, int]]:
+   """Return an L-shaped tunnel between these two points."""
+   x1, y1 = start
+   x2, y2 = end
+   if random.random() < 0.5:  # 50% chance.
+       # Move horizontally, then vertically.
+       corner_x, corner_y = x2, y1
+   else:
+       # Move vertically, then horizontally.
+       corner_x, corner_y = x1, y2

+   # Generate the coordinates for this tunnel.
+   for x, y in tcod.los.bresenham((x1, y1), (corner_x, corner_y)).tolist():
+       yield x, y
+   for x, y in tcod.los.bresenham((corner_x, corner_y), (x2, y2)).tolist():
+       yield x, y


def generate_dungeon(map_width, map_height) -> GameMap:
    ...
```

Let’s dive into this method.

```python
def tunnel_between(
    start: Tuple[int, int], end: Tuple[int, int]
) -> Iterator[Tuple[int, int]]:
```

The function takes two arguments, both Tuples consisting of two integers. It should return an Iterator of a Tuple of two ints. All the Tuples will be “x” and “y” coordinates on the map.

```python
    """Return an L-shaped tunnel between these two points."""
    x1, y1 = start
    x2, y2 = end
```

We grab the coordinates out of the Tuples. Simple enough.

```python
    if random.random() < 0.5:  # 50% chance.
        # Move horizontally, then vertically.
        corner_x, corner_y = x2, y1
    else:
        # Move vertically, then horizontally.
        corner_x, corner_y = x1, y2
```

We’re randomly picking between two options: Moving horizontally, then vertically, or the opposite. Based on what’s chosen, we’ll set the `corner_x` and `corner_y` values to different points.

```python
    # Generate the coordinates for this tunnel.
    for x, y in tcod.los.bresenham((x1, y1), (corner_x, corner_y)).tolist():
        yield x, y
    for x, y in tcod.los.bresenham((corner_x, corner_y), (x2, y2)).tolist():
        yield x, y
```

This part is where the “magic” happens.

tcod includes a function in its line-of-sight module to draw [Bresenham lines](https://en.wikipedia.org/wiki/Bresenham%27s_line_algorithm). While we’re not working with line-of-sight in this case, the function still proves useful to get a line from one point to another. In this case, we get one line, then another, to create an “L” shaped tunnel. `.tolist()` converts the points in the line into, as you might have already guessed, a list.

What’s with the `yield` lines though? [Yield expressions](https://docs.python.org/3.5/reference/expressions.html#yield-expressions) are an interesting part of Python, which allows you to return a “generator”. Essentially, rather than returning the values and exiting the function altogether, we return the values, but keep the local state. This allows the function to pick up where it left off when called again, instead of starting from the beginning, as most functions do.

Why is this helpful? In the next section, we’re going to iterate the `x` and `y` values that we receive from the `tunnel_between` function to dig out our tunnel.

Let’s put this code to use by drawing a tunnel between our two rooms.

Diff Original

```diff
   ...
   dungeon.tiles[room_2.inner] = tile_types.floor

+   for x, y in tunnel_between(room_2.center, room_1.center):
+       dungeon.tiles[x, y] = tile_types.floor

   return dungeon
```

Run the project, and you’ll see a horizontal tunnel that connects the two rooms. It’s starting to come together!

![Part 3 - Two Rooms](https://rogueliketutorials.com/images/part-3-two-rooms-connected.png)

Now that we’ve demonstrated to ourselves that our room and tunnel functions work as intended, it’s time to move on to an actual dungeon generation algorithm. Ours will be fairly simple; we’ll place rooms one at a time, making sure they don’t overlap, and connect them with tunnels.

We’ll want a method that tells us if our room is intersecting with another room. Enter the following into the `RectangularRoom` class:

Diff Original

```diff
+from __future__ import annotations

import random
from typing import Iterator, Tuple

import tcod

from game_map import GameMap
import tile_types


class RectangularRoom:
   def __init__(self, x: int, y: int, width: int, height: int):
       self.x1 = x
       self.y1 = y
       self.x2 = x + width
       self.y2 = y + height

   @property
   def center(self) -> Tuple[int, int]:
       center_x = int((self.x1 + self.x2) / 2)
       center_y = int((self.y1 + self.y2) / 2)

       return center_x, center_y

   @property
   def inner(self) -> Tuple[slice, slice]:
       """Return the inner area of this room as a 2D array index."""
       return slice(self.x1 + 1, self.x2), slice(self.y1 + 1, self.y2)

+   def intersects(self, other: RectangularRoom) -> bool:
+       """Return True if this room overlaps with another RectangularRoom."""
+       return (
+           self.x1 <= other.x2
+           and self.x2 >= other.x1
+           and self.y1 <= other.y2
+           and self.y2 >= other.y1
+       )


def tunnel_between(
   ...
```

`intersects` checks if the room and another room (`other` in the arguments) intersect or not. It returns `True` if the do, `False` if they don’t. We’ll use this to determine if two rooms are overlapping or not.

We’re going to need a few variables to set the maximum and minimum size of the rooms, along with the maximum number of rooms one floor can have. Add the following to `main.py`:

Diff Original

```diff
   ...
   map_height = 45

+   room_max_size = 10
+   room_min_size = 6
+   max_rooms = 30

   tileset = tcod.tileset.load_tilesheet(
   ...
```

At long last, it’s time to modify `generate_dungeon` to, well, generate our dungeon! You can completely remove our old implementation and replace it with the following:

Diff Original

```diff
from __future__ import annotations

import random
-from typing import Iterator, Tuple
+from typing import Iterator, List, Tuple, TYPE_CHECKING

from game_map import GameMap
import tile_types


+if TYPE_CHECKING:
+   from entity import Entity

...

-def generate_dungeon(map_width, map_height) -> GameMap:
-   dungeon = GameMap(map_width, map_height)

-   room_1 = RectangularRoom(x=20, y=15, width=10, height=15)
-   room_2 = RectangularRoom(x=35, y=15, width=10, height=15)

-   dungeon.tiles[room_1.inner] = tile_types.floor
-   dungeon.tiles[room_2.inner] = tile_types.floor

-   create_horizontal_tunnel(dungeon, 25, 40, 23)

-   return dungeon


+def generate_dungeon(
+   max_rooms: int,
+   room_min_size: int,
+   room_max_size: int,
+   map_width: int,
+   map_height: int,
+   player: Entity,
+) -> GameMap:
+   """Generate a new dungeon map."""
+   dungeon = GameMap(map_width, map_height)

+   rooms: List[RectangularRoom] = []

+   for r in range(max_rooms):
+       room_width = random.randint(room_min_size, room_max_size)
+       room_height = random.randint(room_min_size, room_max_size)

+       x = random.randint(0, dungeon.width - room_width - 1)
+       y = random.randint(0, dungeon.height - room_height - 1)

+       # "RectangularRoom" class makes rectangles easier to work with
+       new_room = RectangularRoom(x, y, room_width, room_height)

+       # Run through the other rooms and see if they intersect with this one.
+       if any(new_room.intersects(other_room) for other_room in rooms):
+           continue  # This room intersects, so go to the next attempt.
+       # If there are no intersections then the room is valid.

+       # Dig out this rooms inner area.
+       dungeon.tiles[new_room.inner] = tile_types.floor

+       if len(rooms) == 0:
+           # The first room, where the player starts.
+           player.x, player.y = new_room.center
+       else:  # All rooms after the first.
+           # Dig out a tunnel between this room and the previous one.
+           for x, y in tunnel_between(rooms[-1].center, new_room.center):
+               dungeon.tiles[x, y] = tile_types.floor

+       # Finally, append the new room to the list.
+       rooms.append(new_room)

+   return dungeon
```

That’s quite a lengthy function! Let’s break it down and figure out what’s doing what.

```python
def generate_dungeon(
    max_rooms: int,
    room_min_size: int,
    room_max_size: int,
    map_width: int,
    map_height: int,
    player: Entity,
) -> GameMap:
```

This is the function definition itself. We pass several arguments to it.

- `max_rooms`: The maximum number of rooms allowed in the dungeon. We’ll use this to control our iteration.
- `room_min_size`: The minimum size of one room.
- `room_max_size`: The maximum size of one room. We’ll pick a random size between this and `room_min_size` for both the width and the height of one room to carve out.
- `map_width` and `map_height`: The width and height of the `GameMap` to create. This is no different than it was before.
- `player`: The player Entity. We need this to know where to place the player.

```python
    """Generate a new dungeon map."""
    dungeon = GameMap(map_width, map_height)
```

This isn’t anything new, we’re just creating the initial `GameMap`.

```python
    rooms: List[RectangularRoom] = []
```

We’ll keep a running list of all the rooms.

```python
    for r in range(max_rooms):
```

We iterate from 0 to `max_rooms` - 1. Our algorithm may or may not place a room depending on if it intersects with another, so we won’t know how many rooms we’re going to end up with. But at least we’ll know that number can’t exceed a certain amount.

```python
        room_width = random.randint(room_min_size, room_max_size)
        room_height = random.randint(room_min_size, room_max_size)

        x = random.randint(0, dungeon.width - room_width - 1)
        y = random.randint(0, dungeon.height - room_height - 1)

        # "RectangularRoom" class makes rectangles easier to work with
        new_room = RectangularRoom(x, y, room_width, room_height)
```

Here, we use the given minimum and maximum room sizes to set the room’s width and height. We then get a random pair of `x` and `y` coordinates to try and place the room down. The coordinates must be between 0 and the map’s width and heights.

We use these variables to then create an instance of our `RectangularRoom`.

```python
        # Run through the other rooms and see if they intersect with this one.
        if any(new_room.intersects(other_room) for other_room in rooms):
            continue  # This room intersects, so go to the next attempt.
```

So what happens if a room _does_ intersect with another? In that case, we can just toss it out, by using `continue` to skip the rest of the loop. Obviously there are more elegant ways of dealing with a collision, but for our simplistic algorithm, we’ll just pretend like it didn’t happen and try the next one.

```python
        # If there are no intersections then the room is valid.

        # Dig out this rooms inner area.
        dungeon.tiles[new_room.inner] = tile_types.floor
```

Here, we “dig” the room out. This is similar to what we were doing before to dig out the two connected rooms.

```python
        if len(rooms) == 0:
            # The first room, where the player starts.
            player.x, player.y = new_room.center
```

We put our player down in the center of the first room we created. If this room isn’t the first, we move on to the `else` statement:

```python
        else:  # All rooms after the first.
            # Dig out a tunnel between this room and the previous one.
            for x, y in tunnel_between(rooms[-1].center, new_room.center):
                dungeon.tiles[x, y] = tile_types.floor
```

This is similar to how we dug the tunnel before, except this time, we’re using a negative index with `rooms` to grab the previous room, and connecting the new room to it.

```python
        # Finally, append the new room to the list.
        rooms.append(new_room)
```

Regardless if it’s the first room or not, we want to append it to the list, so the next iteration can use it.

So that’s our `generate_dungeon` function, but we’re not quite finished yet. We need to modify the call we make to this function in `main.py`:

Diff Original

```diff
    ...
    entities = {npc, player}

-   game_map = generate_dungeon(map_width, map_height)
+   game_map = generate_dungeon(
+       max_rooms=max_rooms,
+       room_min_size=room_min_size,
+       room_max_size=room_max_size,
+       map_width=map_width,
+       map_height=map_height,
+       player=player
+   )

    engine = Engine(entities=entities, event_handler=event_handler, game_map=game_map, player=player)
    ...
```

And that’s it! There’s our functioning, albeit basic, dungeon generation algorithm. Run the project now and you should be placed in a procedurally generated dungeon! Note that our NPC isn’t being placed intelligently here, so it may or may not be stuck in a wall.

![Part 3 - Generated Dungeon](https://rogueliketutorials.com/images/part-3-dungeon.png)

_Note: Your dungeon will look different from this one, so don’t worry if it doesn’t match the screenshot._

If you want to see the code so far in its entirety, [click here](https://github.com/TStand90/tcod_tutorial_v2/tree/2020/part-3).

[Click here to move on to the next part of this tutorial.](https://rogueliketutorials.com/tutorials/tcod/v2/part-4)

---
# [Part 4 - Field of View](https://rogueliketutorials.com/tutorials/tcod/v2/part-4/)

We have a dungeon now, and we can move about it freely. But are we really _exploring_ the dungeon if we can just see it all from the beginning?

Most roguelikes (not all!) only let you see within a certain range of your character, and ours will be no different. We need to implement a way to calculate the “Field of View” for our adventurer, and fortunately, tcod makes that easy!

When walking around the dungeon, there will essentially be three “states” a tile can be in, relating to our field of view.

1. Visible
2. Not visible
3. Not visible, but previously seen

What this means is that we should draw the “visible” tiles as well as the “not visible, but previously seen” ones to the screen, but differentiate them somehow. The “not visible” tiles can simply be drawn as an empty tile, with the color black, gray, or whatever you want to use.

In order to differentiate between these tiles, we’ll need two new Numpy arrays: One to keep track of the tiles that are currently visible, and another to keep track of all the tiles that our character has seen before. Add the two arrays to `GameMap` like this:

Diff Original

```diff
class GameMap:
    def __init__(self, width: int, height: int):
        self.width, self.height = width, height
        self.tiles = np.full((width, height), fill_value=tile_types.wall, order="F")

+       self.visible = np.full((width, height), fill_value=False, order="F")  # Tiles the player can currently see
+       self.explored = np.full((width, height), fill_value=False, order="F")  # Tiles the player has seen before
```

We create two arrays, `visible` and `explored`, and fill them with the value `False`. In a moment, we’ll create a function that will update these arrays based on what’s in the field of view.

Let’s turn our attention back to the tile types. Remember when we specified the “walkable”, “transparent”, and “dark” attributes? We called it “dark” because it’s what the tile will look like when its not in the field of view, but what about when it _is_?

For that, we’ll want a new `graphic_dt` in the `tile_dt` type, called `light`. We can add that by modifying `tile_types.py` like this:

Diff Original

```diff
tile_dt = np.dtype(
    [
        ("walkable", np.bool),  # True if this tile can be walked over.
        ("transparent", np.bool),  # True if this tile doesn't block FOV.
        ("dark", graphic_dt),  # Graphics for when this tile is not in FOV.
+       ("light", graphic_dt),  # Graphics for when the tile is in FOV.
    ]
)


def new_tile(
    *,  # Enforce the use of keywords, so that parameter order doesn't matter.
    walkable: int,
    transparent: int,
    dark: Tuple[int, Tuple[int, int, int], Tuple[int, int, int]],
+   light: Tuple[int, Tuple[int, int, int], Tuple[int, int, int]],
) -> np.ndarray:
    """Helper function for defining individual tile types """
-   return np.array((walkable, transparent, dark), dtype=tile_dt)
+   return np.array((walkable, transparent, dark, light), dtype=tile_dt)


+# SHROUD represents unexplored, unseen tiles
+SHROUD = np.array((ord(" "), (255, 255, 255), (0, 0, 0)), dtype=graphic_dt)

floor = new_tile(
-   walkable=True, transparent=True, dark=(ord(" "), (255, 255, 255), (50, 50, 150)),
+   walkable=True,
+   transparent=True,
+   dark=(ord(" "), (255, 255, 255), (50, 50, 150)),
+   light=(ord(" "), (255, 255, 255), (200, 180, 50)),
)
wall = new_tile(
-   walkable=False, transparent=False, dark=(ord(" "), (255, 255, 255), (0, 0, 100)),
+   walkable=False,
+   transparent=False,
+   dark=(ord(" "), (255, 255, 255), (0, 0, 100)),
+   light=(ord(" "), (255, 255, 255), (130, 110, 50)),
)
```

Let’s go through the new additions.

```python
tile_dt = np.dtype(
    [
        ("walkable", np.bool),  # True if this tile can be walked over.
        ("transparent", np.bool),  # True if this tile doesn't block FOV.
        ("dark", graphic_dt),  # Graphics for when this tile is not in FOV.
        ("light", graphic_dt),  # Graphics for when the tile is in FOV.
    ]
)
```

We’re adding a new `graphic_dt` to the `tile_dt` that we use to define our tiles. `light` will hold the information about what our tile looks like when it’s in the field of view.

```python
def new_tile(
    *,  # Enforce the use of keywords, so that parameter order doesn't matter.
    walkable: int,
    transparent: int,
    dark: Tuple[int, Tuple[int, int, int], Tuple[int, int, int]],
    light: Tuple[int, Tuple[int, int, int], Tuple[int, int, int]],
) -> np.ndarray:
    """Helper function for defining individual tile types """
    return np.array((walkable, transparent, dark, light), dtype=tile_dt)
```

We’ve modified the `new_tile` function to account for the new `light` attribute. `light` works the same as `dark`.

```python
# SHROUD represents unexplored, unseen tiles
SHROUD = np.array((ord(" "), (255, 255, 255), (0, 0, 0)), dtype=graphic_dt)
```

`SHROUD` is what we’ll use for when a tile is neither in view nor has been “explored”. It’s set to just draw a black tile.

```python
floor = new_tile(
    walkable=True,
    transparent=True,
    dark=(ord(" "), (255, 255, 255), (50, 50, 150)),
    light=(ord(" "), (255, 255, 255), (200, 180, 50)),
)
wall = new_tile(
    walkable=False,
    transparent=False,
    dark=(ord(" "), (255, 255, 255), (0, 0, 100)),
    light=(ord(" "), (255, 255, 255), (130, 110, 50)),
)
```

Finally, we add `light` to both the `floor` and `wall` tiles. We also modify the functions to fit a bit better on the screen, adding new lines after each argument. This is just for the sake of readability.

`light` in both cases is set to a brighter color, so that when we draw the field of view to the screen, the player can easily differentiate between what’s in view and what’s not. As usual, feel free to play with the color schemes to match whatever you might have in mind.

With all that in place, we need to modify the way `GameMap` draws itself to the screen.

Diff Original

```diff
class GameMap:
    ...

    def render(self, console: Console) -> None:
-       console.tiles_rgb[0:self.width, 0:self.height] = self.tiles["dark"]
+       """
+       Renders the map.
+
+       If a tile is in the "visible" array, then draw it with the "light" colors.
+       If it isn't, but it's in the "explored" array, then draw it with the "dark" colors.
+       Otherwise, the default is "SHROUD".
+       """
+       console.tiles_rgb[0:self.width, 0:self.height] = np.select(
+           condlist=[self.visible, self.explored],
+           choicelist=[self.tiles["light"], self.tiles["dark"]],
+           default=tile_types.SHROUD
+       )
```

The first part of the statement, `console.tiles_rgb[0:self.width, 0:self.height]`, hasn’t changed. But instead of just setting it to `self.tiles["dark"]`, we’re using `np.select`.

`np.select` allows us to conditionally draw the tiles we want, based on what’s specified in `condlist`. Since we’re passing `[self.visible, self.explored]`, it will check if the tile being drawn is either visible, then explored. If it’s visible, it uses the first value in `choicelist`, in this case, `self.tiles["light"]`. If it’s not visible, but explored, then we draw `self.tiles["dark"]`. If neither is true, we use the `default` argument, which is just the `SHROUD` we defined earlier.

If you run the project now, none of the tiles will be drawn to the screen. This is because we need a way to actually modify the `visible` and `explored` tiles. Let’s modify `Engine` to do just that:

Diff Original

```diff
...
from tcod.context import Context
from tcod.console import Console
+from tcod.map import compute_fov

from entity import Entity
...

class Engine:
    def __init__(self, entities: Set[Entity], event_handler: EventHandler, game_map: GameMap, player: Entity):
        self.entities = entities
        self.event_handler = event_handler
        self.game_map = game_map
        self.player = player
+       self.update_fov()

    def handle_events(self, events: Iterable[Any]) -> None:
        for event in events:
            action = self.event_handler.dispatch(event)

            if action is None:
                continue

            action.perform(self, self.player)

+           self.update_fov()  # Update the FOV before the players next action.

+   def update_fov(self) -> None:
+       """Recompute the visible area based on the players point of view."""
+       self.game_map.visible[:] = compute_fov(
+           self.game_map.tiles["transparent"],
+           (self.player.x, self.player.y),
+           radius=8,
+       )
+       # If a tile is "visible" it should be added to "explored".
+       self.game_map.explored |= self.game_map.visible

    def render(self, console: Console, context: Context) -> None:
        self.game_map.render(console)

        for entity in self.entities:
-           console.print(entity.x, entity.y, entity.char, fg=entity.color)
+           # Only print entities that are in the FOV
+           if self.game_map.visible[entity.x, entity.y]:
+               console.print(entity.x, entity.y, entity.char, fg=entity.color)

        context.present(console)

        console.clear()
```

The most important part of our additions is the `update_fov` function.

```python
    def update_fov(self) -> None:
        """Recompute the visible area based on the players point of view."""
        self.game_map.visible[:] = compute_fov(
            self.game_map.tiles["transparent"],
            (self.player.x, self.player.y),
            radius=8,
        )
        # If a tile is "visible" it should be added to "explored".
        self.game_map.explored |= self.game_map.visible
```

We’re setting the `game_map`’s `visible` tiles to equal the result of the `compute_fov`. We’re giving `compute_fov` three arguments, which it uses to compute our field of view.

- `transparency`: This is the first argument, which we’re passing `self.game_map.tiles["transparent"]`. `transparency` takes a 2D numpy array, and considers any non-zero values to be transparent. This is the array it uses to calculate the field of view.
- `pov`: The origin point for the field of view, which is a 2D index. We use the player’s x and y position here.
- `radius`: How far the FOV extends.

There’s more that this function can do, including not lighting up walls, and using different algorithms to calculate the FOV. If you’re interested, you can find the documentation [here](https://python-tcod.readthedocs.io/en/latest/tcod/map.html#tcod.map.compute_fov).

The line `self.game_map.explored |= self.game_map.visible` sets the `explored` array to include everything in the `visible` array, plus whatever it already had. This means that any tile the player can see, the player has also “explored.”

That’s all we need to do to update our field of view. Notice that we call the function when we initialize the `Engine` class, so that the field of view is created before the player can move, and after handling an action, so that whenever the player does move, the field of view will be updated.

Lastly, we modify the part that draws the entities, so that only entities in the field of view are drawn.

Run the project now, and you’ll see something like this:

![Part 4 - FOV](https://rogueliketutorials.com/images/part-4-fov.png "Field of View")

It’s hard to believe, but that’s all we need to do for a functioning field of view!

This chapter was a shorter one, but we’ve accomplished quite a lot. Our dungeon feels a lot more mysterious, and in coming chapters, it will get a lot more dangerous.

If you want to see the code so far in its entirety, [click here](https://github.com/TStand90/tcod_tutorial_v2/tree/2020/part-4).

[Click here to move on to the next part of this tutorial.](https://rogueliketutorials.com/tutorials/tcod/v2/part-5)

---
# [Part 5 - Placing Enemies and kicking them (harmlessly)](https://rogueliketutorials.com/tutorials/tcod/v2/part-5/)

What good is a dungeon with no monsters to bash? This chapter will focus on placing the enemies throughout the dungeon, and setting them up to be attacked (the actual attacking part we’ll save for next time).

When we’re building our dungeon, we’ll need to place the enemies in the rooms. In order to do that, we will need to make a change to the way `entities` are stored in our game. Currently, they’re saved in the `Engine` class. However, for the sake of placing enemies in the dungeon, and when we get to the part where we move between dungeon floors, it will be better to store them in the `GameMap` class. That way, the map has access to the entities directly, and we can preserve which entities are on which floors fairly easily.

Start by modifying `GameMap`:

Diff Original

```diff
+from __future__ import annotations

+from typing import Iterable, TYPE_CHECKING

import numpy as np  # type: ignore
from tcod.console import Console

import tile_types

+if TYPE_CHECKING:
+   from entity import Entity


class GameMap:
-   def __init__(self, width: int, height: int):
+   def __init__(self, width: int, height: int, entities: Iterable[Entity] = ()):
        self.width, self.height = width, height
+       self.entities = set(entities)
        self.tiles = np.full((width, height), fill_value=tile_types.wall, order="F")
```

Then, let’s modify `Engine` to remove the `entities` from it:

Diff Original

```diff
-from typing import Set, Iterable, Any
+from typing import Iterable, Any


class Engine:
-   def __init__(self, entities: Set[Entity], event_handler: EventHandler, game_map: GameMap, player: Entity):
+   def __init__(self, event_handler: EventHandler, game_map: GameMap, player: Entity):
-       self.entities = entities
        self.event_handler = event_handler
        self.game_map = game_map
        self.player = player
        self.update_fov()
```

Because we’ve modified the definition of `Engine.__init__`, we need to modify `main.py` where we create our `game_map` variable. We might as well remove that `npc` as well, since we won’t be needing it anymore.

Diff Original

```diff
    ...
    player = Entity(int(screen_width / 2), int(screen_height / 2), "@", (255, 255, 255))
-   npc = Entity(int(screen_width / 2 - 5), int(screen_height / 2), "@", (255, 255, 0))
-   entities = {npc, player}

    game_map = generate_dungeon(
        max_rooms=max_rooms,
        room_min_size=room_min_size,
        room_max_size=room_max_size,
        map_width=map_width,
        map_height=map_height,
        player=player,
    )

-   engine = Engine(entities=entities, event_handler=event_handler, game_map=game_map, player=player)
+   engine = Engine(event_handler=event_handler, game_map=game_map, player=player)

    with tcod.context.new_terminal(
        ...
```

We can remove the part in `Engine.render` that loops through the entities and renders the ones that are visible. That part will also be handled by the `GameMap` from now on.

Diff Original

```diff
class Engine:
    ...

    def render(self, console: Console, context: Context) -> None:
        self.game_map.render(console)

-       for entity in self.entities:
-           # Only print entities that are in the FOV
-           if self.game_map.visible[entity.x, entity.y]:
-               console.print(entity.x, entity.y, entity.char, fg=entity.color)
```

We can move this block into `GameMap.render`, though take note that the line that checks for visibility has a slight change: it goes from:

`if self.game_map.visible[entity.x, entity.y]:`

To:

`if self.visible[entity.x, entity.y]:`.

Diff Original

```diff
class GameMap:
    ...

    def render(self, console: Console) -> None:
        """
        Renders the map.

        If a tile is in the "visible" array, then draw it with the "light" colors.
        If it isn't, but it's in the "explored" array, then draw it with the "dark" colors.
        Otherwise, the default is "SHROUD".
        """
        console.tiles_rgb[0:self.width, 0:self.height] = np.select(
            condlist=[self.visible, self.explored],
            choicelist=[self.tiles["light"], self.tiles["dark"]],
            default=tile_types.SHROUD
        )

+       for entity in self.entities:
+           # Only print entities that are in the FOV
+           if self.visible[entity.x, entity.y]:
+               console.print(x=entity.x, y=entity.y, string=entity.char, fg=entity.color)
```

Finally, we need to alter the part in `generate_dungeon` that creates the instance of `GameMap`, so that the `player` is passed into the `entities` argument.

Diff Original

```diff
def generate_dungeon(
    max_rooms: int,
    room_min_size: int,
    room_max_size: int,
    map_width: int,
    map_height: int,
    player: Entity,
) -> GameMap:
    """Generate a new dungeon map."""
-   dungeon = GameMap(map_width, map_height)
+   dungeon = GameMap(map_width, map_height, entities=[player])

    rooms: List[RectangularRoom] = []
    ...
```

If you run the project now, things should look the same as before, minus the NPC that we had earlier for testing.

Now, moving on to actually placing monsters in our dungeon. Our logic will be simple enough: For each room that’s created in our dungeon, we’ll place a random number of enemies, between 0 and a maximum (2 for now). We’ll make it so that there’s an 80% chance of spawning an Orc (a weaker enemy) and a 20% chance of it being a Troll (a stronger enemy).

In order to specify the maximum number of monsters that can be spawned into a room, let’s create a new variable, `max_monsters_per_room`, and place it in `main.py`. We’ll also modify our call to `generate_dungeon` to pass this new variable in.

Diff Original

```diff
    ...
    max_rooms = 30

+   max_monsters_per_room = 2

    tileset = tcod.tileset.load_tilesheet(
        "dejavu10x10_gs_tc.png", 32, 8, tcod.tileset.CHARMAP_TCOD
    )

    event_handler = EventHandler()

    player = Entity(int(screen_width / 2), int(screen_height / 2), "@", (255, 255, 255))

    game_map = generate_dungeon(
        max_rooms=max_rooms,
        room_min_size=room_min_size,
        room_max_size=room_max_size,
        map_width=map_width,
        map_height=map_height,
+       max_monsters_per_room=max_monsters_per_room,
        player=player
    )

    engine = Engine(event_handler=event_handler, game_map=game_map, player=player)
    ...
```

Pretty straightforward. Now we’ll need to modify the definition of `generate_dungeon` to take this new variable, like this:

Diff Original

```diff
def generate_dungeon(
    max_rooms: int,
    room_min_size: int,
    room_max_size: int,
    map_width: int,
    map_height: int,
+   max_monsters_per_room: int,
    player: Entity,
) -> GameMap:
    """Generate a new dungeon map."""
    dungeon = GameMap(map_width, map_height, entities=[player])
```

Easy enough, but now how do we actually place the enemies?

After we’ve created our room, we’ll want to call a function to put the entities in their places. Let’s call the function `place_entities`, and it will take three arguments: The `RectangularRoom` that we’ve created, the `dungeon` so that it can add the entities to it (remember that `dungeon` is an instance of `GameMap`, which now holds entities), and the `max_monsters_per_room`, so that we know how many monsters to make.

While we haven’t written the function yet, let’s place our call to it in `generate_dungeon`:

Diff Original

```diff
            ...
                dungeon.tiles[x, y] = tile_types.floor

+       place_entities(new_room, dungeon, max_monsters_per_room)

        # Finally, append the new room to the list.
        rooms.append(new_room)

    return dungeon
```

Now, let’s write the `place_entities` function so that this actually works.

Our first version of `place_entities` won’t actually place the entities. Why not? Because we’ll need to do a few other things to make spawning the entities here work. However, we can at least fill in most of the function, and skip over the part that actually creates the entities for the moment.

Create the function like this:

Diff Original

```diff
class RectangularRoom:
    ...


+def place_entities(
+   room: RectangularRoom, dungeon: GameMap, maximum_monsters: int,
+) -> None:
+   number_of_monsters = random.randint(0, maximum_monsters)

+   for i in range(number_of_monsters):
+       x = random.randint(room.x1 + 1, room.x2 - 1)
+       y = random.randint(room.y1 + 1, room.y2 - 1)

+       if not any(entity.x == x and entity.y == y for entity in dungeon.entities):
+           if random.random() < 0.8:
+               pass  # TODO: Place an Orc here
+           else:
+               pass  # TODO: Place a Troll here


def tunnel_between(
    ...
```

The first line in the function takes a random number between 0 and the provided maximum (2, in this case). From there, it iterates from 0 to the number.

We select a random `x` and `y` to place the entity, and do a quick check to make sure there’s no other entities in that location before dropping the enemy there. This is to ensure we don’t get stacks of enemies.

As described earlier, there should be an 80% chance of there being an Orc, and 20% chance for a Troll. For now, we’re using `pass` to skip over actually putting them down, because that requires a bit more work first.

There’s a few ways we could go about creating the new entities. Assuming that every Orc and Troll we spawn will always have the same attributes as their brethren, we can create initial instances of `orc` and `troll`, then copy those every time we want to create a new one.

Why not just create the entities right here in the function? We could (the 1st version of this tutorial does, in fact), but that’s a bit of a pain to go back and edit. Imagine if you had 100 enemies in your game at some point in the future. Would you rather search for those entity definitions in one file that _only_ exists to define entities, or try finding it in the file that generates our dungeon? Not to mention, what happens if you want to create a new dungeon generator? Are you going to copy over the entity definitions and have them defined in two places?

Let’s modify `Entity` to prepare for this new copying method. Modify `entity.py` like this:

Diff Original

```diff
+from __future__ import annotations

+import copy
-from typing import Tuple
+from typing import Tuple, TypeVar, TYPE_CHECKING

+if TYPE_CHECKING:
+   from game_map import GameMap

+T = TypeVar("T", bound="Entity")


class Entity:
    """
    A generic object to represent players, enemies, items, etc.
    """
-   def __init__(self, x: int, y: int, char: str, color: Tuple[int, int, int]):
+   def __init__(
+       self,
+       x: int = 0,
+       y: int = 0,
+       char: str = "?",
+       color: Tuple[int, int, int] = (255, 255, 255),
+       name: str = "<Unnamed>",
+       blocks_movement: bool = False,
+   ):
        self.x = x
        self.y = y
        self.char = char
        self.color = color
+       self.name = name
+       self.blocks_movement = blocks_movement

+   def spawn(self: T, gamemap: GameMap, x: int, y: int) -> T:
+       """Spawn a copy of this instance at the given location."""
+       clone = copy.deepcopy(self)
+       clone.x = x
+       clone.y = y
+       gamemap.entities.add(clone)
+       return clone

    def move(self, dx: int, dy: int) -> None:
        ...
```

We’ve added two new attributes to `Entity`: `name` and `blocks_movement`. `name` is straightforward: it’s what the Entity is called. `blocks_movement` describes whether or not this `Entity` can be moved over or not. Enemies will have `blocks_movement` set to `True`, while in the future, things like consumable items and equipment will be set to `False`.

Notice that we’ve also provided defaults for each of the attributes in the `__init__` function as well, whereas we were not before. This is because we’ll soon not need to pass `x` and `y` during the initialization. More on that in a second.

The more complex section is the `spawn` method. It takes the `GameMap` instance, along with `x` and `y` for locations. It then creates a `clone` of the instance of `Entity`, and assigns the `x` and `y` variables to it (this is why we don’t need `x` and `y` in the initializer anymore, they’re set here). It then adds the entity to the `gamemap`’s entities, and returns the `clone`.

This new `spawn` method will probably make a lot more sense by putting it to use. To do that, let’s create a new file, called `entity_factories.py`, and fill it with the following contents:

```python
from entity import Entity

player = Entity(char="@", color=(255, 255, 255), name="Player", blocks_movement=True)

orc = Entity(char="o", color=(63, 127, 63), name="Orc", blocks_movement=True)
troll = Entity(char="T", color=(0, 127, 0), name="Troll", blocks_movement=True)
```

This is where we’re defining our entities. `player` should look familiar, and `orc` and `troll` are not all that different, besides their characters and colors.

These are the instances we’ll be cloning to create our new entities. Using these, we can at last fill in our `place_entities` function back in `procgen.py`.

Diff Original

```diff
...
import tcod

+import entity_factories
from game_map import GameMap
...

        ...
            if random.random() < 0.8:
-               pass  # TODO: Place an Orc here
+               entity_factories.orc.spawn(dungeon, x, y)
            else:
-               pass  # TODO: Place a Troll here
+               entity_factories.troll.spawn(dungeon, x, y)
```

Let’s also modify the way we create the `player`:

Diff Original

```diff
#!/usr/bin/env python3
+import copy

import tcod

from engine import Engine
-from entity import Entity
+import entity_factories
from input_handlers import EventHandler
from procgen import generate_dungeon
...

    ...
    event_handler = EventHandler()

-   player = Entity(int(screen_width / 2), int(screen_height / 2), "@", (255, 255, 255))
+   player = copy.deepcopy(entity_factories.player)

    game_map = generate_dungeon(
        ...
```

_Note: We can’t use `player.spawn` here, because `spawn` requires the `GameMap`, which isn’t created until after we create the player._

With that, your dungeon should now be populated with enemies.

![Font File](https://rogueliketutorials.com/images/part-5-monsters.png)

They’re… not exactly intimidating, are they? In fact, they don’t really do much of anything right now. But that’s okay, we’ll work on that.

The first step towards making our monsters scarier is making them stand their ground… literally! The player can currently walk over (or under) the enemies by simply moving into the same space. Let’s fix that, and ensure that when the player tries to move towards an enemy, we attack instead.

To begin, we need to determine if the space the player is trying to move into has an Entity in it. Not just any Entity, however: we’ll check if the Entity has “blocks_movement” set to `True`. If it does, our player can’t move there, and tries to attack instead.

Add the following to the map:

Diff Original

```diff
from __future__ import annotations

-from typing import Iterable, TYPE_CHECKING
+from typing import Iterable, Optional, TYPE_CHECKING

import numpy as np  # type: ignore
from tcod.console import Console

import tile_types

if TYPE_CHECKING:
    from entity import Entity


class GameMap:
    def __init__(self, width: int, height: int, entities: Iterable[Entity] = ()):
        self.width, self.height = width, height
        self.entities = set(entities)
        self.tiles = np.full((width, height), fill_value=tile_types.wall, order="F")

        self.visible = np.full((width, height), fill_value=False, order="F")  # Tiles the player can currently see
        self.explored = np.full((width, height), fill_value=False, order="F")  # Tiles the player has seen before

+   def get_blocking_entity_at_location(self, location_x: int, location_y: int) -> Optional[Entity]:
+       for entity in self.entities:
+           if entity.blocks_movement and entity.x == location_x and entity.y == location_y:
+               return entity

+       return None

    def in_bounds(self, x: int, y: int) -> bool:
        ...
```

This new function iterates through all the `entities`, and if one is found that both blocks movement and occupies the given `location_x` and `location_y` coordinates, it returns that Entity. Otherwise, we return `None` instead.

Where can we check if a tile is occupied or not? And what do we do if it is?

One way to handle all this is to modify our “actions” a bit. Our current `MovementAction` doesn’t take into account what occupies the tile we’re moving into. That’s fine, it doesn’t necessarily need to, but there probably should be an action that does. What if we created an `Action` subclass that could tell what was in the tile, and call either `MovementAction` if it was empty, or some other “attack” action if it wasn’t?

Let’s do a few things. We’ll start by defining a new class, called `ActionWithDirection`, which will actually become the new superclass for `MovementAction`. This new class will take the initializer from `MovementAction`, but won’t implement its own `perform` method. It looks like this:

Diff Original

```diff
...
class EscapeAction(Action):
    def perform(self, engine: Engine, entity: Entity) -> None:
        raise SystemExit()


+class ActionWithDirection(Action):
+   def __init__(self, dx: int, dy: int):
+       super().__init__()

+       self.dx = dx
+       self.dy = dy

+   def perform(self, engine: Engine, entity: Entity) -> None:
+       raise NotImplementedError()


-class MovementAction(Action):
+class MovementAction(ActionWithDirection):
-   def __init__(self, dx: int, dy: int):
-       super().__init__()

-       self.dx = dx
-       self.dy = dy

    def perform(self, engine: Engine, entity: Entity) -> None:
        dest_x = entity.x + self.dx
        dest_y = entity.y + self.dy

        if not engine.game_map.in_bounds(dest_x, dest_y):
            return  # Destination is out of bounds.
        if not engine.game_map.tiles["walkable"][dest_x, dest_y]:
            return  # Destination is blocked by a tile.
+       if engine.game_map.get_blocking_entity_at_location(dest_x, dest_y):
+           return  # Destination is blocked by an entity.

        entity.move(self.dx, self.dy)
```

Notice that we’ve added an extra check in `MovementAction` to ensure we’re not moving into a space with a blocking entity. Theoretically, this bit of code won’t ever trigger, but it’s nice to have it there as a safeguard.

But wait, `MovementAction` still doesn’t do anything differently. So what’s the point? Well, now we can use the new `ActionWithDirection` class to define two more subclasses, which will do what we want.

The first one will be the action we use to actually attack. It looks like this:

Diff Original

```diff
class ActionWithDirection(Action):
    def __init__(self, dx: int, dy: int):
        super().__init__()

        self.dx = dx
        self.dy = dy

    def perform(self, engine: Engine, entity: Entity) -> None:
        raise NotImplementedError()


+class MeleeAction(ActionWithDirection):
+   def perform(self, engine: Engine, entity: Entity) -> None:
+       dest_x = entity.x + self.dx
+       dest_y = entity.y + self.dy
+       target = engine.game_map.get_blocking_entity_at_location(dest_x, dest_y)
+       if not target:
+           return  # No entity to attack.

+       print(f"You kick the {target.name}, much to its annoyance!")


class MovementAction(ActionWithDirection):
    def perform(self, engine: Engine, entity: Entity) -> None:
        dest_x = entity.x + self.dx
        dest_y = entity.y + self.dy

        if not engine.game_map.in_bounds(dest_x, dest_y):
            return  # Destination is out of bounds.
        if not engine.game_map.tiles["walkable"][dest_x, dest_y]:
            return  # Destination is blocked by a tile.
        if engine.game_map.get_blocking_entity_at_location(dest_x, dest_y)
            return  # Destination is blocked by an entity.

        entity.move(self.dx, self.dy)
```

Just like `MovementAction`, `MeleeAction` inherits from `ActionWithDirection`. The `perform` method it implements is what we’ll use to attack… eventually. Right now, we’re just printing out a little message. The actual attacking will have to wait until the next part (this one is getting long as it is).

Still, we’re not actually _using_ `MeleeAction` anywhere, yet. Let’s add one more class, which is what will make the determination on whether our player is moving or attacking:

Diff Original

```diff
class MovementAction(ActionWithDirection):
    def perform(self, engine: Engine, entity: Entity) -> None:
        dest_x = entity.x + self.dx
        dest_y = entity.y + self.dy

        if not engine.game_map.in_bounds(dest_x, dest_y):
            return  # Destination is out of bounds.
        if not engine.game_map.tiles["walkable"][dest_x, dest_y]:
            return  # Destination is blocked by a tile.
        if engine.game_map.get_blocking_entity_at_location(dest_x, dest_y):
            return  # Destination is blocked by an entity.

        entity.move(self.dx, self.dy)


+class BumpAction(ActionWithDirection):
+   def perform(self, engine: Engine, entity: Entity) -> None:
+       dest_x = entity.x + self.dx
+       dest_y = entity.y + self.dy

+       if engine.game_map.get_blocking_entity_at_location(dest_x, dest_y):
+           return MeleeAction(self.dx, self.dy).perform(engine, entity)

+       else:
+           return MovementAction(self.dx, self.dy).perform(engine, entity)
```

This class also inherits from `ActionWithDirection`, but its `perform` method doesn’t actually perform anything, except deciding which class, between `MeleeAction` and `MovementAction` to return. Those classes are what are actually doing the work. `BumpAction` just determines which one is appropriate to call, based on whether there is a blocking entity at the given destination or not. Notice we’re using the function we defined earlier in our map to decide if there’s a valid target or not.

Now that our new actions are in place, we need to modify our `input_handlers.py` file to use `BumpAction` instead of `MovementAction`. It’s a pretty simple change:

Diff Original

```diff
from typing import Optional

import tcod.event

-from actions import Action, EscapeAction, MovementAction
+from actions import Action, BumpAction, EscapeAction


class EventHandler(tcod.event.EventDispatch[Action]):
    def ev_quit(self, event: tcod.event.Quit) -> Optional[Action]:
        raise SystemExit()

    def ev_keydown(self, event: tcod.event.KeyDown) -> Optional[Action]:
        action: Optional[Action] = None

        key = event.sym

        if key == tcod.event.K_UP:
-           action = MovementAction(dx=0, dy=-1)
+           action = BumpAction(dx=0, dy=-1)
        elif key == tcod.event.K_DOWN:
-           action = MovementAction(dx=0, dy=1)
+           action = BumpAction(dx=0, dy=1)
        elif key == tcod.event.K_LEFT:
-           action = MovementAction(dx=-1, dy=0)
+           action = BumpAction(dx=-1, dy=0)
        elif key == tcod.event.K_RIGHT:
-           action = MovementAction(dx=1, dy=0)
+           action = BumpAction(dx=1, dy=0)

        elif key == tcod.event.K_ESCAPE:
            action = EscapeAction()

        # No valid key was pressed
        return action
```

Run the project now. At this point, you shouldn’t be able to move over the enemies, and you should get a message in the terminal, indicating that you’re attacking the enemy (albeit not for any damage).

Before we wrap this part up, let’s set ourselves up to allow for enemy turns as well. They won’t actually be doing anything at the moment, we’ll just get a message in the terminal that indicates something is happening.

Add these small modifications to `engine.py`:

Diff Original

```diff
class Engine:
    def __init__(self, event_handler: EventHandler, game_map: GameMap, player: Entity):
        self.event_handler = event_handler
        self.game_map = game_map
        self.player = player
        self.update_fov()

+   def handle_enemy_turns(self) -> None:
+       for entity in self.game_map.entities - {self.player}:
+           print(f'The {entity.name} wonders when it will get to take a real turn.')

    def handle_events(self, events: Iterable[Any]) -> None:
        for event in events:
            action = self.event_handler.dispatch(event)

            if action is None:
                continue

            action.perform(self, self.player)
+           self.handle_enemy_turns()
            self.update_fov()  # Update the FOV before the players next action.
```

The `handle_enemy_turns` function loops through each entity (minus the player) and prints out a message for them. In the next part, we’ll replace this with some code that will allow those entities to take real turns.

We call `handle_enemy_turns` right after `action.perform`, so that the enemies move right after the player. Other roguelike games have more complex timing mechanisms for when entities take their turns, but our tutorial will stick with probably the simplest method of all: the player moves, then all the enemies move.

That’s all for this chapter. Next time, we’ll look at moving the enemies around on their turns, and doing some real damage to both the enemies and the player.

If you want to see the code so far in its entirety, [click here](https://github.com/TStand90/tcod_tutorial_v2/tree/2020/part-5).

[Click here to move on to the next part of this tutorial.](https://rogueliketutorials.com/tutorials/tcod/v2/part-6)

---
# [Part 6 - Doing (and taking) some damage](https://rogueliketutorials.com/tutorials/tcod/v2/part-6/)

## Check your TCOD installation [](https://rogueliketutorials.com/tutorials/tcod/v2/part-6/#check-your-tcod-installation)

Before proceeding any further, you’ll want to upgrade to TCOD version 11.15, if you don’t already have it. This version of TCOD was released _during_ the tutorial event, so if you’re following along on a weekly basis, you probably _don’t_ have this version installed!

## Refactoring previous code [](https://rogueliketutorials.com/tutorials/tcod/v2/part-6/#refactoring-previous-code)

After parts 1-5 for this tutorial were written, we decided to change a few things around, to hopefully make the codebase a bit cleaner and easier to extend in the future. Unfortunately, this means that code written in previous parts now has to be modified.

I would go back and edit the tutorial text and Github branches to reflect these changes, except for two things:

1. I don’t have time at the moment. Writing the sections that get published every week is taking all of my time as it is.
2. It wouldn’t be fair to those who are following this tutorial on a weekly basis.

Someday, when the event is over, the previous parts will be rewritten, and all will be well. But until then, there’s several changes that need to be made before proceeding with Part 6.

I won’t explain all of the changes (again, time is a limiting factor), but here’s the basic ideas:

- Event handlers will have the `handle_events` method instead of `Engine`.
- The game map will have a reference to `Engine`, and entities will have a reference to the map.
- Actions will be initialized with the entity doing the action
- Because of the above points, Actions will have a reference to the `Engine`, through `Entity`->`GameMap`->`Engine`

Make the changes to each file, and when you’re finished, verify the project works as it did before.

`input_handlers.py`

Diff Original

```diff
+from __future__ import annotations

-from typing import Optional
+from typing import Optional, TYPE_CHECKING

import tcod.event

from actions import Action, BumpAction, EscapeAction

+if TYPE_CHECKING:
+   from engine import Engine


class EventHandler(tcod.event.EventDispatch[Action]):
+   def __init__(self, engine: Engine):
+       self.engine = engine

+   def handle_events(self) -> None:
+       for event in tcod.event.wait():
+           action = self.dispatch(event)

+           if action is None:
+               continue

+           action.perform()

+           self.engine.handle_enemy_turns()
+           self.engine.update_fov()  # Update the FOV before the players next action.


    def ev_quit(self, event: tcod.event.Quit) -> Optional[Action]:
        ...

    def ev_keydown(self, event: tcod.event.KeyDown) -> Optional[Action]:
        action: Optional[Action] = None

        key = event.sym

+       player = self.engine.player

        if key == tcod.event.K_UP:
-           action = BumpAction(dx=0, dy=-1)
+           action = BumpAction(player, dx=0, dy=-1)
        elif key == tcod.event.K_DOWN:
-           action = BumpAction(dx=0, dy=1)
+           action = BumpAction(player, dx=0, dy=1)
        elif key == tcod.event.K_LEFT:
-           action = BumpAction(dx=-1, dy=0)
+           action = BumpAction(player, dx=-1, dy=0)
        elif key == tcod.event.K_RIGHT:
-           action = BumpAction(dx=1, dy=0)
+           action = BumpAction(player, dx=1, dy=0)

        elif key == tcod.event.K_ESCAPE:
-           action = EscapeAction()
+           action = EscapeAction(player)
```

`actions.py`

Diff Original

```diff
from __future__ import annotations

+from typing import Optional, Tuple, TYPE_CHECKING
-from typing import TYPE_CHECKING

if TYPE_CHECKING:
    from engine import Engine
    from entity import Entity


class Action:
+   def __init__(self, entity: Entity) -> None:
+       super().__init__()
+       self.entity = entity

+   @property
+   def engine(self) -> Engine:
+       """Return the engine this action belongs to."""
+       return self.entity.gamemap.engine

+   def perform(self) -> None:
-   def perform(self, engine: Engine, entity: Entity) -> None:
        """Perform this action with the objects needed to determine its scope.

+       `self.engine` is the scope this action is being performed in.
-       `engine` is the scope this action is being performed in.

+       `self.entity` is the object performing the action.
-       `entity` is the object performing the action.

        This method must be overridden by Action subclasses.
        """
        raise NotImplementedError()


class EscapeAction(Action):
+   def perform(self) -> None:
-   def perform(self, engine: Engine, entity: Entity) -> None:
        raise SystemExit()



class ActionWithDirection(Action):
+   def __init__(self, entity: Entity, dx: int, dy: int):
+       super().__init__(entity)
-   def __init__(self, dx: int, dy: int):
-       super().__init__()

        self.dx = dx
        self.dy = dy

+   @property
+   def dest_xy(self) -> Tuple[int, int]:
+       """Returns this actions destination."""
+       return self.entity.x + self.dx, self.entity.y + self.dy

+   @property
+   def blocking_entity(self) -> Optional[Entity]:
+       """Return the blocking entity at this actions destination.."""
+       return self.engine.game_map.get_blocking_entity_at_location(*self.dest_xy)

+   def perform(self) -> None:
-   def perform(self, engine: Engine, entity: Entity) -> None:
        raise NotImplementedError()


class MeleeAction(ActionWithDirection):
+   def perform(self) -> None:
+       target = self.blocking_entity
-   def perform(self, engine: Engine, entity: Entity) -> None:
-       dest_x = entity.x + self.dx
-       dest_y = entity.y + self.dy
-       target = engine.game_map.get_blocking_entity_at_location(dest_x, dest_y)
        if not target:
            return  # No entity to attack.

        print(f"You kick the {target.name}, much to its annoyance!")


class MovementAction(ActionWithDirection):
+   def perform(self) -> None:
+       dest_x, dest_y = self.dest_xy
-   def perform(self, engine: Engine, entity: Entity) -> None:
-       dest_x = entity.x + self.dx
-       dest_y = entity.y + self.dy

+       if not self.engine.game_map.in_bounds(dest_x, dest_y):
-       if not engine.game_map.in_bounds(dest_x, dest_y):
            return  # Destination is out of bounds.
+       if not self.engine.game_map.tiles["walkable"][dest_x, dest_y]:
-       if not engine.game_map.tiles["walkable"][dest_x, dest_y]:
            return  # Destination is blocked by a tile.
+       if self.engine.game_map.get_blocking_entity_at_location(dest_x, dest_y):
-       if engine.game_map.get_blocking_entity_at_location(dest_x, dest_y):
            return  # Destination is blocked by an entity.

+       self.entity.move(self.dx, self.dy)
-       entity.move(self.dx, self.dy)


class BumpAction(ActionWithDirection):
+   def perform(self) -> None:
+       if self.blocking_entity:
+           return MeleeAction(self.entity, self.dx, self.dy).perform()
-   def perform(self, engine: Engine, entity: Entity) -> None:
-       dest_x = entity.x + self.dx
-       dest_y = entity.y + self.dy

-       if engine.game_map.get_blocking_entity_at_location(dest_x, dest_y):
-           return MeleeAction(self.dx, self.dy).perform(engine, entity)

        else:
+           return MovementAction(self.entity, self.dx, self.dy).perform()
-           return MovementAction(self.dx, self.dy).perform(engine, entity)
```

`game_map.py`

Diff Original

```diff
from __future__ import annotations

from typing import Iterable, Optional, TYPE_CHECKING

import numpy as np  # type: ignore
from tcod.console import Console

import tile_types

if TYPE_CHECKING:
+   from engine import Engine
    from entity import Entity


class GameMap:
-   def __init__(self, width: int, height: int, entities: Iterable[Entity] = ()):
+   def __init__(
+       self, engine: Engine, width: int, height: int, entities: Iterable[Entity] = ()
+   ):
+       self.engine = engine
        self.width, self.height = width, height
        self.entities = set(entities)
        self.tiles = np.full((width, height), fill_value=tile_types.wall, order="F")

-       self.visible = np.full((width, height), fill_value=False, order="F")  # Tiles the player can currently see
+       self.visible = np.full(
+           (width, height), fill_value=False, order="F"
+       )  # Tiles the player can currently see
-       self.explored = np.full((width, height), fill_value=False, order="F")  # Tiles the player has seen before
+       self.explored = np.full(
+           (width, height), fill_value=False, order="F"
+       )  # Tiles the player has seen before

-   def get_blocking_entity_at_location(self, location_x: int, location_y: int) -> Optional[Entity]:
+   def get_blocking_entity_at_location(
+       self, location_x: int, location_y: int,
+   ) -> Optional[Entity]:
        for entity in self.entities:
-           if entity.blocks_movement and entity.x == location_x and entity.y == location_y:
+           if (
+               entity.blocks_movement
+               and entity.x == location_x
+               and entity.y == location_y
+           ):
                return entity

        return None

    def in_bounds(self, x: int, y: int) -> bool:
        """Return True if x and y are inside of the bounds of this map."""
        return 0 <= x < self.width and 0 <= y < self.height

    def render(self, console: Console) -> None:
        """
        Renders the map.

        If a tile is in the "visible" array, then draw it with the "light" colors.
        If it isn't, but it's in the "explored" array, then draw it with the "dark" colors.
        Otherwise, the default is "SHROUD".
        """
-       console.tiles_rgb[0:self.width, 0:self.height] = np.select(
+       console.tiles_rgb[0 : self.width, 0 : self.height] = np.select(
            condlist=[self.visible, self.explored],
            choicelist=[self.tiles["light"], self.tiles["dark"]],
-           default=tile_types.SHROUD
+           default=tile_types.SHROUD,
        )
```

`main.py`

Diff Original

```diff
#!/usr/bin/env python3
import copy

import tcod

from engine import Engine
import entity_factories
-from input_handlers import EventHandler
from procgen import generate_dungeon

    ...
+   player = copy.deepcopy(entity_factories.player)
-   event_handler = EventHandler()

+   engine = Engine(player=player)
-   player = copy.deepcopy(entity_factories.player)

+   engine.game_map = generate_dungeon(
-   game_map = generate_dungeon(
        max_rooms=max_rooms,
        room_min_size=room_min_size,
        room_max_size=room_max_size,
        map_width=map_width,
        map_height=map_height,
        max_monsters_per_room=max_monsters_per_room,
+       engine=engine,
-       player=player,
    )
+   engine.update_fov()

-   engine = Engine(event_handler=event_handler, game_map=game_map, player=player)

    with tcod.context.new_terminal(
        ...
        while True:
            engine.render(console=root_console, context=context)

+           engine.event_handler.handle_events()
-           events = tcod.event.wait()

-           engine.handle_events(events)


if __name__ == "__main__":
    main()
```

`entity.py`:

Diff Original

```diff
from __future__ import annotations

import copy
-from typing import Tuple, TypeVar, TYPE_CHECKING
+from typing import Optional, Tuple, TypeVar, TYPE_CHECKING

if TYPE_CHECKING:
    from game_map import GameMap

T = TypeVar("T", bound="Entity")


class Entity:
    """
    A generic object to represent players, enemies, items, etc.
    """

+   gamemap: GameMap

    def __init__(
        self,
+       gamemap: Optional[GameMap] = None,
        x: int = 0,
        y: int = 0,
        char: str = "?",
        color: Tuple[int, int, int] = (255, 255, 255),
        name: str = "<Unnamed>",
        blocks_movement: bool = False,
    ):
        self.x = x
        self.y = y
        self.char = char
        self.color = color
        self.name = name
        self.blocks_movement = blocks_movement
+       if gamemap:
+           # If gamemap isn't provided now then it will be set later.
+           self.gamemap = gamemap
+           gamemap.entities.add(self)

    def spawn(self: T, gamemap: GameMap, x: int, y: int) -> T:
        """Spawn a copy of this instance at the given location."""
        clone = copy.deepcopy(self)
        clone.x = x
        clone.y = y
+       clone.gamemap = gamemap
        gamemap.entities.add(clone)
        return clone

+   def place(self, x: int, y: int, gamemap: Optional[GameMap] = None) -> None:
+       """Place this entity at a new location.  Handles moving across GameMaps."""
+       self.x = x
+       self.y = y
+       if gamemap:
+           if hasattr(self, "gamemap"):  # Possibly uninitialized.
+               self.gamemap.entities.remove(self)
+           self.gamemap = gamemap
+           gamemap.entities.add(self)
```

`procgen.py`:

Diff Original

```diff
import tile_types


if TYPE_CHECKING:
+   from engine import Engine
-   from entity import Entity

...
def generate_dungeon(
    max_rooms: int,
    room_min_size: int,
    room_max_size: int,
    map_width: int,
    map_height: int,
    max_monsters_per_room: int,
+   engine: Engine,
-   player: Entity,
) -> GameMap:
    """Generate a new dungeon map."""
+   player = engine.player
+   dungeon = GameMap(engine, map_width, map_height, entities=[player])
-   dungeon = GameMap(map_width, map_height, entities=[player])

    rooms: List[RectangularRoom] = []
    ...

        ...
        if len(rooms) == 0:
            # The first room, where the player starts.
+           player.place(*new_room.center, dungeon)
-           player.x, player.y = new_room.center
        else:  # All rooms after the first.
            ...
```

`engine.py`:

Diff Original

```diff
+from __future__ import annotations

+from typing import TYPE_CHECKING
-from typing import Iterable, Any

from tcod.context import Context
from tcod.console import Console
from tcod.map import compute_fov

-from entity import Entity
-from game_map import GameMap
from input_handlers import EventHandler

+if TYPE_CHECKING:
+   from entity import Entity
+   from game_map import GameMap


class Engine:
+   game_map: GameMap

+   def __init__(self, player: Entity):
+       self.event_handler: EventHandler = EventHandler(self)
+       self.player = player
-   def __init__(self, event_handler: EventHandler, game_map: GameMap, player: Entity):
-       self.event_handler = event_handler
-       self.game_map = game_map
-       self.player = player
-       self.update_fov()

    def handle_enemy_turns(self) -> None:
        for entity in self.game_map.entities - {self.player}:
            print(f'The {entity.name} wonders when it will get to take a real turn.')

-   def handle_events(self, events: Iterable[Any]) -> None:
-       for event in events:
-           action = self.event_handler.dispatch(event)

-           if action is None:
-               continue

-           action.perform(self, self.player)
-           self.handle_enemy_turns()
-           self.update_fov()  # Update the FOV before the players next action.

    def update_fov(self) -> None:
        ...
```

## Onwards to Part 6 [](https://rogueliketutorials.com/tutorials/tcod/v2/part-6/#onwards-to-part-6)

The last part of this tutorial set us up for combat, so now it’s time to actually implement it.

In order to make “killable” Entities, rather than attaching hit points to each Entity we create, we’ll create a **component**, called `Fighter`, which will hold information related to combat, like HP, max HP, attack, and defense. If an Entity can fight, it will have this component attached to it, and if not, it won’t. This way of doing things is called **composition**, and it’s an alternative to your typical inheritance-based programming model. (This tutorial uses both composition _and_ inheritance).

Create a new Python package (a folder with an empty __init__.py file), called `components`. In that new directory, add two new files, one called `base_component.py`, and another called `fighter.py`. The `Fighter` class in `fighter.py` will inherit from the class we put in `base_component.py`, so let’s start with that one:

```python
from __future__ import annotations

from typing import TYPE_CHECKING

if TYPE_CHECKING:
    from engine import Engine
    from entity import Entity


class BaseComponent:
    entity: Entity  # Owning entity instance.

    @property
    def engine(self) -> Engine:
        return self.entity.gamemap.engine
```

With that, let’s now open up `fighter.py` and put the following into it:

```python
from components.base_component import BaseComponent


class Fighter(BaseComponent):
    def __init__(self, hp: int, defense: int, power: int):
        self.max_hp = hp
        self._hp = hp
        self.defense = defense
        self.power = power

    @property
    def hp(self) -> int:
        return self._hp

    @hp.setter
    def hp(self, value: int) -> None:
        self._hp = max(0, min(value, self.max_hp))
```

We import and inherit from `BaseComponent`, which gives us access to the parent entity and the engine, which will be useful later on.

The `__init__` function takes a few arguments. `hp` represents the entity’s hit points. `defense` is how much taken damage will be reduced. `power` is the entity’s raw attack power.

What’s with the `hp` property? We define both a getter and setter, which will allow the class to access `hp` like a normal variable. The getter (the one with the `@property` thing above the method) doesn’t do anything special: it just returns the HP. The `setter` (`@hp.setter`) is where things get more interesting.

By defining HP this way, we can modify the value as it’s set within the method. This line:

```python
        self._hp = max(0, min(value, self.max_hp))
```

Means that `_hp` (which we access through `hp`) will never be set to less than 0, but also won’t ever go higher than the `max_hp` attribute.

So that’s our `Fighter` component. It won’t do us much good at the moment, because the entities in our game still don’t move or do much of anything (besides the player, anyway). To give some life to our entities, we can add another component, which, when attached to our entities, will allow them to take turns and move around.

Create a file in the `components` directory called `ai.py`, and put the following contents into it:

```python
from __future__ import annotations

from typing import List, Tuple

import numpy as np  # type: ignore
import tcod

from actions import Action
from components.base_component import BaseComponent


class BaseAI(Action, BaseComponent):
    def perform(self) -> None:
        raise NotImplementedError()

    def get_path_to(self, dest_x: int, dest_y: int) -> List[Tuple[int, int]]:
        """Compute and return a path to the target position.

        If there is no valid path then returns an empty list.
        """
        # Copy the walkable array.
        cost = np.array(self.entity.gamemap.tiles["walkable"], dtype=np.int8)

        for entity in self.entity.gamemap.entities:
            # Check that an enitiy blocks movement and the cost isn't zero (blocking.)
            if entity.blocks_movement and cost[entity.x, entity.y]:
                # Add to the cost of a blocked position.
                # A lower number means more enemies will crowd behind each other in
                # hallways.  A higher number means enemies will take longer paths in
                # order to surround the player.
                cost[entity.x, entity.y] += 10

        # Create a graph from the cost array and pass that graph to a new pathfinder.
        graph = tcod.path.SimpleGraph(cost=cost, cardinal=2, diagonal=3)
        pathfinder = tcod.path.Pathfinder(graph)

        pathfinder.add_root((self.entity.x, self.entity.y))  # Start position.

        # Compute the path to the destination and remove the starting point.
        path: List[List[int]] = pathfinder.path_to((dest_x, dest_y))[1:].tolist()

        # Convert from List[List[int]] to List[Tuple[int, int]].
        return [(index[0], index[1]) for index in path]
```

`BaseAI` doesn’t implement a `perform` method, since the entities which will be using AI to act will have to have an AI class that inherits from this one.

`get_path_to` uses the “walkable” tiles in our map, along with some TCOD pathfinding tools to get the path from the `BaseAI`’s parent entity to whatever their target might be. In the case of this tutorial, the target will always be the player, though you could theoretically write a monster that cares more about food or treasure than attacking the player.

The pathfinder first builds an array of `cost`, which is how “costly” (time consuming) it will take to get to the target. If a piece of terrain takes longer to traverse, its cost will be higher. In the case of our simple game, all parts of the map have the same cost, but what this cost array allows us to do is take other entities into account.

How? Well, if an entity exists at a spot on the map, we increase the cost of moving there to “10”. What this does is encourages the entity to move around the entity that’s blocking them from their target. Higher values will cause the entity to take a longer path around; shorter values will cause groups to gather into crowds, since they don’t want to move around.

More information about TCOD’s pathfinding can be [found here](https://python-tcod.readthedocs.io/en/latest/tcod/path.html).

To make use of our new `Fighter` and `AI` components, we could attach them directly onto the `Entity` class. However, it might be useful to differentiate between entities that can act, and those that can’t. Right now, our game only consists of acting entities, but soon enough, we’ll be adding things like consumable items and, eventually, equipment, which won’t be able to take turns or take damage.

One way to handle this is to create a new subclass of `Entity`, called `Actor`, and give it all the same attributes as `Entity`, plus the `ai` and `fighter` components it will need. Modify `entity.py` like this:

Diff Original

```diff
from __future__ import annotations

import copy
-from typing import Tuple, TypeVar, TYPE_CHECKING
+from typing import Optional, Tuple, Type, TypeVar, TYPE_CHECKING


if TYPE_CHECKING:
+   from components.ai import BaseAI
+   from components.fighter import Fighter
    from game_map import GameMap

T = TypeVar("T", bound="Entity")


class Entity:
    ...


+class Actor(Entity):
+   def __init__(
+       self,
+       *,
+       x: int = 0,
+       y: int = 0,
+       char: str = "?",
+       color: Tuple[int, int, int] = (255, 255, 255),
+       name: str = "<Unnamed>",
+       ai_cls: Type[BaseAI],
+       fighter: Fighter
+   ):
+       super().__init__(
+           x=x,
+           y=y,
+           char=char,
+           color=color,
+           name=name,
+           blocks_movement=True,
+       )

+       self.ai: Optional[BaseAI] = ai_cls(self)

+       self.fighter = fighter
+       self.fighter.entity = self

+   @property
+   def is_alive(self) -> bool:
+       """Returns True as long as this actor can perform actions."""
+       return bool(self.ai)
```

The first thing our `Actor` class does in its `__init__()` function is call its superclass’s `__init__()`, which in this case, is the `Entity` class. We’re passing `blocks_movement` as `True` every time, because we can assume that all the “actors” will block movement.

Besides calling the `Entity.__init__()`, we also set the two components for the `Actor` class: `ai` and `fighter`. The idea is that each actor will need two things to function: the ability to move around and make decisions, and the ability to take (and receive) damage.

This new `Actor` class isn’t quite enough to get our enemies up and moving around, but we’re getting there. We actually need to revisit `ai.py`, and add a new class there to handle hostile enemies. Enter the following changes in `ai.py`:

Diff Original

```diff
from __future__ import annotations

-from typing import List, Tuple
+from typing import List, Tuple, TYPE_CHECKING

import numpy as np  # type: ignore
import tcod

-from actions import Action
+from actions import Action, MeleeAction, MovementAction, WaitAction
from components.base_component import BaseComponent

+if TYPE_CHECKING:
+   from entity import Actor


class BaseAI(Action, BaseComponent):
+   entity: Actor

    def perform(self) -> None:
        ...


+class HostileEnemy(BaseAI):
+   def __init__(self, entity: Actor):
+       super().__init__(entity)
+       self.path: List[Tuple[int, int]] = []

+   def perform(self) -> None:
+       target = self.engine.player
+       dx = target.x - self.entity.x
+       dy = target.y - self.entity.y
+       distance = max(abs(dx), abs(dy))  # Chebyshev distance.

+       if self.engine.game_map.visible[self.entity.x, self.entity.y]:
+           if distance <= 1:
+               return MeleeAction(self.entity, dx, dy).perform()

+           self.path = self.get_path_to(target.x, target.y)

+       if self.path:
+           dest_x, dest_y = self.path.pop(0)
+           return MovementAction(
+               self.entity, dest_x - self.entity.x, dest_y - self.entity.y,
+           ).perform()

+       return WaitAction(self.entity).perform()
```

`HostileEnemy` is the AI class we’ll use for our enemies. It defines the `perform` method, which does the following:

- If the entity is not in the player’s vision, simply wait.
- If the player is right next to the entity (`distance <= 1`), attack the player.
- If the player can see the entity, but the entity is too far away to attack, then move towards the player.

The last line actually calls an action that we haven’t defined yet: `WaitAction`. This action will be used when the player or an enemy decides to wait where they are rather than taking a turn.

Implement `WaitAction` by opening `actions.py`:

Diff Original

```diff
class EscapeAction(Action):
    ...


+class WaitAction(Action):
+   def perform(self) -> None:
+       pass


class ActionWithDirection(Action):
    ...
```

As you can see, `WaitAction` does… well, nothing. And that’s what we want it to do, as it represents an actor saying “I’ll do nothing this turn.”

With all that in place, we’ll need to refactor our `entity_factories.py` file to make use of the new `Actor` class, as well as its components. Modify `entity_factories.py` to look like this:

Diff Original

```diff
+from components.ai import HostileEnemy
+from components.fighter import Fighter
+from entity import Actor
-from entity import Entity

+player = Actor(
+   char="@",
+   color=(255, 255, 255),
+   name="Player",
+   ai_cls=HostileEnemy,
+   fighter=Fighter(hp=30, defense=2, power=5),
+)
-player = Entity(char="@", color=(255, 255, 255), name="Player", blocks_movement=True)

+orc = Actor(
+   char="o",
+   color=(63, 127, 63),
+   name="Orc",
+   ai_cls=HostileEnemy,
+   fighter=Fighter(hp=10, defense=0, power=3),
+)
+troll = Actor(
+   char="T",
+   color=(0, 127, 0),
+   name="Troll",
+   ai_cls=HostileEnemy,
+   fighter=Fighter(hp=16, defense=1, power=4),
+)
-orc = Entity(char="o", color=(63, 127, 63), name="Orc", blocks_movement=True)
-troll = Entity(char="T", color=(0, 127, 0), name="Troll", blocks_movement=True)
```

We’ve changed each entity to make use of the `Actor` class, and used the `HostileEnemy` AI class for the Orc and the Troll types. The player doesn’t use the AI, so the AI given to it doesn’t matter other than that an AI must be specified for all `Actor`’s. Also, we defined the `Fighter` component for each, giving a few different values to make the Trolls stronger than the Orcs. Feel free to modify these values to your liking.

How do enemies actually take their turns, though? It’s actually pretty simple: rather than printing the message we were before, we just check if the entity has an AI, and if it does, we call the `perform` method from that AI component. Modify `engine.py` to do this:

Diff Original

```diff
    def handle_enemy_turns(self) -> None:
+       for entity in set(self.game_map.actors) - {self.player}:
+           if entity.ai:
+               entity.ai.perform()
-       for entity in self.game_map.entities - {self.player}:
-           print(f'The {entity.name} wonders when it will get to take a real turn.')
```

But wait, `game_map.actors` isn’t defined. What should it do, though? Same thing as `game_map.entities`, except it should return only the `Actor` entities.

Let’s add this method to `GameMap`:

Diff Original

```diff
from __future__ import annotations

-from typing import Iterable, Optional, TYPE_CHECKING
+from typing import Iterable, Iterator, Optional, TYPE_CHECKING

import numpy as np  # type: ignore
from tcod.console import Console

+from entity import Actor
import tile_types

if TYPE_CHECKING:
    from engine import Engine
    from entity import Entity

class GameMap:
    def __init__(
        ...

+   @property
+   def actors(self) -> Iterator[Actor]:
+       """Iterate over this maps living actors."""
+       yield from (
+           entity
+           for entity in self.entities
+           if isinstance(entity, Actor) and entity.is_alive
+       )

    def get_blocking_entity_at_location(
        ...

+   def get_actor_at_location(self, x: int, y: int) -> Optional[Actor]:
+       for actor in self.actors:
+           if actor.x == x and actor.y == y:
+               return actor

+       return None
```

Our `actors` property will return all the `Actor` entities in the map, but only those that are currently “alive”.

We’ve also went ahead and added a `get_actor_at_location`, which, as the name implies, acts similarly to `get_blocking_entity_at_location`, but returns only an `Actor`. This will come in handy later on.

Run the project now, and the enemies should chase you around! They can’t really attack just yet, but we’re getting there.

![Part 6 - The Chase](https://rogueliketutorials.com/images/part-6-chase.png)

One thing you might have noticed is that we’re letting our enemies move and attack in diagonal directions, but our player can only move in the four cardinal directions (up, down, left, right). We can fix that by adjusting `input_handlers.py`. While we’re at it, we might want to define a more flexible way of defining the movement keys rather than the `if...elif` structure we’ve used so far. While that does work, it gets a bit clunky after more than just a few options. We can fix this by modifying `input_handlers.py` like this:

Diff Original

```diff
from __future__ import annotations

from typing import Optional, TYPE_CHECKING

import tcod.event

-from actions import Action, BumpAction, EscapeAction
+from actions import Action, BumpAction, EscapeAction, WaitAction

if TYPE_CHECKING:
    from engine import Engine


+MOVE_KEYS = {
+   # Arrow keys.
+   tcod.event.K_UP: (0, -1),
+   tcod.event.K_DOWN: (0, 1),
+   tcod.event.K_LEFT: (-1, 0),
+   tcod.event.K_RIGHT: (1, 0),
+   tcod.event.K_HOME: (-1, -1),
+   tcod.event.K_END: (-1, 1),
+   tcod.event.K_PAGEUP: (1, -1),
+   tcod.event.K_PAGEDOWN: (1, 1),
+   # Numpad keys.
+   tcod.event.K_KP_1: (-1, 1),
+   tcod.event.K_KP_2: (0, 1),
+   tcod.event.K_KP_3: (1, 1),
+   tcod.event.K_KP_4: (-1, 0),
+   tcod.event.K_KP_6: (1, 0),
+   tcod.event.K_KP_7: (-1, -1),
+   tcod.event.K_KP_8: (0, -1),
+   tcod.event.K_KP_9: (1, -1),
+   # Vi keys.
+   tcod.event.K_h: (-1, 0),
+   tcod.event.K_j: (0, 1),
+   tcod.event.K_k: (0, -1),
+   tcod.event.K_l: (1, 0),
+   tcod.event.K_y: (-1, -1),
+   tcod.event.K_u: (1, -1),
+   tcod.event.K_b: (-1, 1),
+   tcod.event.K_n: (1, 1),
+}

+WAIT_KEYS = {
+   tcod.event.K_PERIOD,
+   tcod.event.K_KP_5,
+   tcod.event.K_CLEAR,
+}


        ...

-       if key == tcod.event.K_UP:
-           action = BumpAction(dx=0, dy=-1)
-       elif key == tcod.event.K_DOWN:
-           action = BumpAction(dx=0, dy=1)
-       elif key == tcod.event.K_LEFT:
-           action = BumpAction(dx=-1, dy=0)
-       elif key == tcod.event.K_RIGHT:
-           action = BumpAction(dx=1, dy=0)
+       if key in MOVE_KEYS:
+           dx, dy = MOVE_KEYS[key]
+           action = BumpAction(player, dx, dy)
+       elif key in WAIT_KEYS:
+           action = WaitAction(player)

        ...
```

The `MOVE_KEYS` dictionary holds various different possibilities for movement. Some roguelikes utilize the numpad for movement, some use “Vi Keys.” Ours will actually use both for the time being. Feel free to change the key scheme if you’re not a fan of it.

Where we used to do `if...elif` statements for each direction, we can now just check if the key was part of `MOVE_KEYS`, and if it was, we return the `dx` and `dy` values from the dictionary. This is a lot simpler and cleaner than our previous format.

So now that our enemies can chase us down, it’s time to make them do some real damage.

Open up `actions.py`:

Diff Original

```diff
from __future__ import annotations

from typing import Optional, Tuple, TYPE_CHECKING

if TYPE_CHECKING:
    from engine import Engine
-   from entity import Entity
+   from entity import Actor, Entity


class Action:
-   def __init__(self, entity: Entity) -> None:
+   def __init__(self, entity: Actor) -> None:
        super().__init__()
        self.entity = entity

    ...


class ActionWithDirection(Action):
-   def __init__(self, entity: Entity, dx: int, dy: int):
+   def __init__(self, entity: Actor, dx: int, dy: int):
        super().__init__(entity)

        self.dx = dx
        self.dy = dy

    @property
    def dest_xy(self) -> Tuple[int, int]:
        """Returns this actions destination."""
        return self.entity.x + self.dx, self.entity.y + self.dy

    @property
    def blocking_entity(self) -> Optional[Entity]:
        """Return the blocking entity at this actions destination.."""
        return self.engine.game_map.get_blocking_entity_at_location(*self.dest_xy)

+   @property
+   def target_actor(self) -> Optional[Actor]:
+       """Return the actor at this actions destination."""
+       return self.engine.game_map.get_actor_at_location(*self.dest_xy)

    def perform(self) -> None:
        raise NotImplementedError()


class MeleeAction(ActionWithDirection):
    def perform(self) -> None:
+       target = self.target_actor
-       target = self.blocking_entity
        if not target:
            return  # No entity to attack.

+       damage = self.entity.fighter.power - target.fighter.defense

+       attack_desc = f"{self.entity.name.capitalize()} attacks {target.name}"
+       if damage > 0:
+           print(f"{attack_desc} for {damage} hit points.")
+           target.fighter.hp -= damage
+       else:
+           print(f"{attack_desc} but does no damage.")
-       print(f"You kick the {target.name}, much to its annoyance!")


class MovementAction(ActionWithDirection):
    ...


class BumpAction(ActionWithDirection):
    def perform(self) -> None:
-       if self.blocking_entity:
+       if self.target_actor:
            return MeleeAction(self.entity, self.dx, self.dy).perform()

        else:
            return MovementAction(self.entity, self.dx, self.dy).perform()
```

We’re replacing the type hint for `entity` in `Action` and `ActionWithDirection` with `Actor` instead of `Entity`, since only `Actor`s should be taking actions.

We’ve also added the `target_actor` property to `ActionWithDirection`, which will give us the `Actor` at the destination we’re moving to, if there is one. We utilize that property instead of `blocking_entity` in both `BumpAction` and `MeleeAction`.

Lastly, we modify `MeleeAction` to actually do an attack, instead of just printing a message. We calculate the damage (attacker’s power minus defender’s defense), and assign a description to the attack, based on whether any damage was done or not. If the damage is greater than 0, we subtract it from the defender’s HP.

If you run the project now, you’ll see the print statements indicating that the player and the enemies are doing damage to each other. But since neither side can actually die, combat doesn’t feel all that high stakes just yet.

What do we do when an Entity reaches 0 HP or lower? Well, it should drop dead, obviously! But what should our _code_ do to make this happen? To handle this, we can refer back to our `Fighter` component.

Remember when we created a setter for `hp`? It will come in handy right now, as we can utilize it to automatically “kill” the actor when their HP drops to zero. Add the following to `fighter.py`:

Diff Original

```diff
+from __future__ import annotations

+from typing import TYPE_CHECKING

from components.base_component import BaseComponent

+if TYPE_CHECKING:
+   from entity import Actor


class Fighter(BaseComponent):
+   entity: Actor

    def __init__(self, hp: int, defense: int, power: int):
        self.max_hp = hp
        self._hp = hp
        self.defense = defense
        self.power = power

    @property
    def hp(self) -> int:
        return self._hp

    @hp.setter
    def hp(self, value: int) -> None:
        self._hp = max(0, min(value, self.max_hp))
+       if self._hp == 0 and self.entity.ai:
+           self.die()

+   def die(self) -> None:
+       if self.engine.player is self.entity:
+           death_message = "You died!"
+       else:
+           death_message = f"{self.entity.name} is dead!"

+       self.entity.char = "%"
+       self.entity.color = (191, 0, 0)
+       self.entity.blocks_movement = False
+       self.entity.ai = None
+       self.entity.name = f"remains of {self.entity.name}"

+       print(death_message)
```

When the actor dies, we use the `die` method to do several things:

- Print out a message, indicating the death of the entity
- Set the entity’s character to “%” (most roguelikes use this for corpses)
- Set its color to red (for a bloody, gory mess)
- Set `blocks_movement` to `False`, so that the entities can walk over the corpse
- Remove the AI from the entity, so it’ll be marked as dead and won’t take any more turns.
- Change the name to “remains of {entity name}”

Run the project now, and enjoy slaughtering some Orcs and Trolls!

![Part 6 - Killing Enemies](https://rogueliketutorials.com/images/part-6-killing-enemies.png)

As satisfying as it would be to end here, our work is not quite done. If you play the game a bit, you’ll notice two problems.

The first is that, sometimes, corpses actually cover up entities.

![Part 6 - Player under a Corpse](https://rogueliketutorials.com/images/part-6-player-under-corpse.png)

_The player is currently under the corpse in the screenshot._

This not only makes no sense, since the entities should be walking _over_ the corpses, but it can confuse the player rather easily.

The other issue is much more severe. Try playing the game and letting yourself die on purpose.

![Part 6 - Dead Player](https://rogueliketutorials.com/images/part-6-dead-player.png)

The player does indeed turn into a corpse, but… you can still move around, and even attack enemies! This is because the game doesn’t really “end” at the moment when the player dies. The only thing that changes is that the player’s AI component is set to `None`, but that isn’t actually what controls the player, the `EventHandler` class does that.

Let’s focus on the first issue first. Solving it is actually pretty easy. What we’ll do is assign a value to each `Entity`, and this value will represent which order the entities should be rendered in. Lower values will be rendered first, and higher values will be rendered after. Therefore, if we assign a low value to a corpse, it will get drawn before an entity. If two things are on the same tile, whatever gets drawn last will be what the player sees.

To create the render values we’ll need, create a new file, called `render_order.py`, and put the following class in it:

```python
from enum import auto, Enum


class RenderOrder(Enum):
    CORPSE = auto()
    ITEM = auto()
    ACTOR = auto()
```

_Note: You’ll need Python 3.6 or higher for the `auto` function to work._

`RenderOrder` is an `Enum`. An “Enum” is a set of named values that won’t change, so it’s perfect for things like this. `auto` assigns incrementing integer values automatically, so we don’t need to retype them if we add more values later on.

To use this new Enum, let’s edit `entity.py`:

Diff Original

```diff
from __future__ import annotations

import copy
from typing import Optional, Tuple, Type, TypeVar, TYPE_CHECKING

+from render_order import RenderOrder

if TYPE_CHECKING:
    from components.ai import BaseAI
    from components.fighter import Fighter
    from game_map import GameMap

T = TypeVar("T", bound="Entity")


class Entity:
    """
    A generic object to represent players, enemies, items, etc.
    """

    gamemap: GameMap

    def __init__(
        self,
        gamemap: Optional[GameMap] = None,
        x: int = 0,
        y: int = 0,
        char: str = "?",
        color: Tuple[int, int, int] = (255, 255, 255),
        name: str = "<Unnamed>",
        blocks_movement: bool = False,
+       render_order: RenderOrder = RenderOrder.CORPSE,
    ):
        self.x = x
        self.y = y
        self.char = char
        self.color = color
        self.name = name
        self.blocks_movement = blocks_movement
+       self.render_order = render_order
        if gamemap:
            # If gamemap isn't provided now then it will be set later.
            self.gamemap = gamemap
            gamemap.entities.add(self)
    ...

class Actor(Entity):
    def __init__(
        self,
        *,
        x: int = 0,
        y: int = 0,
        char: str = "?",
        color: Tuple[int, int, int] = (255, 255, 255),
        name: str = "<Unnamed>",
        ai_cls: Type[BaseAI],
        fighter: Fighter
    ):
        super().__init__(
            x=x,
            y=y,
            char=char,
            color=color,
            name=name,
            blocks_movement=True,
+           render_order=RenderOrder.ACTOR,
        )
```

We’re now passing the render order to the `Entity` class, with a default of `CORPSE`. Notice that we don’t pass it to `Actor`, and instead, assume that the actor’s default will be the `ACTOR` value.

In order to actually take advantage of the rendering order, we’ll need to modify the part of `GameMap` that renders the entities to the screen. Modify the `render` method in `GameMap` like this:

Diff Original

```diff
    ...
    def render(self, console: Console) -> None:
        """
        Renders the map.

        If a tile is in the "visible" array, then draw it with the "light" colors.
        If it isn't, but it's in the "explored" array, then draw it with the "dark" colors.
        Otherwise, the default is "SHROUD".
        """
        console.tiles_rgb[0:self.width, 0:self.height] = np.select(
            condlist=[self.visible, self.explored],
            choicelist=[self.tiles["light"], self.tiles["dark"]],
            default=tile_types.SHROUD
        )

+       entities_sorted_for_rendering = sorted(
+           self.entities, key=lambda x: x.render_order.value
+       )

-       for entity in self.entities:
+       for entity in entities_sorted_for_rendering:
            if self.visible[entity.x, entity.y]:
-               console.print(x=entity.x, y=entity.y, string=entity.char, fg=entity.color)
+               console.print(
+                   x=entity.x, y=entity.y, string=entity.char, fg=entity.color
+               )
```

The `sorted` function takes two arguments: The collection to sort, and the function used to sort it. By using `key` in `sorted`, we’re defining a custom way to sort the `self.entities`, which in this case, we’re using a `lambda` function (basically, a function that’s limited to one line that we don’t need to write a formal definition for). The lambda function itself tells `sorted` to sort by the value of `render_order`. Since the `RenderOrder` enum defines its order from 1 (Corpse, lowest) to 3 (Actor, highest), corpses should be sent to the front of the sorted list. That way, when rendering, they’ll get drawn first, so if there’s something else on top of them, they’ll get overwritten, and we’ll just see the `Actor` instead of the corpse.

Last thing we need to do is rewrite the `render_order` of an entity when it dies. Go back to the `Fighter` class and add the following:

Diff Original

```diff
from __future__ import annotations

from typing import TYPE_CHECKING

from components.base_component import BaseComponent
+from render_order import RenderOrder

if TYPE_CHECKING:
    from entity import Actor


class Fighter(BaseComponent):
    ...
        ...
        self.entity.ai = None
        self.entity.name = f"remains of {self.entity.name}"
+       self.entity.render_order = RenderOrder.CORPSE

        print(death_message)
```

Run the project now, and the corpse ordering issue should be resolved.

Now, onto the more important issue: solving the player’s death.

One thing that would be helpful right now is being able to see the player’s HP. Otherwise, the player will just kinda drop dead after a while, and it’ll be difficult for the player to know how close they are to death’s door.

Add the following line to the `render` function in the `Engine` class:

Diff Original

```diff
if TYPE_CHECKING:
-   from entity import Entity
+   from entity import Actor
    from game_map import GameMap


class Engine:
    game_map: GameMap

-   def __init__(self, player: Entity):
+   def __init__(self, player: Actor):
        ...

    def render(self, console: Console, context: Context) -> None:
        self.game_map.render(console)

+       console.print(
+           x=1,
+           y=47,
+           string=f"HP: {self.player.fighter.hp}/{self.player.fighter.max_hp}",
+       )

        context.present(console)

        console.clear()
```

Pretty simple. We’re printing the player’s HP current health over maximum health below the map. It’s not the most attractive looking health display, that’s for sure, but it should suffice for now. A better looking way to show the character’s health is coming shortly anyway, in the next chapter.

Notice that we also updated the type hint for the `player` argument in the Engine’s `__init__` function.

The health indicator is great and all, but our player is still animated after death. There’s a few ways to handle this, but the way we’ll go with is swapping out the `EventHandler` class. Why? Because what we want to do right now is disallow the player from moving around after dying. An easy way to do that is to stop reacting to the movement keypresses. By switching to a different `EventHandler`, we can do just that.

What we’ll want to do is actually modify our existing `EventHandler` to be a base class, and inherit from it in two new classes: `MainGameEventHandler`, and `GameOverEventHandler`. `MainGameEventHandler` will actually do what our current implementation of `EventHandler` does, and `GameOverEventHandler` will handle things when the main character meets his or her untimely demise.

Open up `input_handlers.py` and make the following adjustments:

Diff Original

```diff
class EventHandler(tcod.event.EventDispatch[Action]):
    def __init__(self, engine: Engine):
        self.engine = engine

+   def handle_events(self) -> None:
+       raise NotImplementedError()

+   def ev_quit(self, event: tcod.event.Quit) -> Optional[Action]:
+       raise SystemExit()


+class MainGameEventHandler(EventHandler):
    def handle_events(self) -> None:
        for event in tcod.event.wait():
            action = self.dispatch(event)

            if action is None:
                continue

            action.perform()

            self.engine.handle_enemy_turns()
            self.engine.update_fov()  # Update the FOV before the players next action.

-   def ev_quit(self, event: tcod.event.Quit) -> Optional[Action]:
-       raise SystemExit()

    def ev_keydown(self, event: tcod.event.KeyDown) -> Optional[Action]:
        action: Optional[Action] = None

        key = event.sym

        player = self.engine.player

        if key in MOVE_KEYS:
            dx, dy = MOVE_KEYS[key]
            action = BumpAction(player, dx, dy)
        elif key in WAIT_KEYS:
            action = WaitAction(player)

        elif key == tcod.event.K_ESCAPE:
            action = EscapeAction(player)

        # No valid key was pressed
        return action


+class GameOverEventHandler(EventHandler):
+   def handle_events(self) -> None:
+       for event in tcod.event.wait():
+           action = self.dispatch(event)

+           if action is None:
+               continue

+           action.perform()

+   def ev_keydown(self, event: tcod.event.KeyDown) -> Optional[Action]:
+       action: Optional[Action] = None

+       key = event.sym

+       if key == tcod.event.K_ESCAPE:
+           action = EscapeAction(self.engine.player)

+       # No valid key was pressed
+       return action
```

`EventHandler` is now the base class for our other two classes.

`MainGameEventHandler` is almost identical to our original `EventHandler` class, except that it doesn’t need to implement `ev_quit`, as `EventHandler` takes care of that just fine.

`GameOverEventHandler` is what’s really new here. It doesn’t look terribly different from `MainGameEventHandler`, except for a few key differences.

- After performing its actions, it doesn’t call the enemy turns nor update the FOV.
- It also doesn’t respond to the movement keys, just `Esc`, so the player can still exit the game.

Because we’re replacing our old implementation of `EventHandler` with `MainGameEventHandler`, we’ll need to adjust `engine.py` to use `MainGameEventHandler`:

Diff Original

```diff
from tcod.map import compute_fov

-from input_handlers import EventHandler
+from input_handlers import MainGameEventHandler

if TYPE_CHECKING:
    from entity import Actor
    from game_map import GameMap
+   from input_handlers import EventHandler


class Engine:
    game_map: GameMap

    def __init__(self, player: Actor):
-       self.event_handler: EventHandler = EventHandler(self)
+       self.event_handler: EventHandler = MainGameEventHandler(self)
        self.player = player
```

Lastly, we can use the `GameOverEventHandler` in `fighter.py` to ensure the player cannot move after death:

Diff Original

```diff
from __future__ import annotations

from typing import TYPE_CHECKING

from components.base_component import BaseComponent
+from input_handlers import GameOverEventHandler
from render_order import RenderOrder

if TYPE_CHECKING:
    from entity import Actor


class Fighter(BaseComponent):
    ...

    def die(self) -> None:
        if self.engine.player is self.entity:
            death_message = "You died!"
+           self.engine.event_handler = GameOverEventHandler(self.engine)
        else:
            death_message = f"{self.entity.name} is dead!"
```

And with that last change, the main character should die, for real this time! You’ll be unable to move or attack, but you can still exit the game as normal.

If you want to see the code so far in its entirety, [click here](https://github.com/TStand90/tcod_tutorial_v2/tree/2020/part-6).

[Click here to move on to the next part of this tutorial.](https://rogueliketutorials.com/tutorials/tcod/v2/part-7)

---
# [Part 7 - Creating the Interface](https://rogueliketutorials.com/tutorials/tcod/v2/part-7/)

Our game is looking more and more playable by the chapter, but before we move forward with the gameplay, we ought to take a moment to focus on how the project _looks_. Despite what some roguelike traditionalists may tell you, a good UI goes a long way.

One of the first things we can do is define a file that will hold our RGB colors. We’ve just been hard-coding them up until now, but it would be nice if they were all in one place and then imported when needed, so that we could easily update them if need be.

Create a new file, called `color.py`, and fill it with the following:

```python
white = (0xFF, 0xFF, 0xFF)
black = (0x0, 0x0, 0x0)

player_atk = (0xE0, 0xE0, 0xE0)
enemy_atk = (0xFF, 0xC0, 0xC0)

player_die = (0xFF, 0x30, 0x30)
enemy_die = (0xFF, 0xA0, 0x30)

welcome_text = (0x20, 0xA0, 0xFF)

bar_text = white
bar_filled = (0x0, 0x60, 0x0)
bar_empty = (0x40, 0x10, 0x10)
```

Some of these colors, like `welcome_text` and `bar_filled` are things we haven’t added yet, but don’t worry, we’ll utilize them by the end of the chapter.

Last chapter, we implemented a basic HP tracker for the player, with the promise that we’d revisit in this chapter to make it look better. And now, the time has come!

We’ll create a bar that will gradually decrease as the player loses HP. This will help the player visualize how much HP is remaining. To do this, we’ll create a generic `render_bar` function, which can accept different values and change the bar’s length based on the `current_value` and `maximum_value` we give to it.

To house this new function (as well as some other functions that are coming soon), let’s create a new file, called `render_functions.py`. Put the following into it:

```python
from __future__ import annotations

from typing import TYPE_CHECKING

import color

if TYPE_CHECKING:
    from tcod import Console


def render_bar(
    console: Console, current_value: int, maximum_value: int, total_width: int
) -> None:
    bar_width = int(float(current_value) / maximum_value * total_width)

    console.draw_rect(x=0, y=45, width=total_width, height=1, ch=1, bg=color.bar_empty)

    if bar_width > 0:
        console.draw_rect(
            x=0, y=45, width=bar_width, height=1, ch=1, bg=color.bar_filled
        )

    console.print(
        x=1, y=45, string=f"HP: {current_value}/{maximum_value}", fg=color.bar_text
    )
```

We’re utilizing the `draw_rect` functions provided by TCOD to draw rectangular bars. We’re actually drawing two bars, one on top of the other. The first one will be the background color, which in the case of our health bar, will be a red color. The second goes on top, and is green. The one on top will gradually decrease as the player drops hit points, as its width is determined by the `bar_width` variable, which is itself determined by the `current_value` over the `maximum_value`.

We also print the “HP” value over the bar, so the player knows the exact number.

In order to utilize this new function, make the following changes to `engine.py`:

Diff Original

```diff
...
from input_handlers import MainGameEventHandler
+from render_functions import render_bar

if TYPE_CHECKING:
    ...

    ...
    def render(self, console: Console, context: Context) -> None:
        self.game_map.render(console)

+       render_bar(
+           console=console,
+           current_value=self.player.fighter.hp,
+           maximum_value=self.player.fighter.max_hp,
+           total_width=20,
+       )
-       console.print(
-           x=1,
-           y=47,
-           string=f"HP: {self.player.fighter.hp}/{self.player.fighter.max_hp}",
-       )

        context.present(console)

        console.clear()
```

Run the project now, and you should have a functioning health bar!

![Part 7 - Health bar](https://rogueliketutorials.com/images/part-7-health-bar.png)

What next? One obvious problem with our project at the moment is that the messages get printed to the terminal rather than showing up in the actual game. We can fix that by adding a message log, which can display messages along with different colors for a bit of flash.

Create a new file, called `message_log.py`, and put the following contents inside:

```python
from typing import List, Reversible, Tuple
import textwrap

import tcod

import color


class Message:
    def __init__(self, text: str, fg: Tuple[int, int, int]):
        self.plain_text = text
        self.fg = fg
        self.count = 1

    @property
    def full_text(self) -> str:
        """The full text of this message, including the count if necessary."""
        if self.count > 1:
            return f"{self.plain_text} (x{self.count})"
        return self.plain_text


class MessageLog:
    def __init__(self) -> None:
        self.messages: List[Message] = []

    def add_message(
        self, text: str, fg: Tuple[int, int, int] = color.white, *, stack: bool = True,
    ) -> None:
        """Add a message to this log.
        `text` is the message text, `fg` is the text color.
        If `stack` is True then the message can stack with a previous message
        of the same text.
        """
        if stack and self.messages and text == self.messages[-1].plain_text:
            self.messages[-1].count += 1
        else:
            self.messages.append(Message(text, fg))

    def render(
        self, console: tcod.Console, x: int, y: int, width: int, height: int,
    ) -> None:
        """Render this log over the given area.
        `x`, `y`, `width`, `height` is the rectangular region to render onto
        the `console`.
        """
        self.render_messages(console, x, y, width, height, self.messages)

    @staticmethod
    def render_messages(
        console: tcod.Console,
        x: int,
        y: int,
        width: int,
        height: int,
        messages: Reversible[Message],
    ) -> None:
        """Render the messages provided.
        The `messages` are rendered starting at the last message and working
        backwards.
        """
        y_offset = height - 1

        for message in reversed(messages):
            for line in reversed(textwrap.wrap(message.full_text, width)):
                console.print(x=x, y=y + y_offset, string=line, fg=message.fg)
                y_offset -= 1
                if y_offset < 0:
                    return  # No more space to print messages.
```

Let’s go through the additions piece by piece.

```python
class Message:
    def __init__(self, text: str, fg: Tuple[int, int, int]):
        self.plain_text = text
        self.fg = fg
        self.count = 1

    @property
    def full_text(self) -> str:
        """The full text of this message, including the count if necessary."""
        if self.count > 1:
            return f"{self.plain_text} (x{self.count})"
        return self.plain_text
```

The `Message` will be used to save and display messages in our log. It includes three pieces of information:

- `plain_text`: The actual message text.
- `fg`: The “foreground” color of the message.
- `count`: This is used to display something like “The Orc attacks (x3).” Rather than crowding our message log with the same message over and over, we can “stack” the messages by increasing a message’s count. This only happens when the same message appears several times in a row.

The `full_text` property returns the text with its count, if the count is greater than 1. Otherwise, it just returns the message as-is.

Now, the actual message log:

```python
class MessageLog:
    def __init__(self) -> None:
        self.messages: List[Message] = []
```

It keeps a list of the `Message`s received. Nothing too complex here.

```python
    def add_message(
        self, text: str, fg: Tuple[int, int, int] = color.white, *, stack: bool = True,
    ) -> None:
        """Add a message to this log.
        `text` is the message text, `fg` is the text color.
        If `stack` is True then the message can stack with a previous message
        of the same text.
        """
        if stack and self.messages and text == self.messages[-1].plain_text:
            self.messages[-1].count += 1
        else:
            self.messages.append(Message(text, fg))
```

`add_message` is what adds the message to the log. `text` is required, but `fg` will just default to white if nothing is given. `stack` tells us whether to stack messages or not (which allows us to disable this behavior, if desired).

If we are allowing stacking, and the added message matches the previous message, we just increment the previous message’s count by 1. If it’s not a match, we add it to the list.

```python
    def render(
        self, console: tcod.Console, x: int, y: int, width: int, height: int,
    ) -> None:
        """Render this log over the given area.
        `x`, `y`, `width`, `height` is the rectangular region to render onto
        the `console`.
        """
        self.render_messages(console, x, y, width, height, self.messages)

    @staticmethod
    def render_messages(
        console: tcod.Console,
        x: int,
        y: int,
        width: int,
        height: int,
        messages: Reversible[Message],
    ) -> None:
        """Render the messages provided.
        The `messages` are rendered starting at the last message and working
        backwards.
        """
        y_offset = height - 1

        for message in reversed(messages):
            for line in reversed(textwrap.wrap(message.full_text, width)):
                console.print(x=x, y=y + y_offset, string=line, fg=message.fg)
                y_offset -= 1
                if y_offset < 0:
                    return  # No more space to print messages.
```

This `render` calls `render_messages`, which is a static method that actually renders the messages to the screen. It renders them in reverse order, to make it appear that the messages are scrolling in an upwards direction. We use the `textwrap.wrap` function to wrap the text to fit within the given area, and then print each line to the console. We can only print so many messages to the console, however, so if `y_offset` reaches -1, we stop.

To utilize the message log, we’ll first need to add it to the `Engine` class. Modify `engine.py` like this:

Diff Original

```diff
...
from input_handlers import MainGameEventHandler
+from message_log import MessageLog
from render_functions import render_bar

if TYPE_CHECKING:
    from entity import Actor
    from game_map import GameMap
    from input_handlers import EventHandler

class Engine:
    game_map: GameMap

    def __init__(self, player: Actor):
        self.event_handler: EventHandler = MainGameEventHandler(self)
+       self.message_log = MessageLog()
        self.player = player
    ...

    def render(self, console: Console, context: Context) -> None:
        self.game_map.render(console)

+       self.message_log.render(console=console, x=21, y=45, width=40, height=5)

        render_bar(
            ...
```

We’re adding an instance of `MessageLog` in the initializer, and rendering the log in the Engine’s `render` method. Nothing too complicated here.

We need to make a small change to `main.py` in order to actually make room for our message log. We can also add a friendly welcome message here.

Diff Original

```diff
#!/usr/bin/env python3
import copy

import tcod

+import color
from engine import Engine
import entity_factories
...

    ...
    map_width = 80
-   map_height = 45
+   map_height = 43

    room_max_size = 10
    ...

    ...
    engine.update_fov()

+   engine.message_log.add_message(
+       "Hello and welcome, adventurer, to yet another dungeon!", color.welcome_text
+   )

    with tcod.context.new_terminal(
        ...
```

Feel free to experiment with different window and map sizes, if you like.

Run the project, and you should see the welcome message.

![Part 7 - Welcome Message](https://rogueliketutorials.com/images/part-7-welcome-message.png)

Now that we’ve confirmed our message log accepts and displays messages, we’ll need to replace all of our previous `print` statements to push messages to the log instead.

Let’s start with our attack action, in `actions.py`:

Diff Original

```diff
...
from typing import Optional, Tuple, TYPE_CHECKING

+import color

if TYPE_CHECKING:
    ...

        ...
        damage = self.entity.fighter.power - target.fighter.defense

        attack_desc = f"{self.entity.name.capitalize()} attacks {target.name}"
+       if self.entity is self.engine.player:
+           attack_color = color.player_atk
+       else:
+           attack_color = color.enemy_atk

        if damage > 0:
-           print(f"{attack_desc} for {damage} hit points.")
+           self.engine.message_log.add_message(
+               f"{attack_desc} for {damage} hit points.", attack_color
+           )
            target.fighter.hp -= damage
        else:
-           print(f"{attack_desc} but does no damage.")
+           self.engine.message_log.add_message(
+               f"{attack_desc} but does no damage.", attack_color
+           )
```

We determine the color based on who is doing the attacking. Other than that, there’s really nothing new here, we’re just pushing those messages to the log rather than printing them.

Now we just need to update our death messages. Open up `fighter.py` and modify it like this:

Diff Original

```diff
from __future__ import annotations

from typing import TYPE_CHECKING

+import color
from components.base_component import BaseComponent
...

    ...
    def die(self) -> None:
        if self.engine.player is self.entity:
            death_message = "You died!"
+           death_message_color = color.player_die
            self.engine.event_handler = GameOverEventHandler(self.engine)
        else:
            death_message = f"{self.entity.name} is dead!"
+           death_message_color = color.enemy_die

        self.entity.char = "%"
        self.entity.color = (191, 0, 0)
        self.entity.blocks_movement = False
        self.entity.ai = None
        self.entity.name = f"remains of {self.entity.name}"
        self.entity.render_order = RenderOrder.CORPSE

-       print(death_message)
+       self.engine.message_log.add_message(death_message, death_message_color)
```

Run the project now. You should see messages for both attacks and deaths!

![Part 7 - Death Messages](https://rogueliketutorials.com/images/part-7-death-messages.png)

What next? One thing that would be nice is to see the names of the different entities. This will become useful later on if you decide to add more enemy types. It’s easy enough to remember “Orc” and “Troll”, but most roguelikes have a wide variety of enemies, so it’s helpful to know what each letter on the screen means.

We can accomplish this by displaying the names of the entities that are currently under the player’s mouse. We’ll need to make a few changes to our project to capture the mouse’s current position, however.

Edit `main.py` like this:

Diff Original

```diff
        root_console = tcod.Console(screen_width, screen_height, order="F")
        while True:
+           root_console.clear()
+           engine.event_handler.on_render(console=root_console)
+           context.present(root_console)
-           engine.render(console=root_console, context=context)

+           engine.event_handler.handle_events(context)
-           engine.event_handler.handle_events()
```

We’re adding the console’s `clear` back to main, as well as the context’s `present`. Also, we’re calling a method that we haven’t defined yet: `on_render`, but don’t worry, we’ll define it in a moment. Basically, this method tells the engine to render.

We’re also passing the `context` to `handle_events` now, because we need to call an extra method on it to capture the mouse input.

Now let’s modify `input_handlers.py` to contain the methods we’re calling in `main.py`:

Diff Original

```diff
class EventHandler(tcod.event.EventDispatch[Action]):
    def __init__(self, engine: Engine):
        self.engine = engine

-   def handle_events(self) -> None:
-       raise NotImplementedError()

+   def handle_events(self, context: tcod.context.Context) -> None:
+       for event in tcod.event.wait():
+           context.convert_event(event)
+           self.dispatch(event)

    def ev_quit(self, event: tcod.event.Quit) -> Optional[Action]:
        raise SystemExit()

+   def on_render(self, console: tcod.Console) -> None:
+       self.engine.render(console)


class MainGameEventHandler(EventHandler):
-   def handle_events(self) -> None:
+   def handle_events(self, context: tcod.context.Context) -> None:
        for event in tcod.event.wait():
+           context.convert_event(event)

            action = self.dispatch(event)
            ...


class GameOverEventHandler(EventHandler):
-   def handle_events(self) -> None:
+   def handle_events(self, context: tcod.context.Context) -> None:
        ...
```

We’re modifying the `handle_events` method in `EventHandler` to actually have an implementation. It iterates through the events, and uses `context.convert_event` to give the event knowledge on the mouse position. It then dispatches that event, to be handled like normal.

`on_render` just tells the `Engine` class to call its render method, using the given console.

`MainGameEventHandler` and `GameOverEventHandler` have small changes to their `handle_events` methods to match the signature of `EventHandler`, and `MainGameEventHandler` also uses `context.convert_event`.

We’re no longer passing the `context` to the `Engine` class’s `render` method, so let’s change the method now:

Diff Original

```diff
...
from typing import TYPE_CHECKING

-from tcod.context import Context
from tcod.console import Console
...

class Engine:
    ...

-   def render(self, console: Console, context: Context) -> None:
+   def render(self, console: Console) -> None:
        self.game_map.render(console)

        self.message_log.render(console=console, x=21, y=45, width=40, height=5)

        render_bar(
            console=console,
            current_value=self.player.fighter.hp,
            maximum_value=self.player.fighter.max_hp,
            total_width=20,
        )

-       context.present(console)

-       console.clear()
```

We’ve also removed the `console.clear` call, as that’s being handled by `main.py`.

So we’re passing the context around to different classes and converting the events to capture the mouse location. But where does that information actually get stored? Let’s add a data point on to the `Engine` class to hold that information. Add the following to `engine.py`:

Diff Original

```diff
class Engine:
    game_map: GameMap

    def __init__(self, player: Actor):
        self.event_handler: EventHandler = MainGameEventHandler(self)
        self.message_log = MessageLog()
+       self.mouse_location = (0, 0)
        self.player = player
```

Okay, so we’ve got a place to store the mouse location, but where do we actually get that information?

There’s an easy way: by overriding a method in `EventHandler`, which is called `ev_mousemotion`. By doing that, we can write the mouse location to the engine for access later. Here’s how that looks:

Diff Original

```diff
class EventHandler(tcod.event.EventDispatch[Action]):
    def __init__(self, engine: Engine):
        self.engine = engine

    def handle_events(self, context: tcod.context.Context) -> None:
        for event in tcod.event.wait():
            context.convert_event(event)
            self.dispatch(event)

+   def ev_mousemotion(self, event: tcod.event.MouseMotion) -> None:
+       if self.engine.game_map.in_bounds(event.tile.x, event.tile.y):
+           self.engine.mouse_location = event.tile.x, event.tile.y

    def ev_quit(self, event: tcod.event.Quit) -> Optional[Action]:
        raise SystemExit()
```

Great! Now we’re saving the mouse’s location, so it’s time to actually make use of it. Our original goal was to display the entity names that are in the mouse’s current position. The hard part is already done, now all we need to do is check which entities are in the given location, get their names, and print them out to the screen.

Since this has to do with rendering, let’s put these new functions in `render_functions.py`:

Diff Original

```diff
from __future__ import annotations

from typing import TYPE_CHECKING

import color

if TYPE_CHECKING:
    from tcod import Console
+   from engine import Engine
+   from game_map import GameMap


+def get_names_at_location(x: int, y: int, game_map: GameMap) -> str:
+   if not game_map.in_bounds(x, y) or not game_map.visible[x, y]:
+       return ""

+   names = ", ".join(
+       entity.name for entity in game_map.entities if entity.x == x and entity.y == y
+   )

+   return names.capitalize()


def render_bar(
    console: Console, current_value: int, maximum_value: int, total_width: int
) -> None:
    bar_width = int(float(current_value) / maximum_value * total_width)

    console.draw_rect(x=0, y=45, width=20, height=1, ch=1, bg=color.bar_empty)

    if bar_width > 0:
        console.draw_rect(
            x=0, y=45, width=bar_width, height=1, ch=1, bg=color.bar_filled
        )

    console.print(
        x=1, y=45, string=f"HP: {current_value}/{maximum_value}", fg=color.bar_text
    )


+def render_names_at_mouse_location(
+   console: Console, x: int, y: int, engine: Engine
+) -> None:
+   mouse_x, mouse_y = engine.mouse_location

+   names_at_mouse_location = get_names_at_location(
+       x=mouse_x, y=mouse_y, game_map=engine.game_map
+   )

+   console.print(x=x, y=y, string=names_at_mouse_location)
```

We’ve added two new functions, `render_names_at_mouse_location` and `get_names_at_location`. Let’s discuss what each one does.

`render_names_at_mouse_location` takes the console, x and y coordinates (the location to draw the names), and the engine. From the engine, it grabs the mouse’s current x and y positions, and passes them to `get_names_at_location`, which we can assume for the moment will return the list of entity names we want. Once we have these entity names as a string, we can print that string to the given x and y location on the screen, with `console.print`.

`get_names_at_location` also takes “x” and “y” variables, though these represent a spot on the map. We first check that the x and y coordinates are within the map, and are currently visible to the player. If they are, then we create a string of the entity names at that spot, separated by a comma. We then return that string, adding `capitalize` to make sure the first letter in the string is capitalized.

Now all we need to do is modify `engine.py` to import these functions and utilize them in the `render` method. Make the following modifications:

Diff Original

```diff
...
from message_log import MessageLog
-from render_functions import render_bar
+from render_functions import render_bar, render_names_at_mouse_location

if TYPE_CHECKING:
    ...

    ...
    def render(self, console: Console) -> None:
        self.game_map.render(console)

        self.message_log.render(console=console, x=21, y=45, width=40, height=5)

        render_bar(
            console=console,
            current_value=self.player.fighter.hp,
            maximum_value=self.player.fighter.max_hp,
            total_width=20,
        )

+       render_names_at_mouse_location(console=console, x=21, y=44, engine=self)
```

Now if you hover your mouse over an entity, you’ll see its name. If you stack a few corpses up, you’ll notice that it prints a list of the names.

We’re almost finished with this chapter. Before we wrap up, let’s revisit our message log for a moment. One issue with it is that we can’t see messages that are too far back. However, HexDecimal was kind enough to provide a method for viewing the whole log, with the ability to scroll.

Add the following to `input_handlers.py`:

Diff Original

```diff
class GameOverEventHandler(EventHandler):
    ...


+CURSOR_Y_KEYS = {
+   tcod.event.K_UP: -1,
+   tcod.event.K_DOWN: 1,
+   tcod.event.K_PAGEUP: -10,
+   tcod.event.K_PAGEDOWN: 10,
+}


+class HistoryViewer(EventHandler):
+   """Print the history on a larger window which can be navigated."""

+   def __init__(self, engine: Engine):
+       super().__init__(engine)
+       self.log_length = len(engine.message_log.messages)
+       self.cursor = self.log_length - 1

+   def on_render(self, console: tcod.Console) -> None:
+       super().on_render(console)  # Draw the main state as the background.

+       log_console = tcod.Console(console.width - 6, console.height - 6)

+       # Draw a frame with a custom banner title.
+       log_console.draw_frame(0, 0, log_console.width, log_console.height)
+       log_console.print_box(
+           0, 0, log_console.width, 1, "┤Message history├", alignment=tcod.CENTER
+       )

+       # Render the message log using the cursor parameter.
+       self.engine.message_log.render_messages(
+           log_console,
+           1,
+           1,
+           log_console.width - 2,
+           log_console.height - 2,
+           self.engine.message_log.messages[: self.cursor + 1],
+       )
+       log_console.blit(console, 3, 3)

+   def ev_keydown(self, event: tcod.event.KeyDown) -> None:
+       # Fancy conditional movement to make it feel right.
+       if event.sym in CURSOR_Y_KEYS:
+           adjust = CURSOR_Y_KEYS[event.sym]
+           if adjust < 0 and self.cursor == 0:
+               # Only move from the top to the bottom when you're on the edge.
+               self.cursor = self.log_length - 1
+           elif adjust > 0 and self.cursor == self.log_length - 1:
+               # Same with bottom to top movement.
+               self.cursor = 0
+           else:
+               # Otherwise move while staying clamped to the bounds of the history log.
+               self.cursor = max(0, min(self.cursor + adjust, self.log_length - 1))
+       elif event.sym == tcod.event.K_HOME:
+           self.cursor = 0  # Move directly to the top message.
+       elif event.sym == tcod.event.K_END:
+           self.cursor = self.log_length - 1  # Move directly to the last message.
+       else:  # Any other key moves back to the main game state.
+           self.engine.event_handler = MainGameEventHandler(self.engine)
```

To show this new view, all we need to do is this, in `MainGameEventHandler`:

Diff Original

```diff
        ...
        elif key == tcod.event.K_ESCAPE:
            action = EscapeAction(player)
+       elif key == tcod.event.K_v:
+           self.engine.event_handler = HistoryViewer(self.engine)
```

Now all the player has to do is press the “v” key to see a log of all past messages. By using the up and down keys, you can scroll through the log.

If you want to see the code so far in its entirety, [click here](https://github.com/TStand90/tcod_tutorial_v2/tree/2020/part-7).

[Click here to move on to the next part of this tutorial.](https://rogueliketutorials.com/tutorials/tcod/v2/part-8)

---
# [Part 8 - Items and Inventory](https://rogueliketutorials.com/tutorials/tcod/v2/part-8/)

## Quick refactors [](https://rogueliketutorials.com/tutorials/tcod/v2/part-8/#quick-refactors)

Once again, apologies to everyone reading this right now. After publishing the last two parts, there were once again a few refactors on code written in those parts, like at the beginning of part 6. Luckily, the changes are much less extensive this time.

`ai.py`

Diff Original

```diff
...
import numpy as np  # type: ignore
import tcod

from actions import Action, MeleeAction, MovementAction, WaitAction
-from components.base_component import BaseComponent

if TYPE_CHECKING:
    from entity import Actor


-class BaseAI(Action, BaseComponent):
+class BaseAI(Action):
    entity: Actor

    def perform(self) -> None:
        raise NotImplementedError()
    ...
```

`message_log.py`

Diff Original

```diff
+from typing import Iterable, List, Reversible, Tuple
-from typing import List, Reversible, Tuple
import textwrap

import tcod

import color
...


class MessageLog:
    ...

    def render(
        self, console: tcod.Console, x: int, y: int, width: int, height: int,
    ) -> None:
        """Render this log over the given area.

        `x`, `y`, `width`, `height` is the rectangular region to render onto
        the `console`.
        """
        self.render_messages(console, x, y, width, height, self.messages)

+   @staticmethod
+   def wrap(string: str, width: int) -> Iterable[str]:
+       """Return a wrapped text message."""
+       for line in string.splitlines():  # Handle newlines in messages.
+           yield from textwrap.wrap(
+               line, width, expand_tabs=True,
+           )

-   @staticmethod
+   @classmethod
    def render_messages(
+       cls,
        console: tcod.Console,
        x: int,
        y: int,
        width: int,
        height: int,
        messages: Reversible[Message],
    ) -> None:
        """Render the messages provided.

        The `messages` are rendered starting at the last message and working
        backwards.
        """
        y_offset = height - 1

        for message in reversed(messages):
-           for line in reversed(textwrap.wrap(message.full_text, width)):
+           for line in reversed(list(cls.wrap(message.full_text, width))):
                console.print(x=x, y=y + y_offset, string=line, fg=message.fg)
                y_offset -= 1
                if y_offset < 0:
                    return  # No more space to print messages.
```

`game_map.py`

Diff Original

```diff
class GameMap:
    ...
    )  # Tiles the player has seen before

+   @property
+   def gamemap(self) -> GameMap:
+       return self

     @property
     def actors(self) -> Iterator[Actor]:
        ...
```

`entity.py`

Diff Original

```diff
class Entity:
    """
    A generic object to represent players, enemies, items, etc.
    """

-   gamemap: GameMap
+   parent: GameMap

    def __init__(
        self,
-       gamemap: Optional[GameMap] = None,
+       parent: Optional[GameMap] = None,
        x: int = 0,
        y: int = 0,
        char: str = "?",
        color: Tuple[int, int, int] = (255, 255, 255),
        name: str = "<Unnamed>",
        blocks_movement: bool = False,
        render_order: RenderOrder = RenderOrder.CORPSE,
    ):
        self.x = x
        self.y = y
        self.char = char
        self.color = color
        self.name = name
        self.blocks_movement = blocks_movement
        self.render_order = render_order
-       if gamemap:
-           # If gamemap isn't provided now then it will be set later.
-           self.gamemap = gamemap
-           gamemap.entities.add(self)
+       if parent:
+           # If parent isn't provided now then it will be set later.
+           self.parent = parent
+           parent.entities.add(self)

+   @property
+   def gamemap(self) -> GameMap:
+       return self.parent.gamemap

    def spawn(self: T, gamemap: GameMap, x: int, y: int) -> T:
        """Spawn a copy of this instance at the given location."""
        clone = copy.deepcopy(self)
        clone.x = x
        clone.y = y
-       clone.gamemap = gamemap
+       clone.parent = gamemap
        gamemap.entities.add(clone)
        return clone

    def place(self, x: int, y: int, gamemap: Optional[GameMap] = None) -> None:
        """Place this entity at a new location.  Handles moving across GameMaps."""
        self.x = x
        self.y = y
        if gamemap:
-           if hasattr(self, "gamemap"):  # Possibly uninitialized.
-               self.gamemap.entities.remove(self)
-           self.gamemap = gamemap
+           if hasattr(self, "parent"):  # Possibly uninitialized.
+               if self.parent is self.gamemap:
+                   self.gamemap.entities.remove(self)
+           self.parent = gamemap
            gamemap.entities.add(self)

    def move(self, dx: int, dy: int) -> None:
        # Move the entity by a given amount
        self.x += dx
        self.y += dy


class Actor(Entity):
    def __init__(
        self,
        *,
        x: int = 0,
        y: int = 0,
        char: str = "?",
        color: Tuple[int, int, int] = (255, 255, 255),
        name: str = "<Unnamed>",
        ai_cls: Type[BaseAI],
        fighter: Fighter
    ):
        super().__init__(
            x=x,
            y=y,
            char=char,
            color=color,
            name=name,
            blocks_movement=True,
            render_order=RenderOrder.ACTOR,
        )

        self.ai: Optional[BaseAI] = ai_cls(self)

        self.fighter = fighter
-       self.fighter.entity = self
+       self.fighter.parent = self

    @property
    def is_alive(self) -> bool:
        """Returns True as long as this actor can perform actions."""
        return bool(self.ai)
```

`base_component.py`

Diff Original

```diff
from __future__ import annotations

from typing import TYPE_CHECKING

if TYPE_CHECKING:
    from engine import Engine
    from entity import Entity
+   from game_map import GameMap


class BaseComponent:
-   entity: Entity  # Owning entity instance.
+   parent: Entity  # Owning entity instance.

+   @property
+   def gamemap(self) -> GameMap:
+       return self.parent.gamemap

    @property
    def engine(self) -> Engine:
-       return self.entity.gamemap.engine
+       return self.gamemap.engine
```

`fighter.py`

Diff Original

```diff
class Fighter(BaseComponent):
-   entity: Actor
+   parent: Actor

    def __init__(self, hp: int, defense: int, power: int):
        self.max_hp = hp
        self._hp = hp
        self.defense = defense
        self.power = power

    @property
    def hp(self) -> int:
        return self._hp

    @hp.setter
    def hp(self, value: int) -> None:
        self._hp = max(0, min(value, self.max_hp))
-       if self._hp == 0 and self.entity.ai:
+       if self._hp == 0 and self.parent.ai:
            self.die()

    def die(self) -> None:
-       if self.engine.player is self.entity:
+       if self.engine.player is self.parent:
            death_message = "You died!"
            death_message_color = color.player_die
            self.engine.event_handler = GameOverEventHandler(self.engine)
        else:
-           death_message = f"{self.entity.name} is dead!"
+           death_message = f"{self.parent.name} is dead!"
            death_message_color = color.enemy_die

+       self.parent.char = "%"
+       self.parent.color = (191, 0, 0)
+       self.parent.blocks_movement = False
+       self.parent.ai = None
+       self.parent.name = f"remains of {self.parent.name}"
+       self.parent.render_order = RenderOrder.CORPSE
-       self.entity.char = "%"
-       self.entity.color = (191, 0, 0)
-       self.entity.blocks_movement = False
-       self.entity.ai = None
-       self.entity.name = f"remains of {self.entity.name}"
-       self.entity.render_order = RenderOrder.CORPSE

        self.engine.message_log.add_message(death_message, death_message_color)
```

## Part 8 [](https://rogueliketutorials.com/tutorials/tcod/v2/part-8/#part-8)

So far, our game has movement, dungeon exploring, combat, and AI (okay, we’re stretching the meaning of “intelligence” in _artificial intelligence_ to its limits, but bear with me here). Now it’s time for another staple of the roguelike genre: items! Why would our rogue venture into the dungeons of doom if not for some sweet loot, after all?

In this part of the tutorial, we’ll achieve a few things: a working inventory, and a functioning healing potion. The next part will add more items that can be picked up, but for now, just the healing potion will suffice.

For this part, we’ll need four more colors. Let’s get adding those out of the way now. Open up `color.py` and add these colors:

Diff Original

```diff
white = (0xFF, 0xFF, 0xFF)
black = (0x0, 0x0, 0x0)

player_atk = (0xE0, 0xE0, 0xE0)
enemy_atk = (0xFF, 0xC0, 0xC0)

player_die = (0xFF, 0x30, 0x30)
enemy_die = (0xFF, 0xA0, 0x30)

+invalid = (0xFF, 0xFF, 0x00)
+impossible = (0x80, 0x80, 0x80)
+error = (0xFF, 0x40, 0x40)

welcome_text = (0x20, 0xA0, 0xFF)
+health_recovered = (0x0, 0xFF, 0x0)

bar_text = white
bar_filled = (0x0, 0x60, 0x0)
bar_empty = (0x40, 0x10, 0x10)
```

These will become useful shortly.

There’s another thing we can knock out right now that we’ll use later: The ability for a `Fighter` component to recover health, and the ability to take damage directly (without the defense modifier). We won’t use the damage function this chapter, but since the two functions are effectively opposites, we can get writing it over with now.

Open up `fighter.py` and add these two functions:

Diff Original

```diff
class Fighter:
    ...
+   def heal(self, amount: int) -> int:
+       if self.hp == self.max_hp:
+           return 0

+       new_hp_value = self.hp + amount

+       if new_hp_value > self.max_hp:
+           new_hp_value = self.max_hp

+       amount_recovered = new_hp_value - self.hp

+       self.hp = new_hp_value

+       return amount_recovered

+   def take_damage(self, amount: int) -> None:
+       self.hp -= amount
```

`heal` will restore a certain amount of HP, up to the maximum, and return the amount that was healed. If the entity’s health is at full, then just return 0. The function that handles this should display an error if the returned amount is 0, since the entity can’t be healed.

One thing we’re going to need is a way to _not_ consume an item or take a turn if something goes wrong during the process. For our health potion, think about what should happen if the player declares they want to use a health potion, but their health is already full. What should happen?

We could just consume the potion anyway, and have it go to waste, but if you’ve played a game that does that, you know how frustrating it can be, especially if the player clicked the health potion on accident. A better way would be to warn the user that they’re trying to do something that makes no sense, and save the player from wasting both the potion and their turn.

But how can we achieve that? We’ll discuss it a bit more later on, but the idea is that if we do something impossible, we should raise an exception. Which one? Well, we can define a custom exception, which can give us details on what happened. Create a new file called `exceptions.py` and put the following class into it:

```python
class Impossible(Exception):
    """Exception raised when an action is impossible to be performed.

    The reason is given as the exception message.
    """
```

… And that’s it! When we write `raise Impossible("An exception message")` in our program, the `Impossible` exception will be raised, with the given message.

So what do we do with the raised exception? Well, we should catch it! But where?

Let’s modify the `main.py` file to catch the exceptions, like this:

Diff Original

```diff
#!/usr/bin/env python3
import copy
+import traceback

import tcod

...

            context.present(root_console)

+           try:
+               for event in tcod.event.wait():
+                   context.convert_event(event)
+                   engine.event_handler.handle_events(event)
+           except Exception:  # Handle exceptions in game.
+               traceback.print_exc()  # Print error to stderr.
+               # Then print the error to the message log.
+               engine.message_log.add_message(traceback.format_exc(), color.error)
-           engine.event_handler.handle_events(context)
```

This is a generalized, catch all solution, which will print _all_ exceptions to the message log, not just instances of `Impossible`. This can be helpful for debugging your game, or getting error reports from users.

However, this solution doesn’t mesh with our current implementation of the `EventHandler`. `EventHandler` currently loops through the events and converts them (to get the mouse information). We’ll need to edit a few things in `input_handlers.py` to get back on track.

Diff Original

```diff
import tcod

+from actions import (
+   Action,
+   BumpAction,
+   EscapeAction,
+   WaitAction
+)
+import color
+import exceptions
-from actions import Action, BumpAction, EscapeAction, WaitAction


class EventHandler(tcod.event.EventDispatch[Action]):
    def __init__(self, engine: Engine):
        self.engine = engine

+   def handle_events(self, event: tcod.event.Event) -> None:
+       self.handle_action(self.dispatch(event))

+   def handle_action(self, action: Optional[Action]) -> bool:
+       """Handle actions returned from event methods.

+       Returns True if the action will advance a turn.
+       """
+       if action is None:
+           return False

+       try:
+           action.perform()
+       except exceptions.Impossible as exc:
+           self.engine.message_log.add_message(exc.args[0], color.impossible)
+           return False  # Skip enemy turn on exceptions.

+       self.engine.handle_enemy_turns()

+       self.engine.update_fov()
+       return True

-   def handle_events(self, context: tcod.context.Context) -> None:
-       for event in tcod.event.wait():
-           context.convert_event(event)
-           self.dispatch(event)

    ...


class MainGameEventHandler(EventHandler):
-   def handle_events(self, context: tcod.context.Context) -> None:
-       for event in tcod.event.wait():
-           context.convert_event(event)

-           action = self.dispatch(event)

-           if action is None:
-               continue

-           action.perform()

-           self.engine.handle_enemy_turns()

-           self.engine.update_fov()  # Update the FOV before the players next action.

    ...

class GameOverEventHandler(EventHandler):
-   def handle_events(self, context: tcod.context.Context) -> None:
-       for event in tcod.event.wait():
-           action = self.dispatch(event)

-           if action is None:
-               continue

-           action.perform()

-   def ev_keydown(self, event: tcod.event.KeyDown) -> Optional[Action]:
-       action: Optional[Action] = None

-       key = event.sym

-       if key == tcod.event.K_ESCAPE:
-           action = EscapeAction(self.engine.player)

-       # No valid key was pressed
-       return action
+   def ev_keydown(self, event: tcod.event.KeyDown) -> None:
+       if event.sym == tcod.event.K_ESCAPE:
+           raise SystemExit()
```

Now that we’ve got our event handlers updated, let’s actually put the `Impossible` exception to good use. We can start by editing `actions.py` to make use of it when the player tries to move into an invalid area:

Diff Original

```diff
...
import color
+import exceptions

if TYPE_CHECKING:
    ...


class MeleeAction(ActionWithDirection):
    def perform(self) -> None:
        target = self.target_actor
        if not target:
-           return  # No entity to attack.
+           raise exceptions.Impossible("Nothing to attack.")

        ...

class MovementAction(ActionWithDirection):
    def perform(self) -> None:
        dest_x, dest_y = self.dest_xy

        if not self.engine.game_map.in_bounds(dest_x, dest_y):
+           # Destination is out of bounds.
+           raise exceptions.Impossible("That way is blocked.")
-           return  # Destination is out of bounds.
         if not self.engine.game_map.tiles["walkable"][dest_x, dest_y]:
+           # Destination is blocked by a tile.
+           raise exceptions.Impossible("That way is blocked.")
-           return  # Destination is blocked by a tile.
         if self.engine.game_map.get_blocking_entity_at_location(dest_x, dest_y):
+           # Destination is blocked by an entity.
+           raise exceptions.Impossible("That way is blocked.")
-           return  # Destination is blocked by an entity.
```

Now, if you try moving into a wall, you’ll get a message in the log, and the player’s turn won’t be wasted.

So what about when the enemies try doing something impossible? You might want to know when that happens for debugging purposes, but during normal execution of our game, we can simply ignore it, and have the enemy skip their turn. To do this, modify `engine.py` like this:

Diff Original

```diff
...
from tcod.map import compute_fov

+import exceptions
from input_handlers import MainGameEventHandler
from message_log import MessageLog

...

    def handle_enemy_turns(self) -> None:
        for entity in set(self.game_map.actors) - {self.player}:
            if entity.ai:
+               try:
+                   entity.ai.perform()
+               except exceptions.Impossible:
+                   pass  # Ignore impossible action exceptions from AI.
-               entity.ai.perform()
```

This is great and all, but wasn’t this chapter supposed to be about implementing items? And, yes, that’s true, and we’re going to transition to that now, but it’ll be helpful to have a way to stop the player from wasting a turn in just a moment.

The way we’ll implement our health potions will be similar to how we implemented enemies: We’ll create a component that holds the functionality we want, and we’ll create a subclass of `Entity` that holds the relevant component. From the `Consumable` component, we can create subclasses that implement the specific functionality we want for each item. In this case, it’ll be a health potion, but in the next chapter, we’ll be implementing other types of consumables, so we’ll want to stay flexible.

In the `components` directory, create a file called `consumable.py` and fill it with the following contents:

```python
from __future__ import annotations

from typing import Optional, TYPE_CHECKING

import actions
import color
from components.base_component import BaseComponent
from exceptions import Impossible

if TYPE_CHECKING:
    from entity import Actor, Item


class Consumable(BaseComponent):
    parent: Item

    def get_action(self, consumer: Actor) -> Optional[actions.Action]:
        """Try to return the action for this item."""
        return actions.ItemAction(consumer, self.parent)

    def activate(self, action: actions.ItemAction) -> None:
        """Invoke this items ability.

        `action` is the context for this activation.
        """
        raise NotImplementedError()


class HealingConsumable(Consumable):
    def __init__(self, amount: int):
        self.amount = amount

    def activate(self, action: actions.ItemAction) -> None:
        consumer = action.entity
        amount_recovered = consumer.fighter.heal(self.amount)

        if amount_recovered > 0:
            self.engine.message_log.add_message(
                f"You consume the {self.parent.name}, and recover {amount_recovered} HP!",
                color.health_recovered,
            )
        else:
            raise Impossible(f"Your health is already full.")
```

The `Consumable` class knows its parent, and it defines two methods: `get_action` and `activate`.

`get_action` gets `ItemAction`, which we haven’t defined just yet (we will soon). Subclasses can override this to provide more information to `ItemAction` if needed, such as the position of a potential target (this will be useful when we have ranged targeting).

`activate` is just an abstract method, it’s up to the subclasses to define their own implementation. The subclasses should call this method when they’re trying to actually cause the effect that they’ve defined for themselves (healing for healing potions, damage for lightning scrolls, etc.).

`HealingConsumable` is initialized with an `amount`, which is how much the user will be healed when using the item. The `activate` function calls `fighter.heal`, and logs a message to the message log, if the entity recovered health. If not (because the user had full health already), we return that `Impossible` exception we defined earlier. This will give us a message in the log that the player’s health is already full, and it won’t waste the health potion.

So what does this component get attached to? In order to create our health potions, we can create another subclass of `Entity`, which will represent non-actor items. Open up `entity.py` and add the following class:

Diff Original

```diff
from render_order import RenderOrder

if TYPE_CHECKING:
    from components.ai import BaseAI
+   from components.consumable import Consumable
    from components.fighter import Fighter
    from game_map import GameMap

...

    ...
    @property
    def is_alive(self) -> bool:
        """Returns True as long as this actor can perform actions."""
        return bool(self.ai)


+class Item(Entity):
+   def __init__(
+       self,
+       *,
+       x: int = 0,
+       y: int = 0,
+       char: str = "?",
+       color: Tuple[int, int, int] = (255, 255, 255),
+       name: str = "<Unnamed>",
+       consumable: Consumable,
+   ):
+       super().__init__(
+           x=x,
+           y=y,
+           char=char,
+           color=color,
+           name=name,
+           blocks_movement=False,
+           render_order=RenderOrder.ITEM,
+       )

+       self.consumable = consumable
+       self.consumable.parent = self
```

`Item` isn’t too different from `Actor`, except instead of implementing `fighter` and `ai`, it does `consumable`. When we create an item, we’ll assign the `consumable`, which will determine what actually happens when the item gets used.

The next thing we need to implement that we used in the `Consumable` class is the `ItemAction` class. Open up `actions.py` and put the following:

Diff Original

```diff
if TYPE_CHECKING:
    from engine import Engine
-   from entity import Actor, Entity
+   from entity import Actor, Entity, Item
...


class Action:
    ...


+class ItemAction(Action):
+   def __init__(
+       self, entity: Actor, item: Item, target_xy: Optional[Tuple[int, int]] = None
+   ):
+       super().__init__(entity)
+       self.item = item
+       if not target_xy:
+           target_xy = entity.x, entity.y
+       self.target_xy = target_xy

+   @property
+   def target_actor(self) -> Optional[Actor]:
+       """Return the actor at this actions destination."""
+       return self.engine.game_map.get_actor_at_location(*self.target_xy)

+   def perform(self) -> None:
+       """Invoke the items ability, this action will be given to provide context."""
+       self.item.consumable.activate(self)


class EscapeAction(Action):
    ...
```

`ItemAction` takes several arguments in its `__init__` function: `entity`, which is the entity using the item, `item`, which is the item itself, and `target_xy`, which is the x and y coordinates of the “target” of the item, if there is one. We won’t actually use this in this chapter, but it’ll come in handy soon.

`target_actor` gets the actor at the target location. Again, we won’t actually use it this chapter, since health potions don’t “target” anything.

`perform` activates the consumable, with its `activate` method we defined earlier.

To utilize our new `Item`, let’s add the health potion to `entity_factories.py`:

Diff Original

```diff
from components.ai import HostileEnemy
+from components.consumable import HealingConsumable
from components.fighter import Fighter
-from entity import Actor
+from entity import Actor, Item


player = Actor(
    char="@",
    color=(255, 255, 255),
    name="Player",
    ai_cls=HostileEnemy,
    fighter=Fighter(hp=30, defense=2, power=5),
)

orc = Actor(
    char="o",
    color=(63, 127, 63),
    name="Orc",
    ai_cls=HostileEnemy,
    fighter=Fighter(hp=10, defense=0, power=3),
)
troll = Actor(
    char="T",
    color=(0, 127, 0),
    name="Troll",
    ai_cls=HostileEnemy,
    fighter=Fighter(hp=16, defense=1, power=4),
)

+health_potion = Item(
+   char="!",
+   color=(127, 0, 255),
+   name="Health Potion",
+   consumable=HealingConsumable(amount=4),
+)
```

We’re defining a new entity type, called `health_potion` (no surprises there), and utilizing the `Item` and `HealingConsumable` classes we just wrote. The health potion will recover 4 HP of the user’s health. Feel free to adjust that value however you see fit.

Alright, we’re now ready to put some health potions in the dungeon. As you may have already guessed, we’ll need to adjust the `generate_dungeon` and `place_entities` functions in `procgen.py` to actually put the potions in. Edit `procgen.py` like this:

Diff Original

```diff
def place_entities(
-   room: RectangularRoom, dungeon: GameMap, maximum_monsters: int,
+   room: RectangularRoom, dungeon: GameMap, maximum_monsters: int, maximum_items: int
) -> None:
    number_of_monsters = random.randint(0, maximum_monsters)
+   number_of_items = random.randint(0, maximum_items)

    for i in range(number_of_monsters):
        x = random.randint(room.x1 + 1, room.x2 - 1)
        y = random.randint(room.y1 + 1, room.y2 - 1)

        if not any(entity.x == x and entity.y == y for entity in dungeon.entities):
            if random.random() < 0.8:
                entity_factories.orc.spawn(dungeon, x, y)
            else:
                entity_factories.troll.spawn(dungeon, x, y)

+   for i in range(number_of_items):
+       x = random.randint(room.x1 + 1, room.x2 - 1)
+       y = random.randint(room.y1 + 1, room.y2 - 1)

+       if not any(entity.x == x and entity.y == y for entity in dungeon.entities):
+           entity_factories.health_potion.spawn(dungeon, x, y)


def tunnel_between(
    ...


def generate_dungeon(
    map_width: int,
    map_height: int,
    max_monsters_per_room: int,
+   max_items_per_room: int,
    engine: Engine,
) -> GameMap:
    """Generate a new dungeon map."""
    ...

        ...
-       place_entities(new_room, dungeon, max_monsters_per_room)
+       place_entities(new_room, dungeon, max_monsters_per_room, max_items_per_room)
```

We’re doing essentially the same thing we did to create our enemies: Giving a maximum possible number for the number of items in each room, selecting a random number between that and 0, and spawning the items in a random spot in the room, assuming nothing else already exists there.

Lastly, to make the health potions appear, we need to update our call in `main.py` to `generate_dungeon`, since we’ve added the `max_items_per_room` argument. Open up `main.py` and add the following lines:

Diff Original

```diff
    ...
    max_monsters_per_room = 2
+   max_items_per_room = 2

    tileset = tcod.tileset.load_tilesheet(
        "dejavu10x10_gs_tc.png", 32, 8, tcod.tileset.CHARMAP_TCOD
    )

    player = copy.deepcopy(entity_factories.player)

    engine = Engine(player=player)

    engine.game_map = generate_dungeon(
        max_rooms=max_rooms,
        room_min_size=room_min_size,
        room_max_size=room_max_size,
        map_width=map_width,
        map_height=map_height,
        max_monsters_per_room=max_monsters_per_room,
+       max_items_per_room=max_items_per_room,
        engine=engine,
    )
    ...
```

Run the project now, and you should see a few health potions laying around. Success! Well, not really…

![Part 8 - Health Potions](https://rogueliketutorials.com/images/part-8-health-potions.png)

Those potions don’t do our rogue any good right now, because we can’t pick them up! We need to add the items to an inventory before we can start chugging them.

To implement the inventory, we can create a new component, called `Inventory`. Create a new file in the `components` directory, called `inventory.py`, and add this class:

```python
from __future__ import annotations

from typing import List, TYPE_CHECKING

from components.base_component import BaseComponent

if TYPE_CHECKING:
    from entity import Actor, Item


class Inventory(BaseComponent):
    parent: Actor

    def __init__(self, capacity: int):
        self.capacity = capacity
        self.items: List[Item] = []

    def drop(self, item: Item) -> None:
        """
        Removes an item from the inventory and restores it to the game map, at the player's current location.
        """
        self.items.remove(item)
        item.place(self.parent.x, self.parent.y, self.gamemap)

        self.engine.message_log.add_message(f"You dropped the {item.name}.")
```

The `Inventory` class belongs to an `Actor`, and its initialized with a `capacity`, which is the maximum number of items that can be held, and the `items` list, which will actually hold the items. The `drop` method, as the name implies, will be called when the player decides to drop something out of the inventory, back onto the ground.

Let’s add this new component to our `Actor` class. Open up `entity.py` and modify `Actor` like this:

Diff Original

```diff
...
if TYPE_CHECKING:
    from components.ai import BaseAI
    from components.consumable import Consumable
    from components.fighter import Fighter
+   from components.inventory import Inventory
    from game_map import GameMap

...
class Actor(Entity):
    def __init__(
        self,
        *,
        x: int = 0,
        y: int = 0,
        char: str = "?",
        color: Tuple[int, int, int] = (255, 255, 255),
        name: str = "<Unnamed>",
        ai_cls: Type[BaseAI],
        fighter: Fighter,
+       inventory: Inventory,
    ):
        super().__init__(
            x=x,
            y=y,
            char=char,
            color=color,
            name=name,
            blocks_movement=True,
            render_order=RenderOrder.ACTOR,
        )

        self.ai: Optional[BaseAI] = ai_cls(self)

        self.fighter = fighter
        self.fighter.parent = self

+       self.inventory = inventory
+       self.inventory.parent = self
```

Now, each actor will have their own inventory. Our tutorial won’t implement monster inventories (they won’t pick up, hold, or use items), but hopefully this setup gives you a good starting place to implement it yourself, if you so choose.

We’ll need to update `entity_factories.py` to take the new component into account:

Diff Original

```diff
from components.ai import HostileEnemy
from components.consumable import HealingConsumable
from components.fighter import Fighter
+from components.inventory import Inventory
from entity import Actor, Item

player = Actor(
    char="@",
    color=(255, 255, 255),
    name="Player",
    ai_cls=HostileEnemy,
    fighter=Fighter(hp=30, defense=2, power=5),
+   inventory=Inventory(capacity=26),
)

orc = Actor(
    char="o",
    color=(63, 127, 63),
    name="Orc",
    ai_cls=HostileEnemy,
    fighter=Fighter(hp=10, defense=0, power=3),
+   inventory=Inventory(capacity=0),
)
troll = Actor(
    char="T",
    color=(0, 127, 0),
    name="Troll",
    ai_cls=HostileEnemy,
    fighter=Fighter(hp=16, defense=1, power=4),
+   inventory=Inventory(capacity=0),
)

health_potion = Item(
    char="!",
    color=(127, 0, 255),
    name="Health Potion",
    consumable=HealingConsumable(amount=4),
)
```

We’re setting the player’s inventory to 26, because when we implement the menu system, each letter in the (English) alphabet will correspond to one item slot. You can expand the inventory if you want, though you’ll need to come up with an alternative menu system to accommodate having more choices.

In order to actually pick up an item of the floor, we’ll require the rogue to move onto the same tile and press a key. First, we’ll want an easy way to grab all the items that currently exist in the map. Open up `game_map.py` and add the following:

Diff Original

```diff
...
import numpy as np  # type: ignore
from tcod.console import Console

-from entity import Actor
+from entity import Actor, Item
import tile_types
...

    ...
    @property
    def actors(self) -> Iterator[Actor]:
        """Iterate over this maps living actors."""
        yield from (
            entity
            for entity in self.entities
            if isinstance(entity, Actor) and entity.is_alive
        )

+   @property
+   def items(self) -> Iterator[Item]:
+       yield from (entity for entity in self.entities if isinstance(entity, Item))

    def get_blocking_entity_at_location(
        ...
```

We can use this new property in an action to find the item(s) on the same tile as the player. Let’s define a `PickupAction`, which will handle picking up the item and adding it to the inventory.

Open up `actions.py` and define `PickupAction` like this:

Diff Original

```diff
class Action:
    ...


+class PickupAction(Action):
+   """Pickup an item and add it to the inventory, if there is room for it."""

+   def __init__(self, entity: Actor):
+       super().__init__(entity)

+   def perform(self) -> None:
+       actor_location_x = self.entity.x
+       actor_location_y = self.entity.y
+       inventory = self.entity.inventory

+       for item in self.engine.game_map.items:
+           if actor_location_x == item.x and actor_location_y == item.y:
+               if len(inventory.items) >= inventory.capacity:
+                   raise exceptions.Impossible("Your inventory is full.")

+               self.engine.game_map.entities.remove(item)
+               item.parent = self.entity.inventory
+               inventory.items.append(item)

+               self.engine.message_log.add_message(f"You picked up the {item.name}!")
+               return

+       raise exceptions.Impossible("There is nothing here to pick up.")


class ItemAction(Action):
    ...
```

The action gets the entity’s location, and tries to find an item that exists in the same location, iterating through `self.engine.game_map.items` (which we just defined). If an item is found, we try to add it to the inventory, checking the capacity first, and returning `Impossible` if its full. When adding an item to the inventory, we remove it from the game map and store it in the inventory, and print out a message. We then return, since only one item can be picked up per turn (it’ll be possible later for multiple items to be on the same spot).

If no item is found in the location, we just return `Impossible`, informing the player that there’s nothing there.

Let’s add our new action to the event handler. Open up `input_handlers.py` and edit the key checking section of `MainGameEventHandler` to add the key for picking up items:

Diff Original

```diff
from actions import (
    Action,
    BumpAction,
    EscapeAction,
+   PickupAction,
    WaitAction,
)
...

        ...
        elif key == tcod.event.K_v:
            self.engine.event_handler = HistoryViewer(self.engine)

+       elif key == tcod.event.K_g:
+           action = PickupAction(player)

        # No valid key was pressed
        return action
```

Simple enough, if the player presses the “g” key (“g” for “get”), we call the `PickupAction`. Run the project now, and pick up those potions!

Now that the player can pick up items, we’ll need to create our inventory menu, where the player can see what items are in the inventory, and select which one to use. This will require a few steps.

First, we need a way to get input from the user. When the user opens the inventory menu, we need to get the input from the user, and if it was valid, we return to the main game’s event handler, so the enemies can take their turns.

To start, let’s create a new event handler, which will return to the `MainGameEventHandler` when it handles an action successfully. Open `input_handlers.py` and add the following class:

Diff Original

```diff
class EventHandler(tcod.event.EventDispatch[Action]):
    ...


+class AskUserEventHandler(EventHandler):
+   """Handles user input for actions which require special input."""

+   def handle_action(self, action: Optional[Action]) -> bool:
+       """Return to the main event handler when a valid action was performed."""
+       if super().handle_action(action):
+           self.engine.event_handler = MainGameEventHandler(self.engine)
+           return True
+       return False

+   def ev_keydown(self, event: tcod.event.KeyDown) -> Optional[Action]:
+       """By default any key exits this input handler."""
+       if event.sym in {  # Ignore modifier keys.
+           tcod.event.K_LSHIFT,
+           tcod.event.K_RSHIFT,
+           tcod.event.K_LCTRL,
+           tcod.event.K_RCTRL,
+           tcod.event.K_LALT,
+           tcod.event.K_RALT,
+       }:
+           return None
+       return self.on_exit()

+   def ev_mousebuttondown(self, event: tcod.event.MouseButtonDown) -> Optional[Action]:
+       """By default any mouse click exits this input handler."""
+       return self.on_exit()

+   def on_exit(self) -> Optional[Action]:
+       """Called when the user is trying to exit or cancel an action.

+       By default this returns to the main event handler.
+       """
+       self.engine.event_handler = MainGameEventHandler(self.engine)
+       return None
```

`AskUserEventHandler`, by default, just exits itself when any key is pressed, besides one of the “modifier” keys (shift, control, and alt). It also exits when clicking the mouse.

What’s the point of this class? By itself, nothing really. But we can create subclasses of it that actually do something useful, which is what we’ll do now. Let’s keep editing `input_handlers.py` and add this class:

Diff Original

```diff
if TYPE_CHECKING:
    from engine import Engine
+   from entity import Item
...


class AskUserEventHandler(EventHandler):
    ...


+class InventoryEventHandler(AskUserEventHandler):
+   """This handler lets the user select an item.

+   What happens then depends on the subclass.
+   """

+   TITLE = "<missing title>"

+   def on_render(self, console: tcod.Console) -> None:
+       """Render an inventory menu, which displays the items in the inventory, and the letter to select them.
+       Will move to a different position based on where the player is located, so the player can always see where
+       they are.
+       """
+       super().on_render(console)
+       number_of_items_in_inventory = len(self.engine.player.inventory.items)

+       height = number_of_items_in_inventory + 2

+       if height <= 3:
+           height = 3

+       if self.engine.player.x <= 30:
+           x = 40
+       else:
+           x = 0

+       y = 0

+       width = len(self.TITLE) + 4

+       console.draw_frame(
+           x=x,
+           y=y,
+           width=width,
+           height=height,
+           title=self.TITLE,
+           clear=True,
+           fg=(255, 255, 255),
+           bg=(0, 0, 0),
+       )

+       if number_of_items_in_inventory > 0:
+           for i, item in enumerate(self.engine.player.inventory.items):
+               item_key = chr(ord("a") + i)
+               console.print(x + 1, y + i + 1, f"({item_key}) {item.name}")
+       else:
+           console.print(x + 1, y + 1, "(Empty)")

+   def ev_keydown(self, event: tcod.event.KeyDown) -> Optional[Action]:
+       player = self.engine.player
+       key = event.sym
+       index = key - tcod.event.K_a

+       if 0 <= index <= 26:
+           try:
+               selected_item = player.inventory.items[index]
+           except IndexError:
+               self.engine.message_log.add_message("Invalid entry.", color.invalid)
+               return None
+           return self.on_item_selected(selected_item)
+       return super().ev_keydown(event)

+   def on_item_selected(self, item: Item) -> Optional[Action]:
+       """Called when the user selects a valid item."""
+       raise NotImplementedError()
```

`InventoryEventHandler` subclasses `AskUserEventHandler`, and renders the items within the player’s `Inventory`. Depending on where the player is standing, the menu will render off to the side, so the menu won’t cover the player. If there’s nothing in the inventory, it just prints “Empty”. Notice that it doesn’t give itself a title, as that will be defined in a different subclass (more on that in a bit).

The `ev_keydown` function takes the user’s input, from letters a - z, and associates that with an index in the inventory. If the player pressed “b”, for example, the second item in the inventory will be selected and returned. If the player presses a key like “c” (item 3) but only has one item, then the message “Invalid entry” will display. If any other key is pressed, the menu will close.

This class, still, does not actually do anything for us right now, but I promise we’re close. Before we implement the menus to use and drop items, we’ll need to define the `Action` that drops items. Add the following class to `actions.py`:

Diff Original

```diff
class EscapeAction(Action):
    def perform(self) -> None:
        raise SystemExit()


+class DropItem(ItemAction):
+   def perform(self) -> None:
+       self.entity.inventory.drop(self.item)


class WaitAction(Action):
    def perform(self) -> None:
        pass
...
```

`DropItem` will be used to drop something from the inventory. It just calls the `drop` method of the `Inventory` component.

Now, let’s put this new action into… well… action! Open up `input_handlers.py` once again, and let’s add the handlers that will handle both selecting an item and dropping one.

Diff Original

```diff
...
import tcod

+import actions
from actions import (
    Action,
    BumpAction,
    EscapeAction,
    PickupAction,
    WaitAction,
)
...


class InventoryEventHandler(AskUserEventHandler):
    ...


+class InventoryActivateHandler(InventoryEventHandler):
+   """Handle using an inventory item."""

+   TITLE = "Select an item to use"

+   def on_item_selected(self, item: Item) -> Optional[Action]:
+       """Return the action for the selected item."""
+       return item.consumable.get_action(self.engine.player)


+class InventoryDropHandler(InventoryEventHandler):
+   """Handle dropping an inventory item."""

+   TITLE = "Select an item to drop"

+   def on_item_selected(self, item: Item) -> Optional[Action]:
+       """Drop this item."""
+       return actions.DropItem(self.engine.player, item)
```

At long last, we’ve got the `InventoryActivateHandler` and the `InventoryDropHandler`, which will handle using and dropping items, respectively. They both inherit from `InventoryEventHandler`, allowing the player to select an item in both menus using what we wrote in that class (selecting an item with a letter), but both handlers display a different title and call different actions, depending on the selection.

All that’s left now is to utilize these event handlers, based on the key we press. Let’s set the game up to open the inventory menu when pressing “i”, and the drop menu when pressing “d”. Open `input_handlers.py`, and add the following lines to `MainGameEventHandler`:

Diff Original

```diff
        ...
        elif key == tcod.event.K_g:
            action = PickupAction(player)

+       elif key == tcod.event.K_i:
+           self.engine.event_handler = InventoryActivateHandler(self.engine)
+       elif key == tcod.event.K_d:
+           self.engine.event_handler = InventoryDropHandler(self.engine)

        # No valid key was pressed
        return action
```

Now, when you run the project, you can, at long last, use and drop the health potions!

![Part 8 - Using items](https://rogueliketutorials.com/images/part-8-using-items.png)

There’s a major bug with our implementation though: used items won’t disappear after using them. This means the player could keep consuming the same health potion over and over!

Let’s fix that, by opening up `consumable.py` and add the following:

Diff Original

```diff
from __future__ import annotations

from typing import Optional, TYPE_CHECKING

import actions
import color
+import components.inventory
from components.base_component import BaseComponent
from exceptions import Impossible

if TYPE_CHECKING:
    from entity import Actor, Item


class Consumable(BaseComponent):
    parent: Item

    def get_action(self, consumer: Actor) -> Optional[actions.Action]:
        """Try to return the action for this item."""
        return actions.ItemAction(consumer, self.parent)

    def activate(self, action: actions.ItemAction) -> None:
        """Invoke this items ability.

        `action` is the context for this activation.
        """
        raise NotImplementedError()

+   def consume(self) -> None:
+       """Remove the consumed item from its containing inventory."""
+       entity = self.parent
+       inventory = entity.parent
+       if isinstance(inventory, components.inventory.Inventory):
+           inventory.items.remove(entity)


class HealingConsumable(Consumable):
    def __init__(self, amount: int):
        self.amount = amount

    def activate(self, action: actions.ItemAction) -> None:
        consumer = action.entity
        amount_recovered = consumer.fighter.heal(self.amount)

        if amount_recovered > 0:
            self.engine.message_log.add_message(
                f"You consume the {self.parent.name}, and recover {amount_recovered} HP!",
                color.health_recovered,
            )
+           self.consume()
        else:
            raise Impossible(f"Your health is already full.")
```

`consume` removes the item from the `Inventory` container it occupies. Since it no longer belongs to the inventory or the map, it disappears from the game. We use the `consume` method when the health potion is successfully used, and we don’t if it’s not.

With that, the health potions will disappear after use.

There’s two last bits of housekeeping we need to do before moving on to the next part. The `parent` class attribute in the `Entity` class has a bit of a problem: it’s designated as a `GameMap` type right now, but when an item moves from the map to the inventory, that isn’t really true any more. Let’s fix that now:

Diff Original

```diff
from __future__ import annotations

import copy
-from typing import Optional, Tuple, Type, TypeVar, TYPE_CHECKING
+from typing import Optional, Tuple, Type, TypeVar, TYPE_CHECKING, Union

from render_order import RenderOrder

...
class Entity:
    """
    A generic object to represent players, enemies, items, etc.
    """

-   parent: GameMap
+   parent: Union[GameMap, Inventory]

    def __init__(
        ...
```

Lastly, we can actually remove `EscapeAction`, as it can just be handled by the event handlers. Open `actions.py` and remove `EscapeAction`:

Diff Original

```diff
class PickupAction(Action):
    ...


-class EscapeAction(Action):
-   def perform(self) -> None:
-       raise SystemExit()


class ItemAction(Action):
    ...
```

Then, remove `EscapeAction` from `input_handlers.py`:

Diff Original

```diff
...
from actions import (
    Action,
    BumpAction,
-   EscapeAction,
    PickupAction,
    WaitAction
)
...

        ...
        elif key == tcod.event.K_ESCAPE:
-           action = EscapeAction(player)
+           raise SystemExit()
        elif key == tcod.event.K_v:
            self.engine.event_handler = HistoryViewer(self.engine)
        ...
```

This was another long chapter, but this is an important step towards a functioning game. Next chapter, we’ll add a few more item types to use.

If you want to see the code so far in its entirety, [click here](https://github.com/TStand90/tcod_tutorial_v2/tree/2020/part-8).

[Click here to move on to the next part of this tutorial.](https://rogueliketutorials.com/tutorials/tcod/v2/part-9)

---
# [Part 9 - Ranged Scrolls and Targeting](https://rogueliketutorials.com/tutorials/tcod/v2/part-9/)

Adding health potions was a big step, but we won’t stop there. Let’s continue adding a few items, this time with a focus on offense. We’ll add a few scrolls, which will give the player a one-time ranged attack. This gives the player a lot more tactical options to work with, and is definitely something you’ll want to expand upon in your own game.

Before we get to that, let’s start by adding the colors we’ll need for this chapter:

Diff Original

```diff
white = (0xFF, 0xFF, 0xFF)
black = (0x0, 0x0, 0x0)
+red = (0xFF, 0x0, 0x0)

player_atk = (0xE0, 0xE0, 0xE0)
enemy_atk = (0xFF, 0xC0, 0xC0)
+needs_target = (0x3F, 0xFF, 0xFF)
+status_effect_applied = (0x3F, 0xFF, 0x3F)

player_die = (0xFF, 0x30, 0x30)
enemy_die = (0xFF, 0xA0, 0x30)

invalid = (0xFF, 0xFF, 0x00)
impossible = (0x80, 0x80, 0x80)
error = (0xFF, 0x40, 0x40)

welcome_text = (0x20, 0xA0, 0xFF)
health_recovered = (0x0, 0xFF, 0x0)

bar_text = white
bar_filled = (0x0, 0x60, 0x0)
bar_empty = (0x40, 0x10, 0x10)
```

Let’s start simple, with a spell that just hits the closest enemy. We’ll create a scroll of lightning, which automatically targets an enemy nearby the player.

First thing we need is a way to get the closest entity to the entity casting the spell. Let’s add a `distance` function to `Entity`, which will give us the distance to an arbitrary point. Open `entity.py` and add the following function:

Diff Original

```diff
from __future__ import annotations

import copy
+import math
from typing import Optional, Tuple, Type, TypeVar, TYPE_CHECKING, Union
...

    ...
    def place(self, x: int, y: int, gamemap: Optional[GameMap] = None) -> None:
        ...

+   def distance(self, x: int, y: int) -> float:
+       """
+       Return the distance between the current entity and the given (x, y) coordinate.
+       """
+       return math.sqrt((x - self.x) ** 2 + (y - self.y) ** 2)

    def move(self, dx: int, dy: int) -> None:
        ...
```

With that, we can add the component that will handle shooting our lightning bolt. Add the following class to `consumable.py`:

Diff Original

```diff
class HealingConsumable(Consumable):
    ...


+class LightningDamageConsumable(Consumable):
+   def __init__(self, damage: int, maximum_range: int):
+       self.damage = damage
+       self.maximum_range = maximum_range

+   def activate(self, action: actions.ItemAction) -> None:
+       consumer = action.entity
+       target = None
+       closest_distance = self.maximum_range + 1.0

+       for actor in self.engine.game_map.actors:
+           if actor is not consumer and self.parent.gamemap.visible[actor.x, actor.y]:
+               distance = consumer.distance(actor.x, actor.y)

+               if distance < closest_distance:
+                   target = actor
+                   closest_distance = distance

+       if target:
+           self.engine.message_log.add_message(
+               f"A lighting bolt strikes the {target.name} with a loud thunder, for {self.damage} damage!"
+           )
+           target.fighter.take_damage(self.damage)
+           self.consume()
+       else:
+           raise Impossible("No enemy is close enough to strike.")
```

The `__init__` function takes two arguments: `damage`, which dictates how powerful the lightning bolt will be, and `maximum_range`, which tells us how far it can reach.

Similar to `HealingConsumable`, this class has an `activate` function that describes what to do when the player tries using it. It loops through the actors in the current map, and if the actor is visible and within range, it chooses that actor as the one to strike. If a target was found, we strike the target, dealing the damage (using the `take_damage` function we defined last time, which ignores defense) and printing out a message. If no target was found, we give an error, and don’t consume the scroll.

In order to use this, we’ll need to actually place some lightning scrolls on the map. We can do that by adding the scroll to `entity_factories.py`, and then adjusting the `place_entities` function in `procgen.py`. Let’s start with `entity_factories.py`:

Diff Original

```diff
from components.ai import HostileEnemy
-from components.consumable import HealingConsumable
+from components import consumable
from components.fighter import Fighter
from components.inventory import Inventory
from entity import Actor, Item

...
health_potion = Item(
    char="!",
    color=(127, 0, 255),
    name="Health Potion",
-   consumable=HealingConsumable(amount=4),
+   consumable=consumable.HealingConsumable(amount=4),
)
+lightning_scroll = Item(
+   char="~",
+   color=(255, 255, 0),
+   name="Lightning Scroll",
+   consumable=consumable.LightningDamageConsumable(damage=20, maximum_range=5),
+)
```

Notice that we also are importing `consumable` instead of the specific classes inside, which affects our declaration of `health_potion`. This will save us from having to add a new import every time we create a new consumable class.

Now, for `procgen.py`:

Diff Original

```diff
    ...
    for i in range(number_of_items):
        x = random.randint(room.x1 + 1, room.x2 - 1)
        y = random.randint(room.y1 + 1, room.y2 - 1)

        if not any(entity.x == x and entity.y == y for entity in dungeon.entities):
-           entity_factories.health_potion.spawn(dungeon, x, y)
+           item_chance = random.random()

+           if item_chance < 0.7:
+               entity_factories.health_potion.spawn(dungeon, x, y)
+           else:
+               entity_factories.lightning_scroll.spawn(dungeon, x, y)
```

Like with the monsters, we’re getting a random number and deciding what to spawn based on a percentage chance. Most of our items will still be health potions, but we should have a chance of getting a lightning scroll instead now.

Run the project, and try picking up some lightning scrolls and zapping some trolls!

![Part 9 - Lightning Scrolls](https://rogueliketutorials.com/images/part-9-lightning-scrolls.png)

That one was a bit on the easy side. Let’s try something a little more challenging, something that requires us to target an enemy (or an area) before shooting off the spell.

This will take a few steps, but one of the things we can do on the way to that goal is add a way for the player to “look around” the map using either the mouse or keyboard. We already kind of did this with the mouse in part 7, however, most roguelikes allow the user to play the game entirely with the keyboard.

Open up `input_handlers.py` and add the following contents:

Diff Original

```diff
...
WAIT_KEYS = {
    tcod.event.K_PERIOD,
    tcod.event.K_KP_5,
    tcod.event.K_CLEAR,
}

+CONFIRM_KEYS = {
+   tcod.event.K_RETURN,
+   tcod.event.K_KP_ENTER,
+}

...
class InventoryDropHandler(InventoryEventHandler):
    ...


+class SelectIndexHandler(AskUserEventHandler):
+   """Handles asking the user for an index on the map."""

+   def __init__(self, engine: Engine):
+       """Sets the cursor to the player when this handler is constructed."""
+       super().__init__(engine)
+       player = self.engine.player
+       engine.mouse_location = player.x, player.y

+   def on_render(self, console: tcod.Console) -> None:
+       """Highlight the tile under the cursor."""
+       super().on_render(console)
+       x, y = self.engine.mouse_location
+       console.tiles_rgb["bg"][x, y] = color.white
+       console.tiles_rgb["fg"][x, y] = color.black

+   def ev_keydown(self, event: tcod.event.KeyDown) -> Optional[Action]:
+       """Check for key movement or confirmation keys."""
+       key = event.sym
+       if key in MOVE_KEYS:
+           modifier = 1  # Holding modifier keys will speed up key movement.
+           if event.mod & (tcod.event.KMOD_LSHIFT | tcod.event.KMOD_RSHIFT):
+               modifier *= 5
+           if event.mod & (tcod.event.KMOD_LCTRL | tcod.event.KMOD_RCTRL):
+               modifier *= 10
+           if event.mod & (tcod.event.KMOD_LALT | tcod.event.KMOD_RALT):
+               modifier *= 20

+           x, y = self.engine.mouse_location
+           dx, dy = MOVE_KEYS[key]
+           x += dx * modifier
+           y += dy * modifier
+           # Clamp the cursor index to the map size.
+           x = max(0, min(x, self.engine.game_map.width - 1))
+           y = max(0, min(y, self.engine.game_map.height - 1))
+           self.engine.mouse_location = x, y
+           return None
+       elif key in CONFIRM_KEYS:
+           return self.on_index_selected(*self.engine.mouse_location)
+       return super().ev_keydown(event)

+   def ev_mousebuttondown(self, event: tcod.event.MouseButtonDown) -> Optional[Action]:
+       """Left click confirms a selection."""
+       if self.engine.game_map.in_bounds(*event.tile):
+           if event.button == 1:
+               return self.on_index_selected(*event.tile)
+       return super().ev_mousebuttondown(event)

+   def on_index_selected(self, x: int, y: int) -> Optional[Action]:
+       """Called when an index is selected."""
+       raise NotImplementedError()


+class LookHandler(SelectIndexHandler):
+   """Lets the player look around using the keyboard."""

+   def on_index_selected(self, x: int, y: int) -> None:
+       """Return to main handler."""
+       self.engine.event_handler = MainGameEventHandler(self.engine)


class MainGameEventHandler(EventHandler):
    ...
```

`SelectIndexHandler` is what we’ll use when we want to select a tile on the map. It has several methods, which we’ll break down now.

`__init__` simply sets the `mouse_location` to the player’s current location. This is so that the cursor we’re about to draw appears over the player first, rather than somewhere else. Chances are, the tile the player wants to select will be nearby.

`on_render` will render the console as normal, by calling `super().on_render`, but it also adds a cursor on top, that can be used to show where the current cursor position is. This is especially useful if the player is navigating around with the keyboard.

`ev_keydown` gives us a way to move the cursor we’re drawing around using the keyboard instead of the mouse (using the mouse is still possible). By using the same movement keys we use to move the player around, we can move the cursor around, with a few extra options. By holding, shift, control, or alt while pressing a movement key, the cursor will move around faster by skipping over a few spaces. This could be very helpful if you plan on making your map larger. If the user presses a “confirm” key, the method returns the current cursor’s location.

`ev_mousebuttondown` also returns the location, if the clicked space is within the map boundaries.

`on_index_selected` is an abstract method, which will be up to the subclasses to implement. We do that immediately with `LookHandler`.

`LookHandler` inherits from `SelectIndexHandler`, and all it does is return to the `MainGameEventHandler` when receiving a confirmation key. This is because it doesn’t need to do anything special, it’s just used in the case where our player wants to have a look around.

We can utilize `LookHandler` by adding this to `ev_keydown` in `MainGameEventHandler`:

Diff Original

```diff
        ...
        elif key == tcod.event.K_i:
            self.engine.event_handler = InventoryActivateHandler(self.engine)
        elif key == tcod.event.K_d:
            self.engine.event_handler = InventoryDropHandler(self.engine)
+       elif key == tcod.event.K_SLASH:
+           self.engine.event_handler = LookHandler(self.engine)

        # No valid key was pressed
        return action
```

By pressing the forward slash key, you can look around the map with either the mouse or keyboard. Pressing the Escape key (or any non-movement key for that matter) exits this mode.

Alright, with that in place, we can move on to implementing a scroll that asks for a target. Let’s implement a confusion scroll, which will take a target, and change that target’s AI so that it stumbles around for a few turns before returning to normal.

We need to define a new type of AI to handle how enemies act when they’re confused. Open up `ai.py` and add the following:

Diff Original

```diff
from __future__ import annotations

+import random
-from typing import List, Tuple, TYPE_CHECKING
+from typing import List, Optional, Tuple, TYPE_CHECKING

import numpy as np  # type: ignore
import tcod

-from actions import Action, MeleeAction, MovementAction, WaitAction
+from actions import Action, BumpAction, MeleeAction, MovementAction, WaitAction

if TYPE_CHECKING:
    from entity import Actor


class BaseAI(Action):
    ...


+class ConfusedEnemy(BaseAI):
+   """
+   A confused enemy will stumble around aimlessly for a given number of turns, then revert back to its previous AI.
+   If an actor occupies a tile it is randomly moving into, it will attack.
+   """

+   def __init__(
+       self, entity: Actor, previous_ai: Optional[BaseAI], turns_remaining: int
+   ):
+       super().__init__(entity)

+       self.previous_ai = previous_ai
+       self.turns_remaining = turns_remaining

+   def perform(self) -> None:
+       # Revert the AI back to the original state if the effect has run its course.
+       if self.turns_remaining <= 0:
+           self.engine.message_log.add_message(
+               f"The {self.entity.name} is no longer confused."
+           )
+           self.entity.ai = self.previous_ai
+       else:
+           # Pick a random direction
+           direction_x, direction_y = random.choice(
+               [
+                   (-1, -1),  # Northwest
+                   (0, -1),  # North
+                   (1, -1),  # Northeast
+                   (-1, 0),  # West
+                   (1, 0),  # East
+                   (-1, 1),  # Southwest
+                   (0, 1),  # South
+                   (1, 1),  # Southeast
+               ]
+           )

+           self.turns_remaining -= 1

+           # The actor will either try to move or attack in the chosen random direction.
+           # Its possible the actor will just bump into the wall, wasting a turn.
+           return BumpAction(self.entity, direction_x, direction_y,).perform()
```

The `__init__` function takes three arguments:

- `entity`: The actor who is being confused.
- `previous_ai`: The AI class that the actor currently has. We need this, because when the confusion effect wears off, we’ll want to revert the entity back to its previous AI.
- `turns_remaining`: How many turns the confusion effect will last for.

`perform` causes the entity to move in a randomly selected direction. It uses `BumpAction`, which means that it will try to move into a tile, and if there’s an actor there, it will attack it (regardless if its the player or another monster). Each turn, the `turns_remaining` will decrement, and when it’s less than or equal to zero, the AI reverts back and the entity is no longer confused.

In order to inflict this status on an enemy, we’ll need to do a few things. Obviously, we need a consumable that inflicts the `ConfusedEnemy` AI on an enemy, but we also need a way to select which enemy gets confused.

To do that, let’s expand on our `SelectIndexHandler` from earlier. We can create a handler that allows us to select a single enemy and apply some sort of function on it. Open up `input_handlers.py` and add the following class:

Diff Original

```diff
from __future__ import annotations

-from typing import Optional, TYPE_CHECKING
+from typing import Callable, Optional, Tuple, TYPE_CHECKING

import tcod
...


class LookHandler(SelectIndexHandler):
    ...


+class SingleRangedAttackHandler(SelectIndexHandler):
+   """Handles targeting a single enemy. Only the enemy selected will be affected."""

+   def __init__(
+       self, engine: Engine, callback: Callable[[Tuple[int, int]], Optional[Action]]
+   ):
+       super().__init__(engine)

+       self.callback = callback

+   def on_index_selected(self, x: int, y: int) -> Optional[Action]:
+       return self.callback((x, y))


class MainGameEventHandler(EventHandler):
    ...
```

`SingleRangedAttackHandler` doesn’t do much, except define a `callback` function that activates when the user selects a target. `callback` can be any function with a Tuple of two integers (x and y coordinates), so `SingleRangedAttackHandler` can be used for any scroll or ranged attack that targets one location.

So what do we pass as the `callback`? Let’s define that now, in `consumable.py`. We’ll add the component that causes the confusion effect, called `ConfusionConsumable`. It looks like this:

Diff Original

```diff
...
import color
+import components.ai
from components.base_component import BaseComponent
from exceptions import Impossible
+from input_handlers import SingleRangedAttackHandler

if TYPE_CHECKING:
    from entity import Actor, Item


class Consumable(BaseComponent):
    parent: Item

    def consume(self, consumer: Actor) -> None:
        raise NotImplementedError()


+class ConfusionConsumable(Consumable):
+   def __init__(self, number_of_turns: int):
+       self.number_of_turns = number_of_turns

+   def get_action(self, consumer: Actor) -> Optional[actions.Action]:
+       self.engine.message_log.add_message(
+           "Select a target location.", color.needs_target
+       )
+       self.engine.event_handler = SingleRangedAttackHandler(
+           self.engine,
+           callback=lambda xy: actions.ItemAction(consumer, self.parent, xy),
+       )
+       return None

+   def activate(self, action: actions.ItemAction) -> None:
+       consumer = action.entity
+       target = action.target_actor

+       if not self.engine.game_map.visible[action.target_xy]:
+           raise Impossible("You cannot target an area that you cannot see.")
+       if not target:
+           raise Impossible("You must select an enemy to target.")
+       if target is consumer:
+           raise Impossible("You cannot confuse yourself!")

+       self.engine.message_log.add_message(
+           f"The eyes of the {target.name} look vacant, as it starts to stumble around!",
+           color.status_effect_applied,
+       )
+       target.ai = components.ai.ConfusedEnemy(
+           entity=target, previous_ai=target.ai, turns_remaining=self.number_of_turns,
+       )
+       self.consume()


class HealingConsumable(Consumable):
    ...
```

`ConfusionConsumable` takes one argument in `__init__`, which is `number_of_turns`. As you might have guessed, this represents the number of turns that the confusion effect lasts for.

`get_action` will ask the player to select a target location, and switch the game’s event handler to `SingleRangedAttackHandler`. The `callback` is a `lambda` function (an anonymous, inline function), which takes “xy” as a parameter. “xy” will be the coordinates of the target. The lambda function executes `ItemAction`, which receives the consumer, the parent (the item), and the “xy” coordinates.

`activate` is what happens when the player selects a target. First, we get the actor at the location, and make sure that the target is,

1. In sight
2. A valid actor
3. Not the player

If all those things are true, then we apply the `ConfusedEnemy` AI to that target, and consume the scroll.

With the consumable component in place, we can add `confusion_scroll` to `entity_factories.py`:

Diff Original

```diff
troll = Actor(
    ...
)

+confusion_scroll = Item(
+   char="~",
+   color=(207, 63, 255),
+   name="Confusion Scroll",
+   consumable=consumable.ConfusionConsumable(number_of_turns=10),
+)
health_potion = Item(
    ...
```

Now that we can create confusion scrolls, let’s add some to the map. Open up `procgen.py` and adjust the part that places items to look like this:

Diff Original

```diff
            ...
            if item_chance < 0.7:
                entity_factories.health_potion.spawn(dungeon, x, y)
+           elif item_chance < 0.9:
+               entity_factories.confusion_scroll.spawn(dungeon, x, y)
            else:
                entity_factories.lightning_scroll.spawn(dungeon, x, y)
```

Feel free to adjust these percentage values however you see fit. To test out your confusion scrolls, you might want to mess with the numbers here.

Run the project now, and cast some confusion on your enemies!

![Part 9 - Confusion Scrolls](https://rogueliketutorials.com/images/part-9-confusion-scrolls.png)

So we currently have two types of ranged spells to use: One that targets the nearest enemy automatically, and one that asks for a target. We’ll finish this chapter by implementing a third type: One that asks for a target, but affects everything within a certain radius of that target. I’m talking, of course, about an exploding fireball spell!

To implement our fireball, we’ll need a new event handler. `SingleRangedAttackHandler` isn’t quite enough, because it targets one enemy actor and nothing else. For our fireball, we want to select an _area_ to hit which can include multiple targets, and might even burn the player! It’s not actually necessary that the cursor be on an enemy either; the fireball can be offset to catch multiple enemies in its blast radius.

So, with that in mind, let’s implement a new event handler, which will handle area of effect attacks. We can call it `AreaRangedAttackHandler`, and define it like this:

Diff Original

```diff
class SingleRangedAttackHandler(SelectIndexHandler):
    ...


+class AreaRangedAttackHandler(SelectIndexHandler):
+   """Handles targeting an area within a given radius. Any entity within the area will be affected."""

+   def __init__(
+       self,
+       engine: Engine,
+       radius: int,
+       callback: Callable[[Tuple[int, int]], Optional[Action]],
+   ):
+       super().__init__(engine)

+       self.radius = radius
+       self.callback = callback

+   def on_render(self, console: tcod.Console) -> None:
+       """Highlight the tile under the cursor."""
+       super().on_render(console)

+       x, y = self.engine.mouse_location

+       # Draw a rectangle around the targeted area, so the player can see the affected tiles.
+       console.draw_frame(
+           x=x - self.radius - 1,
+           y=y - self.radius - 1,
+           width=self.radius ** 2,
+           height=self.radius ** 2,
+           fg=color.red,
+           clear=False,
+       )

+   def on_index_selected(self, x: int, y: int) -> Optional[Action]:
+       return self.callback((x, y))


class MainGameEventHandler(EventHandler):
    ...
```

`AreaRangedAttackHandler` takes a `callback`, like `SingleRangedAttackHandler`, but also defies a `radius`, which tells us how large the area of effect will be.

`on_render` highlights the cursor, but also draws a “frame” (an empty rectangle) around the area we’ll be targeting. This will help the player determine which area will be in the blast.

`on_index_selected` is the same as the one we defined for `SingleRangedAttackHandler`.

To do the damage, we’ll need to implement the `Consumable` class for the fireball scroll. Open up `consumable.py` and add this class:

Diff Original

```diff
...
from exceptions import Impossible
-from input_handlers import SingleRangedAttackHandler
+from input_handlers import AreaRangedAttackHandler, SingleRangedAttackHandler

if TYPE_CHECKING:
    ...


class HealingConsumable(Consumable):
    ...


+class FireballDamageConsumable(Consumable):
+   def __init__(self, damage: int, radius: int):
+       self.damage = damage
+       self.radius = radius

+   def get_action(self, consumer: Actor) -> Optional[actions.Action]:
+       self.engine.message_log.add_message(
+           "Select a target location.", color.needs_target
+       )
+       self.engine.event_handler = AreaRangedAttackHandler(
+           self.engine,
+           radius=self.radius,
+           callback=lambda xy: actions.ItemAction(consumer, self.parent, xy),
+       )
+       return None

+   def activate(self, action: actions.ItemAction) -> None:
+       target_xy = action.target_xy

+       if not self.engine.game_map.visible[target_xy]:
+           raise Impossible("You cannot target an area that you cannot see.")

+       targets_hit = False
+       for actor in self.engine.game_map.actors:
+           if actor.distance(*target_xy) <= self.radius:
+               self.engine.message_log.add_message(
+                   f"The {actor.name} is engulfed in a fiery explosion, taking {self.damage} damage!"
+               )
+               actor.fighter.take_damage(self.damage)
+               targets_hit = True

+       if not targets_hit:
+           raise Impossible("There are no targets in the radius.")
+       self.consume()


class LightningDamageConsumable(Consumable):
    ...
```

`FireballDamageConsumable` takes `damage` and `radius` as arguments in `__init__`, which shouldn’t be too surprising.

`get_action`, similar to the confusion scroll, asks the user to select a target, and switches the event handler, this time to `AreaRangedAttackHandler`. The callback is once again a `lambda` function, which is similar to how we handled the confusion scroll.

`activate` gets the target location, and ensures that it is within the line of sight. It then checks for entities within the radius, damaging any that are close enough to hit (take note, there’s no exception for the player, so you can get blasted by your own fireball!). If no enemies were hit at all, the `Impossible` exception is raised, and the scroll isn’t consumed, as it would probably be frustrating to waste a scroll on something like a misclick. Assuming at least one entity _was_ damaged, the scroll is consumed.

Let’s add the new fireball scroll to `entity_factories.py` so we can put it to use:

Diff Original

```diff
confusion_scroll = Item(
    ...
)
+fireball_scroll = Item(
+   char="~",
+   color=(255, 0, 0),
+   name="Fireball Scroll",
+   consumable=consumable.FireballDamageConsumable(damage=12, radius=3),
+)
health_potion = Item(
    ...
```

Finally, let’s add it to `procgen.py` so it will show up:

Diff Original

```diff
            if item_chance < 0.7:
                entity_factories.health_potion.spawn(dungeon, x, y)
+           elif item_chance < 0.8:
+               entity_factories.fireball_scroll.spawn(dungeon, x, y)
            elif item_chance < 0.9:
                entity_factories.confusion_scroll.spawn(dungeon, x, y)
            else:
                entity_factories.lightning_scroll.spawn(dungeon, x, y)
```

Run the project now, and blast away your enemies!

![Part 9 - Fireball Targeting](https://rogueliketutorials.com/images/part-9-fireball-targeting.png)

With that, we’ve now got three different types of scrolls, and four types of consumables overall! With the event handlers that are in place, it should be fairly simple to add more types of consumables, if you wish. Feel free to experiment with different types of attacks, and add variety to your game.

If you want to see the code so far in its entirety, [click here](https://github.com/TStand90/tcod_tutorial_v2/tree/2020/part-9).

[Click here to move on to the next part of this tutorial.](https://rogueliketutorials.com/tutorials/tcod/v2/part-10)

---
# [Part 10 - Saving and loading](https://rogueliketutorials.com/tutorials/tcod/v2/part-10/)

Saving and loading is essential to almost every roguelike, but it can be a pain to manage if you don’t start early. By the end of this chapter, our game will be able to save and load one file to the disk, which you could easily expand to multiple saves if you wanted to.

Let’s start by defining the colors we’ll need this chapter, by opening `color.py` and entering the following:

Diff Original

```diff
...
bar_empty = (0x40, 0x10, 0x10)

+menu_title = (255, 255, 63)
+menu_text = white
```

Another thing we’ll need is a new type of exception. This will be used when we want to quit the game, but not save it. Normally, we’ll save the game when the user quits, but if the game is over (because the player is dead), we don’t want to create a save file.

We can put this exception in `exceptions.py`:

Diff Original

```diff
class Impossible(Exception):
    """Exception raised when an action is impossible to be performed.

    The reason is given as the exception message.
    """


+class QuitWithoutSaving(SystemExit):
+   """Can be raised to exit the game without automatically saving."""
```

There’s a bit of refactoring we can do to make things easier for ourselves in the future: By creating a base class that can be either an Action or an EventHandler, we don’t need to set the engine’s “event handler” to the new handler when we want to switch, we can just return that event handler instead. A benefit of this is that the `Engine` class won’t need to store the event handler anymore. This works by keeping track of the handler in the `main.py` file instead, and switching it when necessary.

To make the change, start by adding the following to `input_handlers.py`:

Diff Original

```diff
from __future__ import annotations

-from typing import Callable, Optional, Tuple, TYPE_CHECKING
+from typing import Callable, Optional, Tuple, TYPE_CHECKING, Union

import tcod
...

...
CONFIRM_KEYS = {
    tcod.event.K_RETURN,
    tcod.event.K_KP_ENTER,
}


+ActionOrHandler = Union[Action, "BaseEventHandler"]
+"""An event handler return value which can trigger an action or switch active handlers.

+If a handler is returned then it will become the active handler for future events.
+If an action is returned it will be attempted and if it's valid then
+MainGameEventHandler will become the active handler.
+"""


+class BaseEventHandler(tcod.event.EventDispatch[ActionOrHandler]):
+   def handle_events(self, event: tcod.event.Event) -> BaseEventHandler:
+       """Handle an event and return the next active event handler."""
+       state = self.dispatch(event)
+       if isinstance(state, BaseEventHandler):
+           return state
+       assert not isinstance(state, Action), f"{self!r} can not handle actions."
+       return self

+   def on_render(self, console: tcod.Console) -> None:
+       raise NotImplementedError()

+   def ev_quit(self, event: tcod.event.Quit) -> Optional[Action]:
+       raise SystemExit()
```

As the docstring explains, `ActionOrHandler` can be either an `Action` or an `EventHandler`. If it’s an `Action`, the action will be attempted, and if its a handler, the handler will be changed.

`BaseEventHandler` will be the base class for all of our handlers (we’ll change that to be the case next). It will return a new instance of `BaseEventHandler` or its subclasses if one was returned, or return itself. This allows us to change event handlers based on the context of what happens in the actions.

We also need to adjust `EventHandler`:

Diff Original

```diff
-class EventHandler(tcod.event.EventDispatch[Action]):
+class EventHandler(BaseEventHandler):
    def __init__(self, engine: Engine):
        self.engine = engine

-   def handle_events(self, event: tcod.event.Event) -> None:
-       self.handle_action(self.dispatch(event))
+   def handle_events(self, event: tcod.event.Event) -> BaseEventHandler:
+       """Handle events for input handlers with an engine."""
+       action_or_state = self.dispatch(event)
+       if isinstance(action_or_state, BaseEventHandler):
+           return action_or_state
+       if self.handle_action(action_or_state):
+           # A valid action was performed.
+           if not self.engine.player.is_alive:
+               # The player was killed sometime during or after the action.
+               return GameOverEventHandler(self.engine)
+           return MainGameEventHandler(self.engine)  # Return to the main handler.
+       return self

    def handle_action(self, action: Optional[Action]) -> bool:
        ...

-   def ev_quit(self, event: tcod.event.Quit) -> Optional[Action]:
-       raise SystemExit()

    def on_render(self, console: tcod.Console) -> None:
        self.engine.render(console)
```

The `handle_events` method of `EventHandler` is similar to `BaseEventHandler`, except it includes logic to handle actions as well. It also contains the logic for changing our handler to a game over if the player is dead.

To adjust our existing handlers, we’ll need to continue editing `input_handlers`. This next code section is quite long, but the idea is consistent throughout: We want to modify our return types to return `Optional[ActionOrHandler]` instead of `Optional[Action]`, and instead of setting `self.engine.event_handler` to change the handler, we’ll return the handler instead.

Diff Original

```diff
class AskUserEventHandler(EventHandler):
    """Handles user input for actions which require special input."""

-   def handle_action(self, action: Optional[Action]) -> bool:
-       """Return to the main event handler when a valid action was performed."""
-       if super().handle_action(action):
-           self.engine.event_handler = MainGameEventHandler(self.engine)
-           return True
-       return False

-   def ev_keydown(self, event: tcod.event.KeyDown) -> Optional[Action]:
+   def ev_keydown(self, event: tcod.event.KeyDown) -> Optional[ActionOrHandler]:
        """By default any key exits this input handler."""
        if event.sym in {  # Ignore modifier keys.
            tcod.event.K_LSHIFT,
            tcod.event.K_RSHIFT,
            tcod.event.K_LCTRL,
            tcod.event.K_RCTRL,
            tcod.event.K_LALT,
            tcod.event.K_RALT,
        }:
            return None
        return self.on_exit()

-   def ev_mousebuttondown(self, event: tcod.event.MouseButtonDown) -> Optional[Action]:
+   def ev_mousebuttondown(
+       self, event: tcod.event.MouseButtonDown
+   ) -> Optional[ActionOrHandler]:
        """By default any mouse click exits this input handler."""
        return self.on_exit()

-   def on_exit(self) -> Optional[Action]:
+   def on_exit(self) -> Optional[ActionOrHandler]:
        """Called when the user is trying to exit or cancel an action.

        By default this returns to the main event handler.
        """
-       self.engine.event_handler = MainGameEventHandler(self.engine)
-       return None
+       return MainGameEventHandler(self.engine)


class InventoryEventHandler(AskUserEventHandler):
    ...

-   def ev_keydown(self, event: tcod.event.KeyDown) -> Optional[Action]:
+   def ev_keydown(self, event: tcod.event.KeyDown) -> Optional[ActionOrHandler]:
        player = self.engine.player
        key = event.sym
        index = key - tcod.event.K_a

        if 0 <= index <= 26:
            try:
                selected_item = player.inventory.items[index]
            except IndexError:
                self.engine.message_log.add_message("Invalid entry.", color.invalid)
                return None
            return self.on_item_selected(selected_item)
        return super().ev_keydown(event)

-   def on_item_selected(self, item: Item) -> Optional[Action]:
+   def on_item_selected(self, item: Item) -> Optional[ActionOrHandler]:
        """Called when the user selects a valid item."""
        raise NotImplementedError()


class InventoryActivateHandler(InventoryEventHandler):
    """Handle using an inventory item."""

    TITLE = "Select an item to use"

-   def on_item_selected(self, item: Item) -> Optional[Action]:
+   def on_item_selected(self, item: Item) -> Optional[ActionOrHandler]:
        """Return the action for the selected item."""
        return item.consumable.get_action(self.engine.player)


class InventoryDropHandler(InventoryEventHandler):
    """Handle dropping an inventory item."""

    TITLE = "Select an item to drop"

-   def on_item_selected(self, item: Item) -> Optional[Action]:
+   def on_item_selected(self, item: Item) -> Optional[ActionOrHandler]:
        """Drop this item."""
        return actions.DropItem(self.engine.player, item)


class SelectIndexHandler(AskUserEventHandler):
    ...

-   def ev_keydown(self, event: tcod.event.KeyDown) -> Optional[Action]:
+   def ev_keydown(self, event: tcod.event.KeyDown) -> Optional[ActionOrHandler]:
        ...

-   def ev_mousebuttondown(self, event: tcod.event.MouseButtonDown) -> Optional[Action]:
+   def ev_mousebuttondown(
+       self, event: tcod.event.MouseButtonDown
+   ) -> Optional[ActionOrHandler]:
        ...

-   def on_index_selected(self, x: int, y: int) -> Optional[Action]:
+   def on_index_selected(self, x: int, y: int) -> Optional[ActionOrHandler]:
        """Called when an index is selected."""
        raise NotImplementedError()


class LookHandler(SelectIndexHandler):
    """Lets the player look around using the keyboard."""

-   def on_index_selected(self, x: int, y: int) -> None:
+   def on_index_selected(self, x: int, y: int) -> MainGameEventHandler:
        """Return to main handler."""
-       self.engine.event_handler = MainGameEventHandler(self.engine)
+       return MainGameEventHandler(self.engine)

...

class MainGameEventHandler(EventHandler):
-   def ev_keydown(self, event: tcod.event.KeyDown) -> Optional[Action]:
+   def ev_keydown(self, event: tcod.event.KeyDown) -> Optional[ActionOrHandler]:
        action: Optional[Action] = None

        key = event.sym

        player = self.engine.player

        if key in MOVE_KEYS:
            dx, dy = MOVE_KEYS[key]
            action = BumpAction(player, dx, dy)
        elif key in WAIT_KEYS:
            action = WaitAction(player)

        elif key == tcod.event.K_ESCAPE:
            raise SystemExit()
        elif key == tcod.event.K_v:
-           self.engine.event_handler = HistoryViewer(self.engine)
+           return HistoryViewer(self.engine)

        elif key == tcod.event.K_g:
            action = PickupAction(player)

        elif key == tcod.event.K_i:
-           self.engine.event_handler = InventoryActivateHandler(self.engine)
+           return InventoryActivateHandler(self.engine)
        elif key == tcod.event.K_d:
-           self.engine.event_handler = InventoryDropHandler(self.engine)
+           return InventoryDropHandler(self.engine)
        elif key == tcod.event.K_SLASH:
-           self.engine.event_handler = LookHandler(self.engine)
+           return LookHandler(self.engine)

        # No valid key was pressed
        return action


...
class HistoryViewer(EventHandler):
    ...

-   def ev_keydown(self, event: tcod.event.KeyDown) -> None:
+   def ev_keydown(self, event: tcod.event.KeyDown) -> Optional[MainGameEventHandler]:
        # Fancy conditional movement to make it feel right.
        if event.sym in CURSOR_Y_KEYS:
            adjust = CURSOR_Y_KEYS[event.sym]
            if adjust < 0 and self.cursor == 0:
                # Only move from the top to the bottom when you're on the edge.
                self.cursor = self.log_length - 1
            elif adjust > 0 and self.cursor == self.log_length - 1:
                # Same with bottom to top movement.
                self.cursor = 0
            else:
                # Otherwise move while staying clamped to the bounds of the history log.
                self.cursor = max(0, min(self.cursor + adjust, self.log_length - 1))
        elif event.sym == tcod.event.K_HOME:
            self.cursor = 0  # Move directly to the top message.
        elif event.sym == tcod.event.K_END:
            self.cursor = self.log_length - 1  # Move directly to the last message.
        else:  # Any other key moves back to the main game state.
-           self.engine.event_handler = MainGameEventHandler(self.engine)
+           return MainGameEventHandler(self.engine)
+       return None
```

We’ll also need to make a few adjustments in `consumable.py`, because some of the methods there also affected the input handlers.

Diff Original

```diff
...
import components.ai
import components.inventory
from components.base_component import BaseComponent
from exceptions import Impossible
-from input_handlers import AreaRangedAttackHandler, SingleRangedAttackHandler
+from input_handlers import (
+   ActionOrHandler,
+   AreaRangedAttackHandler,
+   SingleRangedAttackHandler,
+)

if TYPE_CHECKING:
    ...

...
class Consumable(BaseComponent):
    parent: Item

-   def get_action(self, consumer: Actor) -> Optional[actions.Action]:
+   def get_action(self, consumer: Actor) -> Optional[ActionOrHandler]:
        """Try to return the action for this item."""
        return actions.ItemAction(consumer, self.parent)

    ...

class ConfusionConsumable(Consumable):
    def __init__(self, number_of_turns: int):
        self.number_of_turns = number_of_turns

-   def get_action(self, consumer: Actor) -> Optional[actions.Action]:
+   def get_action(self, consumer: Actor) -> SingleRangedAttackHandler:
        self.engine.message_log.add_message(
            "Select a target location.", color.needs_target
        )
-       self.engine.event_handler = SingleRangedAttackHandler(
+       return SingleRangedAttackHandler(
            self.engine,
            callback=lambda xy: actions.ItemAction(consumer, self.parent, xy),
        )
-       return None

    ...

class FireballDamageConsumable(Consumable):
    def __init__(self, damage: int, radius: int):
        self.damage = damage
        self.radius = radius

-   def get_action(self, consumer: Actor) -> Optional[actions.Action]:
+   def get_action(self, consumer: Actor) -> AreaRangedAttackHandler:
        self.engine.message_log.add_message(
            "Select a target location.", color.needs_target
        )
-       self.engine.event_handler = AreaRangedAttackHandler(
+       return AreaRangedAttackHandler(
            self.engine,
            radius=self.radius,
            callback=lambda xy: actions.ItemAction(consumer, self.parent, xy),
        )
-       return None

    def activate(self, action: actions.ItemAction) -> None:
        ...
```

We also need to make a small adjustmet to `fighter.py`:

Diff Original

```diff
from typing import TYPE_CHECKING

import color
from components.base_component import BaseComponent
-from input_handlers import GameOverEventHandler
from render_order import RenderOrder

...

        ...
        if self.engine.player is self.parent:
            death_message = "You died!"
            death_message_color = color.player_die
-           self.engine.event_handler = GameOverEventHandler(self.engine)
        else:
            death_message = f"{self.parent.name} is dead!"
            death_message_color = color.enemy_die
        ...
```

Since the logic associated with setting `GameOverEventHandler` is now handled in the `EventHandler` class, this line in `fighter.py` wasn’t needed anymore.

In order to make these changes work, we need to adjust the way that the input handlers are… well, handled. What we’ll want to do is have the handler exist on its own in `main.py` rather than be part of the `Engine` class.

Open up `main.py` and add make the following changes:

Diff Original

```diff
#!/usr/bin/env python3
import copy
import traceback

import tcod

import color
from engine import Engine
import entity_factories
+import exceptions
+import input_handlers
from procgen import generate_dungeon

def main() -> None:
    ...
    engine.message_log.add_message(
        "Hello and welcome, adventurer, to yet another dungeon!", color.welcome_text
    )

+   handler: input_handlers.BaseEventHandler = input_handlers.MainGameEventHandler(engine)

    with tcod.context.new_terminal(
        screen_width,
        screen_height,
        tileset=tileset,
        title="Yet Another Roguelike Tutorial",
        vsync=True,
    ) as context:
        root_console = tcod.Console(screen_width, screen_height, order="F")
-       while True:
-           root_console.clear()
-           engine.event_handler.on_render(console=root_console)
-           context.present(root_console)

-           try:
-               for event in tcod.event.wait():
-                   context.convert_event(event)
-                   engine.event_handler.handle_events(event)
-           except Exception:  # Handle exceptions in game.
-               traceback.print_exc()  # Print error to stderr.
-               # Then print the error to the message log.
-               engine.message_log.add_message(traceback.format_exc(), color.error)
+       try:
+           while True:
+               root_console.clear()
+               handler.on_render(console=root_console)
+               context.present(root_console)

+               try:
+                   for event in tcod.event.wait():
+                       context.convert_event(event)
+                       handler = handler.handle_events(event)
+               except Exception:  # Handle exceptions in game.
+                   traceback.print_exc()  # Print error to stderr.
+                   # Then print the error to the message log.
+                   if isinstance(handler, input_handlers.EventHandler):
+                       handler.engine.message_log.add_message(
+                           traceback.format_exc(), color.error
+                       )
+       except exceptions.QuitWithoutSaving:
+           raise
+       except SystemExit:  # Save and quit.
+           # TODO: Add the save function here
+           raise
+       except BaseException:  # Save on any other unexpected exception.
+           # TODO: Add the save function here
+           raise
```

We’re now defining our `handler` in `main.py` rather than passing it to the `Engine`. The handler can change if a different handler is returned from `handler.handle_events`. We’ve also added a few exception statements for the various exception types. They all do the same thing at the moment, but soon, once we’ve implemented our save function, the `SystemExit` and `BaseException` exceptions will save the game before exiting. `QuitWithoutSaving` will not, as this will be called when we don’t want to save the game.

If you run the project now, things should work the same as before.

Let’s clean up the `Engine` class by removing the `event_handler` attribute, as we don’t need it anymore:

Diff Original

```diff
from __future__ import annotations

from typing import TYPE_CHECKING

from tcod.console import Console
from tcod.map import compute_fov

import exceptions
-from input_handlers import MainGameEventHandler
from message_log import MessageLog
from render_functions import (
    render_bar,
    render_names_at_mouse_location,
)

if TYPE_CHECKING:
    from entity import Actor
    from game_map import GameMap
-   from input_handlers import EventHandler


class Engine:
    game_map: GameMap

    def __init__(self, player: Actor):
-       self.event_handler: EventHandler = MainGameEventHandler(self)
        self.message_log = MessageLog()
        self.mouse_location = (0, 0)
        self.player = player
        ...
```

If you run the project again, nothing should change. The `event_handler` in `Engine` was not doing anything at this point.

Before we implement saving the game, we need to implement a main menu, where the user can choose to start a new game or load an existing one (or simply quit). It would also be handy to move all the logic of setting up a new game into a function, as we won’t need to call it when we eventually get to loading a game from a save file.

Create a new file, called `setup_game.py`, and put the following contents into it:

```python
"""Handle the loading and initialization of game sessions."""
from __future__ import annotations

import copy
from typing import Optional

import tcod

import color
from engine import Engine
import entity_factories
import input_handlers
from procgen import generate_dungeon


# Load the background image and remove the alpha channel.
background_image = tcod.image.load("menu_background.png")[:, :, :3]


def new_game() -> Engine:
    """Return a brand new game session as an Engine instance."""
    map_width = 80
    map_height = 43

    room_max_size = 10
    room_min_size = 6
    max_rooms = 30

    max_monsters_per_room = 2
    max_items_per_room = 2

    player = copy.deepcopy(entity_factories.player)

    engine = Engine(player=player)

    engine.game_map = generate_dungeon(
        max_rooms=max_rooms,
        room_min_size=room_min_size,
        room_max_size=room_max_size,
        map_width=map_width,
        map_height=map_height,
        max_monsters_per_room=max_monsters_per_room,
        max_items_per_room=max_items_per_room,
        engine=engine,
    )
    engine.update_fov()

    engine.message_log.add_message(
        "Hello and welcome, adventurer, to yet another dungeon!", color.welcome_text
    )
    return engine


class MainMenu(input_handlers.BaseEventHandler):
    """Handle the main menu rendering and input."""

    def on_render(self, console: tcod.Console) -> None:
        """Render the main menu on a background image."""
        console.draw_semigraphics(background_image, 0, 0)

        console.print(
            console.width // 2,
            console.height // 2 - 4,
            "TOMBS OF THE ANCIENT KINGS",
            fg=color.menu_title,
            alignment=tcod.CENTER,
        )
        console.print(
            console.width // 2,
            console.height - 2,
            "By (Your name here)",
            fg=color.menu_title,
            alignment=tcod.CENTER,
        )

        menu_width = 24
        for i, text in enumerate(
            ["[N] Play a new game", "[C] Continue last game", "[Q] Quit"]
        ):
            console.print(
                console.width // 2,
                console.height // 2 - 2 + i,
                text.ljust(menu_width),
                fg=color.menu_text,
                bg=color.black,
                alignment=tcod.CENTER,
                bg_blend=tcod.BKGND_ALPHA(64),
            )

    def ev_keydown(
        self, event: tcod.event.KeyDown
    ) -> Optional[input_handlers.BaseEventHandler]:
        if event.sym in (tcod.event.K_q, tcod.event.K_ESCAPE):
            raise SystemExit()
        elif event.sym == tcod.event.K_c:
            # TODO: Load the game here
            pass
        elif event.sym == tcod.event.K_n:
            return input_handlers.MainGameEventHandler(new_game())

        return None
```

Let’s break this code down a bit.

```python
background_image = tcod.image.load("menu_background.png")[:, :, :3]
```

This line loads the image file we’ll use for our background in the main menu. If you haven’t already, be sure to download that file. You can find it [here](https://github.com/TStand90/tcod_tutorial_v2/blob/bff29c352d76068845730ff61e62cd382e5e2adc/menu_background.png), or download it by right-clicking and saving it from here:

![Main Menu Background Image](https://rogueliketutorials.com/images/menu_background.png)

Save the file to your project directory and you should be good to go.

```python
def new_game() -> Engine:
    """Return a brand new game session as an Engine instance."""
    map_width = 80
    map_height = 43

    room_max_size = 10
    room_min_size = 6
    max_rooms = 30

    max_monsters_per_room = 2
    max_items_per_room = 2

    player = copy.deepcopy(entity_factories.player)

    engine = Engine(player=player)

    engine.game_map = generate_dungeon(
        max_rooms=max_rooms,
        room_min_size=room_min_size,
        room_max_size=room_max_size,
        map_width=map_width,
        map_height=map_height,
        max_monsters_per_room=max_monsters_per_room,
        max_items_per_room=max_items_per_room,
        engine=engine,
    )
    engine.update_fov()

    engine.message_log.add_message(
        "Hello and welcome, adventurer, to yet another dungeon!", color.welcome_text
    )
    return engine
```

This should all look very familiar: it’s the same code we used to initialize our engine in `main.py`. We initialize the same things here, but return the `Engine`, so that `main.py` can make use of it. This will help reduce the amount of code in `main.py` while also making sure that we don’t waste time initializing the engine class if we’re loading from a saved file.

```python
class MainMenu(input_handlers.BaseEventHandler):
    """Handle the main menu rendering and input."""

    def on_render(self, console: tcod.Console) -> None:
        """Render the main menu on a background image."""
        console.draw_semigraphics(background_image, 0, 0)

        console.print(
            console.width // 2,
            console.height // 2 - 4,
            "TOMBS OF THE ANCIENT KINGS",
            fg=color.menu_title,
            alignment=tcod.CENTER,
        )
        console.print(
            console.width // 2,
            console.height - 2,
            "By (Your name here)",
            fg=color.menu_title,
            alignment=tcod.CENTER,
        )

        menu_width = 24
        for i, text in enumerate(
            ["[N] Play a new game", "[C] Continue last game", "[Q] Quit"]
        ):
            console.print(
                console.width // 2,
                console.height // 2 - 2 + i,
                text.ljust(menu_width),
                fg=color.menu_text,
                bg=color.black,
                alignment=tcod.CENTER,
                bg_blend=tcod.BKGND_ALPHA(64),
            )

    def ev_keydown(
        self, event: tcod.event.KeyDown
    ) -> Optional[input_handlers.BaseEventHandler]:
        if event.sym in (tcod.event.K_q, tcod.event.K_ESCAPE):
            raise SystemExit()
        elif event.sym == tcod.event.K_c:
            # TODO: Load the game here
            pass
        elif event.sym == tcod.event.K_n:
            return input_handlers.MainGameEventHandler(new_game())

        return None
```

It might seem strange to put an event handler here rather than in its normal spot (`input_handlers.py`), but this makes sense for two reasons:

1. The main menu is specific to the start of the game.
2. It won’t be called during the normal course of the game, like the other input handlers in that file.

Anyway, the class renders the image we specified earlier, and it displays a title, `"TOMBS OF THE ANCIENT KINGS"`. Of course, you can change this to whatever name you have in mind for your game. It also includes a `"By (Your name here)"` section, so be sure to fill your name in and let everyone know who it was that worked so hard to make this game!

The menu also gives three choices: new game, continue last game, and quit. The `ev_keydown` method, as you might expect, handles these inputs.

- If the player presses “Q”, the game just exits.
- If the player presses “N”, a new game starts. We do this by returing the `MainGameEventHandler`, and calling the `new_game` function to create our new engine.
- If the player presses “C”, theoretically, a saved game should load. However, we haven’t gotten there yet, so as of now, nothing happens.

Let’s utilize our `MainMenu` function in `main.py`, like this:

Diff Original

```diff
#!/usr/bin/env python3
-import copy
import traceback

import tcod

import color
-from engine import Engine
-import entity_factories
import exceptions
import input_handlers
-from procgen import generate_dungeon
+import setup_game


def main() -> None:
    screen_width = 80
    screen_height = 50

-   map_width = 80
-   map_height = 43

-   room_max_size = 10
-   room_min_size = 6
-   max_rooms = 30

-   max_monsters_per_room = 2
-   max_items_per_room = 2

    tileset = tcod.tileset.load_tilesheet(
        "dejavu10x10_gs_tc.png", 32, 8, tcod.tileset.CHARMAP_TCOD
    )

-   player = copy.deepcopy(entity_factories.player)

-   engine = Engine(player=player)

-   engine.game_map = generate_dungeon(
-       max_rooms=max_rooms,
-       room_min_size=room_min_size,
-       room_max_size=room_max_size,
-       map_width=map_width,
-       map_height=map_height,
-       max_monsters_per_room=max_monsters_per_room,
-       max_items_per_room=max_items_per_room,
-       engine=engine,
-   )
-   engine.update_fov()

-   engine.message_log.add_message(
-       "Hello and welcome, adventurer, to yet another dungeon!", color.welcome_text
-   )

-   handler: input_handlers.BaseEventHandler = input_handlers.MainGameEventHandler(engine)
+   handler: input_handlers.BaseEventHandler = setup_game.MainMenu()

    with tcod.context.new_terminal(
        ...
```

We’re removing the code that dealt with setting up the engine, as that’s been moved into the `new_game` function. All we have to do here is set our handler to `MainMenu`, and the `MainMenu` class handles the rest from there.

Run the game now, and you should see the main menu!

![Part 10 - Main Menu](https://rogueliketutorials.com/images/part-10-main-menu.png)

*_Note: If you run the project and get this error: `Process finished with exit code 139 (interrupted by signal 11: SIGSEGV)`, it means you didn’t download the menu image file._

At last, we’ve come to the part where we’ll write the function that will save our game! This will be a method in `Engine`, and we’ll write it like this:

Diff Original

```diff
from __future__ import annotations

+import lzma
+import pickle
from typing import TYPE_CHECKING
...

class Engine:
    ...

+   def save_as(self, filename: str) -> None:
+       """Save this Engine instance as a compressed file."""
+       save_data = lzma.compress(pickle.dumps(self))
+       with open(filename, "wb") as f:
+           f.write(save_data)
```

`pickle.dumps` serializes an object hierarchy in Python. `lzma.compress` compresses the data, so it takes up less space. We then use `with open(filename, "wb") as f:` to write the file (`wb` means “write in binary mode”), calling `f.write(save_data)` to write the data.

It might be hard to believe, but this is all we need to save our game! Because all of the things we need to save exist in the `Engine` class, we can pickle it, and we’re done!

Of course, it’s not quite _that_ simple. We still need to call this method, and handle a few edge cases, like when the user tries to load a save file that doesn’t exist.

To save our game, we’ll call `save_as` from our `main.py` function. We’ll set up another function called `save_game` to call it, like this:

Diff Original

```diff
...
import color
import exceptions
import setup_game
import input_handlers


+def save_game(handler: input_handlers.BaseEventHandler, filename: str) -> None:
+   """If the current event handler has an active Engine then save it."""
+   if isinstance(handler, input_handlers.EventHandler):
+       handler.engine.save_as(filename)
+       print("Game saved.")


def main() -> None:
    ...

        ...
        except exceptions.QuitWithoutSaving:
            raise
        except SystemExit:  # Save and quit.
-           # TODO: Add the save function here
+           save_game(handler, "savegame.sav")
            raise
        except BaseException:  # Save on any other unexpected exception.
-           # TODO: Add the save function here
+           save_game(handler, "savegame.sav")
            raise


if __name__ == "__main__":
    main()
```

Now when you exit the game, you should see a new `savegame.sav` in your project directory.

One thing that would help to handle the case where the user tries to load a saved file when one doesn’t exist would be a pop-up message. This message will appear in the center of the screen, and disappear after any key is pressed.

Add this class to `input_handlers.py`:

Diff Original

```diff
class BaseEventHandler(tcod.event.EventDispatch[ActionOrHandler]):
    ...


+class PopupMessage(BaseEventHandler):
+   """Display a popup text window."""

+   def __init__(self, parent_handler: BaseEventHandler, text: str):
+       self.parent = parent_handler
+       self.text = text

+   def on_render(self, console: tcod.Console) -> None:
+       """Render the parent and dim the result, then print the message on top."""
+       self.parent.on_render(console)
+       console.tiles_rgb["fg"] //= 8
+       console.tiles_rgb["bg"] //= 8

+       console.print(
+           console.width // 2,
+           console.height // 2,
+           self.text,
+           fg=color.white,
+           bg=color.black,
+           alignment=tcod.CENTER,
+       )

+   def ev_keydown(self, event: tcod.event.KeyDown) -> Optional[BaseEventHandler]:
+       """Any key returns to the parent handler."""
+       return self.parent


class EventHandler(BaseEventHandler):
    ...
```

This displays a message on top of the current display, whether it’s the main menu or the main game. When the player presses a key (any key), the message disappears.

Now let’s shift our focus to loading the game. We can add a `load_game` function in our `setup_game.py` file, which will attempt to load the game. We’ll call it when we press the “c” key on the main menu. Open up `setup_game.py` and edit it like this:

Diff Original

```diff
from __future__ import annotations

import copy
+import lzma
+import pickle
+import traceback
from typing import Optional

import tcod
...


def new_game() -> Engine:
    ...


+def load_game(filename: str) -> Engine:
+   """Load an Engine instance from a file."""
+   with open(filename, "rb") as f:
+       engine = pickle.loads(lzma.decompress(f.read()))
+   assert isinstance(engine, Engine)
+   return engine


class MainMenu(input_handlers.BaseEventHandler):
    ...

    def ev_keydown(
        self, event: tcod.event.KeyDown
    ) -> Optional[input_handlers.BaseEventHandler]:
        if event.sym in (tcod.event.K_q, tcod.event.K_ESCAPE):
            raise SystemExit()
        elif event.sym == tcod.event.K_c:
-           # TODO: Load the game here
-           pass
+           try:
+               return input_handlers.MainGameEventHandler(load_game("savegame.sav"))
+           except FileNotFoundError:
+               return input_handlers.PopupMessage(self, "No saved game to load.")
+           except Exception as exc:
+               traceback.print_exc()  # Print to stderr.
+               return input_handlers.PopupMessage(self, f"Failed to load save:\n{exc}")
        elif event.sym == tcod.event.K_n:
            return input_handlers.MainGameEventHandler(new_game())

        return None
```

`load_game` essentially works the opposite of `save_as`, by opening up the file, uncompressing and unpickling it, and returning the instance of the `Engine` class. It then passes that engine to `MainGameEventHandler`. If no save game exists, or an error occured, we display a popup message.

And with that change, we can load our game! Try exiting your game and loading it afterwards.

The implementation as it exists now does have one major issue though: the player can load their save file after dying, and doing so actually allows the player to take an extra turn! The player can’t continue the game after that though, as our game immediately after detects that the player is dead, and the game state reverts to a game over. Still, this is an odd little bug that can be fixed quite simply: by deleting the save game file after the player dies.

To do that, we can override the `ev_quit` method in `GameOverEventHandler`. Open up `input_handlers.py` and make the following fix:

Diff Original

```diff
from __future__ import annotations

+import os

from typing import Callable, Optional, Tuple, TYPE_CHECKING, Union
...


class GameOverEventHandler(EventHandler):
+   def on_quit(self) -> None:
+       """Handle exiting out of a finished game."""
+       if os.path.exists("savegame.sav"):
+           os.remove("savegame.sav")  # Deletes the active save file.
+       raise exceptions.QuitWithoutSaving()  # Avoid saving a finished game.

+   def ev_quit(self, event: tcod.event.Quit) -> None:
+       self.on_quit()

    def ev_keydown(self, event: tcod.event.KeyDown) -> None:
        if event.sym == tcod.event.K_ESCAPE:
-           raise SystemExit()
+           self.on_quit()
```

We use the `os` module to find the save file, and if it exists, we remove it. We then raise `QuitWithoutSaving`, so that the game won’t be saved on exiting. Now when the player meets his or her tragic end (it’s a roguelike, it’s inevitable!), the save file will be deleted.

Last thing before we wrap up: We’re creating the `.sav` files to represent our saved games, but we don’t want to include these in our Git repository, since that should be reserved for just the code. The fix for this is to add this to our `.gitignore` file:

Diff Original

```diff
+# Saved games
+*.sav
```

_The rest of the .gitignore is omitted, as your .gitignore file may look different from mine. It doesn’t matter where you add this in._

If you want to see the code so far in its entirety, [click here](https://github.com/TStand90/tcod_tutorial_v2/tree/2020/part-10).

[Click here to move on to the next part of this tutorial.](https://rogueliketutorials.com/tutorials/tcod/v2/part-11)

---
# [Part 11 - Delving into the Dungeon](https://rogueliketutorials.com/tutorials/tcod/v2/part-11/)

Our game isn’t much of a “dungeon crawler” if there’s only one floor to our dungeon. In this chapter, we’ll allow the player to go down a level, and we’ll put a very basic leveling up system in place, to make the dive all the more rewarding.

Before diving into the code for this section, let’s add the color we’ll need this chapter, for when the player descends down a level in the dungeon. Open up `color.py` and add this line:

Diff Original

```diff
...
enemy_atk = (0xFF, 0xC0, 0xC0)
needs_target = (0x3F, 0xFF, 0xFF)
status_effect_applied = (0x3F, 0xFF, 0x3F)
+descend = (0x9F, 0x3F, 0xFF)

player_die = (0xFF, 0x30, 0x30)
enemy_die = (0xFF, 0xA0, 0x30)
...
```

We will use this color later on, when adding a message to the message log that the player went down one floor.

We’ll also need a new tile type to represent the downward stairs in the dungeon. Typically, roguelikes represent this with the `>` character, and we’ll do the same. Add the following to `tile_types.py`:

Diff Original

```diff
...
wall = new_tile(
    walkable=False,
    transparent=False,
    dark=(ord(" "), (255, 255, 255), (0, 0, 100)),
    light=(ord(" "), (255, 255, 255), (130, 110, 50)),
)
+down_stairs = new_tile(
+   walkable=True,
+   transparent=True,
+   dark=(ord(">"), (0, 0, 100), (50, 50, 150)),
+   light=(ord(">"), (255, 255, 255), (200, 180, 50)),
+)
```

To keep track of where our downwards stairs are located on the map, we can add a new variable in out `__init__` function in the `GameMap` class. The variable needs some sort of default, so to start, we can set that up to be `(0, 0)` by default. Add the following line to `game_map.py`:

Diff Original

```diff
class GameMap:
    def __init__(
        self, engine: Engine, width: int, height: int, entities: Iterable[Entity] = ()
    ):
        ...
        self.explored = np.full(
            (width, height), fill_value=False, order="F"
        )  # Tiles the player has seen before

+       self.downstairs_location = (0, 0)

    @property
    def gamemap(self) -> GameMap:
        ...
```

Of course, `(0, 0)` won’t be the actual location of the stairs. In order to actually place the downwards stairs, we’ll need to edit our procedural dungeon generator to place the stairs at the proper place. We’ll keep things simple and just place the stairs in the last room that our algorithm generates, by keeping track of the center coordinates of the last room we created. Modify `generate_dungeon` function in `procgen.py`:

Diff Original

```diff
    ...
    rooms: List[RectangularRoom] = []

+   center_of_last_room = (0, 0)

    for r in range(max_rooms):
        ...
            ...
            for x, y in tunnel_between(rooms[-1].center, new_room.center):
                dungeon.tiles[x, y] = tile_types.floor

+           center_of_last_room = new_room.center

        place_entities(new_room, dungeon, max_monsters_per_room, max_items_per_room)

+       dungeon.tiles[center_of_last_room] = tile_types.down_stairs
+       dungeon.downstairs_location = center_of_last_room

        # Finally, append the new room to the list.
        rooms.append(new_room)

    return dungeon
```

Whichever room is generated last, we take its center and set the `downstairs_location` equal to those coordinates. We also replace whatever tile type with the `down_stairs`, so the player can clearly see the location.

To hold the information about the maps, including the size, the room variables (size and maximum number), along with the floor that the player is currently on, we can add a class to hold these variables, as well as generate new maps when the time comes. Open up `game_map.py` and add the following class:

Diff Original

```diff
class GameMap:
    ...


+class GameWorld:
+   """
+   Holds the settings for the GameMap, and generates new maps when moving down the stairs.
+   """

+   def __init__(
+       self,
+       *,
+       engine: Engine,
+       map_width: int,
+       map_height: int,
+       max_rooms: int,
+       room_min_size: int,
+       room_max_size: int,
+       max_monsters_per_room: int,
+       max_items_per_room: int,
+       current_floor: int = 0
+   ):
+       self.engine = engine

+       self.map_width = map_width
+       self.map_height = map_height

+       self.max_rooms = max_rooms

+       self.room_min_size = room_min_size
+       self.room_max_size = room_max_size

+       self.max_monsters_per_room = max_monsters_per_room
+       self.max_items_per_room = max_items_per_room

+       self.current_floor = current_floor

+   def generate_floor(self) -> None:
+       from procgen import generate_dungeon

+       self.current_floor += 1

+       self.engine.game_map = generate_dungeon(
+           max_rooms=self.max_rooms,
+           room_min_size=self.room_min_size,
+           room_max_size=self.room_max_size,
+           map_width=self.map_width,
+           map_height=self.map_height,
+           max_monsters_per_room=self.max_monsters_per_room,
+           max_items_per_room=self.max_items_per_room,
+           engine=self.engine,
+       )
```

The `generate_floor` method will create the new maps each time we go down a floor, using the variables that `GameWorld` stores. In this tutorial, we won’t program in the ability to go back up a floor after going down one, but you could perhaps modify `GameWorld` to hold the previous maps.

In order to utilize the new `GameWorld` class, we’ll need to add it to the `Engine`, like this:

Diff Original

```diff
...
if TYPE_CHECKING:
    from entity import Actor
-   from game_map import GameMap
+   from game_map import GameMap, GameWorld


class Engine:
    game_map: GameMap
+   game_world: GameWorld

    def __init__(self, player: Actor):
        ...
```

Pretty simple. To utilize the new `game_world` class attribute, edit `setup_game.py` like this:

Diff Original

```diff
import tcod
import color
from engine import Engine
import entity_factories
+from game_map import GameWorld
import input_handlers
-from procgen import generate_dungeon
...

    ...
    engine = Engine(player=player)

-   engine.game_map = generate_dungeon(
+   engine.game_world = GameWorld(
+       engine=engine,
        max_rooms=max_rooms,
        room_min_size=room_min_size,
        room_max_size=room_max_size,
        map_width=map_width,
        map_height=map_height,
        max_monsters_per_room=max_monsters_per_room,
        max_items_per_room=max_items_per_room,
-       engine=engine,
    )

+   engine.game_world.generate_floor()
    engine.update_fov()
    ...
```

Now, instead of calling `generate_dungeon` directly, we create a new `GameWorld` and allow it to call its `generate_floor` method. While this doesn’t change anything for the first floor that’s created, it will allow us to more easily create new floors on the fly.

In order to actually take the stairs, we’ll need to add an action and a way for the player to trigger it. Adding the action is pretty simple. Add the following to `actions.py`:

Diff Original

```diff
class WaitAction(Action):
    pass


+class TakeStairsAction(Action):
+   def perform(self) -> None:
+       """
+       Take the stairs, if any exist at the entity's location.
+       """
+       if (self.entity.x, self.entity.y) == self.engine.game_map.downstairs_location:
+           self.engine.game_world.generate_floor()
+           self.engine.message_log.add_message(
+               "You descend the staircase.", color.descend
+           )
+       else:
+           raise exceptions.Impossible("There are no stairs here.")


class ActionWithDirection(Action):
    def __init__(self, entity: Actor, dx: int, dy: int):
        super().__init__(entity)
```

To call this action, the player should be able to press the `>` key. This can be accomplished by adding this to `input_handlers.py`:

Diff Original

```diff
class MainGameEventHandler(EventHandler):
    def ev_keydown(self, event: tcod.event.KeyDown) -> Optional[ActionOrHandler]:
        action: Optional[Action] = None

        key = event.sym
+       modifier = event.mod

        player = self.engine.player

+       if key == tcod.event.K_PERIOD and modifier & (
+           tcod.event.KMOD_LSHIFT | tcod.event.KMOD_RSHIFT
+       ):
+           return actions.TakeStairsAction(player)

        if key in MOVE_KEYS:
            dx, dy = MOVE_KEYS[key]
```

`modifier` tells us if the player is holding a key like control, alt, or shift. In this case, we’re checking if the user is holding shift while pressing the period key, which gives us the “>” key.

With that, the player can now descend the staircase to the next floor of the dungeon!

![Part 11 - Stairs](https://rogueliketutorials.com/images/part-11-stairs.png) ![Part 11 - Stairs Taken](https://rogueliketutorials.com/images/part-11-stairs-taken.png)

One little touch we can add before moving on to the next section is adding a way to see which floor the player is on. It’s simple enough: We’ll use the `current_floor` in `GameWorld` to know which floor we’re on, and we’ll modify our `render_functions.py` file to add a method to print this information out to the UI.

Add this function to `render_functions.py`:

Diff Original

```diff
from __future__ import annotations

-from typing import TYPE_CHECKING
+from typing import Tuple, TYPE_CHECKING

import color
...

...
def render_bar(
    console: Console, current_value: int, maximum_value: int, total_width: int
) -> None:
    ...


+def render_dungeon_level(
+   console: Console, dungeon_level: int, location: Tuple[int, int]
+) -> None:
+   """
+   Render the level the player is currently on, at the given location.
+   """
+   x, y = location

+   console.print(x=x, y=y, string=f"Dungeon level: {dungeon_level}")


def render_names_at_mouse_location(
    console: Console, x: int, y: int, engine: Engine
) -> None:
    ...
```

The `render_dungeon_level` function is fairly straightforward: Given a set of `(x, y)` coordinates as a Tuple, it prints to the console which dungeon level was passed to the function.

To call this function, we can edit the `Engine`’s `render` function, like so:

Diff Original

```diff
...
import exceptions
from message_log import MessageLog
-from render_functions import (
-   render_bar,
-   render_names_at_mouse_location,
-)
+import render_functions

if TYPE_CHECKING:
    ...


class Engine:
    ...

    def render(self, console: Console) -> None:
        self.game_map.render(console)

        self.message_log.render(console=console, x=21, y=45, width=40, height=5)

-       render_bar(
+       render_functions.render_bar(
            console=console,
            current_value=self.player.fighter.hp,
            maximum_value=self.player.fighter.max_hp,
            total_width=20,
        )

-       render_names_at_mouse_location(console=console, x=21, y=44, engine=self)
+       render_functions.render_dungeon_level(
+           console=console,
+           dungeon_level=self.game_world.current_floor,
+           location=(0, 47),
+       )

+       render_functions.render_names_at_mouse_location(
+           console=console, x=21, y=44, engine=self
+       )

    def save_as(self, filename: str) -> None:
        ...
```

Note that we’re now importing `render_functions` instead of importing the functions it contains. After awhile, it makes sense to just import the entire module rather than a few functions here and there. Otherwise, the file can get a bit difficult to read.

The call to `render_dungeon_level` shouldn’t be anything too surprising. We use `self.game_world.current_floor` as our `dungeon_level`, and the location of the printed string is below the health bar (feel free to move this somewhere else, if you like).

Try going down a few levels and make sure everything works as expected. If so, congratulations! Your dungeon now has multiple levels!

![Part 11 - Dungeon Level](https://rogueliketutorials.com/images/part-11-dungeon-level.png)

Speaking of “levels”, many roguelikes (not all!) feature some sort of level-up system, where your character gains experience and gets stronger by fighting monsters. The rest of this chapter will be spent implementing one such system.

In order to allow the rogue to level up, we need to modify the actors in two ways:

1. The player needs to gain experience points, keeping track of the XP gained thus far, and know when it’s time to level up.
2. The enemies need to give experience points when they are defeated.

There are several calculations we could use to compute how much XP a player needs to level up (or, theoretically, you could just hard code the values). Ours will be fairly simple: We’ll start with a base number, and add the product of our player’s current level and some other number, which will make it so each level up requires more XP than the last. For this tutorial, the “base” will be 200, and the “factor” will be 150 (so going to level 2 will take 350 XP, level 3 will take 500, and so on).

We can accomplish both of these goals by adding one component: `Level`. The `Level` component will hold all of the information that we need to accomplish these goals. Create a file called `level.py` in the `components` directory, and put the following contents in it:

```python
from __future__ import annotations

from typing import TYPE_CHECKING

from components.base_component import BaseComponent

if TYPE_CHECKING:
    from entity import Actor


class Level(BaseComponent):
    parent: Actor

    def __init__(
        self,
        current_level: int = 1,
        current_xp: int = 0,
        level_up_base: int = 0,
        level_up_factor: int = 150,
        xp_given: int = 0,
    ):
        self.current_level = current_level
        self.current_xp = current_xp
        self.level_up_base = level_up_base
        self.level_up_factor = level_up_factor
        self.xp_given = xp_given

    @property
    def experience_to_next_level(self) -> int:
        return self.level_up_base + self.current_level * self.level_up_factor

    @property
    def requires_level_up(self) -> bool:
        return self.current_xp > self.experience_to_next_level

    def add_xp(self, xp: int) -> None:
        if xp == 0 or self.level_up_base == 0:
            return

        self.current_xp += xp

        self.engine.message_log.add_message(f"You gain {xp} experience points.")

        if self.requires_level_up:
            self.engine.message_log.add_message(
                f"You advance to level {self.current_level + 1}!"
            )

    def increase_level(self) -> None:
        self.current_xp -= self.experience_to_next_level

        self.current_level += 1

    def increase_max_hp(self, amount: int = 20) -> None:
        self.parent.fighter.max_hp += amount
        self.parent.fighter.hp += amount

        self.engine.message_log.add_message("Your health improves!")

        self.increase_level()

    def increase_power(self, amount: int = 1) -> None:
        self.parent.fighter.power += amount

        self.engine.message_log.add_message("You feel stronger!")

        self.increase_level()

    def increase_defense(self, amount: int = 1) -> None:
        self.parent.fighter.defense += amount

        self.engine.message_log.add_message("Your movements are getting swifter!")

        self.increase_level()
```

Let’s go over what was just added.

```python
class Level(BaseComponent):
    parent: Actor

    def __init__(
        self,
        current_level: int = 1,
        current_xp: int = 0,
        level_up_base: int = 0,
        level_up_factor: int = 150,
        xp_given: int = 0,
    ):
        self.current_level = current_level
        self.current_xp = current_xp
        self.level_up_base = level_up_base
        self.level_up_factor = level_up_factor
        self.xp_given = xp_given
```

The values in our `__init__` function break down like this:

- current_level: The current level of the Entity, defaults to 1.
- current_xp: The Entity’s current experience points.
- level_up_base: The base number we decide for leveling up. We’ll set this to 200 when creating the Player.
- level_up_factor: The number to multiply against the Entity’s current level.
- xp_given: When the Entity dies, this is how much XP the Player will gain.

```python
    @property
    def experience_to_next_level(self) -> int:
        return self.level_up_base + self.current_level * self.level_up_factor
```

This represents how much experience the player needs until hitting the next level. The formula is explained above. Again, feel free to tweak this formula in any way you see fit.

```python
    @property
    def requires_level_up(self) -> bool:
        return self.current_xp > self.experience_to_next_level
```

We’ll use this property to determine if the player needs to level up or not. If the `current_xp` is higher than the `experience_to_next_level` property, then the player levels up. If not, nothing happens.

```python
    def add_xp(self, xp: int) -> None:
        if xp == 0 or self.level_up_base == 0:
            return

        self.current_xp += xp

        self.engine.message_log.add_message(f"You gain {xp} experience points.")

        if self.requires_level_up:
            self.engine.message_log.add_message(
                f"You advance to level {self.current_level + 1}!"
            )
```

This method adds experience points to the Entity’s XP pool, as the name implies. If the value is 0, we just return, as there’s nothing to do. Notice that we also return if the `level_up_base` is set to 0. Why? In this tutorial, the enemies don’t gain XP, so we’ll set their `level_up_base` to 0 so that there’s no way they could ever gain experience. Perhaps in your game, monsters _will_ gain XP, and you’ll want to adjust this, but that’s left up to you.

The rest of the method adds the xp, adds a message to the message log, and, if the Entity levels up, posts another message.

```python
    def increase_level(self) -> None:
        self.current_xp -= self.experience_to_next_level

        self.current_level += 1
```

This method adds +1 to the `current_level`, while decreasing the `current_xp` by the `experience_to_next_level`. We do this because if we didn’t it would always just take the `level_up_factor` amount to level up, which isn’t what we want. If you wanted to keep track of the player’s _cumulative_ XP throughout the playthrough, you could skip decrementing the `current_xp` and instead adjust the `experience_to_next_level` formula accordingly.

Lastly, the functions `increase_max_hp`, `increase_power`, and `increase_defense` all do basically the same thing: they raise one of the Entity’s attributes, add a message to the message log, then call `increase_level`.

To use this component, we need to add it to our `Actor` class. Make the following changes to the file `entity.py`:

Diff Original

```diff
if TYPE_CHECKING:
    from components.ai import BaseAI
    from components.consumable import Consumable
    from components.fighter import Fighter
    from components.inventory import Inventory
+   from components.level import Level
    from game_map import GameMap

T = TypeVar("T", bound="Entity")
...

class Actor(Entity):
    def __init__(
        self,
        *,
        x: int = 0,
        y: int = 0,
        char: str = "?",
        color: Tuple[int, int, int] = (255, 255, 255),
        name: str = "<Unnamed>",
        ai_cls: Type[BaseAI],
        fighter: Fighter,
        inventory: Inventory,
+       level: Level,
    ):
        super().__init__(
            x=x,
            y=y,
            char=char,
            color=color,
            name=name,
            blocks_movement=True,
            render_order=RenderOrder.ACTOR,
        )

        self.ai: Optional[BaseAI] = ai_cls(self)

        self.fighter = fighter
        self.fighter.parent = self

        self.inventory = inventory
        self.inventory.parent = self

+       self.level = level
+       self.level.parent = self

    @property
    def is_alive(self) -> bool:
        ...
```

Let’s also modify our entities in `entity_factories.py` now:

Diff Original

```diff
from components.ai import HostileEnemy
from components import consumable
from components.fighter import Fighter
from components.inventory import Inventory
+from components.level import Level
from entity import Actor, Item


player = Actor(
    char="@",
    color=(255, 255, 255),
    name="Player",
    ai_cls=HostileEnemy,
    fighter=Fighter(hp=30, defense=2, power=5),
    inventory=Inventory(capacity=26),
+   level=Level(level_up_base=200),
)

orc = Actor(
    char="o",
    color=(63, 127, 63),
    name="Orc",
    ai_cls=HostileEnemy,
    fighter=Fighter(hp=10, defense=0, power=3),
    inventory=Inventory(capacity=0),
+   level=Level(xp_given=35),
)
troll = Actor(
    char="T",
    color=(0, 127, 0),
    name="Troll",
    ai_cls=HostileEnemy,
    fighter=Fighter(hp=16, defense=1, power=4),
    inventory=Inventory(capacity=0),
+   level=Level(xp_given=100),
)
...
```

As mentioned, the `level_up_base` for the player is set to 200. Orcs give 35 XP, and Trolls give 100, since they’re stronger. These values are completely arbitrary, so feel free to adjust them in any way you see fit.

When an enemy dies, we need to give the player XP. This is as simple as adding one line to the `Fighter` component, so open up `fighter.py` and add this:

Diff Original

```diff
class Fighter(BaseComponent):
    def die(self) -> None:
        ...

        self.engine.message_log.add_message(death_message, death_message_color)

+       self.engine.player.level.add_xp(self.parent.level.xp_given)

    def heal(self, amount: int) -> int:
        ...
```

Now the player will gain XP for defeating enemies!

While the player does gain XP now, notice that we haven’t actually _called_ the functions that increase the player’s stats and levels the player up. We’ll need a new interface to do this. The way it will work is that as soon as the player gets enough experience to level up, we’ll display a message to the player, giving the player three choices on what stat to increase. When chosen, the appropriate function will be called, and the message will close.

Let’s create a new event handler, called `LevelUpEventHandler`, that will do just that. Create the following class in `input_handlers.py`:

Diff Original

```diff
class AskUserEventHandler(EventHandler):
    ...

+class LevelUpEventHandler(AskUserEventHandler):
+   TITLE = "Level Up"

+   def on_render(self, console: tcod.Console) -> None:
+       super().on_render(console)

+       if self.engine.player.x <= 30:
+           x = 40
+       else:
+           x = 0

+       console.draw_frame(
+           x=x,
+           y=0,
+           width=35,
+           height=8,
+           title=self.TITLE,
+           clear=True,
+           fg=(255, 255, 255),
+           bg=(0, 0, 0),
+       )

+       console.print(x=x + 1, y=1, string="Congratulations! You level up!")
+       console.print(x=x + 1, y=2, string="Select an attribute to increase.")

+       console.print(
+           x=x + 1,
+           y=4,
+           string=f"a) Constitution (+20 HP, from {self.engine.player.fighter.max_hp})",
+       )
+       console.print(
+           x=x + 1,
+           y=5,
+           string=f"b) Strength (+1 attack, from {self.engine.player.fighter.power})",
+       )
+       console.print(
+           x=x + 1,
+           y=6,
+           string=f"c) Agility (+1 defense, from {self.engine.player.fighter.defense})",
+       )

+   def ev_keydown(self, event: tcod.event.KeyDown) -> Optional[ActionOrHandler]:
+       player = self.engine.player
+       key = event.sym
+       index = key - tcod.event.K_a

+       if 0 <= index <= 2:
+           if index == 0:
+               player.level.increase_max_hp()
+           elif index == 1:
+               player.level.increase_power()
+           else:
+               player.level.increase_defense()
+       else:
+           self.engine.message_log.add_message("Invalid entry.", color.invalid)

+           return None

+       return super().ev_keydown(event)

+   def ev_mousebuttondown(
+       self, event: tcod.event.MouseButtonDown
+   ) -> Optional[ActionOrHandler]:
+       """
+       Don't allow the player to click to exit the menu, like normal.
+       """
+       return None


class InventoryEventHandler(AskUserEventHandler):
    ...
```

The idea here is very similar to `InventoryEventHandler` (it inherits from the same `AskUserEventHandler` class), but instead of having a variable number of options, it’s set to three, one for each of the primary attributes. Furthermore, there’s no way to exit this menu without selecting something. The user **must** level up before continuing. (Notice, we had to override `ev_mousebutton` to prevent clicks from closing the menu.)

Using `LevelUpEventHandler` is actually quite simple: We can check when the player requires a level up at the same time when we check if the player is still alive. Edit the `handle_events` method of `EventHandler` like this:

Diff Original

```diff
            if not self.engine.player.is_alive:
                # The player was killed sometime during or after the action.
                return GameOverEventHandler(self.engine)
+           elif self.engine.player.level.requires_level_up:
+               return LevelUpEventHandler(self.engine)
            return MainGameEventHandler(self.engine)  # Return to the main handler.
```

Now, when the player gains the necessary number of experience points, the player will have the chance to level up!

![Part 11 - Level Up](https://rogueliketutorials.com/images/part-11-level-up.png)

Before finishing this chapter, there’s one last quick thing we can do to improve the user experience: Add a “character information” screen, which displays the player’s stats and current experience. It’s actually quite simple. Add the following class to `input_handlers.py`:

Diff Original

```diff
class AskUserEventHandler(EventHandler):
    ...

+class CharacterScreenEventHandler(AskUserEventHandler):
+   TITLE = "Character Information"

+   def on_render(self, console: tcod.Console) -> None:
+       super().on_render(console)

+       if self.engine.player.x <= 30:
+           x = 40
+       else:
+           x = 0

+       y = 0

+       width = len(self.TITLE) + 4

+       console.draw_frame(
+           x=x,
+           y=y,
+           width=width,
+           height=7,
+           title=self.TITLE,
+           clear=True,
+           fg=(255, 255, 255),
+           bg=(0, 0, 0),
+       )

+       console.print(
+           x=x + 1, y=y + 1, string=f"Level: {self.engine.player.level.current_level}"
+       )
+       console.print(
+           x=x + 1, y=y + 2, string=f"XP: {self.engine.player.level.current_xp}"
+       )
+       console.print(
+           x=x + 1,
+           y=y + 3,
+           string=f"XP for next Level: {self.engine.player.level.experience_to_next_level}",
+       )

+       console.print(
+           x=x + 1, y=y + 4, string=f"Attack: {self.engine.player.fighter.power}"
+       )
+       console.print(
+           x=x + 1, y=y + 5, string=f"Defense: {self.engine.player.fighter.defense}"
+       )

class LevelUpEventHandler(AskUserEventHandler):
    ...
```

Similar to `LevelUpEventHandler`, `CharacterScreenEventHandler` shows information in a window, but there’s no real “choices” to be made here. Any input will simply close the screen.

To open the screen, we’ll have the player press the `c` key. Add the following to `MainGameEventHandler`:

Diff Original

```diff
        elif key == tcod.event.K_d:
            return InventoryDropHandler(self.engine)
+       elif key == tcod.event.K_c:
+           return CharacterScreenEventHandler(self.engine)
        elif key == tcod.event.K_SLASH:
            return LookHandler(self.engine)
```

![Part 11 - Character Screen](https://rogueliketutorials.com/images/part-11-character-screen.png)

That’s it for this chapter. We’ve added the ability to go down floors, and to level up. While the player can now “progress”, the environment itself doesn’t. The items that spawn on each floor are always the same, and the enemies don’t get tougher as we go down floors. The next part will address that.

If you want to see the code so far in its entirety, [click here](https://github.com/TStand90/tcod_tutorial_v2/tree/2020/part-11).

[Click here to move on to the next part of this tutorial.](https://rogueliketutorials.com/tutorials/tcod/v2/part-12)

---
# [Part 12 - Increasing Difficulty](https://rogueliketutorials.com/tutorials/tcod/v2/part-12/)

Despite the fact that we can go down floors now, the dungeon doesn’t get progressively more difficult as the player descends. This is because the method in which we place monsters and items is the same on each floor. In this chapter, we’ll adjust how we place things in the dungeon, so things get more difficult with each floor.

Currently, we pass `maximum_monsters` and `maximum_items` into the `place_entities` function, and this number does not change. To adjust the difficulty of our game, we can change these numbers based on the floor number. The way we’ll accomplish this is by setting up a list of tuples, which will contain two integers: the floor number, and the number of items/monsters.

Add the following to `procgen.py`:

Diff Original

```diff
...
if TYPE_CHECKING:
    from engine import Engine


+max_items_by_floor = [
+   (1, 1),
+   (4, 2),
+]

+max_monsters_by_floor = [
+   (1, 2),
+   (4, 3),
+   (6, 5),
+]


class RectangularRoom:
    ...
```

As mentioned, the first number in these tuples represents the floor number, and the second represents the maximum of either the items or the monsters.

You might be wondering why we’ve only supplied values for only certain floors. Rather than having to type out each floor number, we’ll provide the floor numbers that have a different value, so that we can loop through the list and stop when we hit a floor number higher than the one we’re on. For example, if we’re on floor 3, we’ll take the floor 1 entry for both items and monsters, and stop iteration when we reach the second item in the list, since floor 4 is higher than floor 3.

Let’s write the function to take care of this. We’ll call it `get_max_value_for_floor`, as we’re getting the maximum value for either the items or monsters. It looks like this:

Diff Original

```diff
...
max_items_by_floor = [
    (1, 1),
    (4, 2),
]

max_monsters_by_floor = [
    (1, 2),
    (4, 3),
    (6, 5),
]


+def get_max_value_for_floor(
+   weighted_chances_by_floor: List[Tuple[int, int]], floor: int
+) -> int:
+   current_value = 0

+   for floor_minimum, value in weighted_chances_by_floor:
+       if floor_minimum > floor:
+           break
+       else:
+           current_value = value

+   return current_value


class RectangularRoom:
    ...
```

Using this function is quite simple: we simply remove the `maximum_monsters` and `maximum_items` parameters from the `place_entities` function, pass the `floor_number` instead, and use that to get our maximum values from the `get_max_value_for_floor` function.

Diff Original

```diff
-def place_entities(
-   room: RectangularRoom, dungeon: GameMap, maximum_monsters: int, maximum_items: int
-) -> None:
-   number_of_monsters = random.randint(0, maximum_monsters)
-   number_of_items = random.randint(0, maximum_items)
+def place_entities(room: RectangularRoom, dungeon: GameMap, floor_number: int,) -> None:
+   number_of_monsters = random.randint(
+       0, get_max_value_for_floor(max_monsters_by_floor, floor_number)
+   )
+   number_of_items = random.randint(
+       0, get_max_value_for_floor(max_items_by_floor, floor_number)
+   )

    for i in range(number_of_monsters):
        ...

...

def generate_dungeon(
    max_rooms: int,
    room_min_size: int,
    room_max_size: int,
    map_width: int,
    map_height: int,
-   max_monsters_per_room: int,
-   max_items_per_room: int,
    engine: Engine,
) -> GameMap:
    ...

            ...
            center_of_last_room = new_room.center

-       place_entities(new_room, dungeon, max_monsters_per_room, max_items_per_room)
+       place_entities(new_room, dungeon, engine.game_world.current_floor)

        dungeon.tiles[center_of_last_room] = tile_types.down_stairs
        dungeon.downstairs_location = center_of_last_room
```

We can also remove `max_monsters_per_room` and `max_items_per_room` from `GameWorld`. Remove these lines from `game_map.py`:

Diff Original

```diff
class GameWorld:
    """
    Holds the settings for the GameMap, and generates new maps when moving down the stairs.
    """

    def __init__(
        self,
        *,
        engine: Engine,
        map_width: int,
        map_height: int,
        max_rooms: int,
        room_min_size: int,
        room_max_size: int,
-       max_monsters_per_room: int,
-       max_items_per_room: int,
        current_floor: int = 0
    ):
        self.engine = engine

        self.map_width = map_width
        self.map_height = map_height

        self.max_rooms = max_rooms

        self.room_min_size = room_min_size
        self.room_max_size = room_max_size

-       self.max_monsters_per_room = max_monsters_per_room
-       self.max_items_per_room = max_items_per_room

        self.current_floor = current_floor

    def generate_floor(self) -> None:
        from procgen import generate_dungeon

        self.current_floor += 1

        self.engine.game_map = generate_dungeon(
            max_rooms=self.max_rooms,
            room_min_size=self.room_min_size,
            room_max_size=self.room_max_size,
            map_width=self.map_width,
            map_height=self.map_height,
-           max_monsters_per_room=self.max_monsters_per_room,
-           max_items_per_room=self.max_items_per_room,
            engine=self.engine,
        )
```

Also remove the same variables from `setup_game.py`:

Diff Original

```diff
def new_game() -> Engine:
    """Return a brand new game session as an Engine instance."""
    map_width = 80
    map_height = 43

    room_max_size = 10
    room_min_size = 6
    max_rooms = 30

-   max_monsters_per_room = 2
-   max_items_per_room = 2

    player = copy.deepcopy(entity_factories.player)

    engine = Engine(player=player)

    engine.game_world = GameWorld(
        engine=engine,
        max_rooms=max_rooms,
        room_min_size=room_min_size,
        room_max_size=room_max_size,
        map_width=map_width,
        map_height=map_height,
-       max_monsters_per_room=max_monsters_per_room,
-       max_items_per_room=max_items_per_room,
    )

    engine.game_world.generate_floor()
    engine.update_fov()

    engine.message_log.add_message(
        "Hello and welcome, adventurer, to yet another dungeon!", color.welcome_text
    )
    return engine
```

Now we’re adjusting the number of items and monsters based on the floor. The next step is to control which entities appear on which floor, instead of allowing any entity to appear on any floor. The first floor will only have health potions and orcs, and we’ll gradually add different items and enemies as the player goes deeper into the dungeon.

We need a function that allows us to get these entities at random, based on a set of weights. We also need to define the weights themselves.

What are “weights” in this context? Basically, we could define all of the odds of generating a type of entity the way we have already, by getting a random number and comparing against a set of values, but that will quickly become cumbersome as we add more entities. Imagine wanting to add a new enemy type, but needing to adjust the values for dozens, or perhaps **hundreds**, of other entities.

Instead, we’ll just give each entity a value, or a “weight”, which we’ll use to determine how common that entity should be. We’ll use Python’s `random.choices` function, which allows the user to pass a list of items and a set of weights. It returns a number of items that you specify, based on the weights you give it.

First, we need to define our weights for the entity types, along with the minimum floor that the item or monster will appear on. Add the following to `procgen.py`:

Diff Original

```diff
from __future__ import annotations

import random
-from typing import Iterator, List, Tuple, TYPE_CHECKING
+from typing import Dict, Iterator, List, Tuple, TYPE_CHECKING

import tcod

import entity_factories
from game_map import GameMap
import tile_types

if TYPE_CHECKING:
    from engine import Engine
+   from entity import Entity


max_items_by_floor = [
    (1, 1),
    (4, 2),
]

max_monsters_by_floor = [
    (1, 2),
    (4, 3),
    (6, 5),
]

+item_chances: Dict[int, List[Tuple[Entity, int]]] = {
+   0: [(entity_factories.health_potion, 35)],
+   2: [(entity_factories.confusion_scroll, 10)],
+   4: [(entity_factories.lightning_scroll, 25)],
+   6: [(entity_factories.fireball_scroll, 25)],
+}

+enemy_chances: Dict[int, List[Tuple[Entity, int]]] = {
+   0: [(entity_factories.orc, 80)],
+   3: [(entity_factories.troll, 15)],
+   5: [(entity_factories.troll, 30)],
+   7: [(entity_factories.troll, 60)],
+}


def get_max_value_for_floor(
    ...
```

They keys in the dictionary represent the floor number, and the value is a list of tuples. The tuples contain an entity and the weights at which they’ll be generated. Notice that Trolls get defined multiple times in `enemy_chances`, and their weights grow higher when the floor number increases. This will allow Trolls to be generated more frequently as the player dives into the dungeon, thus making the dungeon more dangerous with each passing floor.

Why a _list_ of tuples, though? While there isn’t any examples here, we want it to be possible to define many entity types and weights for each floor. For example, imagine we added a new enemy type that appears on floor 5. We could put that as a tuple inside the list, alongside the Troll’s tuple. We’ll see an example of this in the next chapter, when we start adding equipment.

With our weights defined, we need a function to actually pick which entities we want to create. As mentioned, it will utilize `random.choices` from the Python standard library to choose the entities. Add this function to `procgen.py`:

Diff Original

```diff
def get_max_value_for_floor(
    weighted_chances_by_floor: List[Tuple[int, int]], floor: int
) -> int:
    ...


+def get_entities_at_random(
+   weighted_chances_by_floor: Dict[int, List[Tuple[Entity, int]]],
+   number_of_entities: int,
+   floor: int,
+) -> List[Entity]:
+   entity_weighted_chances = {}

+   for key, values in weighted_chances_by_floor.items():
+       if key > floor:
+           break
+       else:
+           for value in values:
+               entity = value[0]
+               weighted_chance = value[1]

+               entity_weighted_chances[entity] = weighted_chance

+   entities = list(entity_weighted_chances.keys())
+   entity_weighted_chance_values = list(entity_weighted_chances.values())

+   chosen_entities = random.choices(
+       entities, weights=entity_weighted_chance_values, k=number_of_entities
+   )

+   return chosen_entities


class RectangularRoom:
    ...
```

This function goes through they keys (floor numbers) and values (list of weighted entities), stopping when the key is higher than the given floor number. It sets up a dictionary of the weights for each entity, based on which floor the player is currently on. So if we were trying to get the weights for floor 6, `entity_weighted_chances` would look like this: `{ orc: 80, troll: 30 }`.

Then, we get both the keys and values in list format, so that they can be passed to `random.choices` (it accepts choices and weights as lists). `k` represents the number of items that `random.choices` should pick, so we can simply pass the number of entities we’ve decided to generate. Finally, we return the list of chosen entities.

Putting this function to use is quite simple. In fact, it will reduce the amount of code in our `place_entities` function quite nicely:

Diff Original

```diff
def place_entities(room: RectangularRoom, dungeon: GameMap, floor_number: int,) -> None:
    number_of_monsters = random.randint(
        0, get_weight_for_floor(max_monsters_by_floor, floor_number)
    )
    number_of_items = random.randint(
        0, get_weight_for_floor(max_items_by_floor, floor_number)
    )

+   monsters: List[Entity] = get_entities_at_random(
+       enemy_chances, number_of_monsters, floor_number
+   )
+   items: List[Entity] = get_entities_at_random(
+       item_chances, number_of_items, floor_number
+   )

-   for i in range(number_of_monsters):
-       x = random.randint(room.x1 + 1, room.x2 - 1)
-       y = random.randint(room.y1 + 1, room.y2 - 1)

-       if not any(entity.x == x and entity.y == y for entity in dungeon.entities):
-           if random.random() < 0.8:
-               entity_factories.orc.spawn(dungeon, x, y)
-           else:
-               entity_factories.troll.spawn(dungeon, x, y)

-   for i in range(number_of_items):
+   for entity in monsters + items:
        x = random.randint(room.x1 + 1, room.x2 - 1)
        y = random.randint(room.y1 + 1, room.y2 - 1)

        if not any(entity.x == x and entity.y == y for entity in dungeon.entities):
-           item_chance = random.random()

-           if item_chance < 0.7:
-               entity_factories.health_potion.spawn(dungeon, x, y)
-           elif item_chance < 0.80:
-               entity_factories.fireball_scroll.spawn(dungeon, x, y)
-           elif item_chance < 0.90:
-               entity_factories.confusion_scroll.spawn(dungeon, x, y)
-           else:
-               entity_factories.lightning_scroll.spawn(dungeon, x, y)
+           entity.spawn(dungeon, x, y)

...
```

Now `place_entities` is just getting the amount of monsters and items to generate, and leaving it up to `get_entities_at_random` to determine which ones to create.

With those changes, the dungeon will get progressively more difficult! You may want to tweak certain numbers, like the strength of the enemies or how much health you recover with potions, to get a more challenging experience (our game is still not _that_ difficult, if you increase your defense by just 1, Orcs are no longer a threat).

If you want to see the code so far in its entirety, [click here](https://github.com/TStand90/tcod_tutorial_v2/tree/2020/part-12).

[Click here to move on to the next part of this tutorial.](https://rogueliketutorials.com/tutorials/tcod/v2/part-13)

---
# [Part 13 - Gearing up](https://rogueliketutorials.com/tutorials/tcod/v2/part-13/)

For the final part of this tutorial, we’ll implement something that _most_ roguelikes have: equipment. Our implementation will be extremely simple: equipping a weapon increases attack power, and equipping armor increases defense. Many roguelikes have more equipment types than just these two, and the effects of equipment can go much further than this, but this should be enough to get you started.

First, we’ll want to define the types of equipment that can be found in the dungeon. As with the `RenderOrder` class, we can use `Enum` to define the types. For now, we’ll leave it at weapons and armor, but feel free to add more types as you see fit.

Create a new file, `equipment_types.py`, and put the following contents in it:

```python
from enum import auto, Enum


class EquipmentType(Enum):
    WEAPON = auto()
    ARMOR = auto()
```

Now it’s time to create the component that we’ll attach to the equipment. We’ll call the component `Equippable`, which will have a few different attributes:

- `equipment_type`: The type of equipment, using the `EquipmentType` enum.
- `power_bonus`: How much the wielder’s attack power will be increased. Currently used for just weapons.
- `defense_bonus`: How much the wearer’s defense will be increased. Currently just for armor.

Create the file `equippable.py` in the `components` directory, and fill it with the following:

```python
from __future__ import annotations

from typing import TYPE_CHECKING

from components.base_component import BaseComponent
from equipment_types import EquipmentType

if TYPE_CHECKING:
    from entity import Item


class Equippable(BaseComponent):
    parent: Item

    def __init__(
        self,
        equipment_type: EquipmentType,
        power_bonus: int = 0,
        defense_bonus: int = 0,
    ):
        self.equipment_type = equipment_type

        self.power_bonus = power_bonus
        self.defense_bonus = defense_bonus


class Dagger(Equippable):
    def __init__(self) -> None:
        super().__init__(equipment_type=EquipmentType.WEAPON, power_bonus=2)


class Sword(Equippable):
    def __init__(self) -> None:
        super().__init__(equipment_type=EquipmentType.WEAPON, power_bonus=4)


class LeatherArmor(Equippable):
    def __init__(self) -> None:
        super().__init__(equipment_type=EquipmentType.ARMOR, defense_bonus=1)


class ChainMail(Equippable):
    def __init__(self) -> None:
        super().__init__(equipment_type=EquipmentType.ARMOR, defense_bonus=3)
```

Aside from creating the `Equippable` class, as described earlier, we’ve also created a few types of equippable components, for each equippable entity that we’ll end up creating, similar to what we did with the `Consumable` classes. You don’t have to do it this way, you could just define these when creating the entities, but you might want to add additional functionality to weapons and armor at some point, and defining the `Equippable` classes this way might make that easier. You might also want to move these classes to their own file, but that’s outside the scope of this tutorial.

To create the actual equippable entities, we’ll want to adjust our `Item` class. We can use the same class that we used for our consumables, and just handle them slightly differently. Another approach would be to create another subclass of `Entity`, but for the sake of keeping the number of `Entity` subclasses in this tutorial short, we’ll adjust `Item`. Make the following adjustments to `entity.py`:

Diff Original

```diff
...
if TYPE_CHECKING:
    from components.ai import BaseAI
    from components.consumable import Consumable
+   from components.equippable import Equippable
    from components.fighter import Fighter
    from components.inventory import Inventory
    from components.level import Level
    from game_map import GameMap
...

class Item(Entity):
    def __init__(
        self,
        *,
        x: int = 0,
        y: int = 0,
        char: str = "?",
        color: Tuple[int, int, int] = (255, 255, 255),
        name: str = "<Unnamed>",
-       consumable: Consumable,
+       consumable: Optional[Consumable] = None,
+       equippable: Optional[Equippable] = None,
    ):
        super().__init__(
            x=x,
            y=y,
            char=char,
            color=color,
            name=name,
            blocks_movement=False,
            render_order=RenderOrder.ITEM,
        )

        self.consumable = consumable
-       self.consumable.parent = self

+       if self.consumable:
+           self.consumable.parent = self

+       self.equippable = equippable

+       if self.equippable:
+           self.equippable.parent = self
```

We’ve added `Equippable` as an optional component for the `Item` class, and also made `Consumable` optional, so that not all `Item` instances will be consumable.

Because `consumable` is now an optional attribute, we need to adjust `actions.py` to take this into account:

Diff Original

```diff
class ItemAction(Action):
    ...

    def perform(self) -> None:
        """Invoke the items ability, this action will be given to provide context."""
-       self.item.consumable.activate(self)
+       if self.item.consumable:
+           self.item.consumable.activate(self)
```

In order to actually create the equippable entities, we’ll want to add a few examples to `entity_factories.py`. The entities we will add will correspond to the `Equippable` subclasses we already made. Edit `entity_factories.py` like this:

Diff Original

```diff
from components.ai import HostileEnemy
-from components import consumable
+from components import consumable, equippable
from components.fighter import Fighter
from components.inventory import Inventory
from components.level import Level

...
lightning_scroll = Item(
    char="~",
    color=(255, 255, 0),
    name="Lightning Scroll",
    consumable=consumable.LightningDamageConsumable(damage=20, maximum_range=5),
)

+dagger = Item(
+   char="/", color=(0, 191, 255), name="Dagger", equippable=equippable.Dagger()
+)
+
+sword = Item(char="/", color=(0, 191, 255), name="Sword", equippable=equippable.Sword())

+leather_armor = Item(
+   char="[",
+   color=(139, 69, 19),
+   name="Leather Armor",
+   equippable=equippable.LeatherArmor(),
+)

+chain_mail = Item(
+   char="[", color=(139, 69, 19), name="Chain Mail", equippable=equippable.ChainMail()
+)
```

The creation of these entities is very similar to the consumables, except we give them the `Equippable` component instead of `Consumable`. This is all we need to do to create the entities themselves, but we’re far from finished. We still need to make these entities appear on the map, make them equippable (there’s nothing for them to attach _to_ on the player right now), and make equipping them actually do something.

To handle the equipment that the player has equipped at the moment, we can create yet another component to handle the player’s (or the monster’s, for that matter) equipment. Create a new file called `equipment.py` in the `components` folder, and add these contents:

```python
from __future__ import annotations

from typing import Optional, TYPE_CHECKING

from components.base_component import BaseComponent
from equipment_types import EquipmentType

if TYPE_CHECKING:
    from entity import Actor, Item


class Equipment(BaseComponent):
    parent: Actor

    def __init__(self, weapon: Optional[Item] = None, armor: Optional[Item] = None):
        self.weapon = weapon
        self.armor = armor

    @property
    def defense_bonus(self) -> int:
        bonus = 0

        if self.weapon is not None and self.weapon.equippable is not None:
            bonus += self.weapon.equippable.defense_bonus

        if self.armor is not None and self.armor.equippable is not None:
            bonus += self.armor.equippable.defense_bonus

        return bonus

    @property
    def power_bonus(self) -> int:
        bonus = 0

        if self.weapon is not None and self.weapon.equippable is not None:
            bonus += self.weapon.equippable.power_bonus

        if self.armor is not None and self.armor.equippable is not None:
            bonus += self.armor.equippable.power_bonus

        return bonus

    def item_is_equipped(self, item: Item) -> bool:
        return self.weapon == item or self.armor == item

    def unequip_message(self, item_name: str) -> None:
        self.parent.gamemap.engine.message_log.add_message(
            f"You remove the {item_name}."
        )

    def equip_message(self, item_name: str) -> None:
        self.parent.gamemap.engine.message_log.add_message(
            f"You equip the {item_name}."
        )

    def equip_to_slot(self, slot: str, item: Item, add_message: bool) -> None:
        current_item = getattr(self, slot)

        if current_item is not None:
            self.unequip_from_slot(slot, add_message)

        setattr(self, slot, item)

        if add_message:
            self.equip_message(item.name)

    def unequip_from_slot(self, slot: str, add_message: bool) -> None:
        current_item = getattr(self, slot)

        if add_message:
            self.unequip_message(current_item.name)

        setattr(self, slot, None)

    def toggle_equip(self, equippable_item: Item, add_message: bool = True) -> None:
        if (
            equippable_item.equippable
            and equippable_item.equippable.equipment_type == EquipmentType.WEAPON
        ):
            slot = "weapon"
        else:
            slot = "armor"

        if getattr(self, slot) == equippable_item:
            self.unequip_from_slot(slot, add_message)
        else:
            self.equip_to_slot(slot, equippable_item, add_message)
```

That’s a lot to take in, so let’s go through it bit by bit.

```python
class Equipment(BaseComponent):
    parent: Actor

    def __init__(self, weapon: Optional[Item] = None, armor: Optional[Item] = None):
        self.weapon = weapon
        self.armor = armor
```

The `weapon` and `armor` attributes are what will hold the actual equippable entity. Both can be set to `None`, which represents nothing equipped in those slots. Feel free to add more slots as you see fit (perhaps you want `armor` to be head, body, legs, etc. instead, or allow for off-hand weapons/shields).

```python
    @property
    def defense_bonus(self) -> int:
        bonus = 0

        if self.weapon is not None and self.weapon.equippable is not None:
            bonus += self.weapon.equippable.defense_bonus

        if self.armor is not None and self.armor.equippable is not None:
            bonus += self.armor.equippable.defense_bonus

        return bonus

    @property
    def power_bonus(self) -> int:
        bonus = 0

        if self.weapon is not None and self.weapon.equippable is not None:
            bonus += self.weapon.equippable.power_bonus

        if self.armor is not None and self.armor.equippable is not None:
            bonus += self.armor.equippable.power_bonus

        return bonus
```

These properties do the same thing, just for different things. Both calculate the “bonus” gifted by equipment to either defense or power, based on what’s equipped. Notice that we take the “power” bonus from both weapons and armor, and the same applies to the “defense” bonus. This allows you to create weapons that increase both attack and defense (maybe some sort of spiked shield) and armor that increases attack (something magical, maybe). We won’t do that in this tutorial (weapons will only increase power, armor will only increase defense), but you should experiment with different equipment types on your own.

```python
    def item_is_equipped(self, item: Item) -> bool:
        return self.weapon == item or self.armor == item
```

This allows us to quickly check if an `Item` is equipped by the player or not. It will come in handy later on.

```python
    def unequip_message(self, item_name: str) -> None:
        self.parent.gamemap.engine.message_log.add_message(
            f"You remove the {item_name}."
        )

    def equip_message(self, item_name: str) -> None:
        self.parent.gamemap.engine.message_log.add_message(
            f"You equip the {item_name}."
        )
```

Both of these methods add a message to the message log, depending on whether the player is equipping or removing a piece of equipment.

```python
    def equip_to_slot(self, slot: str, item: Item, add_message: bool) -> None:
        current_item = getattr(self, slot)

        if current_item is not None:
            self.unequip_from_slot(slot, add_message)

        setattr(self, slot, item)

        if add_message:
            self.equip_message(item.name)

    def unequip_from_slot(self, slot: str, add_message: bool) -> None:
        current_item = getattr(self, slot)

        if add_message:
            self.unequip_message(current_item.name)

        setattr(self, slot, None)
```

`equip_to_slot` and `unequip_from_slot` with add or remove an Item to the given “slot” (`weapon` or `armor`). We use `getattr` to get the slot, whether it’s `weapon` or `armor`. We use `getattr` because we won’t actually know which one we’re getting until the function is called. `getattr` allows us to “get an attribute” on a class (`self` in this case) and do what we want with it. We use `setattr` to “set the attribute” the same way.

`unequip_from_slot` simply removes the item. `equip_to_slot` first checks if there’s something equipped to that slot, and calls `unequip_from_slot` if there is. This way, the player can’t equip two things to the same slot.

What’s with the `add_message` part? Normally, we’ll want to add a message to the message log when we equip/remove things, but in this section, we’ll see an exception: When we set up the player’s initial equipment. We’ll use the same “equip” methods to set up the initial equipment, but there’s no need to begin every game with messages that say the player put on their starting equipment (presumably, the player character did this before walking into the deadly dungeon). `add_message` gives us a simple way to not add the messages if they aren’t necessary. In your game, there might be other scenarios where you don’t want to display these messages.

```python
    def toggle_equip(self, equippable_item: Item, add_message: bool = True) -> None:
        if (
            equippable_item.equippable
            and equippable_item.equippable.equipment_type == EquipmentType.WEAPON
        ):
            slot = "weapon"
        else:
            slot = "armor"

        if getattr(self, slot) == equippable_item:
            self.unequip_from_slot(slot, add_message)
        else:
            self.equip_to_slot(slot, equippable_item, add_message)
```

Finally, we have `toggle_equip`, which is the method that will actually get called when the player selects an equippable item. It checks the equipment’s type (to know which slot to put it in), and then checks to see if the same item is already equipped to that slot. If it is, the player presumably wants to remove it. If not, the player wants to equip it.

To sum up, this component holds references to equippable entities, calculates the bonuses the player gets from them (which will get added to the player’s power and defense values), and gives a way to equip or remove the items.

Let’s add this component to the actors now. `entity.py` and add these lines:

Diff Original

```diff
...
if TYPE_CHECKING:
    from components.ai import BaseAI
    from components.consumable import Consumable
+   from components.equipment import Equipment
    from components.equippable import Equippable
    from components.fighter import Fighter
    from components.inventory import Inventory
    from components.level import Level
    from game_map import GameMap
...

class Actor(Entity):
    def __init__(
        self,
        *,
        x: int = 0,
        y: int = 0,
        char: str = "?",
        color: Tuple[int, int, int] = (255, 255, 255),
        name: str = "<Unnamed>",
        ai_cls: Type[BaseAI],
+       equipment: Equipment,
        fighter: Fighter,
        inventory: Inventory,
        level: Level,
    ):
        super().__init__(
            x=x,
            y=y,
            char=char,
            color=color,
            name=name,
            blocks_movement=True,
            render_order=RenderOrder.ACTOR,
        )

        self.ai: Optional[BaseAI] = ai_cls(self)

+       self.equipment: Equipment = equipment
+       self.equipment.parent = self

        self.fighter = fighter
        self.fighter.parent = self

        ...
```

We also need to update `entity_factories.py` once again, to create the actors with the `Equipment` component:

Diff Original

```diff
from components.ai import HostileEnemy
from components import consumable, equippable
+from components.equipment import Equipment
from components.fighter import Fighter
from components.inventory import Inventory
from components.level import Level


player = Actor(
    char="@",
    color=(255, 255, 255),
    name="Player",
    ai_cls=HostileEnemy,
+   equipment=Equipment(),
    fighter=Fighter(hp=30, base_defense=1, base_power=2),
    inventory=Inventory(capacity=26),
    level=Level(level_up_base=200),
)
orc = Actor(
    char="o",
    color=(63, 127, 63),
    name="Orc",
    ai_cls=HostileEnemy,
+   equipment=Equipment(),
    fighter=Fighter(hp=10, base_defense=0, base_power=3),
    inventory=Inventory(capacity=0),
    level=Level(xp_given=35),
)
troll = Actor(
    char="T",
    color=(0, 127, 0),
    name="Troll",
    ai_cls=HostileEnemy,
+   equipment=Equipment(),
    fighter=Fighter(hp=16, base_defense=1, base_power=4),
    inventory=Inventory(capacity=0),
    level=Level(xp_given=100),
)
...
```

One thing we need to do is change the way `power` and `defense` are calculated in the `Fighter` component. Currently, the values are set directly in the class, but we’ll want to calculate them based on their base values (what gets leveled up), and the bonus values (based on the equipment).

We can redefine `power` and `defense` as properties, and rename what we set in the class to `base_power` and `base_defense`. `power` and `defense` will then get their values from their respective bases and equipment bonuses.

This will require edits to several places, but we’ll start first with the most obvious: the `Fighter` class itself.

Diff Original

```diff
class Fighter(BaseComponent):
    parent: Actor

-   def __init__(self, hp: int, defense: int, power: int):
+   def __init__(self, hp: int, base_defense: int, base_power: int):
        self.max_hp = hp
        self._hp = hp
-       self.defense = defense
-       self.power = power
+       self.base_defense = base_defense
+       self.base_power = base_power

    @property
    def hp(self) -> int:
        return self._hp

    @hp.setter
    def hp(self, value: int) -> None:
        self._hp = max(0, min(value, self.max_hp))
        if self._hp == 0 and self.parent.ai:
            self.die()

+   @property
+   def defense(self) -> int:
+       return self.base_defense + self.defense_bonus

+   @property
+   def power(self) -> int:
+       return self.base_power + self.power_bonus

+   @property
+   def defense_bonus(self) -> int:
+       if self.parent.equipment:
+           return self.parent.equipment.defense_bonus
+       else:
+           return 0

+   @property
+   def power_bonus(self) -> int:
+       if self.parent.equipment:
+           return self.parent.equipment.power_bonus
+       else:
+           return 0

    def die(self) -> None:
        ...
```

`power` and `defense` are now computed based on the base values and the bonus values offered by the equipment (if any exists).

We’ll need to edit `level.py` to reflect the new attribute names as well:

Diff Original

```diff
class Level(BaseComponent):
    ...

    def increase_power(self, amount: int = 1) -> None:
-       self.parent.fighter.power += amount
+       self.parent.fighter.base_power += amount

        self.engine.message_log.add_message("You feel stronger!")

        self.increase_level()

    def increase_defense(self, amount: int = 1) -> None:
-       self.parent.fighter.defense += amount
+       self.parent.fighter.base_defense += amount

        self.engine.message_log.add_message("Your movements are getting swifter!")
```

We also have to adjust the initializations in `entity_factories.py`:

Diff Original

```diff
player = Actor(
    char="@",
    color=(255, 255, 255),
    name="Player",
    ai_cls=HostileEnemy,
    equipment=Equipment(),
-   fighter=Fighter(hp=30, defense=2, power=5),
+   fighter=Fighter(hp=30, base_defense=1, base_power=2),
    inventory=Inventory(capacity=26),
    level=Level(level_up_base=200),
)
orc = Actor(
    char="o",
    color=(63, 127, 63),
    name="Orc",
    ai_cls=HostileEnemy,
    equipment=Equipment(),
-   fighter=Fighter(hp=10, defense=0, power=3),
+   fighter=Fighter(hp=10, base_defense=0, base_power=3),
    inventory=Inventory(capacity=0),
    level=Level(xp_given=35),
)
troll = Actor(
    char="T",
    color=(0, 127, 0),
    name="Troll",
    ai_cls=HostileEnemy,
    equipment=Equipment(),
-   fighter=Fighter(hp=16, defense=1, power=4),
+   fighter=Fighter(hp=16, base_defense=1, base_power=4),
    inventory=Inventory(capacity=0),
    level=Level(xp_given=100),
)
...
```

Notice that we’ve changed the player’s base values a bit. This is to compensate for the fact that the player will be getting bonuses from the equipment soon. Feel free to tweak these values however you see fit.

Now all that’s left to do is allow generate the equipment to the map, and allow the player to interact with it. To create equipment, we can simply edit our `item_chances` dictionary to include weapons and armor on certain floors. Edit `procgen.py` like this:

Diff Original

```diff
item_chances: Dict[int, List[Tuple[Entity, int]]] = {
    0: [(entity_factories.health_potion, 35)],
    2: [(entity_factories.confusion_scroll, 10)],
-   4: [(entity_factories.lightning_scroll, 25)],
-   6: [(entity_factories.fireball_scroll, 25)],
+   4: [(entity_factories.lightning_scroll, 25), (entity_factories.sword, 5)],
+   6: [(entity_factories.fireball_scroll, 25), (entity_factories.chain_mail, 15)],
}
```

This will generate swords and chain mail at levels 4 and 6, respectively. You can change the floor or the weights if you like.

Now that equipment will spawn on the map, we need to allow the user to equip and remove equippable entities. The first step is to add an action to equip things, which we’ll call `EquipAction`. Add this class to `actions.py`:

Diff Original

```diff
...
class DropItem(ItemAction):
    ...


+class EquipAction(Action):
+   def __init__(self, entity: Actor, item: Item):
+       super().__init__(entity)

+       self.item = item

+   def perform(self) -> None:
+       self.entity.equipment.toggle_equip(self.item)


class WaitAction(Action):
    ...
```

The action itself is very straightforward: It holds which item is being equipped/removed, and calls the `toggle_equip` method. The `Equipment` component handles most of the work here.

But how do we _use_ this action? The simplest way would be to expand the functionality of our original inventory menu. If the user selects a piece of equipment from that menu, we’ll either equip the item, or remove it, if it’s already equipped. We should also show the user a visual representation of which items are already equipped.

Modify `input_handlers.py` like this:

Diff Original

```diff
class InventoryEventHandler(AskUserEventHandler):
    def on_render(self, console: tcod.Console) -> None:
        ...

        if number_of_items_in_inventory > 0:
            for i, item in enumerate(self.engine.player.inventory.items):
                item_key = chr(ord("a") + i)
-               console.print(x + 1, y + i + 1, f"({item_key}) {item.name}")

+               is_equipped = self.engine.player.equipment.item_is_equipped(item)

+               item_string = f"({item_key}) {item.name}"

+               if is_equipped:
+                   item_string = f"{item_string} (E)"

+               console.print(x + 1, y + i + 1, item_string)
        else:
            console.print(x + 1, y + 1, "(Empty)")

    ...

class InventoryActivateHandler(InventoryEventHandler):
    """Handle using an inventory item."""

    TITLE = "Select an item to use"

    def on_item_selected(self, item: Item) -> Optional[ActionOrHandler]:
-       """Return the action for the selected item."""
-       return item.consumable.get_action(self.engine.player)
+       if item.consumable:
+           # Return the action for the selected item.
+           return item.consumable.get_action(self.engine.player)
+       elif item.equippable:
+           return actions.EquipAction(self.engine.player, item)
+       else:
+           return None


class InventoryDropHandler(InventoryEventHandler):
    ...
```

The first change is modifying the render function to display an “(E)” next to items that are equipped. Items that aren’t equipped are displayed the same way as before.

The second change has to do with using the item. Before, we were just assuming the item was a consumable. Now, if the item is a consumable, we call the `get_action` method on the `Consumable` component, just like before. If it’s instead equippable, we call the `EquipAction`. If it’s neither, nothing happens.

Run the game now, you’ll be able to pick up and equip things. I recommend adjusting the values in `procgen.py` to make equipment spawn earlier and more often, just for testing purposes.

If you play around a bit, you might notice an odd bug: If the player drops something that’s equipped… it stays equipped! That doesn’t make sense, as dropping something should unequip it as well. Luckily, the fix is quite simple: We can adjust our `DropItem` action to unequip an item if it’s being dropped and it’s equipped. Make the following additions to `actions.py`:

Diff Original

```diff
class DropItem(ItemAction):
    def perform(self) -> None:
+       if self.entity.equipment.item_is_equipped(self.item):
+           self.entity.equipment.toggle_equip(self.item)

        self.entity.inventory.drop(self.item)
```

One last thing we can do is give the player a bit of equipment to start. We’ll spawn a dagger and leather armor, and immediately add them to the player’s inventory.

Diff Original

```diff
def new_game() -> Engine:
    ...

    engine.message_log.add_message(
        "Hello and welcome, adventurer, to yet another dungeon!", color.welcome_text
    )

+   dagger = copy.deepcopy(entity_factories.dagger)
+   leather_armor = copy.deepcopy(entity_factories.leather_armor)

+   dagger.parent = player.inventory
+   leather_armor.parent = player.inventory

+   player.inventory.items.append(dagger)
+   player.equipment.toggle_equip(dagger, add_message=False)

+   player.inventory.items.append(leather_armor)
+   player.equipment.toggle_equip(leather_armor, add_message=False)

    return engine
```

As mentioned earlier, we pass `add_message=False` to signify not to add a message to the message log.

![Part 13 - End](https://rogueliketutorials.com/images/part-13-end.png)

With that, we’ve reached the end of the tutorial! Thank you so much for following along, and be sure to check out the [extras section](https://rogueliketutorials.com/tutorials/tcod/v2). More will be added there over time. If you have a suggestion for an extra, let me know!

Be sure to check out the [Roguelike Development Subreddit](https://www.reddit.com/r/roguelikedev) for help, for inspiration, or to share your progress.

Best of luck on your roguelike development journey!

If you want to see the code so far in its entirety, [click here](https://github.com/TStand90/tcod_tutorial_v2/tree/2020/part-13).

© 2023 · Powered by [Hugo](https://gohugo.io/) & [Coder](https://github.com/luizdepra/hugo-coder/).