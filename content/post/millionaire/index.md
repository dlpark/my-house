---
title: Millionaire - A C++ Text Game Exploration
description: An exploration in turning a 1977 GAMES Magazine competition into a simple text-based game that runs in the command line / terminal.
date: 2025-01-16
categories:
    - Game Design
    - Code
tags:
    - C++
---

## The Backstory

The [Internet Archive](https://archive.org/) is an amazing resource.  Any time I want to find texts on anything I'm interested in learning about, I love having a look there through old books, magazines, software and other media to see if there's anything cool I can find.  In my search for games-related content, I discovered [GAMES Magazine](https://archive.org/details/games_magazine) and within the very first issue I came across this contest that truly captured my imagination.

## The Idea

The issue of GAMES Magazine that this contest appeared in ([Issue #1](https://archive.org/details/games-1-1977-september/page/21/mode/1up)) was from 1977, and largely focused on games and puzzles in a physical context (primarily board and card games).  What I was keen to do was to see if there was anything I could find that would inspire me and that I could work into a digital form.  Millionaire was a perfect first project to take on for a couple of reasons:

1. The rules are incredibly simple.
2. I was immediately able to identify how I would go about coding the game as set out in the magazine.
3. I could see opportunity to iterate on the base concept to expand modes of play to keep players interested once the initial challenge had lost its novelty.

![Millionaire Contest Feature](https://ia804601.us.archive.org/BookReader/BookReaderImages.php?zip=/2/items/games-1-1977-september/games-1-1977-september_jp2.zip&file=games-1-1977-september_jp2/games-1-1977-september_0022.jp2&id=games-1-1977-september&scale=2&rotate=0 "The Millionaire contest from GAMES Magazine #1, September 1977")

## The Game

I have included the scanned image of the Millionaire contest feature from GAMES Magazine above, but to summarise, the flow of Millionaire goes as follows:

1. Each letter in the alphabet is assigned a numerical value from 1 to 26.  In its original form, the values increment in alphabetical order (A = 1, B = 2, so on).
2. The player guesses a word.
3. The values of each letter in the word are multiplied together to give the result.
4. The player wins if they manage to find a word with a value of one million.


## The Basic Implementation
Given that I was learning C++ primarily at this point in time, I wrote up a code snippet in C++ to cover the basic functionality required to calculate the value of a word input by the user

```cpp
#include <iostream>
#include <vector>

std::string getInput() {
  std::string input;
  std::cin >> input;

  return input;
}

int calculateValue(std::string input) {
  // Create a vector to store letters of the alphabet
  std::vector<char> alphabet = {'a', 'b', 'c', 'd', 'e', 'f', 'g', 'h', 'i', 'j', 'k', 'l', 'm', 'n', 'o', 'p', 'q', 'r', 's', 't', 'u', 'v', 'w', 'x', 'y', 'z'};

  // Create a vector to store the values of each letter
  std::vector<int> values = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26};

  // Initialize a variable to store the total value of the word
  int product = 1;

  // Loop through each character in the input string
  for (int i = 0; i < input.length(); i++) {
    // Find the index of the current character in the alphabet vector
    for (int j = 0; j < alphabet.size(); j++) {
      // If the current character is found, multiply the current product by the corresponding value in the values vector
      if (input[i] == alphabet[j]) {
        product *= values[j];
      }
    }
  }
  return product;
}

int main() {
  // Introduce the user to the program and explain the rules
  std::cout << "Welcome to Millionaire!\n";
  std::cout << "The aim of the game is to guess a word which has a value of as close to 1 million as possible.\n";
  std::cout << "Each letter has a value determined by its position in the alphabet, and when multiplied together determine the value of the word.\n";
  std::cout << "For example, in the word 'cat', the value of 'c' is 3, the value of 'a' is 1, and the value of t is 20, so the value of the word is 3 * 1 * 20 = 60.\n";

  std::string user_input;
  int value = 0;
  bool scoreMatch = false;

  do {
    user_input = getInput();
    value = calculateValue(user_input);

    if (value == 1000000) {
      scoreMatch = true;
      std::cout << "Congratulations! You managed to guess a word with a score of 1 million.\n";
    } else if (value != 1000000) {
      std::cout << "Incorrect! The value of your word is " << value << std::endl;
    } else {
      std::cout << "Error! Please try again.\n";
    }
  } while (scoreMatch == false);
  
}
```

### Missing Functionality

This initial snippet, while capable of facilitating the basic flow of the game, is currently missing some important functionality.  I intend to work on these in the near future.

#### Player Instruction

The initial text on running the code snippet does not indicate that the player has the ability to or should provide an input.  This should be made more clear, and not assumed to be obvious to the player.

#### Input Validation

The player is able to enter any text input, including characters which have no assigned value.  If the player attempts to input other characters, the program should handle this by explaining the error and prompting the player for another input.

#### Handling of Capital Letters

If the player enters their word using capital letters, that is currently treated as characters with no assigned value.  This can be handled either by making sure capital letters are also assigned values, or more elegantly converting text inputs to all be lower case before values are assigned.

#### Dictionary Look-Up

The player is able to enter strings of characters that would not satisfy the rules of the game because they are not a valid word.  Adding a dictionary to check words against would help to ensure that guesses are valid words.

## Extensibility

### Score Attack

Instead of trying to find a word that scores as close to 1,000,000 points as possible, the player is instead trying to find a word that gives the highest score possible.

#### Implementation

This should be easy to implement.  The changes required would be as follows:

- Creating a variable to store the player's high score, initialised
- Adjusting the if statements that check if the value is one million to instead check if the value is greater than or equal to the current high score, and then updating the high score variable

### Alternate Letter Values

The amount of valid words that can be used to find a value of exactly one million are extremely limited with the letters of the alphabet assigned as they are in the initial contest and code snippet.  To allow for more replayability, the player could be allowed to shuffle the values of letters, adding new challenge to the game.

## Try It Out

### Replit

The code sample above is also on Replit, where it can be run in a browser simply by pressing the green 'Run' button at the top of the browser window.  You can find it at the following link:
[Replit Code Snippet](https://replit.com/@DamienPark/Millionaire#main.cpp)

## Summary

I think there's something beautiful in being able to look at a page of a magazine that was published when my mum was only 7 years old, and being able to not only find fun in the simplicity of the concept but to use that as a jumping off point for thinking about how I might create it in a digital form, and how it could be extended to add increased replayability.

It can be easy to get bogged down in layers of complexity when it comes to thinking about games.  We're so used to games in the spotlight having incredibly involved systems and mechanics, and there are great things about that, but there's still something special to be found in simplicity.

### Next Steps

I think this would be a fun project to rework in the form of a more polished game that the player would interact with via a more polished UI.  This game's simplicity makes it an ideal candidate for being a pick-up-and-play sort of game that would be best accessed either via a browser or on a smartphone.  It requires little up-front commitment from the player, allowing for short play sessions that would be absolutely ideal to pass some time on a portable device while there are a few minutes free between tasks, or to have on a second (or third if you're fancy) monitor while another task is the primary focus.