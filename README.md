# Game-of-Hog

Introduction
In this project, you will develop a simulator and multiple strategies for the dice game Hog. You will need to use control statements and higher-order functions together, as described in lecture, lecture notes, and Sections 1.2 through 1.6 of Composing Programs.

In Hog, two players alternate turns trying to reach 100 points first. On each turn, the current player chooses some number of dice to roll, up to 10. That player's score for the turn is the sum of the dice outcomes, unless any of the dice comes up a 1, in which case the score for the turn is only 1 point (the Pig out rule).

To spice up the game, we will play with some special rules:

Free bacon. A player who chooses to roll zero dice scores one more than the largest digit in the opponent's score.

Example 1: If Player 1 has 42 points, Player 0 gains 1 + max(4, 2) = 5 points by rolling zero dice.
Example 2: If Player 1 has 48 points, Player 0 gains 1 + max(4, 8) = 9 points.
Example 3: If Player 1 has 7 points, Player 0 gains 1 + max(0, 7) = 8 points by rolling zero dice.
Hog wild. If the sum of both players' total scores is a multiple of seven (e.g., 14, 21, 35), then the current player rolls four-sided dice instead of the usual six-sided dice.
Swine Swap. At the end of each turn, if the last two digits of Player 0's score are the reverse of the last two digits of Player 1's score, the players' score will be swapped.
Example 1: Player 0 has a score of 19 and Player 1 has a score of 91 after Player 0 has rolled. Reversing the last two digits of Player 0's score (19) results in 91, which are the last two digits of Player 1's score. This is considered a swap and the player's scores are switched. Player 0 now has a score of 91, Player 1 now has a score of 19 and Player 0's turn is over.
Example 2: Player 0 has a score of 80 and Player 1 has a score of 8 at the end of Player 1's turn. In this example, Player 1's score is viewed as 08, which is the reverse of 80. The player's scores are swapped, leaving, Player 0 with 8 and Player 1 with 80. Player 1's turn ends.
Example 3: Player 0 begins their turn with a score of 90 while Player 1 has 70 points. Player 0 rolls 7 dice, giving them 17 points. They now have a score of 107 and Player 1 has a score of 70. Swapping the last two digits of 107 will give back 70, so the two scores are swapped. Player 0 ends their turn with a score of 70 while Player 1 now has a score of 107. Because the swap occurs before Player 0's turn is over, Player 1 wins the game.
Download starter files
To get started, download the 2 files for the project code.

hog.py : A starter implementation of Hog
dice.py : Functions for rolling dice

It is not required, but for those interested in extra credit, you can download the starter file:
hog_extra.py : A starter for extra credit on strategies.
If you are interested in running the game in a GUI interface there is an hog.zip archive with additional files that you can download to make this work.

hog_gui.py: A graphical user interface for Hog
images: A directory of images used by hog_gui.py
Logistics
This is a 1-week project. You may work with one other partner. You should not share your code with students who are not your partner or copy from anyone else's solutions.

In the end, you will submit one project for both partners. .

You will turn in the following files:

hog.py
You do not need to modify or turn in any other files to complete the project. Submit the project on BB.

For the functions that we ask you to complete, there may be some initial code that we provide. If you would rather not use that code, feel free to delete it and start from scratch. You may also add new function definitions as you see fit.

However, please do not modify any other functions. Doing so may result in your code failing.

Testing
Throughout this project, you should be testing the correctness of your code. It is good practice to test often, so that it is easy to isolate any problems.

In the first phase, you will develop a simulator for the game of Hog.

Problem 0
The dice.py file represents dice using non-pure zero-argument functions. These functions are non-pure because they may have different return values each time they are called. The documentation of dice.py describes the two different types of dice used in the project:

Dice can be fair, meaning that they produce each possible outcome with equal probability. Examples: four_sided, six_sided.
For testing functions that use dice, deterministic test dice always cycle through a fixed sequence of values that are passed as arguments to the make_test_dice function.
Before we start writing any code, let's understand the make_test_dice function by testing.


$ python3 -i dice.py
>>> dice = make_test_dice(1, 2, 3)
>>> dice()
1
>>> dice()
2
>>> dice()
3
>>> dice()
1
>>> dice()
2 
Problem 1
Implement the roll_dice function in hog.py. It takes two arguments: the number of dice to roll, num_rolls, and a dice function. It returns the number of points scored by rolling that number of dice simultaneously: either the sum of the outcomes or 1 (pig out).

