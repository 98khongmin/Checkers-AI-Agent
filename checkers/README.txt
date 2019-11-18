*************************
Checkers
CS457
11/17/2019
Collin Beckly, Steven Kim, Rohit Gangurde
**************************

OVERVIEW :

A program that runs checkers using Aritificial Intelligence-- through forseeing all the possible 
outcomes using the Minimax and Alpha-Beta Pruning algorithm.

INCLUDED FILES :
* checkers
* checkers.c
* checkers.h
* computer
* crapola
* hang.c
* graphics.c
* graphics.h
* graphics.o
* Makefile
* myprog.c
* myprog.h
* MyProg.java
other_players
    * depth
    * depth.orig
    * dif
    * dif.orig
    * diff
    * rat
    * rat.orig
    * remap
    * remap.c
    * year1
    * year1.orig
    * year2
    * year2.orig

COMPILING AND RUNNING :

To compile the class file, run this:

$ make

After executing the make, run this:

$ checkers <player1> <player2> <SecPerMove> [-MaxDepth x]

Replace <player1> and <player2> with the players you want, such as year1, computer, depth etc.

Replace <SecPerMove> with a natural number that how many seconds you will allow them to go 
through the Minimax branch.

If the opponent allows the maxdepth, include “-MaxDepth x” where x is a natural number of
the opponent's depth limit.

Console output will give the results after the program finishes and it will clear the .class files

PROGRAM DESIGN AND IMPORTANT CONCEPTS :

We used Minimax in our program. Minimax is a decision rule for minimizing the possible loss 
for a worst case (maximum loss) scenario.The CPUs in games like tic-tac-toe, chess, checkers 
are made using this algorithm. Usually, it is important to look at the “big picture”of the game 
or strategy to lead the situation favorable as the game progresses. Computers do not have the ability
to recognize how the atmosphere of the domain is, thus it is necessary to convert this atmosphere into 
scores, which computers can count the scores and compare the possible outcomes. In the heuristics 
function, it needs to find the best move which would be the largest score of the next possible moves. 
Largest score doesn’t necessarily mean the computer will win the game, because the next best move could
allow the computer to lose the whole game. For example, when there is a white pawn and the white rook is
located in the same line, if the black queen decides to capture the white pawn, the black queen will be
captured by the white rook. Eventually, this will be a huge loss for the black team. 
________________________
|                       |
|    R-------------P    |
|                  |    |
|                  |    |
|                  Q    |
|                       |
|                       |
|_______________________|
figure 1: Queen, Pawn, Rook possible interactions

In these kind of scenarios, the board that needs to be evaluated is not the current board, it will be 
the future board after making the move. Since the opponent always want their best moves as well, the 
Minimax algorithm comes in place as a tree of finding the best/worst scored nodes and returning 
to the parent of its node. We used the textbook in order to construct our code from its pseudo code, 
implementing the functions minVal and maxVal.In the original Minimax algorithm, the computer has 
to take a look at every single node in order to evaluate the board. However, there are cases where 
looking at the entire node is not necessary: 1. The case where if it chooses the node, the situation
will get deterministically worse 2. The case where the opponent will not choose our advantageous move.
These two cases have an extremely high probability that the children of the node won't make a difference.
So, we prune(cut off) the unnecessary branch to minimize the effort of going through every single node. 
In our case, that will be the time variable, where the time limit is set to a user-determined second, 
and during that time, the computer will explore through as many possible situations of the board
(board evaluations) as possible, unless the user gives the maximum depth that the program will be limited to. 

THE MYPROG.C CLASS
Initially, we had the choice to program this checkers project in C or java, but at the end we chose C. 
The reason why we choose C over java is because C is a lot faster if the algorithm is implemented
correctly. This will allow us to compete with other group’s players with a speed advantage.

