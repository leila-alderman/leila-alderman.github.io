# Using TDD to Write a Ruby Connect Four Game

This tutorial walks through how to write a game of [Connect Four](https://en.wikipedia.org/wiki/Connect_Four) that can be played in the terminal using the principles of [test-driven development](https://en.wikipedia.org/wiki/Test-driven_development) (TDD). 

[This project](https://www.theodinproject.com/courses/ruby-programming/lessons/testing-your-ruby-code?ref=lnav) is part of [The Odin Project](https://www.theodinproject.com) curriculum, and this post follows much of the methodology in an older [tic-tac-toe game tutorial](https://codequizzes.wordpress.com/2013/10/25/creating-a-tic-tac-toe-game-with-ruby/).

The GitHub repository for this tutorial can be found [here](https://github.com/leila-alderman/connect_four).

## Objective

This Connect Four game can be played by two human players on the command line. The Connect Four board has 7 columns and 6 rows. The players alternate placing a token in one of the columns, which then falls as far it can go, until one of the players has four tokens forming a horizontal, vertical, or diagonal line. The game ends in a tie if neither player has won and all of the spaces on the board are taken. 

## Creating a Gem

To package all of our code nicely, we're going to place it inside of a Ruby gem. [Ruby gems](https://rubygems.org/) follow a [specific structure](https://en.wikipedia.org/wiki/RubyGems#Structure_of_a_gem) that makes it easy for other developers to follow your work and to install your program. In the command line, navigate to the folder that you want to contain your new project folder. Then, type `bundle gem connect_four` to create your new project. The output should be something similar to the following: 

~~~bash
Creating gem 'connect_four'...
Do you want to generate tests with your gem?
Type 'rspec' or 'minitest' to generate those test files now and in the future. rspec/minitest/(none):
~~~

This tutorial uses RSpec, so type in `rspec` and hit `Enter`. The command line will then ask you questions about licensing your code and adding a code of conduct; you're free to answer these questions however you prefer, but the defaults would be to type `y` for both.






