---

title: "Desktop Companion - Building the Reminder Engine and Electron IPC Communication"
date: 2026-07-29
---

# Desktop Companion - Building the Reminder Engine and Electron IPC Communication

## Overview

Today I continued development on Desktop Companion by moving the application beyond a static reminder widget and into a proper background-capable desktop application.

The main focus was creating the foundation for reminders to run independently from the UI and communicate back to the React frontend through Electron.

The goal was to move toward a structure where the application can:

* Track reminder schedules
* Detect when reminders are due
* Notify the UI when actions are required
* Eventually support notifications, sounds, and other desktop features

---

# Reminder Data Refactor

Before adding timers, I needed a better structure for reminder data.

Originally, reminders only contained display information:

* Title
* Icon
* Interval

The reminder model was expanded to include runtime state:

```ts
interface Reminder {
    id: number
    icon: string
    title: string
    intervalMinutes: number
    enabled: boolean
    completed: boolean
    triggered: boolean
    nextTrigger: Date
}
```

This separates the reminder configuration from its current state.

For example:

* `enabled` controls whether a reminder should run
* `completed` tracks whether the user finished the reminder
* `triggered` tracks whether the reminder has fired
* `nextTrigger` determines when it should run again

---

# Reminder Service

Reminder data was moved into its own service layer.

The service is responsible for managing reminder operations:

```text
Reminder Service

- getReminders()
- getReminder(id)
- toggleReminder(id)
- completeReminder(id)
```

This keeps UI components from directly modifying application data.

The structure is now:

```text
WidgetPage
     |
     v
Reminder Service
     |
     v
Reminder Data
```

This makes it easier to expand the application later without placing all logic inside React components.

---

# Creating the Reminder Engine

The next step was creating a dedicated reminder engine.

The engine is responsible for checking time-based conditions.

Instead of the UI managing timers, the application now has a separate process responsible for reminder logic.

The flow became:

```text
Reminder Engine

Every second:
    |
    v
Check reminders
    |
    v
Is nextTrigger reached?
    |
    v
Trigger reminder
```

When a reminder is triggered, the engine updates the next trigger time:

```ts
nextTrigger = new Date(
    Date.now() + intervalMinutes * 60 * 1000
)
```

This prevents reminders from firing repeatedly every second.

---

# Reminder Events

After creating the engine, the next challenge was communication.

The reminder engine knows when something happens, but the UI needs to know as well.

Instead of coupling the engine directly to React or Electron, an event system was added.

The new flow:

```text
Reminder Engine
        |
        v
Reminder Event
        |
        v
Other systems can subscribe
```

This keeps the engine independent.

Future features can subscribe to these events:

* Widget updates
* Desktop notifications
* Sounds
* Reminder history

---

# Electron IPC Communication

The biggest milestone today was connecting the background logic to the React UI.

Electron applications have two separate environments:

## Main Process

Responsible for:

* Window management
* Tray integration
* Background processes
* Reminder engine

## Renderer Process

Responsible for:

* React components
* User interface
* Displaying reminder state

These processes cannot directly share data.

The communication flow is now:

```text
Reminder Engine
        |
        v
Electron Main Process
        |
        | IPC
        v
React Renderer
        |
        v
Widget UI
```

The Electron main process receives reminder events and forwards them using IPC:

```ts
win.webContents.send(
    'reminder-triggered',
    reminder
)
```

The React application can now listen for these events:

```ts
window.ipcRenderer.on(
    'reminder-triggered',
    (_, reminder) => {
        console.log(reminder)
    }
)
```

Testing confirmed that reminders successfully travel from the background engine into the React application.

---

# Current Architecture

The project has evolved from a simple widget into a more structured desktop application:

```text
Desktop Companion

Electron
|
├── Main Process
│   ├── Window Management
│   ├── System Tray
│   └── Reminder Engine
│
├── IPC Communication
│
└── React Renderer
    ├── Widget Page
    └── Reminder Cards
```

---

# Next Steps

The next goal is making triggered reminders visible to the user.

Currently:

* The engine detects reminders
* Electron forwards events
* React receives them

The next feature will be:

* Display active reminders in the widget
* Add a triggered reminder state
* Allow users to complete active reminders
* Prepare for future desktop notifications

The application is moving from simply displaying reminders to actively managing them.