To obtain a single outcome of a dice roll, call dice(). You must call the dice function exactly the number of times specified by the first argument (even if a 1 is rolled) since we are rolling all dice simultaneously in the game.

Test you code for accurracy interactively as follows:


$ python3 -i hog.py
>>> roll_dice(1,make_test_dice(4, 2, 1, 3))
4
>>> roll_dice(2,make_test_dice(4, 2, 1, 3))
6
>>> roll_dice(3,make_test_dice(4, 2, 1, 3))
1
>>> roll_dice(4,make_test_dice(4, 2, 1, 3))
1
The roll_dice function has a default argument value for dice that is a random six-sided dice function. The tests use fixed dice.

Problem 2
Implement the take_turn function, which returns the number of points scored for a turn. You will need to implement the Free bacon rule. You can assume that opponent_score is less than 100. For a score less than 10, assume that the first of two digits is 0. Your implementation should call roll_dice.

You can check your correctness with:


$ python3 -i hog.py
>>> take_turn(2, 0, make_test_dice(4, 6, 1))
10
>>> take_turn(3, 0, make_test_dice(4, 6, 1))
1
>>> take_turn(0, 35)
6
>>> take_turn(0, 71)
8
>>> take_turn(0, 7)
8
>>> take_turn(0, 0)
1
>>> take_turn(0, 9)
10
>>> take_turn(2, 0, make_test_dice(6))
12
>>> take_turn(0, 50)
6
Problem 3
Implement the select_dice function, which helps enforce the Hog wild special rule. This function takes two arguments: the scores for the current and opposing players. It returns either four_sided or six_sided dice that will be used during the turn.

Run the following tests:


$ python3 -i hog.py
>>> select_dice(4, 24) == four_sided
True
>>> select_dice(16, 64) == four_sided
False
>>> select_dice(0, 0) == four_sided
True
>>> select_dice(50, 80) == four_sided
False          
Problem 4
To help you implement the Swine Swap special rule, write a function called is_swap that checks to see if the last two digits of the players' scores are swapped.

Run the following tests:


$ python3 -i hog.py
>>> is_swap(19, 91)
True
>>> is_swap(20, 40)
False
>>> is_swap(41, 14)
True
>>> is_swap(23, 42)
False
>>> is_swap(55, 55)
True
>>> is_swap(114, 41) # We check the last two digits
True
          
Problem 5
Implement the play function, which simulates a full game of Hog. Players alternate turns, each using their respective strategy function (Player 0 uses strategy0, etc.), until one of the players reaches the goal score. When the game ends, play returns the final total scores of both players, with Player 0's score first, and Player 1's score second.

Here are some hints:

Remember to enforce all the special rules! You should enforce the Hog wild special rule here (by calling select_dice), as well as the Swine Swap special rule here.
You should use the take_turn and is_swap functions that you've already written.
You can get the number of the other player (either 0 or 1) by calling the provided function other.
A strategy is a function that, given a player's score and their opponent's score, returns how many dice the player wants to roll. A strategy function (such as strategy0 and strategy1) takes two arguments: scores for the current player and opposing player. A strategy function returns the number of dice that the current player wants to roll in the turn. Don't worry about details of implementing strategies yet. You will develop them in the Extra-Credit Phase 2.
Test your solution using the following tests:


$ python3 -i hog.py
>>> four_sided = make_test_dice(1)
>>> six_sided = make_test_dice(3)
>>> always = always_roll
>>> s0,s1 = play(always(5), always(3), score0=91, score1=10)
>>> s0, s1
(106, 10)

>>> s0,s1 = play(always(5), always(5), goal=10)
>>> s0, s1
(1, 15)

>>> s0,s1 = play(always(5), always(3), score0=36, score1=15, goal=50)
>>> s0, s1
(15, 51)

>>> # Swine swap applies to 3 digit scores
>>> s0,s1 = play(always(5), always(3), score0=98, score1=31)
>>> s0,s1
(31, 113)

>>> # Goal edge case
>>> s0,s1 = play(always(4), always(3), score0=88, score1=20)
>>> s0, s1
(100, 20)
          
Note: that you can fuzz test, which checks your play function works for any arbitrary inputs. Failing this test means something is wrong, but you should look at other tests to see where the problem might be.

$ python3 -i hog.py
>>> import random
>>> four_sided = lambda: random.randrange(1, 5)
>>> six_sided = lambda: random.randrange(1, 7)
>>> random_strat = lambda a, b: (random.randrange(11) + b) % 10
>>> random.seed(4321)
>>> for _ in range(100):
       s0, s1 = play(random_strat,random_strat)
            
