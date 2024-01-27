# Pet Project

Note that this document was edited using the GitHub friendly mark down [format](https://www.markdownguide.org/cheat-sheet/).

## Description
The purpose of this document is to discuss plans for the design of our first game in the pet project for our Solutioneers group.
This is going to be Tic-Tac-Toe.
This was chosen mainly for simplicity and will require a client and server component.  These components will ultimately be developed in various different languages/frameworks, the idea being that we have a common goal but can all work and learn on our languages of choice.
Ultimately we expect the specification (network protocol, game mechanics) to be standardised which will mean that different flavours of the project will be fully interchangeable (e.g. Flutter -> Java, JS -> C++, etc).

Everyone understands Tic-Tac-Toe so it will be a good starting point.

That said, lets break down the game in detail such that we can start to consider how we might develop standard game mechanics).

### Introduction to Tic-Tac-Toe

This Tic Tac Toe project is a classic two-player game developed using TypeScript. The aim is to provide a simple yet interactive experience, showcasing fundamental programming concepts in game development.

### Game Overview

Tic Tac Toe is a paper-and-pencil game for two players, X and O, who take turns marking the spaces in a 3×3 grid. The player who succeeds in placing three of their marks in a horizontal, vertical, or diagonal row wins the game.

### Game States

#### Start Game State

At the beginning of the game, a 3x3 grid is initialized, and players are prompted to choose between X or O. The first turn is randomly assigned.

#### Player Selection State

Players select their symbol (X or O). This choice determines their turns and the symbols they'll use throughout the game.

#### Move State

Players take turns to place their symbol on the grid. A move is considered valid if it's placed in an empty cell. The state of the grid is updated after each move.

#### Check Win State

After each move, the game checks for a winning condition - three symbols in a row, column, or diagonal. It also checks for a draw, which occurs when the grid is full without a winning condition.

#### End Game State

The game concludes when a player wins or a draw occurs. Players are then given the option to restart the game.

### Game Mechanics

#### Board Representation

The game board is represented as a two-dimensional array in TypeScript. Each cell can be empty, X, or O.

#### Player Turn Logic

The game alternates turns between the two players. A player's turn is skipped if they attempt an invalid move, and they are prompted again.

#### Winning Logic

The game checks rows, columns, and diagonals for three consecutive X's or O's. If such a condition is met, the corresponding player is declared the winner.

### User Interface

The user interface is a simple grid layout with interactive cells. Players click on a cell to make a move. The current player's turn and game status are displayed on the screen.

### Error Handling

The game handles errors such as invalid moves (like selecting an already occupied cell) by prompting the player to make a valid move. The game also handles unexpected inputs or errors gracefully.

### Conclusion

This Tic Tac Toe project demonstrates fundamental game development concepts using TypeScript. Future enhancements can include AI opponents, enhanced graphics, and multiplayer support.

# The Network Protocol
How we design this will be key to engagement and will need to be standardised if our relevant client/server implementations are to be compatible with each other.
At the moment there are two basic schools of thought.
1.  Use websockets or some other *close* abstraction over **raw sockets**.  
This could be used with a standard object serializer/deserializer, such as Google ProtoBuffers.
This approach lends itself more a kin to low latency communications that you would normally expect for a game however this also adds to the complexity; it's more difficult to implement and could over complicate the point of entry.
2.  Use a REST API using HTTP(s) + Swagger.
Swagger is an open specification for a web API.  It has great tooling for almost any language and would be a great starting point for communicating between client and server.
This should be much easier to implement for most people.

## Protocol Selection
In the name of keeping things simple @Brad has made a suggestion that would permit both of these protocols.  If you are familiar with SSL then the idea is basically a kin to how the cipherspec works.  Effectively we negotiate with the server as a first point of call; the client asks the server which protocol it implements.  Initially we might just implement the REST API but we ensure that the protocol selection is in place from the get go.

The first thing a client will do will make a http(s) REST call to determine the 'network spec'.
This could return a collection in JSON which would describe, for each supported protocol:
- a name describing the protocol (e.g. Swagger or Socket).
- a url or address + port that can subsequently be used by the client for the remaining comms.

## Initial Steps

To begin with @Res is looking at generating a server of some description in Javascript.  This will serve as a starting point or baseline for clients to work against; we need some server of some description.

The initial draft will be just using REST and http.
We use could Docker for the API to run the backend in isolation or have a central server.  This might be self hosted or using Vercel.

In the meantime, @Martyn will start looking at a Flutter front end, albeit just some preliminary investigation towards drawing some form of board, given that there is currently no API to talk to.

This is all subject to change and there are going to be some thoughts on architecture/design that may follow.  
