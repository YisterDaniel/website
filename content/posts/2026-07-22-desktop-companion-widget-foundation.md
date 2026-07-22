---
title: "Desktop Companion - Building the Widget Foundation"
date: 2026-07-22
tags:
  - electron
  - react
  - typescript
  - desktop-app
  - desktop-companion
---

## Overview

Today I continued development on my Desktop Companion Electron application.

The goal of this project is to build a lightweight desktop utility that sits quietly on the user's screen and helps with small productivity reminders such as drinking water, stretching, and taking screen breaks.

Today's focus was building the foundation required for the application to behave like a real desktop widget.

---

## What I Worked On

Today I implemented several core pieces of functionality:

- Custom draggable header
- Saved widget position
- Typed reminder data
- React component structure
- System tray integration
- Background show/hide behavior

---

## Building the Widget Structure

The project started with organizing the React side of the application into smaller reusable components.

The structure now separates responsibilities:

```
src/
├── components/
│   └── ReminderCard/
│       └── ReminderCard.tsx
│
├── pages/
│   └── WidgetPage/
│       └── WidgetPage.tsx
│
├── types/
│   └── reminder.ts
│
└── styles/
    └── widget.css
```

The widget page is responsible for displaying reminders, while individual reminder cards handle their own UI.

Instead of hardcoding separate UI elements, reminders are now represented as typed data:

```ts
const reminders: Reminder[] = [
    {
        id: 1,
        icon: '💧',
        title: 'Water',
        interval: '30 min',
    },
    {
        id: 2,
        icon: '🧍',
        title: 'Stretch',
        interval: '60 min',
    },
]
```

This creates a cleaner foundation for later features such as dynamic reminders, user settings, and saved preferences.

---

## Adding a Draggable Widget

Since the application uses a frameless Electron window, the default operating system window controls are removed.

This means the widget needed a custom way to move around the screen.

I added a draggable header region using Electron's window dragging support.

The result is a floating widget that can be repositioned naturally without requiring a traditional application frame.

---

## Saving Widget Position

After adding movement, the next problem was persistence.

Without saving the position, the widget would return to the default location every time the application restarted.

I added persistent storage using `electron-store`.

The application now saves:

```ts
{
    x: windowPosition.x,
    y: windowPosition.y
}
```

When the application starts:

1. Stored position is loaded
2. The window is created at that location
3. The user can continue using the widget from where they left it

This creates a much more natural desktop application experience.

---

## Adding System Tray Support

The next step was making the application behave more like a background utility.

A desktop companion should not need to stay open in the taskbar. Instead, it should be accessible through the system tray.

I added a dedicated tray module:

```
electron/
├── main.ts
├── tray.ts
└── store.ts
```

The tray now supports:

- Show Widget
- Hide Widget
- Quit

The application can now continue running while the widget is hidden.

---

## Solving the Window Flash Issue

While implementing tray controls, I ran into an issue with transparent frameless windows.

Using Electron's normal hide/show behavior:

```ts
win.hide()
win.show()
```

caused the widget to briefly flash when being restored.

After debugging, I confirmed:

- Only one window was being created
- Tray events were only firing once
- The issue was related to Electron's handling of transparent window rendering

The solution was keeping the window alive and controlling visibility through opacity:

```ts
win.setOpacity(0)
```

to hide the widget, and:

```ts
win.setOpacity(1)
win.showInactive()
```

to restore it.

This removed the flickering and created a smoother desktop widget experience.

---

## What I Learned

Today's work introduced several important Electron concepts.

### Main Process vs Renderer Process

The React interface belongs in the renderer process, while operating system features belong in Electron's main process.

Examples:

Renderer:

- Widget UI
- Reminder cards
- Styling

Main process:

- Window management
- System tray
- Persistent storage

Keeping these responsibilities separate makes the application easier to maintain.

---

### Desktop Applications Need Different Design Considerations

A floating widget is not the same as a normal application window.

Things like:

- transparency
- window visibility
- focus behavior
- native menus

all require different approaches compared to a standard web application.

---

## Current Progress

The Desktop Companion currently supports:

✅ Transparent floating widget  
✅ Always-on-top behavior  
✅ Custom draggable header  
✅ Saved widget position  
✅ Typed reminder data  
✅ React component structure  
✅ System tray integration  
✅ Background show/hide behavior  

---

## Next Steps

The application now has the foundation needed to become a real reminder tool.

The next focus will be moving from static reminder cards into actual functionality:

- Reminder timer engine
- Countdown tracking
- Windows notifications
- User-configurable intervals
- Reminder completion states

The goal is to evolve the widget from a visual component into an actual productivity companion.