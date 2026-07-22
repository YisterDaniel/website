---
title: "Desktop Companion"
date: 2026-06-24
lastmod: 2026-07-22
---

# Desktop Companion

## Goal

Build an interactive desktop companion that can grow over time.

The goal is to create a lightweight desktop utility that helps users build better habits through customizable reminders and background assistance.

## Technologies

- Electron
- React
- TypeScript
- Vite
- electron-store

## Current Status

Early development.

Current features:

- Transparent always-on-top desktop widget
- Custom draggable widget header
- Saved widget position between sessions
- Typed reminder data structure
- React component-based UI
- System tray integration
- Background show/hide behavior

## Architecture

The application is currently separated into:

- React renderer for the widget interface
- Electron main process for desktop functionality
- Persistent storage for user preferences

## Repository

https://github.com/YisterDaniel/desktop-companion