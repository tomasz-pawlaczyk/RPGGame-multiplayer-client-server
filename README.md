# Wzorce obiektowe




<details>
<summary>Decorator</summary>

### Dekorator
  

```mermaid
classDiagram
    direction TB

    class Weapon {
        +Name
        +Damage
    }

    class Sword {
        +Damage
    }

    class Dagger {
        +Damage
    }

    class Bow {
        +Damage
    }

    class WeaponDecorator {
        #Weapon inner
    }

    class StrongEffect {
        +DamageBonus
    }

    class LuckyEffect {
        +LuckBonus
    }

    Weapon <|-- Sword
    Weapon <|-- Dagger
    Weapon <|-- Bow

    Weapon <|-- WeaponDecorator
    WeaponDecorator o-- Weapon : wraps

    WeaponDecorator <|-- StrongEffect
    WeaponDecorator <|-- LuckyEffect
```

</details>











<details>
<summary>Chain of responsibility</summary>


#### Chain of responsibility

```mermaid
classDiagram
direction LR

class InputHandler {
    <<abstract>>
    #next : InputHandler
    +SetNext(handler)
    +Handle(input)
}

class MoveHandler {
    +Handle(input)
    -MovePlayer()
}

class CombatHandler {
    +Handle(input)
    -Attack()
}

class InventoryHandler {
    +Handle(input)
    -PickItem()
    -DropItem()
}

class LogHandler {
    +Handle(input)
    -ToggleLogView()
}

class DefaultHandler {
    +Handle(input)
}

InputHandler <|-- MoveHandler
InputHandler <|-- CombatHandler
InputHandler <|-- InventoryHandler
InputHandler <|-- LogHandler
InputHandler <|-- DefaultHandler

MoveHandler --> CombatHandler : next
CombatHandler --> InventoryHandler : next
InventoryHandler --> LogHandler : next
LogHandler --> DefaultHandler : next
```
</details>
  
<details>
<summary>Builder</summary>
  
  
#### Builder

```mermaid
classDiagram
direction LR

class DungeonBuilder {
    -steps
    +AddStep(step)
    +Build(board)
}

class IDungeonStep {
    <<interface>>
    +Apply(board, builder)
}

class FixedRoomStep
class CentralChamberStep
class RandomRoomsStep

class Board

DungeonBuilder o-- IDungeonStep : contains
IDungeonStep <|.. FixedRoomStep
IDungeonStep <|.. CentralChamberStep
IDungeonStep <|.. RandomRoomsStep

DungeonBuilder --> Board : builds
```
</details>
  
  


<details>
<summary>Abstract Factory</summary>

#### Abstract Factory

```mermaid
classDiagram
direction LR

class IDungeonThemeFactory {
    <<interface>>
    +CreateGenerationSteps()
    +CreateItems()
    +CreateArtifact()
}

class DungeonThemeFactory {
    +CreateGenerationSteps()
}

class ForestThemeFactory {
    +CreateGenerationSteps()
}

class IDungeonStep {
    <<interface>>
}

class Item {
    <<abstract>>
}


class FilledDungeonStep
class CentralChamberStep
class Food
class Weapon

IDungeonThemeFactory <|.. DungeonThemeFactory
IDungeonThemeFactory <|.. ForestThemeFactory


IDungeonStep <|.. FilledDungeonStep
IDungeonStep <|.. CentralChamberStep
IDungeonStep <|.. RandomRoomsStep

Item <|-- Food
Item <|-- Weapon

DungeonThemeFactory ..> IDungeonStep : creates
ForestThemeFactory ..> IDungeonStep : creates
DungeonThemeFactory ..> Item : creates
ForestThemeFactory ..> Item : creates
```
</details>
  
  
  
<details>
<summary>Singleton</summary>

  
#### Singleton

```mermaid
classDiagram
direction LR

class GameLog {
    <<Singleton>>
    -static instance : GameLog
    -GameLog()
    +Instance : GameLog
    +Log(message)
    +GetEntries()
}

class GameLogUser {
    +Log(message)
}

GameLogUser --> GameLog : uses
GameLog ..> GameLog : single instance
```

w pliku Logging/GameLog.cs
</details>
  
  


<details>
<summary>Visitor I</summary>

#### Visitor