Once you are finished, you will be able to play a graphical version of the game. We have provided a link to a zip archive that contains a file called hog_gui.py that you can run from the terminal:

python3 hog_gui.py
If you don't already have Tkinter (Python's graphics library) installed, you'll need to install it first before you can run the GUI.

The GUI relies on your implementation, so if you have any bugs in your code, they will be reflected in the GUI. This means you can also use the GUI as a debugging tool; however, it's better to run the tests first.

Congratulations! You have finished Homework Project #1 of this project! Go on to the Extra Credit Questions if you are so inclined.

Extra Credit: Strategies
In the second phase, you will experiment with ways to improve upon the basic strategy of always rolling a fixed number of dice. First, you need to develop some tools to evaluate strategies.

Problem 6
Implement the make_averaged function, which is a higher-order function that takes a function fn as an argument. It returns another function that takes the same number of arguments as fn (the function originally passed into make_averaged). This returned function differs from the input function in that it returns the average value of repeatedly calling fn on the same arguments. This function should call fn a total of num_samples times and return the average of the results.

To implement this function, you need a new piece of Python syntax! You must write a function that accepts an arbitrary number of arguments, then calls another function using exactly those arguments. Here's how it works.

Instead of listing formal parameters for a function, we write *args. This is often called a "rest agument", since it is used to capture a list of the unknown number of all the rest of the arguments. To call another function using exactly those arguments, we call it again with *args. For example,

>>> def printed(fn):
...     def print_and_return(*args):
...         result = fn(*args)
...         print('Result:', result)
...         return result
...     return print_and_return
>>> printed_pow = printed(pow)
>>> printed_pow(2, 8)
Result: 256
256
>>> printed_abs = printed(abs)
>>> printed_abs(-10)
Result: 10
10
Read the docstring for make_averaged carefully to understand how it is meant to work.

Problem 7
Implement the max_scoring_num_rolls function, which runs an experiment to determine the number of rolls (from 1 to 10) that gives the maximum average score for a turn. Your implementation should use make_averaged and roll_dice.

Note: If two numbers of rolls are tied for the maximum average score, return the lower number. For example, if both 3 and 6 achieve a maximum average score, return 3.

To run this experiment on randomized dice, call run_experiments using the -r option:

python3 hog.py -r
Running experiments For the remainder of this project, you can change the implementation of run_experiments as you wish. By calling average_win_rate, you can evaluate various Hog strategies. For example, change the first if False: to if True: in order to evaluate always_roll(8) against the baseline strategy of always_roll(5). You should find that it loses more often than it wins, giving a win rate below 0.5.

Some of the experiments may take up to a minute to run. You can always reduce the number of samples in make_averaged to speed up experiments.

Problem 8
A strategy can take advantage of the Free bacon rule by rolling 0 when it is most beneficial to do so. Implement bacon_strategy, which returns 0 whenever rolling 0 would give at least margin points and returns num_rolls otherwise.

Once you have implemented this strategy, change run_experiments to evaluate your new strategy against the baseline. You should find that it wins more than half of the time.

Problem 9
A strategy can also take advantage of the Swine Swap rule. The swap_strategy

Rolls 0 if it would cause a beneficial swap.
Rolls num_rolls if rolling 0 would cause a harmful swap in favor of the opponent.
If rolling 0 does not cause a swapped, then roll 0 if it would give at least margin points and roll num_rolls otherwise.
(Note: if a swap would result in the scores not changing, such as both players having a score of 55, it is considered to be neutral, and should be handled in case 3 as if the scores had not been swapped at all)

Once you have implemented this strategy, update run_experiments to evaluate your new strategy against the baseline. You should find that it performs even better than bacon_strategy, on average.

Problem 10
Implement final_strategy, which combines these ideas and any other ideas you have to achieve a win rate of at least 0.58 (for full extra credit) against the baseline always_roll(5) strategy. Partial credit is also given if you are close. Some ideas:

You only need 100 points to win. If you are near the goal, try not to pig out and give your opponent a chance to win.
If you are in the lead, you might take fewer risks. If you are losing, you might take bigger risks to catch up.
Vary your rolls based on whether you will be rolling four-sided or six-sided dice.
Find a way to leave your opponent with four-sided dice more often.
You can check your final strategy win rate by running:

python3 hog.py --final
You can also play against your final strategy with the graphical user interface:

python3 hog_gui.py -f
The GUI will alternate which player is controlled by you.
