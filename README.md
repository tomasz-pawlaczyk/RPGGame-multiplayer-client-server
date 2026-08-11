# Wzorce obiektowe



#### Dekorator
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



#### Builder







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












