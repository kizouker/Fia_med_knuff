## Microservices – Responsibilities & Interfaces

| Service                 | Responsibility | API / Interface | Interacts With |
|-------------------------|----------------|------------------|----------------|
| **GameSessionService**  | Manage game start, turn order, and game sessions | `GET /session/{id}`<br>`POST /session/start`<br>`PUT /session/next-turn` | PlayerService, NotificationService |
| **PlayerService**       | Register players, assign colors, link to session | `POST /players/register`<br>`GET /players/{id}` | GameSessionService |
| **DiceService**         | Roll dice, provide randomness, log history | `GET /dice/roll` | GameSessionService, PieceService |
| **PieceService**        | Track and move pieces, handle goal and knockouts | `GET /pieces/{playerId}`<br>`POST /pieces/move` | DiceService, BoardService, NotificationService |
| **BoardService**        | Board logic, collisions, zones and goal logic | `GET /board/state`<br>`POST /board/check-collision`<br>`GET /board/goal-zone/{position}` | PieceService |
| **NotificationService** | Send messages: "Your turn", "You were knocked out", etc. | `POST /notify`<br>`GET /notifications/{playerId}` | PieceService, GameSessionService |



## Sequence diagram 
```mermaid
sequenceDiagram
    participant Frontend
    participant GameSessionService
    participant DiceService
    participant PieceService
    participant BoardService
    participant NotificationService

    Frontend->>GameSessionService: GET /session/{id}
    GameSessionService-->>Frontend: current turn: PlayerA

    Frontend->>DiceService: GET /dice/roll
    DiceService-->>Frontend: 5

    Frontend->>PieceService: POST /pieces/move (pieceId, 5)
    PieceService->>BoardService: POST /board/check-collision
    BoardService-->>PieceService: No collision

    PieceService-->>Frontend: Move successful
    PieceService->>GameSessionService: PUT /session/next-turn
    GameSessionService->>NotificationService: POST /notify (Next player)
    NotificationService-->>Frontend: Message sent
