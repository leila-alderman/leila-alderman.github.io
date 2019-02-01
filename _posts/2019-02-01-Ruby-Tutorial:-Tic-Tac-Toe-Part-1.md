---
layout: post
title: Ruby TDD Tutorial: Tic-Tac-Toe Part 1
---

# Ruby TDD Tutorial: Tic-Tac-Toe Part 1

This tutorial walks through how to write a game of [tic-tac-toe](https://en.wikipedia.org/wiki/Tic-tac-toe) that can be played in the terminal using the principles of [test-driven development](https://en.wikipedia.org/wiki/Test-driven_development) (TDD), specifically using [RSpec](https://semaphoreci.com/community/tutorials/getting-started-with-rspec). 

[This project](https://www.theodinproject.com/courses/ruby-programming/lessons/oop) is part of [The Odin Project](https://www.theodinproject.com) curriculum, and this post follows much of the methodology in an older [tic-tac-toe game tutorial](https://codequizzes.wordpress.com/2013/10/25/creating-a-tic-tac-toe-game-with-ruby/).

The GitHub repository for this tutorial can be found [here](https://github.com/leila-alderman/tic-tac-toe).

## Objective

This tic-tac-toe game can be played by two human players on the command line. The tic-tac-toe board is a 3x3 grid, and the players alternate placing their marker ("X" or "O") in one of the squares. A player wins when they have three of their markers in a row horizontally, vertically, or diagonally. The game ends in a tie if neither player has won and all of the spaces on the board are taken. 

## Programming Strategy

This tutorial closely follows the principles of both object-oriented programming (OOP) and test-driven development (TDD). These are both important concepts to follow, and they are closely intertwined. It's difficult to have a TDD project or even code that can be thoroughly and easily tested if the code does not follow best practices of OOP. If much of the logic is wrapped up in long and messy functions, writing clear tests is very difficult. 

To see a clear example of these problems, take a look at [the first version of this game](https://github.com/leila-alderman/tic-tac-toe). The functions are far too long, and I quickly hit a wall in being able to write tests for this code. In fact, I wrote [only three tests](https://github.com/leila-alderman/tic-tac-toe/blob/master/spec/tic_tac_toe_spec.rb) before having to accept just how poorly architected my code was. It worked, sure, but it wasn't clean.

Let's see if we can do better!

One of the helpful guidelines for OOP is to make a class for each noun you use when describing the functionality of your project. Looking at the objective above, the main nouns, and therefore the classes that we're going to build, are "square", "board", "player", and "game". These classes give us an idea of the overall structure of our code. The verbs used in describing the functionality will become methods within these classes.

## Creating a Gem

To package all of our code nicely, we're going to place it inside of a Ruby gem using [Bundler](https://bundler.io/guides/creating_gem.html). [Ruby gems](https://rubygems.org/) follow a [specific structure](https://en.wikipedia.org/wiki/RubyGems#Structure_of_a_gem) that makes it easy for other developers to follow your work and to install your program. In the command line, navigate to the folder that you want to contain your new project folder. Then, type `bundle gem tictactoe` to create your new project. If this is your first time making a gem, the output should be something similar to the following: 

~~~bash
Creating gem 'tictactoe'...
Do you want to generate tests with your gem?
Type 'rspec' or 'minitest' to generate those test files now and in the future. rspec/minitest/(none):
~~~

This tutorial uses RSpec, so type in `rspec` and hit `Enter`. The command line will then ask you questions about licensing your code and adding a code of conduct; you're free to answer these questions however you prefer, but the defaults would be to type `y` for both.

Otherwise, if you've created a gem before, the first and last lines of the output should be similar to the following:

~~~bash
Creating gem 'tictactoe'...
  ...
Gem 'tictactoe' was successfully created. For more information on making a RubyGem visit https://bundler.io/guides/creating_gem.html
~~~

Now that we have the gem structure set up, we need to make sure that everything is working properly. In the command line, move into the main project directory and try to run `bundle install`. 

If you get the following error,

~~~bash
You have one or more invalid gemspecs that need to be fixed.
The gemspec at /home/leila/Documents/Odin_Project/tictactoe/tictactoe.gemspec is not valid. Please fix this gemspec.
The validation error was 'metadata['homepage_uri'] has invalid link: "TODO: Put your gem's website or public repo URL here."'
~~~

then you need to go into the `tictactoe.gemspec` file and comment out lines 18-28 (everything within the `if spec.respond_to?(:metadata)` statement). In Ruby, you can comment out multiple lines using the following:

~~~ruby
=begin
...
=end
~~~

Be aware, however, that these comment flags will only work if they are not preceeded by any spaces or tabs.

Now, try to run `bundle install` again. This time, you should see some output similar to this:

~~~bash
Fetching gem metadata from https://rubygems.org/..........
Fetching gem metadata from https://rubygems.org/.
Resolving dependencies...
Fetching rake 10.5.0
Installing rake 10.5.0
Using bundler 2.0.1
Using diff-lcs 1.3
Using rspec-support 3.8.0
Using rspec-core 3.8.0
Using rspec-expectations 3.8.2
Using rspec-mocks 3.8.0
Using rspec 3.8.0
Using tictactoe 0.1.0 from source at `.`
Bundle complete! 4 Gemfile dependencies, 9 gems now installed.
Use `bundle info [gemname]` to see where a bundled gem is installed.
~~~

## Passing the First Test

Since we're using TDD to create this project, let's go ahead and get started: in the terminal, type `bundle exec rspec` to run the default tests using Bundler. You should see output similar to this: 

~~~bash
Tictactoe
  has a version number
  does something useful (FAILED - 1)

Failures:

  1) Tictactoe does something useful
     Failure/Error: expect(false).to eq(true)
     
       expected: true
            got: false
     
       (compared using ==)
     # ./spec/tictactoe_spec.rb:9:in `block (2 levels) in <top (required)>'

Finished in 0.11098 seconds (files took 0.47657 seconds to load)
2 examples, 1 failure

Failed examples:

rspec ./spec/tictactoe_spec.rb:8 # Tictactoe does something useful
~~~

Great! We ran our first test! The results show that the "has a version number" test passed but that the "does something useful" test failed. The final line in the output tells us that this failure happened in the `./spec/tictactoe_spec.rb` file on line 8. Let's see what that says:

~~~ruby
# ./spec/tictactoe_spec.rb
it "does something useful" do
    expect(false).to eq(true)
  end
~~~

It should be pretty clear why this test is failing: we're asking whether `false == true`, which is always going to fail. This test is here to just make sure that you're paying attention. Let's delete this test and then run `bundle exec rspec` again. The output should now look nice and green:

~~~bash
Tictactoe
  has a version number

Finished in 0.00268 seconds (files took 0.34991 seconds to load)
1 example, 0 failures
~~~

## Square Class

The tic-tac-toe board is a 3x3 grid, so the board consists of nine squares. Each square can have a value of " ", "X", or "O". Since this is the most fundamental aspect of the tic-tac-toe game, we'll start here.

### Writing the Square Spec

Because we're using TDD to create this project, we'll start by creating the test file for this class before we build the class itself. For every class that we create, we'll first create a new RSpec file in the root of the `spec/` directory. 

Create a new file in `spec/` called `square_spec.rb` where we can add our first tests. First, we need to think about what kind of functionality a Square object should have. A new Square object should have a value of " ", and we should be able to change its value to an "X" or "O". Now, we can write tests that describe this behavior: 

~~~ruby
# spec/square_spec.rb
RSpec.describe Tictactoe::Square do

  # Before running the below tests, create an object
  # that can be used throughout these tests
  before do 
    @square = Tictactoe::Square.new
  end

  context "#initialize" do
    it "initializes with a default value of ' '" do
      expect(@square.value).to eql ' '
    end
  end

  context "#value" do
    it "can be changed to 'X'" do
      @square.value = 'X'
      expect(@square.value).to eql 'X'
    end

    it "can be changed to 'O'" do
      @square.value = 'O'
      expect(@square.value).to eql 'O'
    end
  end

end
~~~

Now, run the test using `bundle exec rspec` in the command line. You should see that your new tests have failed: 

~~~bash
NameError:
  uninitialized constant Tictactoe::Square
~~~

Great, we've finished the first part of TDD! Now, we need to write some code to make these tests pass.

### Writing the Square Class

Create a new file in `lib/tictactoe/` called `square.rb` and write out the basic functionality for this class that will make it pass the tests we just wrote. Keep in mind that all of our classes will be wrapped inside the Tictactoe module to follow the conventions of Ruby gems. This approach helps to prevent namespace collisions when multiple gems are included in a project. 

~~~ruby
# lib/tictactoe/square.rb
module Tictactoe
  class Square
    attr_accessor :value

    def initialize
      @value = ' '
    end
  end
end
~~~

Now, run `bundle exec rspec` again. Our tests are still failing! Why is that? 

We need to tell our program where to look for the code it wants. The test files automatically know to look in`lib/tictactoe.rb` for code to test, so we just need to tell `lib/tictactoe.rb` to load `lib/tictactoe/square.rb` by adding a `require` statement to the top of the file:

~~~ruby
# lib/tictactoe.rb
require "tictactoe/square"
~~~

If we run `bundle exec rspec` now, we should see a lot of happy green:

~~~bash
Tictactoe::Square
  #initialize
    initializes with a default value of ' '
  #value
    can be changed to 'X'
    can be changed to 'O'

Tictactoe
  has a version number

Finished in 0.00412 seconds (files took 0.15577 seconds to load)
4 examples, 0 failures
~~~

Excellent! We have successfully created our first class!

## Player Class

The next simplest class that we need to build is one for the players. The Player class needs to be able to keep track of each player's name and their marker.

### Writing the Player Spec

To start writing the tests for the Player class, we need to create a new file in `spec/` called `player_spec.rb`. What functionality do we want to expect from this class?

First, we want a new player object to require both a `name` and a `marker` when initialized. We can write three tests for this requirement: one that checks that an error is raised when a new player is given zero parameters, one that checks that an error is raised when a new player is given one parameter, and one that checks that no error is raised when a player is initialized with two parameters. 

~~~ruby
# spec/player_spec.rb
RSpec.describe Tictactoe::Player do

  context "#initialize" do
    it "raises an error when given zero parameters" do
      expect { Tictactoe::Player.new }.to raise_error(ArgumentError)
    end

    it "raises an error when given one parameter" do
      expect { Tictactoe::Player.new("Alice") }.to raise_error(ArgumentError)
    end

    it "doesn't raise an error when given two parameters" do
      expect { Tictactoe::Player.new("Alice", "X") }.to_not raise_error
    end
  end
  
end
~~~

Notice how we have written out our [`raise_error`](https://relishapp.com/rspec/rspec-expectations/docs/built-in-matchers/raise-error-matcher) tests. When we're expecting an error, it's best practice to specify the exact error that we are expecting to find. In this case, we're expecting an `ArgumentError` since we're passing in the incorrect number of arguments. Conversely, when we aren't expecting an error to be raised, we don't want to specify a specific error type. These best practices will limit any false positives that could occur when different errors occur, which would potentially allow our code to pass the tests without even running the code we want to test. Furthermore, note that tests that use the `raise_error` matcher require being passed a block of code, unlike the other tests that we've written so far. 

Second, we want to be able to access the `name` and `marker` attributes of a player from outside the object. However, we only want to be able to *read* these attributes; we don't want to be able to change their values.

~~~ruby
# spec/player_spec.rb
before do
  @player = Tictactoe::Player.new("Alice", "X") 
end

context "#name" do
  it "returns the name" do
    expect(@player.name).to eql "Alice"
  end

  it "raises an error when trying to change the name" do
    expect { @player = Tictactoe::Player.new("Alice", "X"); 
    @player.name = "Bob" }.to raise_error(NoMethodError)
  end
end

context "#marker" do
  it "returns the marker" do
    expect(@player.marker).to eql "X"
  end

  it "raises an error when trying to change the marker" do
    expect { @player = Tictactoe::Player.new("Alice", "X"); 
    @player.marker = "G" }.to raise_error(NoMethodError)    
  end
end
~~~

That covers all of the functionality that we want for our Player class, so it's now time to write the class itself.

### Writing the Player Class

We need to start by creating a new file for our new class. In the `lib/tictactoe` folder, we'll create the file `player.rb`. This time, before we get into writing our new class, we'll go ahead and add the `require` statement for our new file.

~~~ruby
# lib/tictactoe.rb
require "tictactoe/player"
~~~

Now, we can run our tests and get them to pass one at a time. If we run `bundle exec rspec` now, we'll see a familiar error message:

~~~bash
NameError:
  uninitialized constant Tictactoe::Player
# ./spec/player_spec.rb:1:in `<top (required)>'
~~~

To resolve this first error, we need to actually create a Player class. Let's go ahead and write out a Player class with `name` and `marker` instance variables. Keep in mind that when we start writing code in TDD, we want to write the absolute least amount of code to get it to pass one test at a time. Here, we're going to start by getting the code to pass the tests we wrote for the `#initialize` method.

~~~ruby
# lib/tictactoe/player.rb
module Tictactoe
  class Player

    def initialize(name, marker)
      @name = name
      @marker = marker
    end
  end
end
~~~

If we run `bundle exec rspec` now, we see the following output:

~~~bash
Tictactoe::Player
  #initialize
    raises an error when given zero parameters
    raises an error when given one parameter
    doesn't raise an error when given two parameters
  #name
    returns the name (FAILED - 1)
    raises an error when trying to change the name (FAILED - 2)
  #marker
    returns the marker (FAILED - 3)
    raises an error when trying to change the marker (FAILED - 4)
~~~

Great - all of our tests for the `#initialize` method are passing! Next, we need to get the `#name` method tests to pass. This should be pretty straight-forward; we just need to add an `attr_reader` to our class since we want read access but not write access to this attribute.

~~~ruby
# lib/tictactoe/player.rb
class Player
    attr_reader :name
~~~

Let's run `bundle exec rspec` to see how we're doing now.

~~~bash
Tictactoe::Player
  #initialize
    raises an error when given zero parameters
    raises an error when given one parameter
    doesn't raise an error when given two parameters
  #name
    returns the name
    raises an error when trying to change the name
  #marker
    returns the marker (FAILED - 1)
    raises an error when trying to change the marker (FAILED - 2)
~~~

Both of the `#name` tests are passing, so now we can apply the technique to get the code to pass the `#marker` tests.

~~~ruby
# lib/tictactoe/player.rb
class Player
    attr_reader :name, :marker
~~~

Let's run our tests once more to make sure everything is working properly.

~~~bash
Tictactoe::Player
  #initialize
    raises an error when given zero parameters
    raises an error when given one parameter
    doesn't raise an error when given two parameters
  #name
    returns the name
    raises an error when trying to change the name
  #marker
    returns the marker
    raises an error when trying to change the marker

Tictactoe::Square
  #initialize
    initializes with a default value of ' '
  #value
    can be changed to 'X'
    can be changed to 'O'

Tictactoe
  has a version number

Finished in 0.00753 seconds (files took 0.15355 seconds to load)
11 examples, 0 failures
~~~

Revel in all that green! 

We've finished making the first two classes that our game requires along with their associated tests. The next step is to make the far more complicated Board class, which is where we'll start in Part 2 of this tutorial. See you there!
