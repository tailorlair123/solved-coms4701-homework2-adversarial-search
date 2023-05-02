Download Link: https://assignmentchef.com/product/solved-coms4701-homework2-adversarial-search
<br>
In this assignment, you will create an adversarial search agent to play the ​<strong>2048-puzzle</strong>​ game. A demo of the game is available here: ​<strong><u><a href="https://gabrielecirulli.github.io/2048">gabrielecirulli</a><a href="https://gabrielecirulli.github.io/2048">.</a><a href="https://gabrielecirulli.github.io/2048">github</a><a href="https://gabrielecirulli.github.io/2048">.</a><a href="https://gabrielecirulli.github.io/2048">i</a><a href="https://gabrielecirulli.github.io/2048">o</a><a href="https://gabrielecirulli.github.io/2048">/2048</a></u></strong><u>​</u><a href="https://gabrielecirulli.github.io/2048">.</a>

<strong>I.</strong>​ 2048 As A Two-Player Game

<strong>II.</strong>​ Choosing a Search Algorithm: Expectiminimax

<strong>III.</strong>​ Using The Skeleton Code

<strong>IV.</strong>​ What You Need To Submit

<strong>V.</strong>​ Important Information <strong>VI.</strong>​ Before You Submit

<h1>I. 2048 As A Two-Player Game</h1>

2048 is played on a ​<strong>4×4 grid</strong>​ with numbered tiles which can slide up, down, left, or right. This game can be modeled as a two player game, in which the computer AI generates a 2- or 4-tile placed randomly on the board, and the player then selects a direction to move the tiles. Note that the tiles move until they either (1) collide with another tile, or (2) collide with the edge of the grid. If two tiles of the same number collide in a move, they merge into a single tile valued at the sum of the two originals. The resulting tile cannot merge with another tile again in the same move.

Usually, each role in a two-player games has a similar set of moves to choose from, and similar objectives (e.g. chess). In 2048 however, the player roles are inherently <strong>a</strong>​ <strong>symmetric</strong>,​ as the Computer AI places tiles and the Player moves them. Adversarial search can still be applied! Using your previous experience with objects, states, nodes, functions, and implicit or explicit search trees, along with our ​<strong>skeleton code</strong>​, focus on optimizing your player algorithm to solve 2048 as efficiently and consistently as possible.

II. Choosing A Search Algorithm: Expectiminimax

Review the lecture on ​<strong>adversarial search</strong>​. Is 2048 a zero-sum game? What are the minimax and expectiminimax principles?

The tile-generating Computer AI of 2048 is not particularly adversarial as it spawns tiles irrespective of whether a spawn is the most adversarial to the user’s progress, with a 90% probability of a 2 and 10% for a 4 (from GameManager_3.py). However, our Player AI will play ​<strong>as if</strong>​ the computer is adversarial since this proves more effective in beating the game. We will specifically use the ​<strong>expectiminimax </strong>​algorithm.

With expectiminimax, your game playing ​<strong>strategy</strong>​ assumes the Computer AI chooses a tile to place in a way that minimizes the Player’s outcome. Note whether or not the Computer AI is optimally adversarial is a question to consider. As a general principle, how far the opponent’s behavior deviates from the player’s assumption certainly affects how well the AI performs. However you will see that this strategy works well in this game.

Expectiminimax is a natural extension of the minimax algorithm, so think about how to implement minimax first. As we saw in the simple case of tic-tac-toe, it is useful to employ the minimax algorithm assuming the opponent is a perfect “minimizing” agent. In practice, an algorithm with the perfect opponent assumption deviates from reality when playing a <strong>s</strong>​ <strong>ub-par opponent</strong> ​ making silly moves, but still leads to the desired outcome of never losing. If the deviation goes the other way, however, (a “maximax” opponent in which the opponent wants us to win), winning is obviously not guaranteed.

<strong> </strong>III. Using The Skeleton Code

The skeleton code includes the following files. Note that you will only be working in <strong>o</strong>​ <strong>ne</strong> ​ of them, and the rest are read-only:

