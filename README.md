# How I Built This:

I cannot, for the life of me, figure out why I am getting this error:

```shell
Uncaught TypeError: this.findSpotForCol is not a function
    at HTMLTableRowElement.handleClick (connect4.js:77:20)
```



## Part One: Make Game Into a Class
Right now, our Connect Four is a bunch of disconnected functions and a few global variables.

This can make it hard to see how things work, and would make it hard to restart a game (quick—which variables would you have to reset to start a game?)
##### I would reset the board array (and it'll revert back to all being filled with null).

Let’s move this to being a class.

Initially, we’ll start with one class, Game. The players will still just be numbers for player #1 and #2.

* What are the instance variables you’ll need on the Game?
    ##### HEIGHT, WIDTH, currPlayer variable...
    * for example: height, width, and the board will move from global variables to instance attributes on the class. What else should move?
* Make a constructor that sets default values for these
    ##### See step 1. below:
* Move the current functions onto the class as methods
    ##### See step 3 below: 
    * This will require mildly rewriting some of these to change how you access variables and call other methods
    ##### Rewriting to change access to variables and calling other methods in step N:
You should end up with all of the code being in the Game class, with the only other code being a single line at the bottom:

```js
new Game(6, 7);   // assuming constructor takes height, width
```

1.  Make a constructor that sets default values for these:
```js
class Game {
  constructor(HEIGHT=6, WIDTH=7) {
    this.HEIGHT = HEIGHT;
    this.WIDTH = WIDTH;
    this.currPlayer = 1;
  }
}
```

2. At this point, we can check the browser/localhost and see our project. Go into 
developer mode and see an error stating the following: 
```shell
Uncaught ReferenceError: HEIGHT is not defined
    at makeBoard (connect4.js:15:23)
    at connect4.js:146:1
```
From this error, we can see that the makeBoard function is outside of the Game class scope. 

3. Move the current functions onto the class as methods: 

```js
class Game {
  constructor(HEIGHT=6, WIDTH=7) {
    this.HEIGHT = HEIGHT;
    this.WIDTH = WIDTH;
    this.currPlayer = 1;
  }

  makeBoard() {
    for (let y = 0; y < this.HEIGHT; y++) {
      board.push(Array.from({ length: this.WIDTH }));
    }
  }
}
let board = []; 
```

The following error is from line #148 where we call the makeBoard function. 
```shell
Uncaught ReferenceError: makeBoard is not defined
    at connect4.js:148:1
```
We can resolve this ReferenceError by moving makeBoard into the constructor.
```js
class Game {
  constructor(HEIGHT=6, WIDTH=7) {
    this.HEIGHT = HEIGHT;
    this.WIDTH = WIDTH;
    this.currPlayer = 1;
    this.makeBoard();
  }

  makeBoard() {
    let board = []; // Decision to move board into the local scope of makeBoard function
    for (let y = 0; y < this.HEIGHT; y++) {
      this.board.push(Array.from({ length: this.WIDTH }));
    }
  }
}
```

4. We now have a console error with the following message: 
```shell
Uncaught ReferenceError: WIDTH is not defined
    at makeHtmlBoard (connect4.js:33:23)
    at connect4.js:150:1
```
Following step 3's pattern, we will add makeHtmlBoard into our Game class:
Note: This step is quite simple - just chance WIDTH to this.WIDTH and HEIGHT to this.HEIGHT.

5. Move findSpotForCol(x) function into Game class. This is also just a simple refactoring. Change HEIGHT to this.HEIGHT (to reference HEIGHT properly).


# Notes to Self:
How to console.log(this) => [Object Object]
### console.log(JSON.stringify(result))
[Console.log(this)](https://stackoverflow.com/questions/41336663/console-logresult-returns-object-object-how-do-i-get-result-name)

6. Inside makeHtmlBoard() function,

```js
makeHtmlBoard() {
  // ...
  this.handleGameClick = this.handleClick.bind(this);
  top.addEventListener('click', this.handleGameClick);
  // Make sure to overwrite the addEventListener click attached to top.
}

```

## Part Two: Small Improvements
Make it so that you have a button to “start the game” — it should only start the game when this is clicked, and you should be able to click this to restart a new game.

Add a property for when the game is over, and make it so that you can’t continue to make moves after the game has ended.

## Part Three: Make Player a Class
Right now, the players are just numbers, and we have hard-coded player numbers and colors in the CSS.

Make it so that there is a Player class. It should have a constructor that takes a string color name (eg, “orange” or “#ff3366”) and store that on the player instance.

The Game should keep track of the current player object, not the current player number.

Update the code so that the player pieces are the right color for them, rather than being hardcoded in CSS as red or blue.

Add a small form to the HTML that lets you enter the colors for the players, so that when you start a new game, it uses these player colors.