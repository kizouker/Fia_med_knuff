# Game Service - Process View

```mermaid
sequenceDiagram
    participant Player
    participant GameService
    participant DiceService
    participant BoardService

    Player->>GameService: requestRollDice()
    GameService->>DiceService: roll()
    DiceService-->>GameService: diceResult
    GameService->>BoardService: moveToken(diceResult)
    BoardService-->>GameService: updatedBoardState
    GameService->>GameService: checkWinCondition()
    GameService-->>Player: notifyMoveCompleted()
