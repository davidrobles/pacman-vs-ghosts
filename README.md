# Pacman vs Ghosts Simulator v6.2

A Java implementation of the game [Ms. Pac-Man](https://en.wikipedia.org/wiki/Ms._Pac-Man)
for the *Ms Pac-Man vs Ghosts Competition* that ran in 2011-2012. The code is written by [Philipp Rohlfshagen](https://www.linkedin.com/in/philipp-rohlfshagen-6b602219)
based on earlier implementations of the game by [Simon Lucas](http://dces.essex.ac.uk/staff/lucas/) 
and [David Robles](https://www.linkedin.com/in/drobles).

# Run Software

##1. Download

Clone this repository:

```
git clone git@github.com:davidrobles/pacman-vs-ghosts.git
```

or download the zip file:

[pacman-vs-ghosts-v6.2.zip](https://github.com/davidrobles/pacman-vs-ghosts/archive/master.zip)

##4. Run the Code
You can execute the code by running the class:

```
Executor.java
```

This class contains a main method with numerous options for execution. Just uncomment the one you wish to run.

# Start Coding

#1. Inspect the Files

The files you need to edit are:

- [pacman.entries.pacman.MyPacMan.java](https://github.com/davidrobles/pacman-vs-ghosts/tree/master/src/pacman/entries/pacman/MyPacMan.java)
- [pacman.entries.ghosts.MyGhosts.java](https://github.com/davidrobles/pacman-vs-ghosts/tree/master/src/pacman/entries/ghosts/MyGhosts.java)

These files extend `Controller.java`. You will need to provide code for the:

```java
getMove(Game game, long timeDue)
```

#2. The getMove() Method

In order to create a controller, you need to provide code for the method:

```java
getMove(Game game, long timeDue)
```

in `MyPacMan.java` or `MyGhosts.java`. These files are already included in the code.

In the case of Ms Pac-Man, this method returns an element of the `MOVE` enumeration found in `Constants.java`. The elements are:

- `MOVE.UP`
- `MOVE.RIGHT`
- `MOVE.DOWN`
- `MOVE.LEFT`
- `MOVE.NEUTRAL`

In the case of the ghosts, this method returns an `EnumMap` thae maps every element from the enumeration `GHOST` to `MOVE`:

- `GHOST.BLINKY`
- `GHOST.PINKY`
- `GHOST.INKY`
- `GHOST.SUE`

To calculate a good move, you can query the game object to access information about the game state. The `long timeDue` indicates the system time the next move is due. You have 40ms per game tick to provide a response.

#3. The Game object

Every game tick, your controller receives a copy of the current game state via the `getMove()` method. The game object contains all the information required to make an informed decision. The class `Game.java` is the only class you need to be concerned with (you may also need to know about `Constants.java`, which holds all the game's enumerations and constants, and GameView in case you would like to use visual aids. The game is represented as a graph: you move from one node to another throughout the game. Pills and powerpills are located on specific nodes throughout the maze. Nodes are generally referred to by their indices.

#4. Putting it together
Whenever the `getMove()` method gets called, you receive a new copy of the game and the time the move is due. Query the game state using its many methods to obtain information about the pills, powerpills, positions of Pac-Man and the Ghosts etc. The game also provides numerous additional helper methods to obtains things like shortest paths or moves to take to approach a specific target. Below is an example of the simplest possible controller: a ghost teams that always chooses random moves to make.

```java
  public final class RandomGhosts extends Controller<EnumMap<GHOST,MOVE>>
  {
      private EnumMap<GHOST,MOVE> moves=new EnumMap<GHOST,MOVE>(GHOST.class);
      private MOVE[] allMoves=MOVE.values();
      private Random rnd=new Random();

      public EnumMap<GHOST,MOVE> getMove(Game game,long timeDue)
      {
          moves.clear();

          for(GHOST ghostType : GHOST.values())
              moves.put(ghostType,allMoves[rnd.nextInt(allMoves.length)]);

          return moves;
      }
  }
```
