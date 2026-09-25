---
title: "Workout Tracker - Building the Initial Workout Tracking Flow"
date: 2026-09-25
---

# Workout Tracker - Building the Initial Workout Tracking Flow

## Overview

Today I continued development on Workout Tracker by moving the project from a basic workout display into a functional workout tracking flow.

The main focus was:

- Building the exercise tracking screen
- Adding set-by-set weight and rep input
- Creating reusable components
- Adding local data persistence
- Testing that workout data survives navigation and app restarts

The basic flow is now:

```text
Workout Plan
     |
     v
Exercise
     |
     v
Enter Sets
     |
     v
Save Workout
     |
     v
Load Saved Data
```

---

# Workout Tracking

The workout screen now displays exercises from the workout plan using reusable `ExerciseCard` components.

Selecting an exercise opens a dedicated exercise screen through Expo Router:

```text
/exercise/[id]
```

The exercise screen includes:

- Exercise name and category
- Target sets and reps
- Set-by-set weight and rep inputs
- Add Set functionality
- Save Workout functionality

A reusable `SetRow` component was also created for entering individual sets:

```text
Set    Weight    Reps
 1     [ 90 ]    [10]
 2     [ 90 ]    [10]
 3     [ 90 ]    [ 9 ]
```

The workout set model currently supports weight, reps, duration, and difficulty so it can eventually support different types of exercises.

---

# Local Persistence

AsyncStorage was added to provide local workout persistence.

A separate storage layer was created:

```text
src/storage/workouts.ts
```

It currently handles:

```text
saveWorkout()
loadWorkout()
```

The structure is:

```text
Exercise Screen
      |
      v
Storage Functions
      |
      v
AsyncStorage
```

This keeps persistence logic separate from the UI.

I tested the system using a Cable Row workout:

```text
90 × 10
90 × 10
90 × 9
```

After saving, I left and reopened the exercise and also closed and reopened Expo Go. The recorded data was still present, confirming that the workout was being persisted locally.

---

# TypeScript Fixes

A few TypeScript issues came up while building the exercise screen, particularly around optional values and exercise lookups.

These were resolved by properly narrowing the exercise data before accessing it.

The screen also now handles invalid exercise IDs by displaying:

```text
Exercise not found.
```

---

# Current Architecture

The project now has a basic separation of responsibilities:

```text
Workout Tracker

├── app/          → Screens and navigation
├── components/   → Reusable UI
├── data/         → Workout plans and exercises
├── types/        → TypeScript models
├── storage/      → Local persistence
└── constants/    → Shared values
```

The application can now go from a predefined workout plan to recording and saving actual workout performance.

---

# Current Limitation

The current storage implementation only keeps one saved workout per exercise.

Saving Cable Row again will replace the previous Cable Row data.

This was intentional for the initial persistence milestone, but the next step is to build proper workout history where every completed session is preserved.

The eventual structure will be:

```text
Workout Session
      |
      ├── Exercise Performance
      │       ├── Set 1
      │       ├── Set 2
      │       └── Set 3
      |
      └── Exercise Performance
              ├── Set 1
              └── Set 2
```

This will provide the foundation for exercise history, progress calculations, and graphs.

---

# Next Steps

The next major goal is replacing the current single-workout storage model with proper workout sessions and historical performance tracking.

After that, the project can move toward:

- Exercise history
- Progress calculations
- Progress graphs
- Offline-first storage
- Supabase cloud storage
- Multiple users and data synchronization

The first functional version of the workout tracking flow is now working end-to-end.