# Clue-Less
Simplified version of board game Clue

## Run the server
- The commands below are based on macOS. Modify them to match your development environment.
1. Install dependencies (Python version 3.10.16)
- % cd move-to-the-root-of-project-directory
- % activate-virtual-environment
- % pip install -r requirements.txt
    * To uninstall all dependencies
        - % python uninstall_packages.py
2. Run the server
    2-1. Run Redis server
        - % redis-server
    2-2. Setting for ASGI/Channels
        - % python run_daphne.py
        * To run the server listens both HTTP (pages) and WebSocket (live) connections.
            You can see why it is needed and how it works on [https://www.notion.so/hyunjinkimdeveloper/Clue-Less-1a421801a53980059dbcc9c29b1b382f#1a821801a53980b39c8ced3d368ff56d]
    2-3. Run Django server
        - % python manage.py runserver
        * Server log shows “Starting ASGI/Daphne version [version] development server at [url]”
- Development Log
    The development log contains debugging cases<br>
    You can track updates on [https://hyunjinkimdeveloper.notion.site/Clue-Less-1a421801a53980059dbcc9c29b1b382f?pvs=4]

## Directory Tree
```
clue-less/
├── manage.py              # Django's command-line utility for administrative tasks
├── requirements.txt       # Lists project dependencies (Django, Channels, etc.)
├── README.md              # Project overview, setup, and feature documentation
├── clueless/             # Main Django project directory
│   ├── __init__.py       # Marks directory as a Python package
│   ├── asgi.py           # ASGI entry point for Channels (WebSocket support)
│   ├── settings.py       # Project configuration (database, apps, Channels setup)
│   ├── urls.py           # Root URL routing for the project
│   └── wsgi.py           # WSGI entry point for traditional HTTP (not used with Daphne)
├── game/                 # Django app for game logic
│   ├── __init__.py       # Marks directory as a Python package
│   ├── admin.py          # Admin panel configuration (auto-generated, optional, currently minimal)
│   ├── apps.py           # App configuration (auto-generated)
│   ├── constants.py      # Game constants (SUSPECTS, ROOMS, etc.)
│   ├── consumers.py      # WebSocket consumer for real-time game updates
│   ├── management/       # Customized testing codes
│   │   ├── __init__.py   # Marks directory as a Python package
│   │   └── commands/
│   │       ├── delete_all_players.py   # Run the script: python manage.py delete_all_players
│   │       ├── logout_all_players.py
│   │       └── print_all_users.py
│   ├── migrations/       # Database migration files
│   │   ├── __init__.py   # Marks directory as a Python package
│   │   └── 0001_initial.py  # Initial migration for Game/Player models
│   ├── models.py         # Database models (Game, Player)
│   ├── templates/        # HTML templates
│   │   └── game/         # Subdirectory for game-related templates
│   │       ├── game.html  # Main game page with board and movement
│   │       └── login.html # Login/signup page
│   ├── routing.py        # WebSocket Routing
│   ├── tests.py          # Unit tests (auto-generated, currently minimal)
│   ├── views.py          # HTTP view functions (login, game, logout)
│   └── urls.py           # App-specific URL routing
├── uninstall_packages.py # Uninstall all packages in the virtual environment
└── run_daphne.py         # Custom script to start the Daphne server with predefined settings, bypassing the command-line daphne invocation.
```

## Features
### Current Features
* Login/Logout:
    Players authenticate and join Game 1,
    with logout deactivating their Player instance.
* Character Assignment:
    Unique characters assigned on first login,
    preserved across logouts (stored in game.players and game.players_list).
* Movement:
    Click adjacent rooms or hallways to move;
    all players can move freely without turn restrictions, with updates synced via WebSocket.
* Board Display:
    5x5 grid shows all players’ positions, active or inactive.
    This allows the game to continue even when some players log out.
### Known Issues
* Reload Problem:
    Reloading a tab displays another player’s page.
    Assuming the tab displays shows the latest login or log out player's information. Need to verification.
### Planned Features
* Turn Rotation:
    Implement sequential turns for Clue’s traditional gameplay (currently disabled per request).
* Suggestions/Accusations:
    Add mechanics for players to suggest or accuse, integrating card-based disproving and winning conditions.
* Game End:
    Define victory conditions and reset mechanics.
