# ASL AI Engine	

> Play **Advanced Squad Leader Starter Kit** against an AI opponent using structured JSON game data.

---
# How to play

1) Open your favorite AI agent
2) Find scenario you want to play and get its url. For example: https://github.com/MarekVachal/asl_ai_engine/blob/master/scenarios/S1.json
3) Put this text to your AI agent:

I want to play an Advanced Squad Leader Starter Kit scenario.

Scenario:
https://github.com/MarekVachal/asl_ai_engine/blob/master/scenarios/S1.json

Load the scenario and automatically resolve all required resources from the same GitHub repository using registry.json.

During the game:
- use the scenario, rules, maps and units JSON files as the only authoritative source,
- never invent map data or unit statistics,
- if any required information is missing, ask me instead of making assumptions,
- act as my opponent, rules assistant and game engine.

4) Win the game

---

# Project Goal

The purpose of this project is **not** to recreate ASL as a computer game.

Instead, it provides a structured data format that allows any Large Language Model (ChatGPT, Claude, Gemini, local LLMs, etc.) to act as an intelligent opponent, coach, or referee while players use the physical board game.

The AI should think like a human ASLSK player.

Players continue using:

- original maps
- original counters
- original dice
- original rules

The AI provides:

- tactical decisions
- legal move verification
- rules assistance
- post-game analysis

---

# Philosophy

The project separates information into four independent layers.

```
                +----------------+
                |   AI Agent     |
                +-------+--------+
                        |
        +---------------+----------------+
        |                                |
   Scenario JSON                  Rules JSON
        |                                |
        +------+---------------+---------+
               |               |
           Map JSON        Units JSON
               |               |
               +-------+-------+
                       |
                Game State JSON
```

Each layer has a single responsibility.

---

# Components

## Map

Contains only static map information.

Example:

```json
{
    "N3": {
        "terrain":"OpenGround",
        "road_connections":[
            "O3",
            "O4",
            "P2",
            "P3"
        ],
        "los":{}
    }
}
```

The map never changes during a game.

---

## Rules

Contains the generic ASLSK rules.

Example:

```json
{
    "terrain_types": {
        "StoneBuilding": {
            "movement_cost":2,
            "tem":3
        }
    }
}
```

The rules file is shared by every scenario.

---

## Scenario

Defines:

- setup
- reinforcements
- victory conditions
- number of turns
- special scenario rules (SSR)

---

## Game State

Contains everything that changes during the game.

Example:

```json
{
    "turn":2,

    "phase":"Movement",

    "units":{

        "G12":{

            "location":"K2",

            "status":"GoodOrder",

            "markers":[
                "FirstFire"
            ]

        }

    }

}
```

---

# LOS Database

Unlike many board games, LOS cannot be determined only from terrain type.

Buildings and woods do **not** occupy the whole hex.

Therefore LOS is stored separately.

Initially:

```json
"los":{}
```

During games the community gradually expands it.

Example:

```json
"los":{

    "K2":{

        "exists":true,

        "hindrance":0,

        "blocked_by":[]

    }

}
```

This creates an ever improving LOS database.

---

# AI Responsibilities

The AI should:

- choose tactical decisions
- verify legal actions
- explain rules
- recommend attacks
- perform post-game analysis

The AI should NOT:

- invent map information
- assume unknown LOS
- ignore official rules

If map information is missing, the AI asks the player.

---

# Human Responsibilities

The player provides:

- dice rolls
- LOS confirmation (if unknown)
- physical counter movement

The player remains the final authority for the physical game.

---

# Design Principles

The project follows several principles.

## Single Source of Truth

Every piece of information exists only once.

Example:

Terrain type:

```json
"terrain":"StoneBuilding"
```

There is no duplicated field:

```json
"building":true
```

---

## Static vs Dynamic Data

Static:

- maps
- rules
- scenarios

Dynamic:

- game state

---

## AI Independence

The data format should be usable by any AI model.

---

# Future Goals

- Complete Starter Kit 1 support
- Starter Kit 2 support
- Starter Kit 3 support
- Full ASL support
- Community maintained LOS database
- Automatic tactical analysis
- Automatic After Action Reports

---

# Copyright

This project does **not** contain:

- official maps
- official rulebook
- official counter artwork

It contains only community-created structured metadata describing game mechanics.

Users must own the original Advanced Squad Leader Starter Kit 1.

# My other work

You can check my own games on https://marekvachal.github.io/marks2games/
