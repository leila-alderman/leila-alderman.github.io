---
title: Ruby TDD Tutorial | Connect Four Part 1
---

# Ruby TDD Tutorial | Connect Four Part 1

This tutorial walks through how to write a game of [Connect Four](https://en.wikipedia.org/wiki/Connect_Four) that can be played in the terminal using the principles of [test-driven development](https://en.wikipedia.org/wiki/Test-driven_development) (TDD). 

[This project](https://www.theodinproject.com/courses/ruby-programming/lessons/testing-your-ruby-code?ref=lnav) is part of [The Odin Project](https://www.theodinproject.com) curriculum.

The GitHub repository for this tutorial can be found [here](https://github.com/leila-alderman/connect_four).

## Objective

This Connect Four game can be played by two human players on the command line. The Connect Four board has 7 columns and 6 rows. The players alternate placing a token in one of the columns, which then falls as far it can go, until one of the players has four tokens forming a horizontal, vertical, or diagonal line. The game ends in a tie if neither player has won and all of the spaces on the board are taken. 

## Initialie RSpec

run `rspec --init`
run `git init`

In `.rspec` file, add `--format documentation`.