---
layout: post
title: Ruby TDD Tutorial | Tic-Tac-Toe Part 2
---

This tutorial walks through how to write a game of [tic-tac-toe](https://en.wikipedia.org/wiki/Tic-tac-toe) that can be played in the terminal using the principles of [test-driven development](https://en.wikipedia.org/wiki/Test-driven_development) (TDD), specifically using [RSpec](https://semaphoreci.com/community/tutorials/getting-started-with-rspec). 

This post follows much of the methodology in an out-dated [tic-tac-toe tutorial](https://codequizzes.wordpress.com/2013/10/25/creating-a-tic-tac-toe-game-with-ruby/), and [this project](https://www.theodinproject.com/courses/ruby-programming/lessons/oop) is part of [The Odin Project](https://www.theodinproject.com) curriculum.

The GitHub repository for this tutorial can be found [here](https://github.com/leila-alderman/tictactoe).

In [Part 1](https://leila-alderman.github.io/2019/02/01/Ruby-Tutorial-Tic-Tac-Toe-Part-1.html), we went over how to set up our project as a gem, and we created our first two classes: the Square class and the Player class.

## Board Class

We've already built our Square class, so now we need to pull those squares together into a tic-tac-toe board. The Board class needs to be able to keep track of the board state and its changes when a player makes a move, and it should check to see if the game has ended with a win or with a draw. 

Wait right there! That description of the intended responsibilities for the Board class is too long. It doesn't follow the tenant of object-oriented programming (OOP) that each class should have only one responsibility. Every class should perform the smallest possible useful action. The very fact that the Board class description above used the word "and" should throw up a big OOP red flag! 

Alright, so now we know that we're going to need two classes that can handle two separate responsibilies: 1) keep track of the board state and 2) apply game over logic to the board. We'll start out by writing the Board class, which will keep track of the board state.

To keep things simple, the board state will be represented as an array of arrays to create the 3x3 grid.

~~~ruby
grid_array =  [ [1, 2, 3],
                [4, 5, 6],
                [7, 8, 9] ]
~~~

This structure provides a convenient coordinate system that we can use to access elements. For example, the `4` can be accessed with `grid_array[1][0]`, and the `8` with `grid_array[2][1]`. (Remember that Ruby uses zero-based indexing.)

Because this class is more complicated than the other classes we've made so far, we'll go through the process of creating the Board class method by method.

### Board.new

We'll start by creating a new file in `spec/` called `board_spec.rb` and thinking about what functionality we expect this class to have. 

The `#initialize` method is always a good place to start. The Board class should have an array structure similar to the one shown above; however, instead of the integers, it should have a default value of a new Square instance, which will have a value of `' '`. 

~~~ruby
# spec/board_spec.rb
RSpec.describe Tictactoe::Board do
  
  before do
    @board = Tictactoe::Board.new
  end

  context "#initialize" do
    it "has three rows" do
      expect(@board.state.length).to eql 3
    end

    it "has three elements in each row" do
      @board.state.each do |row|
        expect(row.length).to eql 3
      end
    end

    it "has default elements of Square instances" do
      @board.state.each do |row|
        row.each do |element|
          expect(element.value).to eql ' '
        end
      end
    end 
  end

end
~~~

Note that these tests are written quite generally and do not actually rely on the Board class knowing the name of the Square class. This provides more long-term resiliancy to our tests because they will still work even if we end up renaming the Square class to, for example, Cell. Right now, the only thing that the Board class tests rely on is being passed some sort of default object that will respond to the `#value` method call.

Now that the tests are written, we can start to create the class. We'll start by creating a new file, `lib/tictactoe/board.rb` and adding it to our main `tictactoe.rb` file.

~~~ruby
# lib/tictactoe.rb
require "tictactoe/board"
~~~

Now, we can write the `#initialize` method for the Board class. We'll go ahead and add the `attr_reader` line in this step so that our tests can see the `#state` attributue.

~~~ruby
# lib/tictactoe/board.rb
module Tictactoe
  class Board
    attr_reader :state

    def initialize
      @state = Array.new(3) { Array.new(3) { Square.new } }
    end
  end
end
~~~

When we run `bundle exec rspec`, we can see that our code is passing all of the tests. 

![RSpec passing tests for Board#initialize](/assets/tictactoe/Board-1.jpg)

### Board.state

We have already included `attr_attribute :state` in our class, but it doesn't currently have any tests. We need to check that this method will return the `state` attribute without being able to write to it.

~~~ruby
# spec/board_spec.rb
context "#state" do
  it "returns the state" do
    expect(@board.state[1][1].value).to eql ' '
  end

  it "raises an error when trying to change the state" do
    expect { @board = Tictactoe::Board.new; 
    @board.state = "X" }.to raise_error(NoMethodError)
  end
end
~~~

We can go straight to running our tests to see if they pass.

![RSpec passing tests for Board#state](/assets/tictactoe/Board-2.jpg)



Notes:
 - Rearrange: Square, Board, Player, Logic, Game
 - give Player the ability to make moves
 - add method for getting/setting Board values