<ul>

 <li><strong>Read-only</strong>​: ​py​. This is the driver program that loads your Computer AI and Player AI and begins a game where they compete with each other. See below on how to execute this program.</li>

 <li><strong>Read-only</strong>​: ​py​. This module defines the Grid object, along with some useful operations: move()​, ​getAvailableCells()​, ​insertTile()​, and ​clone()​, which you may use in your code. These are by no means the most efficient methods available, so if you wish to strive for better performance, feel free to ignore these and write your own helper methods in a separate file.</li>

 <li><strong>Read-only</strong>​: ​py​. This is the base class for any AI component. All AIs inherit from this module, and implement the ​getMove()​ function, which takes a Grid object as parameter and returns a move (there are different “moves” for different AIs).</li>

 <li><strong>Read-only</strong>​: ​py​. This inherits from BaseAI. The g​ etMove() ​ function returns a computer action that is a tuple (x, y) indicating the place you want to place a tile.</li>

 <li><strong>Writable</strong>​:​ ​py.​ You will create this file. The PlayerAI class should inherit from BaseAI. The getMove()​ function to implement must return a number that indicates the player’s action. In particular, <strong>0</strong>​<strong> stands for “Up”, 1 stands for “Down”, 2 stands for “Left”, and 3 stands for “Right”</strong>.​ This is where your player-optimizing logic lives and is executed. Feel free to create submodules for this file to use, and include any submodules in your submission.</li>

 <li><strong>Read-only</strong>​: ​py​ and D​isplayer_3.py​. These print the grid.</li>

</ul>

To test your code, execute the game manager like so: ​$ python GameManager_3.py

The progress of the game will be displayed on your terminal screen with one snapshot printed after each move that the Computer AI or Player AI makes. Your Player AI is allowed ​<strong>0.2 seconds</strong> ​ to come up with each move. The process continues until the game is over; that is, until no further legal moves can be made. At the end of the game, the ​<strong>maximum tile value</strong>​ on the board is printed.

<strong>IMPORTANT</strong>​: Do not modify the files that are specified as read-only. When your submission is graded, the grader will first automatically ​<strong>over-write</strong>​ all read-only files in the directory before executing your code. This is to ensure that all students are using the same game-play mechanism and computer opponent, and that you cannot “work around” the skeleton program and manually output a high score.

<h1>IV. What You Need To Submit</h1>

Your job in this assignment is to write ​PlayerAI_3.py​, which intelligently plays the 2048-puzzle game. Here is a snippet of ​<strong>starter code</strong> ​ to allow you to observe how the game looks when it is played out. In the following “naive” Player AI. The ​getMove() ​function simply selects a next move in random out of the available moves:




import random

from BaseAI_3 import BaseAI

class PlayerAI(BaseAI):     def getMove(self, grid):

# Selects a random move and returns it    moveset = grid.getAvailableMoves()

return random.choice(moveset)[0] if moveset else None

Of course, that is indeed a very naive way to play the 2048-puzzle game. If you submit this as your finished product, you will likely receive a low grade. You should implement your Player AI with the following points in mind:

<ul>

 <li>Employ the ​<strong>expectiminimax algorithm</strong>.​ This is a requirement. There are many viable strategies to beat the 2048-puzzle game, but in this assignment we will be using he expectiminimax algorithm. Note that <strong>90% of tiles placed by the computer are 2’s</strong>,​ while the remaining <strong>1</strong>​ <strong>0% are 4’s</strong>.​ It may be helpful to first implement regular minimax.</li>

 <li>Implement ​<strong>alpha-beta pruning</strong>​. This is a requirement. This should speed up the search process by eliminating irrelevant branches. In this case, is there anything we can do about move ordering?</li>

 <li>Use ​<strong>heuristic functions</strong>.​ What is the maximum height of the game tree? Unlike elementary games like tic-tac-toe, in this game it is highly impracticable to search the entire depth of the theoretical game tree. To be able to cut off your search at any point, you must employ <strong>h</strong>​ <strong>euristic functions</strong> ​ to allow you to assign approximate values to nodes in the tree. Remember, the time limit allowed for each move is 0.2 seconds, so you must implement a systematic way to cut off your search before time runs out.</li>

 <li>Assign <strong>h</strong>​ <strong>euristic weights</strong>.​ You will likely want to include more than one heuristic function. In that case, you will need to assign weights associated with each individual heuristic. Deciding on an appropriate set of weights will take careful reasoning, along with careful experimentation. If you feel adventurous, you can also simply write an optimization meta-algorithm to iterate over the space of weight vectors, until you arrive at results that you are happy enough with.</li>

 <li>V. Important Information</li>

</ul>

Please read the following information carefully. Before you post a clarifying question on the discussion board, make sure that your question is not already answered in the following sections.




<h2>1. Note on Python 3</h2>

.

We will only accept homeworks from now and on in python 3 (python 2 is being discontinued).

To test your algorithm in Python 3, execute the game manager like so: $ python3 GameManager_3.py

<h2>2. basic Requirements</h2>

Your submission <strong>m</strong>​ <strong>ust</strong> ​ fulfill the following requirements:

<ul>

 <li>You must use adversarial search in your PlayerAI (expectiminimax with alpha-beta pruning).</li>

 <li>You must provide your move within the time limit of 0.2 seconds.</li>

 <li>You must name your file ​py ​ (Python 3).</li>

 <li>Your grade will depend on the maximum tile values your program usually gets to.</li>

</ul>