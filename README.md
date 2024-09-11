# Snowbug

## Goal

There are 5 bugs in this implementation of snowman. Practice using your debugging skills to locate, diagnose, and fix the bugs. For each bug note:

1. In which function it was found
1. The line (or lines) causing the problem
1. What about the line was causing the problem
1. What needed to be done to fix the problem
1. How you located the bug
   
## Activity Setup

1. Navigate to the folder where you wish to save activities. This could be your `projects` folder, or you may want to create a new folder for all of your activities.

   If you followed Ada's recommended file system structure from the Intro to Dev Environment lesson in Learn, you can navigate to your projects folder with the following command:

   ```bash
   $ cd ~/Developer/projects
   ```

   Or, if you want to create a new folder for all of your activities:

   ```bash
   $ cd ~/Developer
   $ mkdir activities
   $ cd activities
   ```

   If you've already created an activities directory, you can navigate to it with the following command:

   ```bash
   $ cd ~/Developer/activities
   ```

2. In Github click on the "Fork" button to fork the repository to your Github account.  This will make a copy of the activity in your Github account. 

3. "Clone" the activity into your working folder. This command makes a new folder named for the activity repository, and then puts the activity into this new folder.

   ```bash
   $ git clone <clone_url_for_the_activity>
   ```

   The `<>` syntax indicates a placeholder. You should replace `<clone_url_for_the_activity>` with the actual URL you'd use to clone this repository. If you click the green "Code" button on the GitHub page for this repository, you'll see a URL that you can copy to your clipboard.
 
   Use `ls` to confirm there's a new activity folder

4. Move your location into this activity folder

   ```bash
   $ cd <repository_directory>
   ```

   The `<repository_directory>` placeholder should be replaced with the name of the activity folder. If you're not sure what the folder is named, remember that you can use `ls` to list the contents of your current location.

5. Create a virtual environment named `venv` for this activity:

   ```bash
   $ python3 -m venv venv
   ```

6. Activate this environment:

   ```bash
   $ source venv/bin/activate
   ```

   Verify that you're in a python3 virtual environment by running:
   
   - `$ python --version` should output a Python 3 version
   - `$ pip --version` should output that it is working with Python 3

7. Install dependencies once at the beginning of this activity with

   ```bash
   # Must be in activated virtual environment
   $ pip install -r requirements.txt
   ```

   Not all activities will have dependencies, but there will still be an included `requirements.txt` file.

Summary of one-time activity setup:
- [ ] Fork the activity repository
- [ ] `cd` into your working folder, such as your `projects` or `activities` folder
- [ ] Clone the activity onto your machine
- [ ] `cd` into the folder for the activity
- [ ] Create the virtual environment `venv`
- [ ] Activate the virtual environment `venv`
- [ ] Install the dependencies with `pip`

## Activity Workflow

1. When working on this activity, always ensure that your virtual environment is activated:

   ```bash
   $ source venv/bin/activate
   ```

2. If you want to work on another project from the same terminal window, you should deactivate the virtual environment when you are done working on the activity:

   ```bash
   $ deactivate
   ```

## Getting Started

1. While in the activity directory, launch VS Code.

   ```bash
   $ code .
   ```

1. Perform test configuration by going to the Testing panel (shaped like a beaker) and clicking Configure Python Tests. Select pytest as the framework and tests as the location of the tests.

1. Run the tests using the VS Code testing tools.

1. Focus on the top test failure. Read through the test failure, and understand why the failure is happening.

1. Make a plan to fix the test failure.

1. Write code to fix the test failure.

1. Re-run the tests.

1. Continue running tests until all bugs have been fixed.

## Activity Completion

Make note of your investigation, especially the 5 questions in the goal, and be prepared to share your findings with your classmates!

## Solutions

### build_word_dict

Error in how the word dictionary was being built. It should filter out any non-alpha letters from its representation, so that only letters the player can actually guess are tracked.

```py
def build_word_dict(word):
   word_dict = {}
   for letter in word:
       # keep track of any character a player might guess (alphabetic)
       word_dict[letter] = False


   return word_dict
```

could resemble

```py
def build_word_dict(word):
   word_dict = {}
   for letter in word:
       # keep track of any character a player might guess (alphabetic)
       if letter.isalpha():
           word_dict[letter] = False


   return word_dict
```

### build_game_board

Error in how the current guessed letters are displayed to the user. It currently reverses the sense of checking for whether a letter is in the dict at all.

Recall that non-alpha characters won't be added, so if a character is found that is not in the dict at all, we should add that character to the output. If the character is in the dict, we need to check the value for truthiness. True means the letter was guessed, so we should add it to the output. False indicates it has NOT been guessed, so use the underscore placeholder.

```py
def build_game_board(word, word_dict):
   output_letters = []
   for elem in word:
       if elem in word_dict:
           # automatically add any character a player wouldn't be able to guess
           output_letters += elem
       elif word_dict[elem]:
           # add any letters the player has guessed
           output_letters += elem
       else:
           # add a blank for any letter not yet guessed
           output_letters += "_"

   return " ".join(output_letters)
```

could resemble

```py
def build_game_board(word, word_dict):
   output_letters = []
   for elem in word:
       if elem not in word_dict:
           # automatically add any character a player wouldn't be able to guess
           output_letters += elem
       elif word_dict[elem]:
           # add any letters the player has guessed
           output_letters += elem
       else:
           # add a blank for any letter not yet guessed
           output_letters += "_"

   return " ".join(output_letters)
```

### build_snowman_graphic

This function has an off-by-one in how it is picking the lines from the snowman image.

```py
def build_snowman_graphic(num_wrong_guesses):
   """This function extracts a portion of the
   snowman depending on the number of
   wrong guesses and converts it to a single string
   """

   # get the part of the snowman for the number of wrong guesses
   lines = []
   for line_no in range(num_wrong_guesses - 1):
       lines.append(SNOWMAN_IMAGE[line_no])

   return "\n".join(lines)
```

could resemble

```py
def build_snowman_graphic(num_wrong_guesses):
   """This function extracts a portion of the
   snowman depending on the number of
   wrong guesses and converts it to a single string
   """

   # get the part of the snowman for the number of wrong guesses
   lines = []
   for line_no in range(num_wrong_guesses):
       lines.append(SNOWMAN_IMAGE[line_no])

   return "\n".join(lines)
```

### add_wrong_letter

The related unit test wants this result to be sorted (to help users see which letters they've already guessed).

```py
def add_wrong_letter(wrong_letters, letter):
   # track the wrong guesses in alphabetical order
   wrong_letters.append(letter)
```

could resemble

```py
def add_wrong_letter(wrong_letters, letter):
   # track the wrong guesses in alphabetical order
   wrong_letters.append(letter)
   wrong_letters.sort()
```

### snowman

The error message display logic in snowman has a variable name typo. Instead of the correct snowman_word, it prints snowman, a reference to the function itself, resulting in output like "Sorry, you lose! The word was <function snowman at 0x10687da60>" appearing in the terminal.

```py
       if len(wrong_letters) == SNOWMAN_MAX_WRONG_GUESSES:
           # show a losing message along with the full word
           print(f"Sorry, you lose! The word was {snowman}")
           return
```

could resemble

```py
       if len(wrong_letters) == SNOWMAN_MAX_WRONG_GUESSES:
           # show a losing message along with the full word
           print(f"Sorry, you lose! The word was {snowman_word}")
           return
```