# Game Service - User Stories

- As a Player, I want to roll a dice, so that I can determine how far I can move.
- As a Player, I want to move a token, so that I can advance on the board.
- As a Player, I want the system to switch turns automatically, so that the next player can play.
- As a Player, I want to see if I have won, so that I know when the game is over.
- As a Player, I want to start a new game, so that I can play again.

# Game Service - sequence?



# Game Service - Logical View (Real Architecture Style)

```mermaid
classDiagram
    class Player {
        +requestMove()
        +requestRollDice()
    }
    class GameService {
        +startGame()
        +handleMove()
        +checkWin()
        +manageTurns()
    }
    class DiceService {
        +roll()
    }
    class BoardService {
        +updateTokenPosition()
        +resetBoard()
    }
    class Token {
        +position
        +color
    }

    Player --> GameService : sends requests
    GameService --> DiceService : rolls dice
    GameService --> BoardService : updates board
    BoardService --> Token : manipulates tokens
    GameService --> Player : notifies next turn / win