```mermaid
classDiagram
direction LR

class ICombatVisitor {
    <<interface>>
    +Attack(HeavyWeapon, Player, damage)
    +Attack(LightWeapon, Player, damage)
    +Attack(MagicWeapon, Player, damage)
    +Defense(HeavyWeapon, Player, defense)
    +Defense(LightWeapon, Player, defense)
    +Defense(MagicWeapon, Player, defense)
}

class NormalAttackVisitor
class StealthAttackVisitor
class MagicAttackVisitor

class Weapon {
    <<abstract>>
}
class HeavyWeapon
class LightWeapon
class MagicWeapon
class Player

ICombatVisitor <|.. NormalAttackVisitor
ICombatVisitor <|.. StealthAttackVisitor
ICombatVisitor <|.. MagicAttackVisitor

Weapon <|-- HeavyWeapon
Weapon <|-- LightWeapon
Weapon <|-- MagicWeapon

Player --> ICombatVisitor : uses

ICombatVisitor --> HeavyWeapon : visits
ICombatVisitor --> LightWeapon : visits
ICombatVisitor --> MagicWeapon : visits
```

w folderze Model/Combat/CombatSystem.cs
</details>
  
  
  
  
  
  
<details>
<summary>Visitor II</summary>
  
#### Visitor

```mermaid
classDiagram
direction TB

class ICombatVisitor {
    <<interface>>
    +Attack(HeavyWeapon, Player, damage)
    +Attack(LightWeapon, Player, damage)
    +Attack(MagicWeapon, Player, damage)
    +Defense(HeavyWeapon, Player, defense)
    +Defense(LightWeapon, Player, defense)
    +Defense(MagicWeapon, Player, defense)
}

class NormalAttackVisitor {
    +Attack()
    +Defense()
}

class StealthAttackVisitor {
    +Attack()
    +Defense()
}

class MagicAttackVisitor {
    +Attack()
    +Defense()
}

class Weapon {
    <<abstract>>
}

class HeavyWeapon
class LightWeapon
class MagicWeapon

class Player

ICombatVisitor <|.. NormalAttackVisitor
ICombatVisitor <|.. StealthAttackVisitor
ICombatVisitor <|.. MagicAttackVisitor

Weapon <|-- HeavyWeapon
Weapon <|-- LightWeapon
Weapon <|-- MagicWeapon

Player --> ICombatVisitor : uses

ICombatVisitor --> HeavyWeapon : visits
ICombatVisitor --> LightWeapon : visits
ICombatVisitor --> MagicWeapon : visits
```
</details>
  
  
  
  
<details>
<summary>Observer</summary>
#### Observer

```mermaid
classDiagram
direction LR

class IMonsterSpeciesSubject {
    <<interface>>
    +Subscribe(observer)
    +Unsubscribe(observer)
    +NotifySpeciesDeath(notice)
}

class IMonsterSpeciesObserver {
    <<interface>>
    +OnSpeciesDeath(notice)
}

class MonsterSpecies {
    -observers
    +Subscribe(observer)
    +NotifySpeciesDeath(notice)
}

class Monster {
    +OnSpeciesDeath(notice)
}

class IMonsterSpeciesReaction {
    <<interface>>
    +Apply(monster, notice)
}

class AggressiveSpeciesReaction {
    +IncreaseAttack(5)
}

class CowardSpeciesReaction {
    +LoseDefense()
}

IMonsterSpeciesSubject <|.. MonsterSpecies
IMonsterSpeciesObserver <|.. Monster

MonsterSpecies --> IMonsterSpeciesObserver : notifies
Monster --> IMonsterSpeciesReaction : reacts with

IMonsterSpeciesReaction <|.. AggressiveSpeciesReaction
IMonsterSpeciesReaction <|.. CowardSpeciesReaction
```

/Model/Entities/Species 
</details>
  
  
  
  
  
<details>
<summary>Startegy</summary>
  
### Strategy

```mermaid
classDiagram
direction TB

class IMonsterMovementBehaviour {
    <<interface>>
    +TryGetMove(monster, state)
}

class RandomMovement {
    +TryGetMove(monster, state)
}

class FollowSound {
    +TryGetMove(monster, state)
}

class FleeSound {
    +TryGetMove(monster, state)
}

class FollowPlayer {
    +TryGetMove(monster, state)
}

class FleePlayer {
    +TryGetMove(monster, state)
}

class MonsterMovementSystem {
    +Update(state)
}

IMonsterMovementBehaviour <|.. RandomMovement
IMonsterMovementBehaviour <|.. FollowSound
IMonsterMovementBehaviour <|.. FleeSound
IMonsterMovementBehaviour <|.. FollowPlayer
IMonsterMovementBehaviour <|.. FleePlayer

MonsterMovementSystem --> IMonsterMovementBehaviour : uses
```
</details>