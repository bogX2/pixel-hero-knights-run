# Pixel Hero: Knight's Run

**2D pixel-art platformer for Android, built with Unity.** A pixelated knight runs through a side-scrolling world in the style of classic platformers: fight enemies, collect food to restore health, and survive to the end of the level.

Team project (2 people, with [@AleUmi](https://github.com/AleUmi)) for the *Mobile Computing* course, Bachelor's degree in Computer Engineering, Roma Tre University. Both members worked across gameplay, UI and integration.

[![Gameplay video](https://img.youtube.com/vi/9ULoxAikbqc/hqdefault.jpg)](https://youtu.be/9ULoxAikbqc)

▶️ **[Watch the gameplay on YouTube](https://youtu.be/9ULoxAikbqc)**

---

## Features

- **Player movement**: walk, run and jump with physics-based 2D movement (`Rigidbody2D`) and ground/wall/ceiling detection
- **Combat**: melee attacks and a **ranged attack** that fires projectiles, with damage, knockback and invincibility frames
- **Enemies with simple AI**:
  - **Knight**: patrols the ground, detects the player through a detection zone and attacks with a cooldown
  - **Flying Eye**: flies along a waypoint path and bites the player when it comes within range
- **Pickups**: food items scattered through the level restore health
- **UI**: health bar, floating damage/heal text, main menu, pause menu (resume, restart, back to menu) and end screen
- **Visuals and audio**: pixel-art tilesets, **parallax** scrolling backgrounds, a smooth camera with Cinemachine, background music and sound effects
- **Account login** with **Firebase Authentication** (email and password)
- **Mobile-first**: built for Android with Unity's new **Input System**

## How it's built

| Area | Technology |
|---|---|
| Engine | Unity 2022.3 LTS (2022.3.11f1), Universal Render Pipeline (2D) |
| Language | C# |
| Input | Unity Input System (action-based callbacks: move, run, jump, attack, ranged attack) |
| Camera | Cinemachine |
| Animations | Animator state machines with custom `StateMachineBehaviour`s (fade-out on death, one-shot sounds, bool/float parameter setters) |
| Backend | Firebase Authentication |
| Target | Android |

**Architecture highlights**

- **Component-based design**: shared behaviour lives in reusable components (`Damageable`, `TouchingDirections`, `DetectionZone`, `Attack`, `ProjectileLauncher`) used by both the player and the enemies.
- **Event-driven UI**: damage and healing raise `CharacterEvents`, which the `UIManager` listens to in order to spawn floating text, so gameplay code stays decoupled from the UI.
- **Animation-driven logic**: animation strings are centralised in `AnimationStrings`, and states such as "can move" or "is alive" are read from the Animator.

## Project structure

```
Assets/
├── Scripts/
│   ├── PlayerController.cs     # movement, jump, run, melee and ranged attacks
│   ├── Knight.cs, FlyingEye.cs # enemy AI
│   ├── Damageable.cs           # health, damage, knockback, invincibility
│   ├── TouchingDirections.cs   # ground / wall / ceiling checks
│   ├── DetectionZone.cs, Attack.cs, Projectile*.cs
│   ├── HealthPickup.cs, ParallaxEffect.cs, MusicPlayer.cs
│   ├── UIManager.cs, Menu.cs, PauseMenu.cs
│   ├── Events/                 # CharacterEvents (damage / heal)
│   └── StateMachine/           # custom Animator behaviours
├── Scenes/                     # MainMenu, GameplayScene, QuitScene
├── Characters/, Objects/, UI/  # prefabs
├── Art/, Audio/                # sprites, tilesets, music and SFX
└── LoginController.cs          # Firebase email/password login
```

## Getting started

Requirements: **Unity Hub** with **Unity 2022.3.11f1** (or a later 2022.3 LTS) and **Android Build Support**.

1. Clone the repository and open the folder from Unity Hub (*Add project from disk*). The first import takes a few minutes, since Unity rebuilds the `Library` folder.
2. Open `Assets/Scenes/MainMenu.unity` and press **Play** to try it in the editor.
3. To build for Android: *File → Build Settings → Android → Switch Platform → Build*.

**Firebase login**: the Firebase config files (`google-services.json`) are **not included** in this repository. To use the login, create your own Firebase project, enable *Email/Password* authentication and place your `google-services.json` in `Assets/StreamingAssets/`. The gameplay itself does not need Firebase.

## Credits

- **Developers**: Bogdan Andrei Tutuianu ([@bogX2](https://github.com/bogX2)) and [@AleUmi](https://github.com/AleUmi)
- **Art**: free pixel-art asset packs (knight, monsters, tileset, pixel food, RPG icons). All rights belong to their original authors.
