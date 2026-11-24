# Optimus Control

**Working project plan**\
**Status:** Early concept and planning\
**Repository:** `optimus-control`\
**Repository location:** `/opt/optimus-control`\
**Suggested GitHub description:** *A Stream Deck-inspired control center
with a focus on open-source integrations.*

------------------------------------------------------------------------

## 1. Project vision

Optimus Control is a standalone, touch-first control center intended to
turn a docked Steam Deck into a dedicated desktop control surface.

The idea is to make useful use of the Deck's built-in screen while it
sits beneath the user's dual-monitor workstation. The experience should
borrow the best parts of Stream Deck---actions, integrations, shortcuts,
profiles, and extensibility---without being constrained by a fixed grid
of small buttons.

The application should be useful independently of the Optimus Home Lab.
It may integrate with Optimus services, but local controls should
continue to work even when the Home Lab server or network services are
unavailable.

### Core product principle

> A button should open the interface appropriate to the function, not
> force the function into a button.

For example, selecting Music should be able to open a full music-player
interface with artwork, track or station information, playback controls,
and volume---not merely navigate to another page of tiny buttons.

## 2. User experience and interaction

### 2.1 Flexible home screen

The home screen should be customizable and composed of useful tiles or
widgets. Tiles may represent:

-   Music and radio
-   System volume and audio output
-   Home Lab status
-   Donetick tasks
-   Holmes diagnostics
-   Automations and shortcuts
-   Workstation controls
-   Other integrations added later

The home screen is a launch point, not a requirement that every feature
remain inside a button grid.

### 2.2 Different interaction types

The interface should support several kinds of destinations and actions:

-   **Quick actions:** Execute immediately, such as mute, toggle,
    launch, or trigger an automation.
-   **Widget views:** Open a larger interactive interface, such as a
    full music player, audio mixer, or system monitor.
-   **Navigation views:** Open a collection of related actions, such as
    Home Lab controls or automation categories.
-   **Status views:** Show useful information, such as service health,
    pending tasks, or diagnostic results.

A single integration may offer both a compact home-screen tile and a
more complete widget view.

### 2.3 Touch and physical controls

The design should support touchscreen interaction and the Steam Deck's
physical controls together.

Touch interaction should be natural for a docked touchscreen:

-   Large, easy-to-hit targets
-   Tap to activate
-   Swipe for navigation where appropriate
-   Sliders for values such as volume
-   Long press for secondary actions or configuration where useful
-   Drag-and-drop or other direct manipulation where it improves
    customization

The physical controls should provide a usable alternative to touch.
Potential mappings to investigate include:

-   D-pad or joystick navigation
-   A button to select
-   B button to go back
-   Shoulder buttons to move between major pages or tabs
-   Triggers for contextual controls such as volume

These are design ideas, not finalized bindings. The interface should
make focus and selection clear when operated with a controller.

### 2.4 Display assumptions

The initial target is the Steam Deck's built-in display in a docked,
landscape-oriented setup. The interface should be designed around the
Deck's available screen space, with readable text and touch-friendly
controls. Exact layout, scaling, and orientation behavior remain to be
validated on the device.

A full-screen or kiosk-like presentation is worth investigating.

## 3. Music and media experience

Music is an important example of the difference between Optimus Control
and a conventional Stream Deck layout.

Selecting Music should be able to open a dedicated player view that
makes good use of the available display. Depending on the source, it
could show:

-   Album, station, or stream artwork
-   Track, artist, or station information
-   Play/pause and skip controls where supported
-   Playback progress or a live-stream indicator
-   Volume
-   Source or station selection

The user likes the Lofi Girl experience but is open to independent radio
streams if they provide a more reliable and straightforward integrated
player.

Spotify is a possible integration to investigate, but its API,
authentication, playback-device requirements, and Linux/SteamOS behavior
need to be validated before committing to it. The player experience
should not assume every music source supports the same controls.

## 4. Integrations and capabilities

The project should prioritize useful integrations and reuse of existing
open-source work rather than reimplementing every capability.

### 4.1 Local device integrations

These should be useful without depending on the Optimus server:

-   System volume
-   Audio output/device selection
-   Media playback controls
-   Local applications and shortcuts
-   Workstation actions that can be performed locally

Potential technologies discussed for investigation include Linux
audio/session interfaces such as PipeWire and media-control interfaces
such as MPRIS over D-Bus. These are candidates, not final implementation
decisions.

### 4.2 Home Lab and remote integrations

Potential integrations include:

-   **Donetick:** View tasks and, eventually, create tasks from the
    control surface.
-   **Holmes:** Trigger or view diagnostics and display relevant
    findings.
-   **Optimus/Home Lab:** Display service status and expose carefully
    scoped actions.
-   **Docker-based services:** Show useful status and provide selected
    operational controls.

Remote integrations should be optional. A failed connection to the Home
Lab should not break local audio, media, or workstation controls.

Any operational actions---especially actions that affect containers or
services---should be explicit, appropriately authenticated, and designed
with safeguards. Read-only status and diagnostics are a sensible
starting point.

### 4.3 Integration model

Keep integrations separate from presentation:

-   An integration communicates with a local subsystem or remote
    service.
-   It exposes capabilities and state in a consistent way.
-   The UI decides whether to display that capability as a compact tile,
    a full widget, a status view, or an action.