TESTING :
In order to test, we decided to match with most of other opponents, including depth, rat, year1, year2 
etc. Most of the exceptions including printing out the usage of the program is already implemented, 
so we only had to worry about our algorithm of implementing the Minimax and the timed Alpha-Beta Pruning.
The test result wins the random player 100% of the time, proving that we're actually utilizing the 
algorithm that we built in order to win the oppoenent with strategy.

[rohitgangurde@onyxnode136 checkers]$ ./checkers computer other_players/depth 2 -MAXDEPTH 0
No graphics
waiting
computer SecPerMove == 2
I'm Player 1
Waiting for player 1
.
.
.
Move: 19-28
   a
  aa
    
    
 a  
 a  
   a
   A
Player 2 has lost the game.
----------------------------------------------------------------------------------------------
[rohitgangurde@onyxnode136 checkers]$ ./checkers computer other_players/depth 2 -MAXDEPTH 3
No graphics
waiting
computer SecPerMove == 2
I'm Player 1
Waiting for player 1
.
.
.
Move: 10-19-28
   a
  a 
  aa
   b
   b
 a  
   a
 A  
Player 2 has lost the game.
----------------------------------------------------------------------------------------------
[rohitgangurde@onyxnode136 checkers]$ ./checkers computer other_players/depth 1 -MAXDEPTH 5
No graphics
waiting
computer SecPerMove == 1
I'm Player 1
Waiting for player 1
.
.
.
Move: 30-21
    
    
    
    
    
A   
 a a
    
Player 2 has lost the game.
----------------------------------------------------------------------------------------------
[rohitgangurde@onyxnode136 checkers]$ checkers computer /other_players diff
No graphics
waiting
exec for /other_players failed
computer SecPerMove == 0
I'm Player 1
Waiting for player 1
.
.
.
Move: 10-15
aaaa
aaaa
a aa
  a 
    
bbbb
bbbb
bbbb
Waiting for player 2
Player 2 has lost the game (time limit reached).
----------------------------------------------------------------------------------------------
[rohitgangurde@onyxnode136 checkers]$ checkers computer other_players/rat 1
No graphics
waiting
computer SecPerMove == 1
I'm Player 1
Waiting for player 1
.
.
.
<STUCK IN RECURRRENT LOOP>
-------------------------------------------------------------------------------------------------
[rohitgangurde@onyxnode136 checkers]$ ./checkers other_players/depth computer 1 -MAXDEPTH 5
No graphics
waiting
computer SecPerMove == 1
I'm Player 2
.
.
.
Move: 22-13
 B  
    
 b  
b   
    
   b
   b
 b  
Player 1 has lost the game.
-------------------------------------------------------------------------------------------------

DISCUSSION :

For our evaluation, we use two heuristics, whitesum & redsum. We step through legal peices on the 
board and manipulate the whitesum/redsum heuristics if the piece is king or a piece. If the player is 
us, we return whtiesum - redsum, otherwise we return redsum - whitesum. 

When we tried to implement Time restrictions, we first used p_thread class. There was something about 
pthread_detach and pthread_join that sabotaged memory and stomped it. We tried several different ways 
to make a succesful implementation using the p_thread class. But it didn't work. So we used the 
LowOnTime() method. But, we changed the method. Our clock_t current variable is set to:

clock_t current= (clock() - start)/CLOCKS_PER_SEC;

We have an if block where we check the if the time remaining is 0.5 sec. If yes, it returns 1, else 0.

We made attempts at improving Iterative Deepening and time restricted searches. One of the problems 
we still face are that our player keeps on making same moves. One of the most notable is when our 
player goes to the top-left corner and keeps moving between top left and one below it. Our player 
thinks that in that situation the only best move is to get to the top left corner and switch between 
up and down. We attempted at fixing this issue by checking if rval and alpha are same in minVal and 
maxVal methods.

CONTRIBUTION :

All three of us spent time doing pair programming. Steven and Rohit worked on README together. Colin 
worked on comments. We all spent about 15-16 hours on this project collectively.
