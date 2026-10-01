# 🪢 Hangman

> A command-line Hangman game in Python: guess the hidden word one letter at a time before the gallows is complete.

![Python](https://img.shields.io/badge/Python-3.x-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Status](https://img.shields.io/badge/status-completed-brightgreen?style=for-the-badge)
![Project](https://img.shields.io/badge/project-personal-blueviolet?style=for-the-badge)

---

## 📌 About the Project

**Hangman** is an interactive console game written in Python. The computer picks a secret word at random from a list of 213 words and shows it as a row of blanks. The player guesses one letter at a time: correct letters are revealed in place, and every wrong guess costs a life and adds a piece to an ASCII-art gallows. The player wins by revealing the whole word and loses after six wrong guesses.

This is a **personal project developed independently** as part of my programming portfolio. It was built to practise loops, lists and string handling in a game that runs until a win or loss condition is met, and to organise a program across more than one file.

---

## 🎯 Learning Objectives

- **While loops and game state**: keeping the game running with a `while` loop controlled by a `game_over` flag.
- **Lists**: tracking the letters the player has guessed correctly in `correct_letters`.
- **For loops over strings**: walking through each letter of the secret word to rebuild the display.
- **String building**: creating the masked word step by step with `display += ...`.
- **Modules and imports**: splitting the project into separate files and loading them with `from ... import ...`.
- **Random selection**: using `random.choice()` to pick the secret word.
- **List indexing**: using the remaining lives as the index of the drawing to display.

---

## ⚙️ How It Works

```
┌──────────────┐    ┌──────────────┐    ┌──────────────────────────────────┐
│ Pick a random│ -> │ Show the word│ -> │ Loop until win or lose:          │
│ word from    │    │ as blanks    │    │  1. Ask for a letter             │
│ the list     │    │ (_ _ _ _)    │    │  2. Reveal matching letters      │
└──────────────┘    └──────────────┘    │  3. Wrong guess? Lose a life     │
                                        │  4. Check win / lose             │
                                        │  5. Draw the gallows stage       │
                                        └──────────────────────────────────┘
```

### Project Structure

```
Hangman/
├── main.py             # Game logic
├── hangman_words.py    # word_list: the 213 possible secret words
└── hangman_art.py      # logo and stages: the ASCII art (7 gallows drawings)
```

### Code Walkthrough

```python
import random

from hangman_words import word_list
from hangman_art import stages, logo

lives = 6

print(logo)

chosen_word = random.choice(word_list)

placeholder = ""
word_length = len(chosen_word)
for position in range(word_length):
    placeholder += "_"
print("Word to guess: " + placeholder)

game_over = False
correct_letters = []

while not game_over:

    print(f"**************************** {lives} LIVES LEFT****************************")
    guess = input("Guess a letter: ").lower()

    if guess in correct_letters:
        print(f"You've already guessed {guess}")

    display = ""

    for letter in chosen_word:
        if letter == guess:
            display += letter
            correct_letters.append(guess)
        elif letter in correct_letters:
            display += letter
        else:
            display += "_"

    print("Word to guess: " + display)

    if guess not in chosen_word:
        lives -= 1
        print(f"You guessed {guess}, that's not in the word. You lose a life.")

        if lives == 0:
            game_over = True

            print(f"***********************IT WAS {chosen_word}! YOU LOSE**********************")

    if "_" not in display:
        game_over = True
        print("****************************YOU WIN****************************")

    print(stages[lives])
```

| Line / Section | Responsibility |
|---|---|
| `from hangman_words import word_list` | Loads the list of secret words from its own file |
| `from hangman_art import stages, logo` | Loads the logo and the seven gallows drawings |
| `lives = 6` | Sets the number of wrong guesses the player can afford |
| `chosen_word = random.choice(word_list)` | Picks the secret word |
| First `for` loop | Builds the initial row of blanks, one `_` per letter |
| `game_over`, `correct_letters` | Track whether the game has ended and which letters were guessed correctly |
| `while not game_over` | Repeats one round per guess until the game ends |
| `guess = input(...).lower()` | Reads the guess and makes it lowercase |
| `if guess in correct_letters` | Warns when a correct letter is guessed again |
| `for letter in chosen_word` | Rebuilds the display: shows guessed letters and blanks for the rest |
| `if guess not in chosen_word` | Takes a life away; at zero lives the game is lost and the word is revealed |
| `if "_" not in display` | No blanks left means the player wins |
| `print(stages[lives])` | Draws the gallows that matches the lives left |

**Technical note.** The `stages` list is ordered from the finished drawing (index `0`, no lives left) to the empty gallows (index `6`, all lives left). Because of that, the number of lives is directly the index of the drawing to print: `stages[lives]` needs no extra logic. The ASCII art that contains backslashes is stored in raw strings (`r'''...'''`) so the backslashes are printed as they are.

Two behaviours worth knowing:

- **Only correct letters are remembered.** Repeating a wrong letter costs another life, and there is no "already guessed" message for it.
- **Empty input or several letters at once** (for example `ab`) does not cost a life and does not reveal anything; the game simply asks again.

---

## 🚀 Getting Started

### Prerequisites

- [Python 3.x](https://www.python.org/downloads/) installed
- The three files (`main.py`, `hangman_words.py`, `hangman_art.py`) in the same folder
- No external libraries required

### Installation and Execution

```bash
git clone https://github.com/guiggsp-hub/Hangman.git
cd Hangman
python main.py
```

### Sample Run

```
 _
| |
| |__   __ _ _ __   __ _ _ __ ___   __ _ _ __
| '_ \ / _` | '_ \ / _` | '_ ` _ \ / _` | '_ \
| | | | (_| | | | | (_| | | | | | | (_| | | | |
|_| |_|\__,_|_| |_|\__, |_| |_| |_|\__,_|_| |_|
                    __/ |
                   |___/
Word to guess: ___
**************************** 6 LIVES LEFT****************************
Guess a letter: a
Word to guess: ___
You guessed a, that's not in the word. You lose a life.

  +---+
  |   |
  O   |
      |
      |
      |
=========

**************************** 5 LIVES LEFT****************************
Guess a letter: i
Word to guess: i__

  +---+
  |   |
  O   |
      |
      |
      |
=========

**************************** 5 LIVES LEFT****************************
Guess a letter: v
Word to guess: iv_

  +---+
  |   |
  O   |
      |
      |
      |
=========

**************************** 5 LIVES LEFT****************************
Guess a letter: y
Word to guess: ivy
****************************YOU WIN****************************

  +---+
  |   |
  O   |
      |
      |
      |
=========
```

---

## 🧠 Skills Demonstrated

`Python` · `While Loops` · `Lists` · `For Loops` · `String Manipulation` · `Modules and Imports` · `Random Module` · `CLI Applications`

---

## 👤 Author

Developed independently by **Guilherme Garcia** as part of a personal programming portfolio.

[![GitHub](https://img.shields.io/badge/GitHub-guiggsp--hub-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/guiggsp-hub)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Guilherme%20Garcia-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/guilherme-garcia-796633287/)
