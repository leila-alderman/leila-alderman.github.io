---
title: Ruby TDD Tutorial | Tic-Tac-Toe Part 2
---

# Ruby TDD Tutorial | Tic-Tac-Toe Part 2

This tutorial walks through how to write a game of [tic-tac-toe](https://en.wikipedia.org/wiki/Tic-tac-toe) that can be played in the terminal using the principles of [test-driven development](https://en.wikipedia.org/wiki/Test-driven_development) (TDD), specifically using [RSpec](https://semaphoreci.com/community/tutorials/getting-started-with-rspec). 

[This project](https://www.theodinproject.com/courses/ruby-programming/lessons/oop) is part of [The Odin Project](https://www.theodinproject.com) curriculum, and this post follows much of the methodology in an older [tic-tac-toe game tutorial](https://codequizzes.wordpress.com/2013/10/25/creating-a-tic-tac-toe-game-with-ruby/).

The GitHub repository for this tutorial can be found [here](https://github.com/leila-alderman/tictactoe).

In [Part 1](https://leila-alderman.github.io/2019/02/01/Ruby-Tutorial-Tic-Tac-Toe-Part-1.html), we went over how to set up our project as a gem, and we created our first two classes: the Square class and the Player class.

## Board Class

We've already built our Square class, so now we need to pull those squares together into a tic-tac-toe board. The Board class needs to be able to keep track of the board state, change values when a player makes a move, and check to see if the game has ended with a win or with a draw. 


### Writing the Board Spec


### Creating the Board Class