# Wzorce obiektowe



<details>
<summary><h2>Builder</h2></summary> 
#### Builder
`DungeonBuilder` is responsible for constructing the dungeon map step by step by storing and executing a sequence of generation steps on the `Board`. This separates individual room-generation strategies, such as `FixedRoomStep`, `CentralChamberStep`, and `RandomRoomsStep`, from the building process and allows different combinations of steps to create different dungeon layouts.


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
<summary><h3>Decorator</h3></summary>
### Decorator
`WeaponDecorator` adds effects to weapons dynamically without modifying their original classes. The weapon itself does not know that it is decorated, allowing effects such as `StrongEffect` and `LuckyEffect` to be combined flexibly while keeping weapons independent.



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
<summary><strong>Chain of responsibility</strong></summary>
#### Chain of responsibility
`Chain of Responsibility` handles keyboard input in the game by passing each pressed key through a chain of handlers. Each handler checks whether the key belongs to its responsibility, such as movement, combat, inventory or log view; if not, it passes the input to the next handler. This keeps keyboard handling separated into independent parts of the game.


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
<summary>Abstract Factory</summary>
#### Abstract Factory
`Abstract Factory` allows the game to create complete dungeon themes such as **Dungeon, Forest and Bank** through the same factory interface. Each theme can provide its own map-generation steps, items and artifacts without duplicating the code responsible for creating and assembling the game world.



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
<summary>Visitor I</summary>
#### Visitor
`Visitor` is used in the combat system to calculate attack and defense differently depending on the weapon type and the selected combat style. The key feature is that **new operations can be added without modifying the weapon classes**: `NormalAttackVisitor`, `StealthAttackVisitor` and `MagicAttackVisitor` define how each weapon type behaves, while the weapons themselves remain unchanged.


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
`Visitor` is used in the combat system to calculate attack and defense differently depending on the weapon type and the selected combat style. The key feature is that **new operations can be added without modifying the weapon classes**: `NormalAttackVisitor`, `StealthAttackVisitor` and `MagicAttackVisitor` define how each weapon type behaves, while the weapons themselves remain unchanged.


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
`Observer` is used in the game to react to the death of a monster belonging to a species. `MonsterSpecies` notifies all subscribed `Monster` objects when one of their members dies, and each monster reacts according to its assigned strategy — for example, `AggressiveSpeciesReaction` increases attack by 5, while `CowardSpeciesReaction` removes its defense. The key feature is that the subject does not need to know how observers react, so new reactions can be added without changing the notification mechanism.


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
<summary>Singleton</summary>
#### Singleton
`Singleton` provides global access to logging from anywhere in the game while ensuring that only one `GameLog` instance exists. This means every part of the project writes to the same centralized log, keeping logging consistent across the entire application.


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
<summary>Strategy</summary>
### Strategy
`Strategy` is used in the game to define different movement behaviours for monsters while keeping the `MonsterMovementSystem` independent from their specific logic. Depending on the assigned strategy, a monster can move randomly, follow or flee from sounds, or follow or flee from players. The key feature is that these behaviours are interchangeable, so the movement logic can be changed without modifying `MonsterMovementSystem`.


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