This separation should allow the same integration to support different
layouts without duplicating its underlying logic.

## 5. Architecture direction

The architecture is not selected yet. The first technical task should be
to evaluate viable frameworks and existing projects against the actual
Steam Deck/SteamOS environment.

Principles to preserve:

1.  **Standalone operation:** The application must not require the
    Optimus Home Lab to be online.
2.  **Separation of concerns:** UI, integrations, actions, and
    navigation should not be tightly coupled.
3.  **Extensibility:** New integrations and widgets should be addable
    without redesigning the application.
4.  **Reuse:** Prefer mature open-source libraries, projects, and
    documented system interfaces where practical.
5.  **SteamOS-conscious:** Avoid unnecessary changes to the SteamOS base
    system. Investigate a deployment approach compatible with the user's
    existing Steam Deck workflow.
6.  **Safe remote actions:** Use narrow permissions and explicit
    confirmation for consequential operations.
7.  **Graceful degradation:** Missing integrations should not prevent
    the rest of the control center from working.

Qt Quick/QML was discussed as one possible UI option. SDL or another
suitable input layer was discussed for controller support. These are
exploratory candidates only; no framework or implementation stack has
been chosen.

## 6. Open-source reuse and licensing

The project should reuse existing open-source capabilities wherever
doing so makes sense.

Before adopting a dependency or copying code, record:

-   Project and version
-   Exact license
-   Whether it is linked, embedded, modified, or run as a separate
    process
-   Required notices, source offers, or other obligations
-   Whether the intended distribution model is compatible

Permissive licenses such as MIT, BSD, and Apache-2.0 may offer more
flexibility for a project that could remain proprietary. LGPL can also
be usable under its conditions. GPL and AGPL dependencies require closer
review, especially when code is incorporated into or combined with the
application.

Running an independent GPL-licensed program as a separate process may be
a useful architectural option, but process separation alone is not a
universal legal guarantee. Review the actual dependency and integration
before deciding.

**Current decision:** No project license has been selected. Keep the
repository private while the project direction and dependency choices
are being explored. Decide on a license before public distribution or
accepting outside contributions.

## 7. Repository and project organization

The project has its own Git repository:

``` text
optimus-control/
```

It is separate from the existing Home Lab repository. The two
repositories can be open together in the same VS Code multi-root
workspace, but their Git histories, branches, and remotes remain
independent.

A possible future layout is:

``` text
optimus-control/
├── README.md
├── LICENSE                 # Add once a project license is chosen
├── docs/
│   ├── architecture.md
│   ├── design-principles.md
│   └── integrations.md
├── app/
│   ├── ui/
│   ├── widgets/
│   ├── actions/
│   └── navigation/
├── integrations/
│   ├── audio/
│   ├── media/
│   ├── system/
│   └── optimus/
└── tests/
```

This is an illustrative structure, not a requirement to create all these
directories immediately. The final structure should follow the selected
framework and architecture.

## 8. Initial scope and suggested milestones

### Phase 1 --- Define and investigate

-   Document the product vision and interaction principles.
-   Inspect the Steam Deck's SteamOS environment and available
    development workflow.
-   Evaluate candidate UI frameworks and controller-input approaches.
-   Identify existing projects and libraries that can be reused.
-   Check licenses and project activity before adopting dependencies.
-   Decide how the application will be launched and updated on the Deck.

### Phase 2 --- Build a local-only proof of concept

Start with capabilities that do not depend on the Home Lab:

-   Full-screen home screen with flexible tiles
-   Navigation using touch and physical controls
-   System volume control
-   Audio output selection
-   A media-player/widget view using a practical local or internet-radio
    source

The proof of concept should validate the central design idea: selecting
a tile can open a purpose-built full-screen widget rather than another
fixed button grid.

### Phase 3 --- Establish the integration framework

-   Define a consistent integration interface for capabilities and
    state.
-   Support compact tiles and expanded widget views.
-   Add integration configuration and connection/error states.
-   Make integrations independently enableable or disableable.

### Phase 4 --- Add Home Lab integrations

Potential order:

1.  Read-only Home Lab/service status
2.  Donetick task visibility
3.  Holmes diagnostic visibility
4.  Carefully scoped actions and task creation
5.  Additional Docker or workstation controls as justified

### Phase 5 --- Polish and expand

-   Customizable home-screen layout
-   Profiles or context-specific pages
-   User-configurable actions and shortcuts
-   More media sources and integrations
-   Accessibility, responsiveness, and reliability improvements
-   Packaging, installation, and update workflow

These phases are a proposed sequence, not a fixed delivery commitment.

## 9. Decisions still open

The following should be decided through research or prototyping rather
than assumed:

-   Final application framework and programming language
-   SteamOS deployment and update strategy
-   Whether the UI is landscape-only or supports other orientations
-   Exact touchscreen and controller mappings
-   Music sources and the feasibility of Spotify integration
-   Plugin/integration discovery and configuration model
-   How users customize tiles, widgets, and pages
-   Whether the project will remain private/proprietary or become open
    source
-   Final project name, branding, and visual design

## 10. Guiding statement

**Optimus Control transforms a docked Steam Deck into a flexible desktop
control surface: Stream Deck-style actions and integrations combined
with full-screen, purpose-built widgets and touch/controller
interaction.**

It should be useful on its own first, with the Optimus Home Lab adding
capabilities rather than being a requirement.
