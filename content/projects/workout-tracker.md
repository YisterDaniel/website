---
title: "Workout Tracker"
date: 2026-09-25
lastmod: 2026-09-25
---

# Workout Tracker

## Goal

Build a personal workout tracking application that makes it easy to follow workout plans, record performance, and track progress over time.

The goal is to eventually support workout history, progress tracking, graphs, and multiple users while keeping workout data available offline.

## Technologies

- Expo
- React Native
- TypeScript
- Expo Router
- AsyncStorage
- Supabase
- PostgreSQL

## Current Status

Early development.

Current features:

- Workout plan display
- Exercise-specific workout screens
- Set tracking
- Weight and rep input
- Add set functionality
- Local workout data persistence
- Basic exercise and workout plan data models
- TypeScript type checking
- File structure separating screens, components, data, types, and storage

## Architecture

The application is currently separated into:

- Expo Router screens for navigation and app pages
- React Native components for reusable UI
- Workout data and exercise definitions
- TypeScript types for workout-related data
- Local storage for saved workout data

The planned architecture will eventually separate:

- Workout plans and target prescriptions
- Completed workout sessions
- Exercise performances
- Individual sets and recorded performance
- Local offline data
- Cloud data and synchronization

## Repository

https://github.com/YisterDaniel/workout-tracker